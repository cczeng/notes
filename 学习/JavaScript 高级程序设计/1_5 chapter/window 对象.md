# window

> 在全局作用域中声明的所有变量和函数，都属于window对象的属性

```javascript
var color = 'red';

function sayColor(){
    alert(window.color);
}
window.sayColor();	// red
```

