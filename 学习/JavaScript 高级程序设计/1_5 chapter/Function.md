# Function 篇

> Function 实际上是个对象，每个函数都是Function类型的实例。
>
> **函数名是指向函数对象的指针, 所以js里面没有函数重载(可通过argument实现重载)**



重点： 

- 函数名是一个指向函数对象的指针
- 函数声明具有 **函数声明提升(function declaration hoisting)**  - 即使声明函数的代码在调用它的代码后面，JavaScript引擎也能把函数声明提升到顶部。函数表达式则需要解析器执行到它所在的代码行。
- 函数可以作为另一个函数的值或者返回值(可实现闭包、高阶函数等)
- 函数具有两个内部属性
  - **arguments** 主要用途保存函数参数
    - 拥有 `callee` 属性，该属性是一个指针，**指向拥有这个arguments对象的函数**  - 用于递归操作防止函数变量被替换
    - `callee.caller`: 指向调用当前函数的函数的引用
  - `this`:  **this 引用的是函数据以执行的环境对象——或者也可以说是this值（当在网页全局作用于种调用函数时，this对象的引用就是window）**
- 属性和方法
  - `length` : 参数个数
  - `prototype`: 原型链 —— 保存所有实例方法的真正所在。





```javascript
function sum(num1, num2){
    return num1 + num2;
}
var sum = function(num1, num2){
    return num1 + num2;
}
```

上面两个是等价的，正因如此

```javascript
function sum(num){
    return num + 100;
}
function sum(num){
    return num * 100;
}
alert(sum(2)); // 200
```

因为等价

```javascript
var sum = function(num){ return num + 100 };
sum = function(num){ return num * 100 };
```





## 函数声明方式

一、 函数声明：

```javascript
function sum(num){
    return num + 100;
}
```

二、 函数表达式(与函数声明几乎无差)： 

```javascript
var num = function(num){
    return num + 100;
}
```

区别在于函数声明具有 **函数声明提升**， 意思是函数声明会被优先解析。不需要按照一行一行解析，在使用函数之后声明函数也可以。

三、Function构造函数(不推荐的写法)

```javascript
var sum = new Function('num1', 'num2', 'return num1 + num2');
```



## this

> this 引用的是函数据以执行的**环境对象** —— 或者也可以说是 this 值（当在网页的全局作用域中调用函数时，this对象引用的就是window

```javascript
window.color = 'red';
var o = { color: 'blue' };
function sayColor(){
    alert(this.color);
}

sayColor(); // 'red'
o.sayColor = sayColor;
o.sayColor(); // 'blue'
```

- 当在全局作用域中调用 sayColor() 时， this 引用的是全局对象window; —— 对this.color求值会换算成对window.color求值。
- 而把这个函数赋值给对象 o 并调用 o.sayColor() 时，this 引用的是对象 o ，因此对 this.color 的求值会转换成对 o.color 求值



## caller

> 这个属性保存着调用当前函数的函数的引用，如果在全局作用域中调用当前函数，它的值为 null

```javascript
function outer(){
    inner();
}
function inner(){
    alert(arguments.callee.caller);
}
outer();
```

会显示 outer() 函数的源代码。



## 函数属性和方法

每个函数都包含两个属性： 

- `length`: 表示函数希望接收的命名参数的个数
- `prototype`:  原型链 —— 保存所有实例方法的真正所在。



### prototype

> 诸如 toString() 和 valueOf() 等方法实际上都保存在 prototype 名下，只不过是通过各自对象的实例访问罢了
>
> **prototype** 属性是不可枚举，因此使用 for-in 无法发现。

每个函数都包含两个非继承而来的方法： `apply()` 和 `call()` 。这两个方法的用途都是**在特定的作用域中调用函数**， 实际上等于**设置函数体内this对象的值**

#### apply()

> 接收两个参数： 一个是在其中运行函数的作用于，另一个是参数数组。

```javascript
function sum(num1, num2){
    return num1 + num2;
}
function callSum1(num1, num2){
    return sum.apply(this, arguments); // 传入 arguments 对象
}
function callSum2(num1, num2){
    return sum.apply(this, [num1, num2]);	// 传入数组
}
alert(callSum1(10,10));		//20
alert(callSum2(10,10));		//20

```

#### call()

> call() 方法与 apply() 方法**作用相同**， 区别在于**接收参数的方式不同**。
>
> 第一个参数 this值，其后参数直接传递给函数（逐个列出）

```javascript
function sum(num1, num2){
    return num1 + num2;
}
function callSum2(num1, num2){
    return sum.call(this, num1, num2);	// 逐个列出
}
alert(callSum1(10,10));		//20

```



上面两种方法，传递参数并非是它们的用武之地；**它们真正强大的地方是能够扩充函数赖以运行的作用于** 

```javascript
window.color = 'red';
var o = { color: 'blue' };

function sayColor(){
    alert(this.color);
}

sayColor();		// red

sayColor.call(this);	// red
sayColor.call(window);	// red
sayColor.call(o);		// blue

```

**使用 call() 或者 apply() 来扩充作用域的最大好处，就是对象不需要与方法有任何耦合关系**



#### bind()

> 这个方法会创建一个函数的实例，其 this 值会绑定到传给 bind() 函数的值。

```javascript
window.color = 'red';
var o = { color: 'blue' };

function sayColor(){
    alert(this.color);
}
var objectSayCOlor = sayColor.bind(o);
objectSayColor();	// blue

```

