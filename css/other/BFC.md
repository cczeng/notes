# BFC

> **BFC(Block formatting context)直译为"块级格式化上下文"。所谓的BFC就是css布局的一个概念，是一块区域，一个环境。**



[摘录文章](https://juejin.im/post/5909db2fda2f60005d2093db)



文档流实际上有三种：

- 定位流
- 浮动流
- 普通流(BFC中的FC)

FC: Formatting Context(格式化上下文)，它是页面中的一块渲染区域，有一套渲染规则，决定了其子元素如何布局，以及和其他元素之间的关系和作用。



常见的FC有：

- BFC(普通流)
- IFC(行级格式化上下文)
- GFC(网格布局格式化上下文)
- FFC(自适应格式化上下文)

BFC 可以简单的理解为**某个元素的一个 CSS 属性**，只不过这个属性**不能被开发者显式的修改**，拥有这个属性的元素对内部元素和外部元素会表现出一些特性，这就是BFC。



## 触发条件 | 哪些元素会生成BFC

满足下列条件之一就可触发BFC

　　【1】根元素，即HTML元素

　　【2】float的值不为none

　　【3】overflow的值不为visible

　　【4】display的值为inline-block、table-cell、table-caption

　　【5】position的值为absolute或fixed 　



## 作用

1. 自适应两栏布局
2. 可以阻止元素被浮动元素覆盖
3. 可以包含浮动元素——清除内部浮动
4. 分属于不同的BFC时可以阻止 margin 重叠



## BFC布局规则

1. 内部的Box会在垂直方向，一个接一个地放置。

2. Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠

3. 每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。

4. BFC的区域不会与float box重叠。

5. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。

6. 计算BFC的高度时，浮动元素也参与计算



### 1. 内部的Box会在垂直方向，一个接一个的放置

>  我们平常说的盒子是由margin、border、padding、content组成的，实际上每种类型的四条边定义了一个盒子，分别是分别是**content box、padding box、border box、margin box**，这四种类型的盒子一直存在，即使他们的值为0.决定块盒在包含块中与相邻块盒的垂直间距的便是margin-box。



### 2. Box 垂直方向的距离由margin决定，属于同一个BFC的两个相领Box的margin会发生重叠

![img](https://lc-gold-cdn.xitu.io/6b0fc0e3d34f94875d35.gif?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



### 3. 每个元素的margin box的左边，与包含块border box的左边相接触（对于从左往右的格式化，否则相反）。即使存在浮动也是如此







### 4. BFC的区域不会与float box重叠

