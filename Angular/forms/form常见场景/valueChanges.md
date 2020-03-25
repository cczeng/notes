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

	formIgnore$ = new Subject();

	ngOnInit(){
          merge(this.formIgnore$, this.form.valueChanges).pipe(
          bufferTime(1000),
          switchMap(values => {
            const hasIgnore = values.some(v => !v);
            if (hasIgnore) {
              // 这里删掉保存按钮状态, 如果不需要的话直接写 return of(false) 即可
              const header = new Set(this.headerOption);
              header.delete('save');
              this.headerOption = Array.from(header);
              return of(false);
            }
            return of(values[values.length - 1]);
          }),
          filter(val => !!val),
        ).subscribe(value => {
          console.log(value);
          this.headerOption = Array.from(new Set(this.headerOption).add('save'));
        });
    }

	fetch(){
        // 这里发起请求或者任何要给form里面字段写值操作的动作
        // 在这之后直接调用一次忽略
        this.formIgnore$.next(false);
    }
}
```

