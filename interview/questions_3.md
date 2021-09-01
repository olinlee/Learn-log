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

14. ~~跟我讲讲重排重绘吧~~

15. ~~跨域讲一下, 为什么会出现跨域~~

16. ~~jsonp的原理已经他如何拿到服务器的信息~~

17. ~~能讲讲Event Loop吗~~

18. ~~webpack熟悉吗~~

19. ~~你知道webpack的loader和plugin吗~~

20. 那你知道webpack的热更新原理吗

21. ~~vue和react都使用过吗~~

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

32. websockets有用过吗

33. 能跟我讲讲你是怎么理解ssr的吗

34. ~~uniapp和taro有用过吗~~





1. ~~你最近哪个项目的印象比较深刻~~
2. ~~你在你们团队中处于一个什么样的角色~~
3. ~~你的开源项目都遇到过哪些问题~~
4. ~~你的开源项目做过哪些性能优化~~
5. ~~浏览器的缓存规则详细说一下~~
6. ~~浏览器的事件循环是怎样的~~
7. ~~HTTP状态码说一下~~
8. ~~前端性能优化都能做哪些~~
9. ~~CDN有了解过吗~~
10. ~~js里面的垃圾回收机制都有哪些~~
11. 小程序熟悉吗, 最近做过哪些
12. nginx和nodejs相关的做过吗
13. ~~vue里数据双向绑定原理是怎样的~~
14. ~~你觉得es6的proxy有怎样的问题~~
15. ~~这三种声明方式var, let, const的区别~~
16. ~~cosnt为什么不可以更改~~
17. ~~箭头函数跟普通函数之间有什么区别~~
18. ~~es6里面的promise和async/await有什么区别~~
19. ~~你个人会倾向于哪个方向的业务开发~~

