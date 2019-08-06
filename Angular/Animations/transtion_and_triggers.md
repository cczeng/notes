# 转场和触发  

## 预定义状态和通配符匹配  

在Angular中，可以通过 `state()`函数或使用预定义`*(通配符)`和`void`状态显示定义转换状态。  



### 通配符状态  

星号`*`或通配符匹配任何动画状态。这对于定义适用的转换非常有用，无论HTML元素的开始或结束状态如何。   

例如， `open => *` 当元素的状态从打开变为其他任何状态时，应用转场效果。   

![动画转场](https://angular.io/generated/images/guide/animations/wildcard-state-500.png)

```js
animations: [
    trigger('openClose', [
        //...
        state('open', style({
            height: '200px',
            opacity: 1,
            backgroundColor: 'yellow'
        })),
        state('closed', style({
            height: '100px',
            opacity: 0.5,
            backgroundColor: 'green',
        })),
        transition('* => closed', [
            animate('1s')
        ]),
        transition('* => open', [
            animate('0.5s')
        ])
    ])
]
```

双箭头语法指定两个方向上的状态到状态转换。   

```js
transition('open <=> closed', [
    animate('0.5s')
])
```

  

### 使用具有多个多个过度状态   

![多个状态](https://angular.io/generated/images/guide/animations/wildcard-3-states.png)

  

```js
animations: [
    trigger('openClose', [
        // ...
        state('open', style({
            height: '200px',
            opacity: 1,
            backgroundColor: 'yellow'
        })),
        state('closed', style({
            height: '100px',
            opacity: 0.5,
            backgroundColor: 'green'
        })),
        transition('open => closed', [ // 和router一样，排在前面的会替换后面的。 open => closed 会覆盖 * => *
            animate('1s')
        ]),
        transition('closed => open', [
            animate('0.5s')
        ]),
        transition('* => open', [
            animate(1s)
        ]),
        transition('* => closed', [
            animate('0.5s')
        ]),
        transition('open <=> closed', [
            animate('0.5s')
        ]),
        transition('* => open', [
            animate('1s', {
                style({
                    opacity: '*'   // 带有样式的通配符
                })
            })
        ]),
        transition('* => *',[  // 两个任何状态转场效果使用 * =>
            animate('1s')
        ])
    ])
]
```

转场按其定义的顺序进行匹配。因此，可以在 `* => *`任意到任意转场之上应用其他转换。   



**使用带有样式          通配符**  

```js
transition('* => open', [
    animate('1s', {
        style({
            opacity: '*'   // 带有样式的通配符
        })
    })
]),
```

  

## 进入动画和离开动画   

> 出于我们的目的，进入或离开视图的元素相当于从DOM中插入或删除。   



```js
animations: [
    trigger('flyInOut',[
        state('in', style({ transform: 'translateX(0)' })),	// 对应状态的时候样式,可理解为目标样式
        transition('void => *',[ // 进场
            style({transform: 'translateX(-100%)'}),
            animate(100)
        ]),
        transition('* => void', [ // 离场
            animate(100,style({ transform: 'translateX(100%)' }))
        ])
    ])
]
```

也可以写成:   

**:enter     :leave** 别名  

```js
transition(':enter', [...]); // alias for void => *
transition(':leave', [...]); // alias for * => void
```

  

**使用*ngIf 和 *ngFor   :enter    :leave**  使用   

```html
<div @myInsertRemoveTrigger *ngIf="isShown" class="insert-remove-container">
    <p>
        The box is inserted
    </p>
</div>
```

在组件文件中， `:enter` 转换将初始不透明度设置为0, 然后将其设置为动画，以便在将元素插入视图时将该不透明度变更为1。   

```js
trigger('myInsertRemoveTrigger', [
    transition(':enter', [
        style({opacity: 0}),	// 转场初始值
        animate('5s',style({opacity: 1})) // 写在 animate() 里面的为目标值
    ]),
    transition(':leave', [
        animate('5s', style({opacity: 0})) // 目标值 opacity: 0
    ])
])
```



> 要点： transition(':enter', [
>
> ​	style()  // 函数里面的为转场之前的初始值
>
> ​	animate() // 函数里面的为转场的目标值
>
> ])  

###  转场中的递增和递减   

`transition()` 函数采用其他选择器值， `:increment` 和 `:decrement` 。当数值增加或减少时，应用此转场。   

```js
trigger('filterAnimation', [
    transition(':enter,* => 0, * => -1', []),  // 多个转场一起应用,且直接写值
    transition(':increment',[
    	query(':enter', [ // 使用组动画
            style({ opcation: 0, width: '0px'}), // 应用转场动画之前的状态
            stagger(50, [ // 错开时间顺序，而不是一次全部应用
                animate('300ms ease-out', style({opacity: 1,width: '*'})),
            ])
        ], {optional: true }) // 如果此查询是可选的，则为True;如果需要，则为false。默认值为false。如果在执行查询时未检索到任何元素，则必需的查询将引发错误。可选查询不会。
    ]),
    transition(':decrement', [
        query(':leave', [
            stagger(50, [
                animate('300ms ease-out', style({ opacity: 0, width: '0px'}))
            ])
        ])
    ])
])
```

  

**使用转场          布尔值**   

```html
<div [@openClose]="isOpen ? true : false" class="open-close-container"></div>
```



```js
animation: [
    trigger('openClose', [
        state('true', style({height: '*'})),	// 对应状态的时候样式,可以理解为目标值
        state('false', style({height: '0px'})),	// 对应状态的时候样式,可以理解为目标值
        transition('false <=> true', animate(500))
    ])
]
```

  

## 多个动画触发  



### 父子动画  



#### 禁用父动画    

每次Angular中触发动画时，父动画适中获得优先级，并且子动画被阻止

```html
<div [@.disabled]="isDisabled">
  <div [@childAnimation]="isOpen ? 'open' : 'closed'"
    class="open-close-container">
    <p>The box is now {{ isOpen ? 'Open' : 'Closed' }}!</p>
  </div>
</div>
```

  

#### 禁用所有动画  

```js
@Component({
  selector: 'app-root',
  templateUrl: 'app.component.html',
  styleUrls: ['app.component.css'],
  animations: [
    slideInAnimation
    // animation triggers go here
  ]
})
export class AppComponent {
  @HostBinding('@.disabled')
  public animationsDisabled = false;	// 禁用所有动画
}
```

  

### 动画回调    

动画`trigger()`函数在启动和完成时会发出回调。   

```js
@Component({
  selector: 'app-open-close',
  animations: [
    trigger('openClose', [
      // ...
    ]),
  ],
  templateUrl: 'open-close.component.html',
  styleUrls: ['open-close.component.css']
})
export class OpenCloseComponent {
  onAnimationEvent ( event: AnimationEvent ) {	// 动画回调
  }
}
```

 ```html
<div [@openClose]="isOpen ? 'open' : 'closed'"
    (@openClose.start)="onAnimationEvent($event)"
    (@openClose.done)="onAnimationEvent($event)"
    class="open-close-container">
</div>
 ```

  

## 关键帧  

在之前，都是简单的双状态转换。现在创建一个动画，其中使用关键帧按顺序运行多个步骤。    

![关键帧](https://angular.io/generated/images/guide/animations/keyframes-500.png)

  

```js
transition('* => active', [
    animate('2s', keyframes([
        style({backgroundColor: 'blue'}),
        style({backgroundColor: 'red'}),
        style({backgroundColor: 'orange'})
    ]))
])
```

  #### 偏移   

![偏移](https://angular.io/generated/images/guide/animations/keyframes-offset-500.png)

  

```js
transition('* => active', [
  animate('2s', keyframes([
    style({ backgroundColor: 'blue', offset: 0}),	// 偏移量
    style({ backgroundColor: 'red', offset: 0.8}),	// 0-1
    style({ backgroundColor: 'orange', offset: 1.0})
  ])),
]),
transition('* => inactive', [
  animate('2s', keyframes([
    style({ backgroundColor: 'orange', offset: 0}),
    style({ backgroundColor: 'red', offset: 0.2}),
    style({ backgroundColor: 'blue', offset: 1.0})
  ]))
]),
```



#### 具有缓动   

![具有缓动](https://angular.io/generated/images/guide/animations/keyframes-pulsation.png)

  

```js
trigger('openClose', [
  state('open', style({
    height: '200px',
    opacity: 1,
    backgroundColor: 'yellow'
  })),
  state('close', style({
    height: '100px',
    opacity: 0.5,
    backgroundColor: 'green'
  })),
  // ...
  transition('* => *', [
    animate('1s', keyframes ( [
      style({ opacity: 0.1, offset: 0.1 }),
      style({ opacity: 0.6, offset: 0.2 }),
      style({ opacity: 1,   offset: 0.5 }),
      style({ opacity: 0.2, offset: 0.7 })
    ]))
  ])
])
```

#### 动画属性和单位   

Angular的动画支持建立在Web动画之上，因此您可以为浏览器认为可动画的任何属性设置动画。这包括位置，大小，变换，颜色，边框等。W3C在其[CSS Transitions](https://www.w3.org/TR/css-transitions-1/)页面上维护一个可动画属性列表。  

可使用的有:   

- 50px: '50px'
- 相对字体大小: '3em'
- 百分比： '100%'

  

#### 使用通配符   自动计算属性

```js
animations: [
    trigger('shrinkOut',[
        state('in', style({ height: '*' })),	// 自动匹配元素的高度
        transition('* => void', [
            style({ height: '*' }),	// 自动匹配元素的高度
            animate(250, style({ height: 0 }))
        ])
    ])
]
```

