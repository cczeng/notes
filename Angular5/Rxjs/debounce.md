# 去抖动   

在很多场合我们需要到去抖动,例如`input`输入的时候需要去抖动,以及有一个事件可能存在经常被触发，但是并不想要频繁触发.这个时候我们就需要用到 `Rxjs` 中 `Subject` 的 `pipe debounceTime` 去抖动.   

> [参考国外的一片文章](https://coryrylan.com/blog/creating-a-custom-debounce-click-directive-in-angular)

一个简单的调用方法时候去抖动的demo:   

```javascripe
import { Subject } from 'rxjs/Subject';
...

export class AppComponent implements {
    sendData = new Subject<any>();
    sendData$;    

    ngOnInit(){
        this.sendData$ = this.sendData.pipe(
            debounceTime(3000)  // 3秒
        ).subscribe(data => {
            this.send(data);    // 此方法为可能会频繁调用的方法
        });
    }

    update(data){
        this.sendData.next(data);
    }
}
```