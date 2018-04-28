# 屏幕中间/单个元素居中  
  
使用 css 的 `flex` 可以很简单的做到居中效果.  
  
```html
<div class="box">
    <span></span>
</div>
```   

```css
body,html {
    width:100%;
    height:100%;
}
.box {
    width:100%;
    height:100%;
    display: flex;
    justify-content: center;    // x 轴居中 也叫主轴
    align-items: center;        // y 轴居中 也叫交叉轴
}
```   

