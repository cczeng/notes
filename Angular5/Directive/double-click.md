# double-click 双击事件与单击事件冲突

> 触发双击事件之前拦截单击事件.

使用指令:  

```javascript
import { Directive, Output, EventEmitter, HostListener } from '@angular/core';

@Directive({
  selector: '[myClick],[myDblClick]'
})
export class DoubleClickDirective {
  @Output() myClick = new EventEmitter<any>();
  @Output() myDblClick = new EventEmitter<any>();

  preventSimpleClick = false;
  timer;

  @HostListener('click', ['$event'])
  onClick(e) {
    this.preventSimpleClick = false;
    this.timer = setTimeout(() => {
      if (!this.preventSimpleClick) {
        console.log('simple click');
        this.myClick.next(e);
      }
    }, 300);
  }

  @HostListener('dblclick', ['$event'])
  onDblClick(e) {
    this.preventSimpleClick = true;
    clearTimeout(this.timer);
    this.myDblClick.next(e);
    console.log('double click');
  }



  constructor() { }

}

```

组件使用:   

```javascript
<a (myClick)="one()" (myDblClick)="two()" DoubleClick>test double click</a>
```