1. ~~说一下盒模型吧~~

   它包括：边距，边框，填充，和实际内容。

   ```
   块级盒子：
   
   * 盒子会在内联的方向上扩展并占据父容器在该方向上的所有可用空间，在绝大数情况下意味着盒子会和父容器一样宽
   * 每个盒子都会换行
   * `width` 和 `height` 属性可以发挥作用
   * 内边距（padding）, 外边距（margin） 和 边框（border） 会将其他元素从当前盒子周围“推开”
   
   行内盒子：
   
   * 盒子不会产生换行。
   *  `width` 和 `height`属性将不起作用。
   * 垂直方向的内边距、外边距以及边框会被应用但是不会把其他处于 `inline` 状态的盒子推开。
   * 水平方向的内边距、外边距以及边框会被应用且会把其他处于 `inline` 状态的盒子推开。
   ```

2. ~~什么是BFC~~

   是块盒子的布局过程发生的区域，也是浮动元素与其他元素交互的区域，块格式化上下文对浮动定位（参见 [`float`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/float)）与清除浮动（参见 [`clear`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/clear)）都很重要。

   产生bfc的情况：

   ```
   * 根元素（<html>）
   * 浮动元素（元素的 float 不是 none）
   * 绝对定位元素（元素的 position 为 absolute 或 fixed）
   * 行内块元素（元素的 display 为 inline-block）
   * 弹性元素（display 为 flex 或 inline-flex 元素的直接子元素）
   * overflow 计算值(Computed)不为 visible 的块元素
   
   表格单元格（元素的 display 为 table-cell，HTML表格单元格默认为该值）
   表格标题（元素的 display 为 table-caption，HTML表格标题默认为该值）
   匿名表格单元格元素（元素的 display 为 table、table-row、 table-row-group、table-header-group、table-footer-group（分别是HTML table、row、tbody、thead、tfoot 的默认属性）或 inline-table）
   display 值为 flow-root 的元素
   contain 值为 layout、content 或 paint 的元素
   网格元素（display 为 grid 或 inline-grid 元素的直接子元素）
   多列容器（元素的 column-count 或 column-width (en-US) 不为 auto，包括 column-count 为 1）
   column-span 为 all 的元素始终会创建一个新的BFC，即使该元素没有包裹在一个多列容器中（标准变更，Chrome bug）。
   ```

3. ~~垂直居中有几种方式~~

   ```
   1. 使用绝对定位和transform
   2. 绝对定位结合margin: auto
   3. 使用flex布局
   4. 父元素不设置高度的情况下设置上下相等的padding
   5. 行内元素：使用 line-height（父级：等于高度） 和 vertical-align（子级：middle） 对图片进行垂直居中
   ```

   

4. ~~说一下js基础数据类型~~

5. ~~平时Symbol有用到过吗~~

   ```
   一，避免对象的键被覆盖。Symbol用于对象的属性名时，能保证对象不会出现同名的属性。这对于一个对象由多个模块构成的情况非常有用，能防止某一个键被不小心改写或覆盖。
   二，避免魔术字符串。魔术字符串的诠释是：在代码之中多次出现、与代码形成强耦合的某一个具体的字符串或者数值。风格良好的代码，应该尽量消除魔术字符串，改由含义清晰的变量代替。
   ```

6. ~~那你讲讲闭包~~

7. ~~一般工作中什么时候会用到闭包~~

8. ~~防抖和节流什么区别~~

9. ~~es5的原型链继承如何实现~~

10. ~~普通函数和箭头函数有什么区别~~

11. ~~箭头函数为什么不能new~~

12. ~~set和map什么区别, map和obj啥区别~~

    ```
    set 是集合，没有key，value唯一
    map 是映射，k-v键值对，k唯一，且可以是任意值
    可以利用对象的属性名-属性值模仿 map，但是属性名作为键只能取值字符串（其他值会隐式转换）或者symbol
    ```

    

