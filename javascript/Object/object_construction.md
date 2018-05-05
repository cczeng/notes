# 对象结构   
  
> 可参考 [阮一峰-ECMAScript 6 入门](http://es6.ruanyifeng.com/#docs/destructuring)    
   
## 数组的结构赋值

```javascript
let [a,b,c] = [1,2,3];   

let [foo, [[bar], baz]] = [1, [[2], 3]];
foo // 1
bar // 2
baz // 3

let [ , , third] = ["foo", "bar", "baz"];
third // "baz"

let [x, , y] = [1, 2, 3];
x // 1
y // 3

let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4]

let [x, y, ...z] = ['a'];
x // "a"
y // undefined
z // []
```   

如果结构不成功，变量的值则为`undefined`   

```javascript
let [foo] = [];
let [bar, foo] = [1];
```   

## 默认值   

```javascript
let [foo = true] = [];
foo // true

let [x, y = 'b'] = ['a']; // x='a', y='b'
let [x, y = 'b'] = ['a', undefined]; // x='a', y='b'
```

## 对象结构赋值   

```javascript
let {foo,bar}={foo:'aaa',bar:'bbb'};
foo // 'aaa'
bar // 'bbb'

// 变量名与属性不一致的写法
let { foo: baz } = { foo: 'aaa', bar: 'bbb' };
baz // "aaa"

let obj = { first: 'hello', last: 'world' };
let { first: f, last: l } = obj;
f // 'hello'
l // 'world'


let obj = {};
let arr = [];

({ foo: obj.prop, bar: arr[0] } = { foo: 123, bar: true });

obj // {prop:123}
arr // [true]


var {x = 3} = {};
x // 3

var {x, y = 5} = {x: 1};
x // 1
y // 5

var {x: y = 3} = {};
y // 3

var {x: y = 3} = {x: 5};
y // 5

var { message: msg = 'Something went wrong' } = {};
msg // "Something went wrong"
```

## 字符串结构赋值   
  
```javascript
const [a,b,c,d,e] = 'Hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"
```  
`length`   

```javascript
let {length: len} = 'hello';
len // 5
```  

## 数值和布尔值的结构赋值   

```javascript
let {toString:s } = 123;
s === Number.prototype.toString //true

let {toString: s} = true;
s === Boolean.prototype.toString //true
```  
## 函数参数的结构赋值   

```javascript
function add([x,y]){ return x + y ;}

add([1,2]); //3

[[1,2],[3,4]].map(([a,b])=>a+b);
// [3,7]

function move({x=0,y=0}={}){return [x,y];}

move({x:3,y:8});    // [3,8]
move({x: 3}); // [3, 0]
move({}); // [0, 0]
move(); // [0, 0]


[1,undefined,3].map((x='yes')=>x);
// [1,'yes',3]
```    
## 用途  

1. 交换变量的值   

```javascript
let x = 1;
let y = 2;

[x, y] = [y, x];
```   

2. 从函数返回多个值   

```javascript
// 返回一个数组

function example() {
  return [1, 2, 3];
}
let [a, b, c] = example();

// 返回一个对象

function example() {
  return {
    foo: 1,
    bar: 2
  };
}
let { foo, bar } = example();
```   

3. 函数参数的定义   

```javascript
// 参数是一组有次序的值
function f([x, y, z]) { ... }
f([1, 2, 3]);

// 参数是一组无次序的值
function f({x, y, z}) { ... }
f({z: 3, y: 2, x: 1});
```   
  
4. 提取JSON数据   

```javascript
let jsonData = {
  id: 42,
  status: "OK",
  data: [867, 5309]
};

let { id, status, data: number } = jsonData;

console.log(id, status, number);
// 42, "OK", [867, 5309]
```   
  
5. 函数参数的默认值   

```javascript
jQuery.ajax = function (url, {
  async = true,
  beforeSend = function () {},
  cache = true,
  complete = function () {},
  crossDomain = false,
  global = true,
  // ... more config
} = {}) {
  // ... do stuff
};
```   
