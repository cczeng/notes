# 在组件中使用 onPush   

> 为了提升组件性能在可控制的情况下使用 `onPush` 策略。`onPush` 策略下，组件只有`@Input` 变动才会触发变更检测(仅限基础数据类型, 不包含引用类型)    



` ChangeDetectorRef ` 方法：   

- `.detectChanges()`  告诉Angular对组件及其子代运行更改检测
- ` ApplicationRef.tick() ` 告诉Angular对整个应用程序运行更改检测
- `.markForCheck()` 将所有onPush祖先标记为要进行一次检查，作为当前或下一个变更检测周期的一部分



```typescript
@Component({
  selector: 'counter',
  template: `{{count}}`,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class CounterComponent { 
  count = 0;

  constructor(private cdr: ChangeDetectorRef) {

    setTimeout(() => {
      this.count = 5;
      this.cdr.detectChanges();
    }, 1000);

  }

}
```



由于上面这个组件并没有@Input 所以在 count 变更的时候，并不会重新渲染 html 里面的值，也就避免了一些没必要的渲染动作。如果需要在特定情况下(这里为count)重新渲染html，则手动调用方法即可.