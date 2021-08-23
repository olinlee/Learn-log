# questions

1. 说一下你认为比较有亮点的功能

   \

2. 我看你之前还写过服务端渲染

   \

3. 你们的项目做过错误兼容和埋点吗

   

4. 说一下你的开源项目

5. 关于打包工具你是怎么考虑的

6. 你感觉rollup和webpack有什么区别

   ```
   Gulp 是一个基于任务驱动的自动化构建工具。
   Webpack 是当下最热门的前端资源模块化 管理和打包工具。它可以将许多松散的模块按照依赖和规则打包成符合生产环境部署的前端资源。还可以将按需加载的模块进行代码分割，等到实际需要的时候再异步加载。
   Rollup 是一个 JavaScript 模块打包器，可以将小块代码编译成大块复杂的代码，例如 library 或应用程序。
   Rollup 对代码模块使用新的标准化格式，这些标准都包含在 JavaScript 的 ES6 版本中，而不是以前的特殊解决方案，如 CommonJS 和 AMD。ES6 模块可以使你自由、无缝地使用你最喜爱的 library 中那些最有用独立函数，而你的项目不必携带其他未使用的代码。ES6 模块最终还是要由浏览器原生实现，但当前 Rollup 可以使你提前体验。
   简单点来说，就是 gulp 适合小项目，基于流程构建；webpack 适用于大型的应用项目，以模块划分，按需加载；而 rollup 适用于工具库的构建，优化代码。现在前端的 vue、 react 框架都是用 rollup 来打包的，也有一些 ui 框架的打包也是用的 rollup。
   ```

7. 如果是让你封装一个组件库, 这种按需引入怎么做

   ```
   1. 首先组件库应该先对每个单独组件分包
   2. 使用插件配置对应的组件库
   babel-plugin-import
   // .babelrc
   {
     "plugins": [
       [
         "import", {
           "libraryName": "react-ui-components-library",
           "libraryDirectory": "lib/components",
           "camel2DashComponentName": false
         }
       ]
     ]
   }
   ```

   

8. 你觉得CJS和ES6 Moudule直接有什么区别

   ```
   cjs: 1. CJS 是同步导入模块 2. 当 CJS 导入时，它会给你一个导入对象的副本 3. CJS 不能在浏览器中工作。它必须经过转换和打包
   ES6: 1. 它兼具两方面的优点：具有 CJS 的简单语法和 AMD 的异步 2. 在很多现代浏览器可以使用
   ```

   

9. react有用过吗, react@17有了解过吗

   ```
   - [一、全新的 JSX 转换](https://juejin.cn/post/6894204813970997256#heading-0)
   - [二、事件委托的变更](https://juejin.cn/post/6894204813970997256#heading-1)
   - [三、事件系统相关更改](https://juejin.cn/post/6894204813970997256#heading-2)
   - [四、去除事件池](https://juejin.cn/post/6894204813970997256#heading-3)
   - [五、副作用清理时间](https://juejin.cn/post/6894204813970997256#heading-4)
   - [六、返回一致的 undefined 错误](https://juejin.cn/post/6894204813970997256#heading-5)
   - [七、原生组件栈](https://juejin.cn/post/6894204813970997256#heading-6)
   - [八、移除私有导出](https://juejin.cn/post/6894204813970997256#heading-7)
   - [九、启发式更新算法更新](https://juejin.cn/post/6894204813970997256#heading-8)
   ```

10. react-hooks你有用到过吗

    1. 用过部分常用的：useState, useEffect,useRef,useMemo,useCallback, 		memo

11. ~~vue3你有了解过吗~~

12. ~~vue里面的两个路由有什么区别~~

13. ~~vue里面的$nextTick说一说~~

14. 小程序和移动端有做过吗

    ```
    没有从0-1，接手了，两个taro的项目。负责日常的维护和新功能迭代
    ```

15. 如果我想在数组索引为3的位置上插入一个元素

    ```
    arr.splice(3,0,element)
    ```

16. splice会改变原数组吗

    ```
    splice，push，pop，shift，unshift 这些函数都是会对原数组进行修改的函数
    ```

17. 如果我想截取数组索引3-5该怎么做

    ```
    slice（3，6）
    ```

