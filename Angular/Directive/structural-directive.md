# 结构型指令   

> 结构型指令 - 通过添加和移除DOM元素改变DOM布局的指令  

> 结构型指令的职责是HTML布局。它们塑造或重塑DOM的机构，比如添加、移除或维护这些元素    



### 星号（*）前缀   



```html
<div *ngIf="hero" class="name">
    {{hero.name}}
</div>
```

星号是一个用来简化更复杂语法的“语法糖”。从内部实现来说，Angular把 *ngIf 属性翻译成一个 `<ng-template>` 元素并用它来包裹宿主元素   

```html	
<ng-template [ngIf]="hero">
	<div class="name">
        {{hero.name}}
    </div>
</ng-template>
```

- *ngIf 指令被移到了`<ng-template>`元素上。在那里它变成了一个属性绑定[ngIf]
- `<div>` 上的其余部分，包括它的class属性在内，移到了内部的`<ng-template>`元素上



### 模板输入变量

**模板输入变量**是一种变量，你可以**在单个实例的模板中**引用它的值。   

**模板输入变量**和**模板引用变量是不同的**，无论是在语义上还是语法上。   

你使用 let 关键词(如 let hero) 在模板中声明一个模板输入变量。这个变量的范围被限制在所重复模板的**单一实例**上。实际上，你可以在其它结构型指令中使用同样的变量名。     



而**声明模板引用变量** 使用的是给变量名加 # 前缀的方式(#var)。 **一个引用变量引用的是它所附着到的元素、组件或指令。**他可以在整个模板的任意位置访问。   



### 每个宿主元素上只能有一个结构型指令    

 有时你会希望只有当特定的条件为真时才重复渲染一个 HTML 块。 你可能试过把 `*ngFor` 和 `*ngIf` 放在同一个宿主元素上，但 Angular 不允许。这是因为你在一个元素上只能放一个*结构型*指令。   

 原因很简单。结构型指令可能会对宿主元素及其子元素做很复杂的事。当两个指令放在同一个元素上时，谁先谁后？`NgIf` 优先还是 `NgFor` 优先？`NgIf` 可以取消 `NgFor` 的效果吗？ 如果要这样做，Angular 应该如何把这种能力泛化，以取消其它结构型指令的效果呢？     



### `<ng-template>` 元素   

`<ng-template>` 是一个Angular元素，用来渲染HTML。 **它永远不会直接显示出来。** 事实上，在渲染视图之前，Angular 会把 `<ng-template>` 及其内容替换为一个注释。   

如果没有使用结构型指令，而仅仅把一些别的元素包装进`<ng-template>`中，那些元素就是不可见的。   



### `<ng-container>` 元素

> Angular 的 <ng-container> 是一个分组元素，但它不会污染样式或元素布局，因为Angular压根不会把它放进DOM中。   



## 自定义结构型指令   

写一个`UnlessDirective`的结构型指令，它是NgIf的反义词。  

```html
<p *appUnless="condition">
    Show this sentence unless the condition is true.
</p>
```



指令：   

```typescript
import { Directive, Input, TemplateRef, ViewContainerRef } from '@angular/core';

@Directive({ selector: '[appUnless]' })		// 选择器不能使用ng开头,因为ng是Angular内置
export class UnlessDirective {
    @Input() set appUnless(condition: boolean){
        if(!condition && !this.hasView){
            // 条件为假，并且视图未创建过，就告诉视图容器根据模板创建一个内嵌视图。
            this.viewContainer.createEmbeddedView(this.templateRef);
            this.hasView = true;
        }else if(condition && this.hasView){
            // 如果条件为真, 并且视图已经显示出来了，就清楚该容器，并销毁
            this.viewContainer.clean();
            this.hasView = false;
        }
    }
    
    private hasView = false;
    
    constructor(
    	private templateRef: TemplateRef<any>,
        private viewContainer: ViewContainerRef
    ) {}
    
    
}
```