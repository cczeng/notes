# ng-content 和 内容投影    

> 另附一篇参考 [Angular 开发实战-使用ng-content进行组件内容投射]( https://www.cnblogs.com/laixiangran/p/8723166.html )  
>
> 国外文章参考[Angular ng-content and Content Projection]( https://blog.angular-university.io/angular-ng-content/ )



为什么要使用 `ng-content` 内容投影？ 内容投影可以做到组件能够接收外部投射进来的内容，也就是说组件最终呈现的内容不仅仅是本身定义的那些。   

例如`Material`的部分组件，只需要在某个`div`或者`input`上绑定对应的`属性`(组件本身可以当做标签也可以当做属性使用)，但是附加了好多内容在`div`里面，并且你真正的元素还被包裹在里面，达到一种在你要的内容之外套了一层`壳`



## 简单投射

`collapse.component.html`:   

```html
<div class="collapse">
    <button (click)="taggle()">
        这是一个切换开关的按钮
    </button>
    <div *ngIf="isOpen" class="content">
        <ng-content></ng-content>
    </div>
</div>
```

这是一个最简单的应用场景，也是很简陋的例子。 这里由 `ng-content` 将内容投射到对应区域。   

`app.component.html`：   

```html
<collapse>
	这是我要的内容，位于可折叠区域，在这外面应该会有一个按钮控制开关。
</collapse>
```



这里需要注意的是：**ng-content** 投射进来的内容，**不会因为*ngIf而销毁或者重新创建**。    



**这里不过多解释，仅做一个案例查看。如需深入了解，请点击最上方的两个链接自行阅读**   