# [Compass](http://compass-style.org/install/)  - Sass 的工具库



参考[Compass用法指南](http://www.ruanyifeng.com/blog/2012/11/compass.html) ——阮一峰



## 安装



首先需要安装 `ruby` [点击下载](http://www.ruby-lang.org/zh_cn/)  

确定安装完`ruby`之后:  

```bash
gem update --system
gem install compass
```



## 创建项目 



进入需要创建的文件夹: 



```bash
compass create projectName
```

接下来就会创建一个`projectName`的子目录。



里面会有`config.rb` `sass/` `stylesheets/` 则为创建成功。也可以自行创建 `config.rb` 文件，自行制定目录.



## 编译



```bash
compass compile
```



执行这一句会将项目目录下的 `sass` 目录中文件编译成css,并存放在 `stylesheets` 中



默认情况下，编译出来的css文件带有**大量的注释**。但是，生产环境需要压缩后的css文件，这时要使用 `—output-style` 参数.



去掉注释且进行压缩:

```bash
compass compile --output-style compressed
```



编译未变动的文件，需要使用`—force`参数:

```bash
compass compile --force
```



在`config.rb`中制定编译模式： 

```bash
output_style = :expanded

// :expanded 保留原格式, :nested、:compact、:compressed 进入生产阶段采用:compressed
```



也可以通过指定 `environment` 的值(:production或者:development)，智能判断模式：  



```bash
environment = :development
ouput_style = (environment == :production) ? :compressed : :expanded
```



自动编译:  

```bash
compass watch
```

执行该命令后，只要`sass`文件发生变化，就会被自动编译成`css`文件 





## 模块

`compass`采用模块结构，不同模块提供不同的功能。目前拥有五个内置模块：  

```
* reset
* css3
* layout
* typography
* utilities
```





## reset 模块

通常，编写自己的样式之前，有必要重置浏览器的默认样式。



```sass
@import "compass/reset";
```

上面的`@import`命令，用来指定加载模块，这里加载`reset`模块。编译后会产生对应的`css reset`代码。





## [css3 模块](http://compass-style.org/reference/compass/css3/)

只列出常用，如果要全部清点击标题进入官网查看文档。



### 圆角



```sass
@import "compass/css3";

.rounded {
    @include broder-radius(5px);
}
```

编译后代码为: 

```css
.rounded {
    -moz-border-radius: 5px;
    -webkit-border-radius: 5px;
    -o-border-radius: 5px;
    -ms-border-radius: 5px;
    -khtml-border-radius: 5px;
    border-radius: 5px;
}
```



只要左上角圆角，写法为: 

```css
@include border-corner-radius(top,left,5px);
```





### 透明



```sass
@import "compass/css3";
.opacity {
    @include opacity(0.5);
}
```

编译后的代码： 

```css
#opacity {
    filter: progid:DXImageTransform.Microsoft.Alpha(Opacity=0.5);
    opacity: 0.5;
}
```



### 行内区块

```sass
@import "compass/css3";
.inline-block {
	@include inline-block;
}
```

编译后代码：

```css
#inline-block {
    display: -moz-inline-stack;
    display: inline-block;
    vertical-align: middle;
    *vertical-align: auto;
    zoom: 1;
    *display: inline;
}
```





## [layout 模块](http://compass-style.org/reference/compass/layout/)

提供布局功能。

例如，指定页面的`footer`部分总是出现在浏览器最底端:  

```sass
@import "compass/layout";
#footer {
	@include sticky-footer(54px);
}
```



占满父元素的空间:  

```sass
@import "compass/layout";
.stretch-full {
    @include stretch;
}
```



## [typography 模块](http://compass-style.org/reference/compass/typography/)

Typography模块为常见的文本样式模式提供了一些基本的mixin。简单来说就是提供板式功能。



例如，指定`链接颜色`的minxin为：

```
link-colors($noraml,$hover,$active,$visited,$focus);
```

使用时写成:  

```sass
@import "compass/typography";
a {
    @include link-colors(#00c,#0cc,#c0c,#ccc,#cc0);
}
```



## [utilities 模块](http://compass-style.org/reference/compass/utilities/) 

[该模块](http://compass-style.org/reference/compass/utilities/)提供某些不属于其他模块的功能。



例如，清除浮动: 

```sass
@import "compass/utilities";
.clearfix {
    @include clearfix;
}
```



表格: 

```sass
@import "compass/utilities";
table {
	$table-color: #7a98c6;
    @include table-scaffolding;
    @include inner-table-borders(1px, darken($table-color, 40%));
    @include outer-table-borders(2px);
    @include alternating-rows-and-columns($table-color, adjust-hue($table-color, -120deg), #222222);
}
```



![image-20190314105220460](/Users/liang/Desktop/github/notes/css/Sass/images/compass-table.png)



## [Helper 函数](http://compass-style.org/reference/compass/helpers/)

除了模块，Compass还提供一系列[函数](http://compass-style.org/reference/compass/helpers/)。

有些函数非常有用，比如[image-width()](http://compass-style.org/reference/compass/helpers/image-dimensions/#image-width)和[image-height()](http://compass-style.org/reference/compass/helpers/image-dimensions/#image-height)返回图片的宽和高。



再比如，[inline-image()](http://compass-style.org/reference/compass/helpers/inline-data/)可以将图片转为data协议的数据。

```sass
@import "compass";
.icon {
    background-image: inline-image("icon.png");
}
```







**函数与minxin最主要区别是不需要使用`@include`命令**