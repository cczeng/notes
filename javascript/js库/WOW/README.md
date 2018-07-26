# [WOWJS](https://github.com/matthieua/WOW)  - 鼠标滚轮移动到指定元素位置显示动画效果


# 安装: 

注意: 需要先安装 [animate.css](https://daneden.github.io/animate.css/)

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/animate.css@3.5.2/animate.min.css">
<!-- or -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css">
```


```npm
npm install wowjs
```  

  
# 引入:   

```javascript
import { WOW } from 'wowjs/dist/wow.min';

constructor() {
    new WOW().init();
}
```   

# 使用:   

```html
<section class="wow slideInLeft" data-wow-duration="2s" data-wow-delay="5s"></section>

<section class="wow slideInRight" data-wow-offset="10"  data-wow-iteration="10"></section>
```