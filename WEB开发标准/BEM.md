# [BEM(B__E_M)](http://getbem.com/introduction/)



BEM 是一个编写标准，结构化的CSS命名规则。   



**B**: 对应一个块，这里的块并不是指`div`这种块。而是**独立实体**

**E**: 对应元素，是属于块的一部分，没有独立的含义。与块关联

**M**: 块或者元素上的标识，用来改变外观或者行为。



![image-20190316170056364](/Users/liang/Desktop/github/notes/WEB开发标准/images/bem.png)



如[官网](http://getbem.com/introduction/)所写。



例子:  



**B(Block)**:   `header` , `container`,`menu`,`checkbox`,`input`		——具有意义的独立实体

**E(Element)**： `menu item` ,`list item`,`checkbox caption`,`header`,`title`    ——没有独立含义，属于块的一部分，与块相关联。

**M(Modifier)**:  `disabled`,`highlighted`,`checked`,`fixed`,`size big`,`color yellow`  —— 改变外观或者行为





**注意:**  <u>BEM 最多只有 B E M 三级</u>，不可能出现 <u>B E E E … M</u>超长class名，也要求 E 不能同名。



## 连接符号



可以采用[如何看待 CSS 中 BEM 的命名方式？ - 大猫的回答 - 知乎](
https://www.zhihu.com/question/21935157/answer/20116700) 上面的回答:  



`__` 双下划线代表 B 和 E 连接，例如 `menu__item` 

`_` 单下划线代表B和M或E和M的连接，例如 `menu_active` 或 `menu__item_active`

`-` 中划线代表字符连接，例如 `mod-menu` 或 `mod-menu_item`。**B或者E或者M需要多个单词来描述，则使用中划线**



