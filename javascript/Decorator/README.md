# [装饰器 - Decorator](http://es6.ruanyifeng.com/#docs/decorator)   

> 装饰器是一种函数，写成 `@函数名`,可以放在类和类方法的定义前面。     

**Typescript装饰器和原生的有点出入 [点击查看](https://www.tslang.cn/docs/handbook/decorators.html)**   

**最后编辑日期(因为此方法还在提案中,不一定和目前完全一样):** 2019年8月14日14:29:46

主要有以下几种`常用`：   

- 类装饰器
- 方法装饰器
- 属性装饰器   

## 1. 类的装饰器   

```js
@testable
class MyTestableClass {
    //...
}
function testable(target){
    target.isTestable = true;
}
MyTestableClass.isTestable // true
```

`@testable` 就是一个装饰器，修改了`MyTestableClass`这个类的的行为，添加了一个静态属性`isTestable`。`testable`函数的参数`target`是`MyTestableClass`类本身。   

基本上，装饰器的行为就类似下面：   

```js
@decorator
class A{}

class A{}
A = decorator(A) || A;
```



带有参数的装饰器：   

```js
function tastable(isTestable){
    return function(target){
        target.isTestable = isTestable;
    }
}

@testable(true)
class MyTestable{}

@testable(false)
class MyTestable{}
```



前面都是对类添加静态属性，如果想添加实例属性，则需要通过目标类的`prototype`对象操作。   

```js
function testable(target){
    target.prototype.isTestable = true;
}
@testable
class MyTestable {}
const obj = new MyTestable();
obj.isTestable // true
```

```js
// mixins.js
export function mixins(...list){
    return function(target){
        Object.assign(target.prototype, ...list);
    }
}
// mian.js
import { mixins } from './mixins';
const Foo = {
    foo() {console.log('foo')}
};
@mixins(Foo)
class MyClass{}
let obj = new MyClass();
obj.foo(); // 'foo'
```

## 2. 方法装饰器  

```js
class Person {
    @readonly
    name(){return `${this.first} ${this.last}`}
}
```

**方法装饰器一共可接受3个参数。**   

```js
function readonly(target,name,descriptor){
    // descriptor 是 object.defineProperty方法里面的
    // {
    //   value: specifiedFunction,
    //   enumerable: false,
    //   configurable: true,
    //   writable: true
    // }
    descriptor.writable = false;
    return descriptor;
}

readonly(Person.prototype,'name',descriptor);
Object.defineProperty(Person.prototype,'name',descriptor);
```

参数说明:   

- 第一个参数： 类的原型对象
- 第二个参数： 所要装饰的属性名
- 第三个参数： 属性的[描述对象](https://javascript.ruanyifeng.com/stdlib/attributes.html)

 

另一个例子，修改描述对象的`enumerable`，使得该属性不可遍历。   

```js
class Person {
    @nonenumerable
    get kidCount() { return this.children.length }
}
function nonenumberable(target,name,descriptor){
    descriptor.enumerable = false;
    return descriptor;
}
```

下面的`@log`装饰器，可以起到输出日志的作用：   

```js
class Math{
    @log
	add(a,b){
        return a + b;
    }
}
function log(target,name,descriptor){
    var oldValue = descriptor.value;
    
    descriptor.value = function(){
        console.log(`Calling ${name} width`, arguments);
        return oldValue.apply(this,arguments);
    };
    
    return descriptor;
}
const math = new Math();
math.add(2,4);
```

  

## 3. 属性装饰器   

```js
export function logProperty(constructor,propertyKey){
    let propertyValue;
    
    function getter(){
        return propertyValue;
    }
    
    function setter(value){
        propertyValue = value;
        console.log(`%c logProperty Decorator - ${propertyKey} setter`,`color: #4CAF50; font-weight: bold;`,value);
    }
    
    Object.defineProperty(constructor,propertyKey, {
        get: getter,
        set: setter,
        enumerable: true,
        configurable: true
    })
}
```

