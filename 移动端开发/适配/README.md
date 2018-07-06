# 移动端适配方案


**[可参考 - 移动端Web页面适配方案](https://segmentfault.com/a/1190000008767416)**

1. 使用淘宝的rem + 动态设置font-size值
2. 网易适配方案(不推荐)
3. vw+rem适配方案(推荐)



## vw

最简单的就是根据设计稿的尺寸来直接转换成vw:

注意：**这里需要使用sass**：   
   

```css
$vw_base: 750;
@function vw($px) {
    @return ($px / $vw_base) * 100vw;
}
```

## vw + rem 适配方案

```css
// rem 单位换算：定为 75px 只是方便运算，750px-75px、640-64px、1080px-1080px，如此类推
$vw_fontsize: 75; // iPhone 6尺寸的根元素大小基准值
@function rem($px) {
     @return ($px / $vw_fontsize ) * 1rem;
}
// 根元素大小使用 vw 单位
$vw_design: 750;
html {
    font-size: ($vw_fontsize / ($vw_design / 2)) * 100vw; //就是相当于20vw
    // 同时，通过Media Queries 限制根元素最大最小值
    @media screen and (max-width: 320px) {
        font-size: 64px;
    }
    @media screen and (min-width: 540px) {
        font-size: 108px;
    }
}
// body 也增加最大最小宽度限制，避免默认100%宽度的 block 元素跟随 body 而过大过小
body {
    max-width: 540px;
    min-width: 320px;
}
```
