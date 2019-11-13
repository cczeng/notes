# Angular Change Detection Strategy - Angular中的变更检测   

>每当应用程序由于用户事件或从网络请求接收到的数据而发生更改时，Angular都会对所有组件（从上到下）执行变更检测。变更检测非常有效，但是随着应用程序变得越来越复杂以及组件数量的增加，变更检测将不得不执行越来越多的工作。有一种方法可以避免这种情况，并将特定组件上的更改检测策略设置为`OnPush`。这样做将指示`Angular`仅在将新引用传递给它们以及仅对数据进行突变时才对这些组件及其子树进行变更检测。    —— [引用地址]( https://alligator.io/angular/change-detection-strategy/ )



> **变更检测策略**：  
>
> 在`OnPush`组件上使用策略时，您基本上对Angular说，它不应对何时需要执行更改检测做出任何猜测。**它仅对依赖于Input引用的更改，一些由其自身（组件）或其子项触发的事件。**最后，可以手动调用Angular的`componentRef.markForCheck()`方法。
>
> 使用`OnPush`，该组件仅取决于其输入并不包含不变性，更改检测将在以下情况下启动：   
>
> 1. 输入发生变化(@Input)
> 2. 事件源自组件或其子组件之一
> 3. 明确运行变更检测(`componentRef.markForCheck()`)
> 4. 在视图中使用异步管道





##  例子   

`app.component.ts`:   

```typescript
import { Component } from '@angular/core'; 

@Component({
    selector: 'app-root',
    templateUrl: './app.component.html'
})
export class AppComponent {
    foods = ['Bacon', 'Lettuce', 'Tomatoes'];
    
    addFood(food){
        this.foods.push(food);
    }
}
```

`app.component.html`:   

```html
<input #newFood type="text" placeholder="Enter a new food">
<button (click)="addFood(newFood.value)">Add food</button>

<app-child [data]="foods"></app-child>
```



子组件：  

`child.component.ts`:   

```typescript
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-child',
  templateUrl: './child.component.html'
})
export class ChildComponent {
  @Input() data: string[];
}
```

`child.component.ts`:    

```html
<ul>
  <li *ngFor="let item of data">{{ item }}</li>
</ul>
```



由于我们在子组件中从父组件接受其数据的输入，一切都按预期工作，并且新食品项被添加到列表中，现在，将子组件中的变更检测策略设置为`OnPush`:   

```typescript
import { Component, Input, ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-child',
  templateUrl: './child.component.html',
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class ChildComponent {
  @Input() data: string[];
}
```

这样，事情似乎不再起作用。新数据仍然会被推送到父组件中的foods数组中，但是Angular看不到数据输入的新引用，因此不会在组件上运行更改检测。

为了使其再次起作用，诀窍是将一个全新的引用传递给我们的数据输入。可以使用类似这样的方法来代替父组件的*addFood*方法中的*Array.push*：  

```typescript
addFood(food){
    this.foods = [...this.foods, food];
}
```

使用这种方法，我们不再改变数组，而是返回了一个全新的数组。  



## ChangeDetectorRef   

当使用`OnPush`的变更检测策略时，除了确保每次应更改某些内容时都传递新的引用外，我们还可以使用`ChangeDetectorRef`进行完全控制。    

```typescript
import { Component,
         Input,
         ChangeDetectionStrategy,
         ChangeDetectorRef } from '@angular/core';

@Component({
  selector: 'app-child',
  templateUrl: './child.component.html',
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class ChildComponent {
  @Input() data: string[];

  constructor(private cd: ChangeDetectorRef) {}

  refresh() {
    this.cd.detectChanges();
  }
}
```

现在，点击刷新按钮的时候，Angular会在组件上运行更改检测。   



## ChangeDetectorRef.markForCheck()    

假设您的数据输入实际上是可观察的，让我们使用Rxjs进行演示：   

```typescript
import { Component } from '@angular/core';
import { BehaviorSubject } from 'rxjs/BehaviorSubject';

@Component({ ... })
export class AppComponent {
  foods = new BehaviorSubject(['Bacon', 'Letuce', 'Tomatoes']);

  addFood(food) {
    this.foods.next(food);
  }
}
```

在子组件中订阅：  

```typescript
import { Component,
         Input,
         ChangeDetectionStrategy,
         ChangeDetectorRef,
         OnInit } from '@angular/core';

import { Observable } from 'rxjs/Observable';

@Component({
  selector: 'app-child',
  templateUrl: './child.component.html',
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class ChildComponent implements OnInit {
  @Input() data: Observable<any>;
  foods: string[] = [];

  constructor(private cd: ChangeDetectorRef) {}

  ngOnInit() {
    this.data.subscribe(food => {
      this.foods = [...this.foods, ...food];
    });
  }
}
```

 通常，这与我们的初始示例一样可以立即使用，但是这里新数据会使我们的*数据*可观察到的变化，因此Angular不会运行更改检测。解决方案是，当我们订阅可观察*对象*时，调用*ChangeDetectorRef*的*markForCheck*： 

```typescript
ngOnInit() {
  this.data.subscribe(food => {
    this.foods = [...this.foods, ...food];
    this.cd.markForCheck();
  });
}
```

