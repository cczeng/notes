# double-click 双击事件与单击事件冲突

> 触发双击事件之前拦截单击事件.

使用指令:  

```javascript
import { Directive, Output, EventEmitter, HostListener } from '@angular/core';

@Directive({
  selector: '[appDoubleClick]'
})
export class DoubleClickDirective {
  @Output() myClick = new EventEmitter<any>();
  @Output() mydblClick = new EventEmitter<any>();

  preventSimpleClick: boolean = false;
  timer;

  constructor() { }

  @HostListener('click', ['$event'])
  onClick(e) {
    this.preventSimpleClick = false;
    this.timer = setTimeout(() => {
      if (!this.preventSimpleClick) {
        this.myClick.emit(e);
      }
    }, 300);
  }

  @HostListener('dblclick', ['$event'])
  ondblClick(e) {
    this.preventSimpleClick = true;
    clearTimeout(this.timer);
    this.mydblClick.emit(e);
  }

}



```

组件使用:   

```javascript
<a (myClick)="one()" (mydblClick)="two()" appDoubleClick>test double click</a>
```