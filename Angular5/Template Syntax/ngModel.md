# [[(ngModel)]](https://angular.io/guide/template-syntax#ngmodel---two-way-binding-to-form-elements-with-ngmodel) 双向绑定   
  
> 其实[(ngModel)]是angular的语法糖  

更多请参考Angular官方 [NgModel 属性指令](https://angular.io/api/forms/NgModel)

下面为其实现方式:   

```html

<input [(ngModel)]="currentHero.name">
<input [value]="currentHero.name" (input)="currentHero.name=$event.target.value">

<!-- 下面是angular官方的实现方法 -->
<input [ngModel]="currentHero.name" (ngModelChange)="currentHero.name=$event">

```
