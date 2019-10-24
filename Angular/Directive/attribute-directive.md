# 属性型指令   

> 属性型指令 — 改变*元素、组件或其它指令*的外观和行为的指令

> 属性型指令改变一个元素的外观或行为。例如，内置的 NgStyle 指令可以同时修改元素的多个样式   



注意：   **属性型指令至少需要一个带有 @Directive装饰器的控制器类**     



```bash
ng generate directive directiveName
```



生成文件如下：   

```typescript
import { Directive } from '@angular/core';

@Directive({
    selector: '[appHighlight]'
})
export class HighlightDirective {
    constructor(){}
}
```



这里的方括号([])表示它的属性选择器。Angular会在模板中定位每个拥有名叫appHighlight属性的元素，并且为这些元素加上本指令的逻辑。   



> #### 为什么不直接叫做 "highlight"？
>
> 尽管 *highlight* 是一个比 *appHighlight* 更简洁的名字，而且它确实也能工作。 但是最佳实践是在选择器名字前面添加前缀，以确保它们不会与标准 HTML 属性冲突。 它同时减少了与第三方指令名字发生冲突的危险。
>
> 确认你**没有**给 `highlight` 指令添加**`ng`**前缀。 那个前缀属于 Angular，使用它可能导致难以诊断的 bug。例如，这个简短的前缀 `app` 可以帮助你区分自定义指令。



简单的用法：   

```typescript
import { Directive, ElementRef, HostListener } from '@angular/core';

@Directive({
    selector: '[appHighlight]'
})
export class HighlightDirective {
    
    @HostListener('mouseenter') onMouseEnter(){
        this.highlight('yellow');
    }
    @HostListener('mouseleave') onMouseLeave(){
        this.highlight(null);
    }
    
    constructor(el: ElementRef){}
    
    private highligh(color: string){
        this.el.nativeElement.style.backgroundColor = color;
    }
}
```



> 当然，你可以通过标准的 JavaScript 方式手动给宿主 DOM 元素附加一个事件监听器。 但这种方法至少有三个问题：
>
> 1. 必须正确的书写事件监听器。
> 2. 当指令被销毁的时候，必须*拆卸*事件监听器，否则会导致内存泄露。
> 3. 必须直接和 DOM API 打交道，应该避免这样做。

