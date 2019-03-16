# [form control](https://angular.io/api/forms/FormControlDirective) 表单控制 - 可实现input去抖动   
  
今天偶然在一篇[博客](http://codin.im/2016/09/17/observables-in-angular2-translate/)中发现了 `input` 去抖动的另一种写法，这种写法更方便,且使用的是`Angular` 官方的表单一章节中,因为直接未接触过表单所以没有详细去看文档。   

## input 去抖动，可用于搜索   

``` html
<!-- 这里直接使用 formControl 指令 -->
<input type="text" [formControl]="term"/>
```   

在 `component` 中:   
里面涉及到 `forms`, 其中 `Validators` 是用来控制输入大小的可参考[官方文档](https://angular.io/api/forms/FormControlDirective).  


```javascript
...
import {FormControl,Validators} from '@angular/forms';

export class AppComponent{
    items: Array<string>;
    term = new FormControl('value', Validators.maxLength(10));  // 这里需要在module里面导入 @angular/forms   NgModule: ReactiveFormsModule  
    constructor(
        private myService: MyService
    ){
        this.term.valueChanges
                 .debounceTime(500)             //延迟500秒
                 .distinctUntilChanged()        // 防止相同输入触发两次,如果两次数据一样则不触发
                 .subscribe(term => this.myService.search(term).then(items => this.items = items));
    }
}
```