# Global 对象

> Global（全局） 对象可以说是ECMAScript中最特别的一个对象了，因为不管从什么角度上看，这个对象都是不存在的。——不属于任何其他对象的属性和方法，最终都是它的属性和方法。

`isNaN()` `isFinite()` `parseInt()` `parseFloat()` 都是Global的方法



## URI 编码方法

> `encodeURI()`和`encodeURIComponent()` 方法可以对URI（Uniform Resource Identifiers, 通用资源标识符）进行编码，以便发送给浏览器



## eval() 方法

> 整个 ECMAScript 语言中最强大的一个方法。
>
> 此方法非常强大也非常危险，谨慎使用。特别是在执行用户输入数据的情况下，可能会有恶意用户输入威胁你的站点或应用程序安全的代码（即所谓的代码注入）

**eval() 方法就像是一个完整的 ECMAScript 解析器，它只接受一个参数，即要执行的ECMAScript（或JavaScript) 字符串.**



```javascript
eval("alert('hi')");
// 作用等价于下面代码
alert('hi');
```

拥有与当前执行环境相同的作用域。

```javascript
var msg = 'hello, world';
eval("alert(msg)"); // 'hello, world'
```



在 eval() 中创建的任何变量或函数都不会被提升。

