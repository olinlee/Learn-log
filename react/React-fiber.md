# React-fiber （16）

✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘

React-fiber：`React`内部实现的一套状态更新机制。支持任务不同`优先级`，可中断与恢复，并且恢复后可以复用之前的`中间状态`。其中每个任务更新单元为`React Element`对应的`Fiber节点`。

`Fiber`包含三层含义：

1. 作为架构来说，之前`React15`的`Reconciler`采用递归的方式执行，数据保存在递归调用栈中，所以被称为`stack Reconciler`。`React16`的`Reconciler`基于`Fiber节点`实现，被称为`Fiber Reconciler`。
2. 作为静态的数据结构来说，每个`Fiber节点`对应一个`React element`，保存了该组件的类型（函数组件/类组件/原生组件...）、对应的DOM节点等信息。
3. 作为动态的工作单元来说，每个`Fiber节点`保存了本次更新中该组件改变的状态、要执行的工作（需要被删除/被插入页面中/被更新...）。

### 架构简介

- Scheduler（调度器）—— 调度任务的优先级，高优任务优先进入**Reconciler**
- Reconciler（协调器）—— 负责找出变化的组件
- Renderer（渲染器）—— 负责将变化的组件渲染到页面上

### 双缓存Fiber树

当前屏幕上显示内容对应的`Fiber树`称为`current Fiber树`，正在内存中构建的`Fiber树`称为`workInProgress Fiber树`。

两棵树的fiber节点之间相互通过`alternate`属性连接。

```js
currentFiber.alternate === workInProgressFiber;
workInProgressFiber.alternate === currentFiber;
```

`React`应用的根节点通过使`current`指针在不同`Fiber树`的`rootFiber`间切换来完成`current Fiber`树指向的切换（完成更新）。

* ##### mount

  > 页面第一次挂载时：
  >
  > 1. 首次执行时， `ReactDom.render` 会创建 `fiberRootNode` 和 `rootFiber`。
  >
  >    ```js
  >    // fiberRootNode是整个应用的挂载点（只有一个）， rootFiber是组件树的根节点（可以改变）
  >    fiberRootNode.current = rootFiber;
  >    ```
  >
  >    由于是第一次渲染，目前还没有还没有挂载`dom`，`fiberRootNode.current` 指向的 `rootFiber` 没有任何子节点（`current fiber` 树为空）
  >
  > 2. 接下来进入`render`阶段，`render`函数执行完成在内存中生成`workInProgressFiber树`，但是`currentFiber`树还是空的。（在构建`workInProgressFiber树`阶段，会尝试复用`currentFiber树`）
  > 3. 在`commit`阶段，会将上一阶段生成的树渲染到页面上，并且将`currentFiber树`指向该树（也就是`workInProgressFiber树`转变为了`current Fiber树`）

* ##### update

  >页面出现更新时：
  >
  >1. 同挂载，`render`阶段，构建一棵`workInProgressFiber树` -- 会尝试复用`current`树：这个决定是否复用的过程就是Diff算法
  >2. 同挂载，`commit`阶段，渲染页面，并且将`currentFiber树`指向`workInProgressFiber树`。

### React项目结构

```
1. react (opens new window)文件夹，包含所有全局 React API，如：
 - React.createElement
 - React.Component
 - React.Children
2. scheduler (opens new window)文件夹，Scheduler（调度器）的实现。
3. shared (opens new window)文件夹，源码中其他模块公用的方法和全局变量。
4. Renderer相关的文件夹
 - react-art
 - react-dom                 # 注意这同时是DOM和SSR（服务端渲染）的入口
 - react-native-renderer
 - react-noop-renderer       # 用于debug fiber（后面会介绍fiber）
 - react-test-renderer
5. react-reconciler (opens new window)文件夹*************

其它。。。
```

### 理解JSX

`JSX`是一种描述当前组件内容的数据结构，他不包含组件**schedule**、**reconcile**、**render**所需的相关信息。

比如如下信息就不包括在`JSX`中：

- 组件在更新中的`优先级`
- 组件的`state`
- 组件被打上的用于**Renderer**的`标记`

这些内容都包含在`Fiber节点`中。

所以，在组件`mount`时，`Reconciler`根据`JSX`描述的组件内容生成组件对应的`Fiber节点`。

在`update`时，`Reconciler`将`JSX`与`Fiber节点`保存的数据对比，生成组件对应的`Fiber节点`，并根据对比结果为`Fiber节点`打上`标记`。

### Render阶段

`render阶段`的工作可以分为“递”阶段和“归”阶段。其中“递”阶段会执行`beginWork`，“归”阶段会执行`completeWork`。

![Fiber架构](https://react.iamkasong.com/img/fiber.png)

✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘✘



