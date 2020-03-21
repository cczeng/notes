# 监听Form值变化



## 直接监听整个form值变化

```ts
@Component({...})
export class demoComponent implements OnInit {
	@ViewChild('form') form: NgForm;

	form$;

	ngOnInit(){
        this.form.valueChanges.pipe(
            takeUntil(this.form$),
            debounceTime(500),
        ).subscribe(values => {
           console.log(values); 
        });
    }
}
```





## form变化后让保存按钮可用  

```ts
@Component({...})
export class demoComponent implements OnInit {
	@ViewChild('form') form: NgForm;

	formValueSub$ = new Subject();

	ngOnInit(){
         merge(this.formValueSub$, this.form.valueChanges)
          .pipe(
            takeUntil(this.componentDestroy()),
            debounceTime(500),
            throttleTime(500),
            switchMap(val => val ? of(this.form.value) : empty()),
          ).subscribe(values => {
            console.log(values);
            this.headerOption = Array.from(new Set(this.headerOption).add('save'));
          });
    }

	fetch(){
        // 这里发起请求或者任何要给form里面字段写值操作的动作
        // 在这之后直接调用一次忽略
        this.formValueSub$.next(false);
    }
}
```