13. ~~async/await如何处理错误~~

    ```
    function to(promise) {
        if (!promise || !Promise.prototype.isPrototypeOf(promise)) {
            return new Promise((resolve, reject) => {
                reject(new Error('参数必须是 promise'));
            }).catch((err) => {
                return [err, null];
            });
        }
        return promise.then(data => {
            return [null, data];
        }).catch(err => {
            return [err, null];
        });
    }
    ```

14. ~~跟我讲讲重排重绘吧~~

15. ~~跨域讲一下, 为什么会出现跨域~~

    ```
    原因：同源策略
    解决：	1.jsonp 
    		2.cors： 非简单请求有预检机制options
    		3.内嵌iframe
    
    options请求就是非简单请求的跨域预检机制
    	简单：1. get。post。head 2. 请求头字段只在某种规定之下（自己查）
    	非简单：1. 其它类型 2. 请求头操过限定
    ```

    

16. ~~jsonp的原理以及他如何拿到服务器的信息~~

17. ~~能讲讲Event Loop吗~~

18. ~~webpack熟悉吗~~

19. ~~你知道webpack的loader和plugin吗~~

    ```
    Loader直译为"加载器"。Loader的作用是让webpack拥有了加载和解析非JavaScript文件的能力。
    Plugin直译为"插件"。Plugin可以扩展webpack的功能，让webpack具有更多的灵活性。 在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。
    ```

    

20. 那你知道webpack的热更新原理吗

    ```
    首先，介绍webpack-dev-server:
    webpack-dev-server 主要包含了三个部分：
    1.webpack: 负责编译代码
    2.webpack-dev-middleware: 主要负责构建内存文件系统，把webpack的 OutputFileSystem 替换成 InMemoryFileSystem。同时作为Express的中间件拦截请求，从内存文件系统中把结果拿出来。
    3.express：负责搭建请求路由服务。
    其次，介绍工作流程:
    1.启动dev-server，webpack开始构建，在编译期间会向 entry 文件注入热更新代码；
    2.Client 首次打开后，Server 和 Client 基于Socket建立通讯渠道；
    3.修改文件，Server 端监听文件发送变动，webpack开始编译，直到编译完成会触发"Done"事件；
    4.Server通过socket 发送消息告知 Client；
    5.Client根据Server的消息（hash值和state状态），通过ajax请求获取 Server 的manifest描述文件；
    6.Client对比当前 modules tree ，再次发请求到 Server 端获取新的JS模块；
    7.Client获取到新的JS模块后，会更新 modules tree并替换掉现有的模块；
    8.最后调用 module.hot.accept() 完成热更新；
    ```

21. ~~vue和react都使用过吗~~

    ​	

22. ~~vue的computed和watch有什么区别~~

23. ~~v-model平时你都怎么使用~~

24. import和require之间什么区别

25. ~~说一下vue的缓存组件~~

26. ~~vue3.0为什么使用proxy拦截数据~~

27. ~~能讲讲vuex吗, 刷新页面会怎样~~

28. http1.1和http2.0之间什么区别

29. ~~iframe是什么, 正常页面如何与iframe通信~~

30. ~~强缓存和协商缓存有了解吗~~

31. ~~axios请求数据, POST为什么会发送一个OPTIONS~~

    ```
    OPTIONS不是用来鉴权的，是用来跨域预检的（非简单请求）
    ```

32. websockets有用过吗

33. 能跟我讲讲你是怎么理解ssr的吗

34. ~~uniapp和taro有用过吗~~

    

35. ~~你最近哪个项目的印象比较深刻~~

36. ~~你在你们团队中处于一个什么样的角色~~

    

37. ~~你的开源项目都遇到过哪些问题~~

38. ~~你的开源项目做过哪些性能优化~~

39. ~~浏览器的缓存规则详细说一下~~

40. ~~浏览器的事件循环是怎样的~~

