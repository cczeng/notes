# 同时取消多个订阅  

参考来自: [这里](https://blog.strongbrew.io/rxjs-best-practices-in-angular/)  

> 在`service`、`component`中一般会创建多个流,如果在组件或者服务被销毁的时候不`取消订阅`则会引发内存泄漏，可以在 `ngOnDestroy()` 钩子中一个一个取消订阅，但是这里有更好的方法 [tackUntil](https://rxjs-cn.github.io/learn-rxjs-operators/operators/filtering/takeuntil.html).   


```javascript
class AppComponent implements OnInit {
    destroy$ = new Subject();
    ngOnInit(){
        const interval$ = interval(1000);
        interval$.pipe(
            takeUntil(this.destroy$)            // tackUntil() 用于停止/完成本次订阅
        ).subscribe(v => console.log(v));
    }

    ngOnDestroy(){
        this.destroy$.next(true);
    }
}
```