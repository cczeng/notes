# web开发适配方案



详细适配方案可[参考这里](https://segmentfault.com/a/1190000008767416) 

阿里[lib-flexible方案](https://github.com/amfe/lib-flexible) (过渡方案,已停止)，推荐 `vw` 方案适配，无论手机端和PC端都是最理想的适配单位。



- Sass + VW  仅适用现代浏览器
- JS + Rem  动态计算Rem



无论采用哪种方案都需要添加 `meta` :

```html
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
```



- `visual viewport` 可是窗口:  屏幕宽度 window.innerWidth/Height
- `layout viewport` 布局视口: DOM宽document.documentElement.clientWidth/Height
- `ideal viewport` 理想视口: 使布局视口就是可见视口



`content`参数:

- `width=device-width` : 表示宽度是设备屏幕的宽度
- `initial-scale`: 表示初始的缩放比例
- `minimum-scale`: 表示最小的缩放比例
- `maximum-scale`: 表示最大的缩放比例
- `user-scalable`: 用户是否可以调整缩放比例



##  VW适配



需要使用`sass`进行自动计算。



例如**设计稿**尺寸为 `1920px` : 



```sass
$vw_base: 1920;
$vh_base: 1080;

@function vw($px){
  @return ($px / $vw_base) * 100vw;
}
@function vh($px){
  $return ($px / $vh_base) * 100vw;
}
```





## REM



REM 适配是根据js动态设置 `font-size` 大小，也需要用到`sass`。 



具体可参考[淘宝 Flexible](https://www.w3cplus.com/mobile/lib-flexible-for-html5-layout.html)



此方案建议启用,因为 `vw(viewport width)` 已经得到了 ie9+ 的全部现代浏览器支持。



