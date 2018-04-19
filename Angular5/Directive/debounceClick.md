# debounce click 去抖动

**指令:**

```javascript
import { Directive, EventEmitter, Output, Input, OnInit, OnDestroy, HostListener } from '@angular/core';
import { Subject } from 'rxjs/Subject';
import { Subscription } from 'rxjs/Subscription';
import { debounceTime } from 'rxjs/operators';

@Directive({
  selector: '[appDebounceClick]'
})
export class DebounceClickDirective implements OnInit, OnDestroy {
  @Input() debounceTime = 500;
  @Output() debounceClick = new EventEmitter();

  private clicks = new Subject();
  private subscribe: Subscription;


  constructor() { }

  ngOnInit() {
    this.subscribe = this.clicks.pipe(
      debounceTime(this.debounceTime)
    ).subscribe(e => this.debounceClick.emit(e));
  }
  ngOnDestroy() {
    this.subscribe.unsubscribe();
  }

  @HostListener('click', ['$event'])
  clickEvent(event) {
    event.preventDefault();
    event.stopPropagation();
    this.clicks.next(event);
  }

}

```

**使用:**  

```html
<button appDebounceClick (debounceClick)="log()" [debounceTime]="700">Debounced Click</button>
```