18. slice的返回值是深拷贝还是浅拷贝

    ```
    原生没有直接实现了深拷贝的方法。
    但是有一些方法可以实现：
    1. 某些原生值不支持，不能克隆：JSON.parse / stringfy
    2. 结构化clone（基本都能）：
    	MessageChannel
    		in:postMessage/out:onmessage
    	History API
    		in:tory.replaceState(obj, document.title)/out:history.state
    	Notification API
    		in/out:new Notification('', {data: obj, silent: true}).data
    ```

19. 实现一个红绿灯依次亮起的逻辑, 并循环

    通过异步：1.回调 2.Promise 3.async/await 4.generator+promise

20. Promise的all和race有什么区别

    ```
    all：全部前置Promise执行完成，all生成的Promise才会完成；任意一个前置Promise失败，all生成的Promise直接失败。
    race: 前置Promise任意一个完成或者失败，race生成的Promise就会完成；
    ```

21. 假如现在有两个后台管理系统, 怎么共用登录状态

    ```
     1. 通过cookie携带token来判断登录
     	两个后台管理系统使用相同的父域，实现cookie的共享
     2. 用其它的手段实现跨域的登录状态共享
     	iframe + (postMessage/window.name/location.hash)
    ```

22. 你最近有关心一些新的技术吗

    微前端：将大型项目不同的业务模块拆分成不同的工程，通过主模块组合，每个模块之间互不干扰，连技术选型都可以不同，模块间进行解耦

1. ~~vue中$nextTick的作用是什么~~

2. 能说一下浏览器中的事件循环吗

   首先要说事件循环不得不聊异步：

   1. 主线程（js），工作线程（外部环境，如浏览器）：主线程将会执行所有的同步任务，而异步任务是交给了工作线程来执行
   2. 工作线程完成自己的任务后，将任务完成的消息及任务所对应的回调放入任务队列
   3. 而主线程在完成所有当前任务后，就会去任务队列里面去读取任务（不考虑宏任务/微任务），并执行其回调
   4. 任务会以 一个宏任务 + 所有微任务 的形式不断进行，直至所有任务都执行完成
   5.  后面主线程会不断循环的去查看任务队列

3. 假设定时器设置的6s, 主线程占用了10秒, 定时器什么时候输出

   10秒

4. H5做的多吗, 做过微信的公众号授权吗, 怎么获取微信的`openId`

   ```
   官方流程 
   网页授权流程分为四步： 
   1、引导用户进入授权页面同意授权，获取code 
   2、通过code换取网页授权access_token（与基础支持中的access_token不同） 
   3、如果需要，开发者可以刷新网页授权access_token，避免过期 
   4、通过网页授权access_token和openid获取用户基本信息（支持UnionID机制）
   ```

5. webpack用的多吗, `hash` `chunkhash` `contenthash`之间什么区别

   ```
   升级webpack5： 
   hash：有内容修改，hash就会改变，真个项目同一个hash
   chunkhash：内容修改，只有对应的chunk的hash才会改变，例如：一般同一模块的js和css的chunkhash是相同的，修改一个，全部改变
   contenthash： 内容修改，只有涉及该代码的文件hash才会修改
   ```

6. webpack做过哪些优化, 具体实现细节还记得吗

   ```
   1. Happypack 的作用就是将文件解析任务分解成多个子进程并发执行,线程数一般设置为核心数。项目越大，效果越明显。
   2. webpack5，启用缓存，明显增加了二次构建的速度（以前好像是cache-laoder），
   3. analys插件，分析包构成以及大小，针对性优化，比如某次发现moment太大，使用dayjs替换插件
   4. 比较基础的常用包，使用cdn，不计入打包，如react
   5. js和css压缩
   6. 懒加载
   7. loader配置的优化：include，exclude，后缀匹配的优化（常用优先，尽量少），开启缓存
   8. splitchunk设置==》提取公共依赖
   ```

7. 负责过团队内部的哪些部分的技术支撑

   ```
   
   ```

8. 搭建一些基础组件的时候, 你会考虑哪些东西

   

9. 函数式编程你是怎么理解的, 高阶函数有用到过吗

10. 函数柯里化有什么作用

11. 详细的介绍一下你的开源项目

12. ~~我看你还用改nuxt开发项目~~, 除此之外你还了解哪些vue的ssr方案

13. 你对首屏渲染做过哪些优化

14. ~~vue中的mixins用过吗, 你还有其他的替代方案吗~~

15. ~~vuex有用过吗, vuex是如何实现响应式的~~

16. ~~vue的响应式是如何实现的~~

17. ~~vue的router你用的那种~~, 你觉得hash相较于history会好吗

