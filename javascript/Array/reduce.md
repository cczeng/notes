# reduce() 对累加器和数组中的每个元素应用一个函数   

> arr.reduce(callback[,initialValue])

**callback**  
执行数组中每个值的函数,包含四个参数:   

`accumulator` : 累加器的返回值   
`currentValue`: 数组中正在处理的元素   
`currentIndex`: 数组中正在处理的元素的索引  
`array` 可选   : 调用 reduce 的数组   

`initialValue ` 可选: 用做第一个调用 `callback` 的第一个参数的值，如果没有提供初始值，则将使用数组中的第一个元素。 **在没有初始值的空数组上调用reduce将报错**.  
  
例子:    
  

同时进行映射和过滤:   
```javascript
const numbers = [10,20,30,40];  
const doubledOver50 = number.reduce((finalList,num)=>{
    num = num * 2;

    // filter
    if(num > 50) finalList.push(num);

    return finalList;
},[]);
```  
  

```javascript
[0,1,2,3,4].reduce((prev,curr)=> prev + curr);
```   
提供一个初始值为第二个参数:   
```javascript
[0,1,2,3,4].reduce((accumulator,currentValue,currentIndex,array)=> {return accumulator + currentValue; },10)
```   

```javascript
const sum = [0,1,2,3,4].reduce((a,b)=> a+b, 0);
```   
计算数组中每个元素出现的次数     

```javascript
const names = ['xiaoming','xiaohong','laowang','zhangsan'];  

const countedNames = names.reduce((allNames,name)=>{
    name in allNames ? allNames[name]++ : allNames[name] = 1;

    return allNames;
},{});
```  
