# Calc()



在使用 LESS 或者 SASS 的 calc() 函数计算高度或者宽度的时候经常会计算失败。



例如:  

```css
height: calc(100% - 50px);

// 实际显示的为
height: calc(50%);
```



这是因为 LESS/SASS 将 `calc` 中表达式识别为字符串，解决办法:   

```Css
height: calc(~'100% - 50px');
```

