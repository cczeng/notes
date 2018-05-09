# [Swiper.js 轮播图](http://idangero.us/swiper/get-started/)    
  
> 一个强大的轮播图js库

## Getting Started With Swiper
```
npm install swiper
```   

## js 
```html
import Swiper from 'swiper';
<!-- 或者 -->
<script src="path/to/swiper.min.js"></script>
```

## Css

```javascript
"../node_modules/swiper/dist/css/swiper.min.css"
//或者
<link rel="stylesheet" href="path/to/swiper.min.css">
```

## Add Swiper HTML Layout

```html
<!-- Slider main container -->
<div class="swiper-container">
    <!-- Additional required wrapper -->
    <div class="swiper-wrapper">
        <!-- Slides -->
        <div class="swiper-slide">Slide 1</div>
        <div class="swiper-slide">Slide 2</div>
        <div class="swiper-slide">Slide 3</div>
        ...
    </div>
    <!-- If we need pagination -->
    <div class="swiper-pagination"></div>

    <!-- If we need navigation buttons -->
    <div class="swiper-button-prev"></div>
    <div class="swiper-button-next"></div>

    <!-- If we need scrollbar -->
    <div class="swiper-scrollbar"></div>
</div>
```

## Initialize Swiper

```html
<body>
  ...
  <script>
  var mySwiper = new Swiper ('.swiper-container', {
    // Optional parameters
    direction: 'vertical',
    loop: true,

    // If we need pagination
    pagination: {
      el: '.swiper-pagination',
    },

    // Navigation arrows
    navigation: {
      nextEl: '.swiper-button-next',
      prevEl: '.swiper-button-prev',
    },

    // And if we need scrollbar
    scrollbar: {
      el: '.swiper-scrollbar',
    },
  })
  </script>
</body>
```