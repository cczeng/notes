# [Rxjs 入门](https://cn.rx.js.org/manual/overview.html)  

> RxJS 是一个库，它通过使用 observable 序列来编写异步和基于事件的程序。它提供了一个核心类型 Observable，附属类型 (Observer、 Schedulers、 Subjects) 和受 [Array#extras] 启发的操作符 (map、filter、reduce、every, 等等)，这些数组操作符可以把异步事件作为集合来处理。  

**ReactiveX 结合了 观察者模式、迭代器模式 和 使用集合的函数式编程，以满足以一种理想方式来管理事件序列所需要的一切。**

**基本概念:**  

1. Observable(可观察对象): 表示一个概念，这个概念是一个可调用的未来值或事件的集合。
2. Observer(观察者): 一个回调函数的集合，它知道如何去监听由Observable提供的值。
3. Subscription(订阅): 表示Observable的执行，主要用于取消Observable的执行。

