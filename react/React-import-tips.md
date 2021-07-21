# 重点内容记录

### 创建组件

```js
//##类组件##
// 1. 普通
class XXX extends React.Component{}
// 2. 纯组件
class XXX extends React.PureComponent{}
// 3. createReactClass
var XXX = createReactClass({ 
  render: function() {
    return <h1>Hello, {this.props.name}</h1>;
  }
});
//##函数组件##
```

继承方式和`createReactClass`的区别：

1. 初始化：

   * 默认属性`defaultProps：createReactClass`方式使用函数`getDefaultProps`来处理
   * `state`：`createReactClass`方式使用函数`getInitialState`来处理

2. `this`的自动绑定：

   继承方式`this`不自动绑定，函数可以通过`bind`绑定`this`，而`createReactClass`方式的`this`自动绑定好了的

   ```
   // 这里说的箭头函数指的是函数属性上的箭头函数☞
   class XXX extends React.Component {
     // 警告：这种语法还处于试验性阶段！
     // 在这里使用箭头函数就可以把方法绑定给实例：
     handleClick = () => {
       alert(this.state.message);
     }
   }
   // render函数中声明的箭头函数，与这里不同，由于render函数内部this就是组件实例，所以能够正常绑定
   ```

   

   > 注意：我们常用的箭头函数方式绑定 this 是一个处于试验阶段的方式，他需要与bable插件[Class Properties](https://babeljs.io/docs/plugins/transform-class-properties/)来实现
   >
   > 为了保险起见，以下三种做法都是可以的：
   >
   > - 在 constructor 中绑定方法。
   > - 使用箭头函数，比如：`onClick={(e) => this.handleClick(e)}`。
   > - 继续使用 `createReactClass`。

3. mixins

   ```
   createReactClass({
     mixins: [anyMixin] // 可通过数组混入多个
   });
   ```

   > 混入的相同生命周期方法会按数组顺序依次执行，最后执行组件本身的生命周期方法

### 类组件的api

类组件才以下api（包括生命周期，状态...），函数组件只能通过一些方式模拟，官方提供hooks就是最好的方式

##### 挂载

当组件实例被创建并插入 DOM 中时，其生命周期调用顺序如下：

- [**`constructor()`**](https://zh-hans.reactjs.org/docs/react-component.html#constructor)
- [`static getDerivedStateFromProps()`](https://zh-hans.reactjs.org/docs/react-component.html#static-getderivedstatefromprops)
- [**`render()`**](https://zh-hans.reactjs.org/docs/react-component.html#render)
- [**`componentDidMount()`**](https://zh-hans.reactjs.org/docs/react-component.html#componentdidmount)

##### 更新

当组件的 props 或 state 发生变化时会触发更新。组件更新的生命周期调用顺序如下：

* [`static getDerivedStateFromProps()`](https://zh-hans.reactjs.org/docs/react-component.html#static-getderivedstatefromprops)
* [`shouldComponentUpdate()`](https://zh-hans.reactjs.org/docs/react-component.html#shouldcomponentupdate)
* [**`render()`**](https://zh-hans.reactjs.org/docs/react-component.html#render)
* [`getSnapshotBeforeUpdate()`](https://zh-hans.reactjs.org/docs/react-component.html#getsnapshotbeforeupdate)
* [**`componentDidUpdate()`**](https://zh-hans.reactjs.org/docs/react-component.html#componentdidupdate)

##### 卸载

当组件从 DOM 中移除时会调用如下方法：

- [**`componentWillUnmount()`**](https://zh-hans.reactjs.org/docs/react-component.html#componentwillunmount)

##### 错误处理

当渲染过程，生命周期，或子组件的构造函数中抛出错误时，会调用如下方法：

- [`static getDerivedStateFromError()`](https://zh-hans.reactjs.org/docs/react-component.html#static-getderivedstatefromerror)

- [`componentDidCatch()`](https://zh-hans.reactjs.org/docs/react-component.html#componentdidcatch)

  > 注意
  >
  > 如果发生错误，你可以通过调用 `setState` 使用 `componentDidCatch()` 渲染降级 UI，但在未来的版本中将不推荐这样做。 可以使用静态 `getDerivedStateFromError()` 来处理降级渲染。

##### 其他 APIs

组件还提供了一些额外的 API：

- [`setState()`](https://zh-hans.reactjs.org/docs/react-component.html#setstate)
- [`forceUpdate()`](https://zh-hans.reactjs.org/docs/react-component.html#forceupdate)

##### class 属性

- [`defaultProps`](https://zh-hans.reactjs.org/docs/react-component.html#defaultprops)
- [`displayName`](https://zh-hans.reactjs.org/docs/react-component.html#displayname)

##### 实例属性

- [`props`](https://zh-hans.reactjs.org/docs/react-component.html#props)
- [`state`](https://zh-hans.reactjs.org/docs/react-component.html#state)

##### 关于事件处理器的那些事

**事件处理器绑定this**：

* 在 render 方法中使用 `Function.prototype.bind` 会在每次组件渲染时创建一个新的函数，可能会影响性能
* 在 render 方法中使用箭头函数也会在每次组件渲染时创建一个新的函数，这会破坏基于恒等比较的性能优化。

**传递参数给事件处理器**：

* 可以使用箭头函数包裹事件处理器，并传递参数

* 调用 `.bind` 

* 以上两个方案在性能上都会有影响，这里是考虑性能的第三个方案：通过 data-attributes 传递参数

  ```
  //代码片段1 - 使用dom自带的自定义参数
  <li key={letter} data-letter={letter} onClick={this.handleClick}> {letter} </li>
  //代码片段2 - 事件中通过事件对象获取对应值
   handleClick(e) {
      this.setState({
        justClicked: e.target.dataset.letter
      });
    }
  ```

**高频触发的事件优化**：

- **节流**：基于时间的频率来进行抽样更改 (例如 [`_.throttle`](https://lodash.com/docs#throttle))

- **防抖**：一段时间的不活动之后发布更改 (例如 [`_.debounce`](https://lodash.com/docs#debounce))

- **`requestAnimationFrame` 节流**：基于 requestAnimationFrame 的抽样更改 (例如 [raf-schd](https://zh-hans.reactjs.org/docs/[](https://github.com/alexreardon/raf-schd)))

  > **注意：**
  >
  > requestAnimationFrame 只能获取某一帧中最后发布的值。也可以在 [`MDN`](https://developer.mozilla.org/en-US/docs/Web/Events/scroll) 中看优化的示例。

### setState

`setState()` 异步处理的情况：

1. 合成事件中
2. hook中（包含生命周期函数和hook）

因为生命**合成事件**和**hook**的调用顺序在更新之前，react会在这些函数调用时，将isBatchingUpdates 标志变量修改。而通过isBatchingUpdates来判断是先存进state队列还是直接更新。值为true则执行异步操作，false则直接更新

> 总结：
>
> * `setState` 只在合成事件和hook（）中是“异步”的，在**原生事件和 `setTimeout`** 中都是**同步**的。
>
> * `setState`的“异步”并不是说内部由异步代码实现，其实本身执行的过程和代码都是同步的，只是**合成事件和hook（）的调用顺序在更新之前**，导致在合成事件和钩子函数中没法立马拿到更新后的值，形式了所谓的“异步”，当然可以通过第二个参数 setState(partialState, **callback**) 拿到更新后的结果。
> * `setState` 的批量更新优化也是建立在“异步”（合成事件、hook（））之上的，**在原生事件和setTimeout 中不会批量更新**，在“异步”中如果对同一个值进行多次 `setState` ， `setState` 的批量更新策略会对其进行覆盖，取**最后一次的执行**，如果是同时 `setState` 多个不同的值，在更新时会**对其进行合并批量更新**。

```
 // 两个很典型的事例： 判断最终输出结果
 // 1题 判断执行时机
 componentDidMount() {
        this.setState({ val: this.state.val + 1 })
        console.log(this.state.val) // 输出1： 0

        this.setState({ val: this.state.val + 1 })
        console.log(this.state.val) // 输出2： 0

        setTimeout( () => {
            this.setState({ val: this.state.val + 1 })
            console.log(this.state.val) // 输出3： 2

            this.setState({ val: this.state.val + 1 })
            console.log(this.state.val) // 输出4： 3
        }, 0)
    }
 // 2题 判断执行队列
ComponentDidMount（）{ 
     this.setState({ val: this.state.val + 1 })
        console.log('console: ' + this.state.val) // 输出1： 0
        this.setState({ val: this.state.val + 1 }, () => {
            console.log('console from callback: ' + this.state.val) // 输出3： 2
        })
        this.setState(
            prevState => {
                console.log('console from func: ' + prevState.val) // 输出2： 1
                return { val: prevState.val + 1 }
            },
            () => console.log('last console: ' + this.state.val) //输出4： 2
        )
}
```

### 注入数据：Context

##### **创建：**

```
// 1.创建 Context
export const ToggleContext = createContext({
    toggle: true,
    handleToggle: () => {}
})
// 2.创建 Provider
export class ToggleProvider extends React.Component {

    // 注意书写顺序；handleToggle 作为箭头函数不能 bind 因此需要写在上面；如果不喜欢这样的顺序则可以书写普通函数放在下面但记得 bind
    handleToggle = () => {
        this.setState({ toggle: !this.state.toggle })
    }

    // 2-1. 重写 state 
    state = {
        toggle: true,
        handleToggle: this.handleToggle
    }

    render() {
        // 2-2. 通过 Provider 组件的 value 将 state 提供出去
        return (
            <ToggleContext.Provider value={this.state}>
                {this.props.children}
            </ToggleContext.Provider>
        ) 
    }
}
// 3.创建 Consumer
export const ToggleConsumer = ToggleContext.Consumer
```

##### **使用：**

* 注入：

  ```js
  // ToggleProvider中已经将数据通过value属性注入了，这里直接包裹组件，数据就能向下传递了
  <ToggleProvider>
     <Switcher></Switcher>
     {/* 其他组件仍然可以通过 props 访问到共享的 state */}
  </ToggleProvider>
  ```

- 接收：

  ```
  // 我们provider上传入的value，这里都可以通过函数的第一个参数获取到
  <ToggleConsumer>
        {({ toggle, handleToggle }) => <div onClick={() => handleToggle()}>{ toggle ? '✔' : '❌'}</div>}
  </ToggleConsumer>
  ```

  

  



### 造成组件更新的原因及优化处理

### 合成事件

总结：

- React 所有事件都挂载在 `document` 对象上；
- 当真实 DOM 元素触发事件，会冒泡到 `document` 对象后，再处理 React 事件；
- 所以会先执行原生事件，然后处理 React 事件；
- 最后真正执行 `document` 上挂载的事件。

原生事件中如果执行了`stopPropagation`方法，则会导致其他`React`事件失效。因为所有元素的事件将无法冒泡到`document`上。

### 其它汇总

#### Refs向下转发

```js
const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="FancyButton">
    {props.children}
  </button>
));
// 你可以直接获取 DOM button 的 ref
// 注意：原本ref是绑定在FancyButton组件上，现在向下转发了，在button上
const ref = React.createRef();
<FancyButton ref={ref}>Click me!</FancyButton>;
```

> 注意
>
> 1. 第二个参数 `ref` 只在使用 `React.forwardRef` 定义组件时存在。常规函数和 class 组件不接收 `ref` 参数，且 props 中也不存在 `ref`。
>
> 2. Ref 转发不仅限于 DOM 组件，你也可以转发 refs 到 class 组件实例中。
> 3. refs 将不会透传下去。这是因为 `ref` 不是 prop 属性。就像 `key` 一样，其被 React 进行了特殊处理。如果你对 HOC 添加 ref，该 ref 将引用最外层的容器组件，而不是被包裹的组件。

#### Fragments

```
 <React.Fragment key={item.id}>  //使用显式语法声明的片段可能具有 key
   <dt>{item.term}</dt>
   <dd>{item.description}</dd>
</React.Fragment>
// 或者
 <>  //使用隐式语法声明的片段不能有 key ，因为会造成q
   <dt>{item.term}</dt>
   <dd>{item.description}</dd>
</>
```

#### 高级组件（HOC）

**高阶组件是参数为组件，返回值为新组件的函数。**

组件是将 props 转换为 UI，而高阶组件是将组件转换为另一个组件。

#### Portals

Portal 提供了一种将子节点渲染到存在于父组件以外的 DOM 节点的优秀的方案。

```js
import ReactDOM from 'react-dom'
ReactDOM.createPortal(child, container)
```

> 尽管 portal 可以被放置在 DOM 树中的任何地方，但在任何其他方面，其行为和普通的 React 子节点行为一致。由于 portal 仍存在于 *React 树*， 且与 *DOM 树* 中的位置无关，那么无论其子节点是否是 portal，像 context 这样的功能特性都是不变的。
>
> 这包含事件冒泡。一个从 portal 内部触发的事件会一直冒泡至包含 *React 树*的祖先，即便这些元素并不是 *DOM 树* 中的祖先。（也就是说无论dom上存不存在上下级关系，React的事件会遵从React虚拟dom的上下级关系来进行冒泡，反之原生事件则不能）
>
> ```
> <html>
>   <body>
>     <div id="app-root"></div>
>     <div id="modal-root"></div>
>   </body>
> </html>
> ```

#### Profiler API

```js
// 使用 
<Profiler id="Panel" onRender={onRenderCallback}>/*包裹需要测量的组件及内容*/</Profiler>
// 回调分析
function onRenderCallback(
  id, // 发生提交的 Profiler 树的 “id”
  phase, // "mount" （如果组件树刚加载） 或者 "update" （如果它重渲染了）之一
  actualDuration, // 本次更新 committed 花费的渲染时间
  baseDuration, // 估计不使用 memoization 的情况下渲染整颗子树需要的时间
  startTime, // 本次更新中 React 开始渲染的时间
  commitTime, // 本次更新中 React committed 的时间
  interactions // 属于本次更新的 interactions 的集合
) {
  // 合计或记录渲染时间。。。
}
```

#### diff 算法

##### 浅析：

* 对比不同类型的元素：React 会拆卸原有的树并且建立起新的树

* 对比同一类型的元素：React 会保留 DOM 节点，仅比对及更新有改变的属性。

* 对比同类型的“组件”元素：组件实例（state）会保持不变，更新props

* 对子节点进行递归：依次对比，所以头部插入开销很大可以通过key来优化

  > 1. 该算法不会尝试匹配不同组件类型的子树。如果你发现你在两种不同类型的组件中切换，但输出非常相似的内容，建议把它们改成同一类型。在实践中，我们没有遇到这类问题。
  > 2. Key 应该具有**稳定，可预测，以及列表内唯一的特质**。不稳定的 key（比如通过 `Math.random()` 生成的）会导致许多组件实例和 DOM 节点被不必要地重新创建，这可能导致性能下降和子组件中的状态丢失。

##### 深度解析(看源码先):

#### Refs and the DOM

**下面是几个适合使用 refs 的情况**：

- 管理焦点，文本选择或媒体播放。
- 触发强制动画。
- 集成第三方 DOM 库。

**ref 的值根据节点的类型而有所不同**：

- 当 `ref` 属性用于 HTML 元素时，构造函数中使用 `React.createRef()` 创建的 `ref` 接收底层 DOM 元素作为其 `current` 属性。
- 当 `ref` 属性用于自定义 class 组件时，`ref` 对象接收组件的挂载实例作为其 `current` 属性。
- **你不能在函数组件上使用 `ref` 属性**，因为他们没有实例。

> 注意：
>
> 1. React 会在组件挂载时给 `current` 属性传入 DOM 元素，并在组件卸载时传入 `null` 值。`ref` 会在 `componentDidMount` 或 `componentDidUpdate` 生命周期钩子触发前更新。
> 2. 如果要在函数组件中使用 `ref`，你可以使用 [`forwardRef`](https://zh-hans.reactjs.org/docs/forwarding-refs.html)（可与 [`useImperativeHandle`](https://zh-hans.reactjs.org/docs/hooks-reference.html#useimperativehandle) 结合使用），或者可以将该组件转化为 class 组件。不管怎样，你可以**在函数组件内部使用 `ref` 属性**，只要它指向一个 DOM 元素或 class 组件。

#### Render Props

**render prop 是一个用于告知组件需要渲染什么内容的函数 prop**

>  一般我们的组件使共享整套逻辑（ui和行为），而render prop这项技术使我们单独的共享行为非常容易

#### 严格模式

严格模式会在**开发模式下**采用故意重复调用方法的方式来审查组件，重复调用的函数如下：

- class 组件的 `constructor`，`render` 以及 `shouldComponentUpdate` 方法
- class 组件的生命周期方法 `getDerivedStateFromProps`
- 函数组件体
- 状态更新函数 (即 `setState` 的第一个参数）
- 函数组件通过使用 `useState`，`useMemo` 或者 `useReducer`

所以这里告数我们这些函数**都应该是纯函数，不能有副作用**

具体使用：

```js
 <React.StrictMode>
```

`StrictMode` 目前有助于：

- [识别不安全的生命周期](https://zh-hans.reactjs.org/docs/strict-mode.html#identifying-unsafe-lifecycles)
- [关于使用过时字符串 ref API 的警告](https://zh-hans.reactjs.org/docs/strict-mode.html#warning-about-legacy-string-ref-api-usage)
- [关于使用废弃的 findDOMNode 方法的警告](https://zh-hans.reactjs.org/docs/strict-mode.html#warning-about-deprecated-finddomnode-usage)
- [检测意外的副作用](https://zh-hans.reactjs.org/docs/strict-mode.html#detecting-unexpected-side-effects)
- [检测过时的 context API](https://zh-hans.reactjs.org/docs/strict-mode.html#detecting-legacy-context-api)

#### 副作用

副作用（Side effect）指一个function做了和本身运算返回值无关的事。比如：修改了全局变量、修改了入参、console.log()...

比较经典的ajax操作、修改dom都是副作用

#### 使用 [`prop-types` 库](https://www.npmjs.com/package/prop-types)

非ts情况的类型检测：

```js
import PropTypes from 'prop-types';

class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

Greeting.propTypes = {
  name: PropTypes.string
};
```

```js
import PropTypes from 'prop-types'

function HelloWorldComponent({ name }) {
  return (
    <div>Hello, {name}</div>
  )
}

HelloWorldComponent.propTypes = {
  name: PropTypes.string
}

export default HelloWorldComponent
```

#### 非受控组件

值的获取：

非受控组件一般通过ref来获取组件的值

默认值：

React 给表单元素提供了defaultValue属性（只在挂载时更新一次），用来设置默认值

[文件输入](https://developer.mozilla.org/zh-CN/docs/Web/API/File/Using_files_from_web_applications)：

在 React 中，`<input type="file" />` 始终是一个非受控组件，因为它的值只能由用户设置，而不能通过代码控制。

#### Web Components

原生的组件定义与使用方法

#### react-dom

- [`render()`](https://zh-hans.reactjs.org/docs/react-dom.html#render)
- [`hydrate()`](https://zh-hans.reactjs.org/docs/react-dom.html#hydrate)：服务器端渲染，SSR
- [`unmountComponentAtNode()`](https://zh-hans.reactjs.org/docs/react-dom.html#unmountcomponentatnode)：从 DOM 中卸载组件
- [`findDOMNode()`](https://zh-hans.reactjs.org/docs/react-dom.html#finddomnode)：严格模式下该方法已弃用。
- [`createPortal()`](https://zh-hans.reactjs.org/docs/react-dom.html#createportal)

### HOOK

##### 简介：

- [基础 Hook](https://zh-hans.reactjs.org/docs/hooks-reference.html#basic-hooks)
  - [`useState`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usestate)
  - [`useEffect`](https://zh-hans.reactjs.org/docs/hooks-reference.html#useeffect)
  - [`useContext`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usecontext)
- [额外的 Hook](https://zh-hans.reactjs.org/docs/hooks-reference.html#additional-hooks)
  - [`useReducer`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usereducer)
  - [`useCallback`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usecallback)
  - [`useMemo`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usememo)
  - [`useRef`](https://zh-hans.reactjs.org/docs/hooks-reference.html#useref)
  - [`useImperativeHandle`](https://zh-hans.reactjs.org/docs/hooks-reference.html#useimperativehandle)
  - [`useLayoutEffect`](https://zh-hans.reactjs.org/docs/hooks-reference.html#uselayouteffect)
  - [`useDebugValue`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usedebugvalue)

##### 使用规则：

1. 只在最顶层使用 Hook，**不要在循环，条件或嵌套函数中调用 Hook**， 确保总是在你的 React 函数的最顶层以及任何 return 之前调用他们。

   >  React 通过 Hook 调用的顺序知道哪个 state 对应哪个 `useState`.（内部实现是一个列表）条件或者循环语句会影响列表元素的顺序和数量，造成不能一一对应的问题。

2. 只在 React 函数中（ React 的函数组件和自定义 Hook）调用 Hook，不要在普通的 JavaScript 函数中调用 Hook。

>  可以通过官方提供的lint矫正平时hook书写的错误-- [`eslint-plugin-react-hooks`](https://www.npmjs.com/package/eslint-plugin-react-hooks) 

##### 自定义hook

### Fiber - The official introduction

##### goal of Fiber：

- pause work and come back to it later. 暂停工作，稍后再回来
- assign priority to different types of work. 优先处理不同类型的工作
- reuse previously completed work. 重复使用先前已完成的工作
- abort work if it's no longer needed. 如果不再需要，就中止工作

hat's the purpose of React Fiber. Fiber is reimplementation of the stack, specialized for React components. You can think of a single fiber as a **virtual stack frame**.（这就是反应纤维的用途。是堆栈的重新实现，专门用于 React 组件。您可以将单个光纤看作一个虚拟堆栈帧。）

The advantage of reimplementing the stack is that you can [keep stack frames in memory](https://www.facebook.com/groups/2003630259862046/permalink/2054053404819731/) and execute them however (and *whenever*) you want. This is crucial for accomplishing the goals we have for scheduling.（重新实现堆栈的好处是，您可以将堆栈帧保存在内存中，并随时执行它们。这对于实现我们的日程安排目标至关重要。）

##### Structure of a fiber：

`type` and `key`

* The type of a fiber describes the component that it corresponds to. For composite components, the type is the function or class component itself. For host components (`div`, `span`, etc.), the type is a string. Conceptually, the type is the function (as in `v = f(d)`) whose execution is being tracked by the stack frame.
*  the key is used during reconciliation to determine whether the fiber can be reused.

`child` and `sibling`

* The child fibers form a singly-linked list whose head is the first child. So in this example, the child of `Parent` is `Child1` and the sibling of `Child1` is `Child2`.

`return`

* The return fiber is the fiber to which the program should return after processing the current one. It is conceptually the same as the return address of a stack frame. It can also be thought of as the parent fiber.

`pendingProps` and 及`memoizedProps`

* Conceptually, props are the arguments of a function. A fiber's `pendingProps` are set at the beginning of its execution, and `memoizedProps` are set at the end.

* When the incoming `pendingProps` are equal to `memoizedProps`, it signals that the fiber's previous output can be reused, preventing unnecessary work.

`pendingWorkPriority`

* A number indicating the priority of the work represented by the fiber. The [ReactPriorityLevel](https://github.com/facebook/react/blob/master/src/renderers/shared/fiber/ReactPriorityLevel.js) module lists the different priority levels and what they represent.

`flush`

* To flush a fiber is to render its output onto the screen.