41. ~~HTTP状态码说一下~~

    ```
    200 OK
    请求正常处理完毕
    204 No Content
    请求成功处理，没有实体的主体返回
    206 Partial Content
    GET范围请求已成功处理
    301 Moved Permanently
    永久重定向，资源已永久分配新URI
    302 Found
    临时重定向，资源已临时分配新URI
    303 See Other
    临时重定向，期望使用GET定向获取
    304 Not Modified 一般用于协商缓存失败，代表使用协商缓存
    发送的附带条件请求未满足
    400 Bad Request
    请求报文语法错误或参数错误
    401 Unauthorized
    需要通过HTTP认证，或认证失败
    403 Forbidden
    请求资源被拒绝
    404 Not Found
    无法找到请求资源（服务器无理由拒绝）
    500 Internal Server Error
    服务器故障或Web应用故障
    503 Service Unavailable
    服务器超负载或停机维护
    
    ```

    

42. ~~前端性能优化都能做哪些~~

43. ~~CDN有了解过吗~~

    ```
    既内容分发网络：负载均衡和分布式存储技术
    ```

    

44. ~~js里面的垃圾回收机制都有哪些~~

    ```
    1. 标记清理：垃圾回收器将定期从根开始，找所有从根开始引用的对象，然后找这些对象引用的对象……从根开始，垃圾回收器将找到所有可以获得的对象和收集所有不能获得的对象。
    2. 引用计数：如果没有引用指向该对象（零引用），对象将被垃圾回收机制回收。
    3. v8：
    ```

    

45. 小程序熟悉吗, 最近做过哪些

    

46. nginx和nodejs相关的做过吗

    

47. ~~vue里数据双向绑定原理是怎样的~~

48. ~~你觉得es6的proxy有怎样的问题~~

49. ~~这三种声明方式var, let, const的区别~~

    

50. ~~const为什么不可以更改~~

    ```
    * 执行上下文：词法环境（let、const声明的变量） + 变量环境（兼容var声明和function声明） 
    * 词法环境：（环境记录 + 外部环境的引用）
    
    * 环境记录器：
      环境记录器分为两种：
      1）声明式环境记录器
      存在于函数作用域中，存储变量、函数、参数。
      2）对象式环境记录器
      存在于全局作用域和块级作用域中，存储变量、函数。====》 块级作用域产生的原因
    * 外部环境的引用：如果在当前环境内找不到变量，引擎可以通过引用在外部环境继续查找。
    
    执行上下创建后，生成的词法环境中用于记录变量的是一个叫‘环境记录’的东西，相较与以前的变量对象，环境记录 除了绑定可变的变量与值，还能绑定不可变的变量与值，后者一旦绑定后，值就无法修改了。
    ```

    

51. ~~箭头函数跟普通函数之间有什么区别~~

52. ~~es6里面的promise和async/await有什么区别~~

    ```
    语法糖
    ```

    

53. ~~你个人会倾向于哪个方向的业务开发~~

54. 安全相关：

    ```
    CSRF / XSRF（跨站请求伪造：Cross-site request forgery）：攻击者盗用了你的身份，以你的名义进行恶意请求。
    1. 添加 token
    2. 涉及到数据修改操作严格使用 post 请求而不是 get 请求
    3. HTTP 协议中使用 Referer 属性来确定请求来源进行过滤
    4. 添加验证码，密码
    
    XSS/CSS（跨站脚本攻击）：攻击者在目标网站植入恶意脚本（js / html）
    1. 输入内容和服务端返回内容进行过滤和转译
    2. 加密传输
    3. URL携带参数谨慎使用
    
    ClickJacking（点击劫持）：利用透明 iframe 覆盖原网页诱导用户进行某些操作达成目的
    1. 判断当前网页是否被 iframe 嵌套
    2. 在HTTP投中加入 X-FRAME-OPTIONS 属性，此属性控制页面是否可被嵌入 iframe 中
    
    CDN劫持：攻击者劫持了CDN，或者对CDN中的资源进行了污染
    1. CDN以支持SRI为荣，可对资源进行校验（使用 SRI 需要两个条件：一是要保证 资源同域 或开启跨域，二是在<script>中 提供签名 以供校验。）
    
    HSTS（HTTP Strict Transport Security：HTTP严格传输安全）：网站接受从 HTTP 请求跳转到 HTTPS 请求的做法可能被劫持，也就是，302重定向有可能会被劫持，篡改成一个恶意或钓鱼网站。
    
    ```

    

