# [Animations - 动画](https://angular.io/guide/animations)  



## 1. 启用动画模块  

导入`BrowserAnimationsModule`，将动画功能引入Angular根应用程序模块。

```js
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';

@NgModule({
    imports: [
        BrowserModule,
        BrowserAnimationsModule
    ],
    declarations: [],
    bootstrap: []
})

export class AppModule {}
```



## 2. 将动画功能导入组件文件  

```js
import { Component, HostBinding } from '@angular/core';
import { trigger, state, style, animate, transion } from '@angular/animations';
```



## 3. 添加动画元数据属性  

```js
@Component({
    selector: 'app-root',
    templateUrl: 'app.component.html',
    styleUrls: ['app.component.css'],
    animations: [
        // animation triggers go here
    ]
})
```



# 3.4 基础动画使用   

## 3.4.1 两个状态之间转场

```js
animations: [
    trigger('openClose', [
        // ...
        state('open', style({ // sate 设置一个状态, style 为在这个状态的时候样式,可以理解为目标样式
            height: '200px',
            opacity: 1,
            backgroundColor: 'yellow'
        })),
        state('closed', style({
            height: '100px',
            opacity: 0.5,
            backgroundColor: 'green'
        })),
        // transition 代表从某个状态转场到另一个状态的动作，这里为从任何状态到 closed
        transition('* => closed', [
            animate('1s')	// 动画时间
        ]),
        transition('* => open', [
            // 在 animate 里面的style代表目标样式,例如希望从 closed 状态下 opacity: 0.5 转到 opacity: 1 
            animate('0.5s', style({
                opacity: '*' // 通配符
            }))
        ]),
        // 两个状态之间的动画
        transition('open <=> closed', [
            animate('0.5s')
        ])
    ])
]
```

进场和离场也有个别名:   

```js
transition ( ':enter', [ ... ] );  // alias for void => *
transition ( ':leave', [ ... ] );  // alias for * => void

trigger('myInsertRemoveTrigger', [
  transition(':enter', [
    style({ opacity: 0 }), // 转场之前的样式
    animate('5s', style({ opacity: 1 })), // 转场的目标样式以及过度时间
  ]),
  transition(':leave', [
    animate('5s', style({ opacity: 0 }))
  ])
]),
```

使用：  

```html
<div @myInsertRemoveTrigger *ngIf="isShown" class="insert-remove-container">
  <p>The box is inserted</p>
</div>
```



## 3.4.2 递增和递减 - 具体需要看复杂动画  

`:increment`和`:decrement`。当数值增加或减少时，使用这些来启动转换。  

```js
trigger('filterAnimation', [
  transition(':enter, * => 0, * => -1', []),	// 多个转场一起
  transition(':increment', [
    query(':enter', [	// 组动画
      style({ opacity: 0, width: '0px' }), // 转场之前的样式
      stagger(50, [	// 50 代表的是间隔, 一组里面的动画间隔。这样可以显示成一个一个离开，而不是全部一起离开或者进入
        animate('300ms ease-out', style({ opacity: 1, width: '*' })),
      ]),
    ], { optional: true })	// 如果此查询是可选的，则为True;如果需要，则为false。默认值为false。如果在执行查询时未检索到任何元素，则必需的查询将引发错误。可选查询不会
  ]),
  transition(':decrement', [
    query(':leave', [
      stagger(50, [
        animate('300ms ease-out', style({ opacity: 0, width: '0px' })),
      ]),
    ])
  ]),
]),
```

  

## 3.4.3 布尔值  

```html
<div [@openClose]="isOpen ? true : false" class="open-close-container">
</div>
```

```js
animations: [
  trigger('openClose', [
    state('true', style({ height: '*' })),
    state('false', style({ height: '0px' })),
    transition('false <=> true', animate(500))
  ])
],
```



## 3.4.4 多个动画触发   

**禁用父动画**  

```html
<div [@.disabled]="isDisabled">	<!-- 使用@.disabled 禁用, 否则优先执行父动画 -->
  <div [@childAnimation]="isOpen ? 'open' : 'closed'"
    class="open-close-container">
    <p>The box is now {{ isOpen ? 'Open' : 'Closed' }}!</p>
  </div>
</div>
```

```js
@Component({
  animations: [
    trigger('childAnimation', [
      // ...
    ]),
  ],
})
export class OpenCloseChildComponent {
  isDisabled = false;
  isOpen = false;
}
```



**禁用所有动画**   

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
  public animationsDisabled = false;
}
```

  

## 3.4.5 动画回调  

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
  onAnimationEvent ( event: AnimationEvent ) {
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

  

## 3.4.6 关键帧  

```js
transition('* => active', [
  animate('2s', keyframes([
    style({ backgroundColor: 'blue', offset: 0}),	// offset 为偏移量
    style({ backgroundColor: 'red', offset: 0.8}),
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

  

# 4. 复杂动画  

- 使用`query()`和`stagger()` 函数为多个元素设置动画
- 使用`group()`函数的并行动画
- 顺序动画与平行动画

  

控制复杂动画序列的函数如下：   

- `query()` 找到一个或多个内部HTML元素 
- `stagger()` 对多个元素的动画应用练级延迟
- `group()` 并行运行多个动画步骤
- `sequence()` 一个接一个地运行动画步骤    



**多个元素设置动画**  

```js
animations: [
    trigger('pageAnimations', [
      transition(':enter', [
        query('.hero, form', [ // 查找.hero 和 form 元素
          style({opacity: 0, transform: 'translateY(-100px)'}), // 转场前的样式
          stagger(-30, [ // 延迟30ms
            animate('500ms cubic-bezier(0.35, 0, 0.25, 1)', style({ opacity: 1, transform: 'none' }))
          ])
        ])
      ])
    ]),
  ]
})
export class HeroListPageComponent implements OnInit {
  @HostBinding('@pageAnimations')
  public animatePage = true;
}
```

  

**并行动画**  

```js
animations: [
  trigger('flyInOut', [
    state('in', style({
      width: 120,
      transform: 'translateX(0)', opacity: 1
    })),
    transition('void => *', [
      style({ width: 10, transform: 'translateX(50px)', opacity: 0 }),
      group([
        // 动画会一起执行
        animate('0.3s 0.1s ease', style({
          transform: 'translateX(0)',
          width: 120
        })),
        // 动画会一起执行
        animate('0.3s ease', style({
          opacity: 1
        }))
      ])
    ]),
    transition('* => void', [
      group([
        animate('0.3s ease', style({
          transform: 'translateX(50px)',
          width: 10
        })),
        animate('0.3s 0.2s ease', style({
          opacity: 0
        }))
      ])
    ])
  ])
]
```

  

**实例**  

```html
<ul class="heroes" [@filterAnimation]="heroTotal"></ul>
```

```js
@Component({
  animations: [
    trigger('filterAnimation', [
      transition(':enter, * => 0, * => -1', []),
      transition(':increment', [
        query(':enter', [
          style({ opacity: 0, width: '0px' }),
          stagger(50, [
            animate('300ms ease-out', style({ opacity: 1, width: '*' })),
          ]),
        ], { optional: true })
      ]),
      transition(':decrement', [
        query(':leave', [
          stagger(50, [
            animate('300ms ease-out', style({ opacity: 0, width: '0px' })),
          ]),
        ])
      ]),
    ]),
  ]
})
export class HeroListPageComponent implements OnInit {
  heroTotal = -1;
}
```

# 5. [可重复使用的动画](<https://angular.io/guide/reusable-animations>)

## 创建可重复使用的动画  

在单独的 `.ts` 文件中编写  

```js
import {
  animation, trigger, animateChild, group,
  transition, animate, style, query
} from '@angular/animations';

export const transAnimation = animation([
  style({
    height: '{{ height }}',
    opacity: '{{ opacity }}',
    backgroundColor: '{{ backgroundColor }}'
  }),
  animate('{{ time }}')
]);
```

> **注：**该`height`，`opacity`，`backgroundColor`，和`time`输入运行时更换。   

您可以`transAnimation`在组件类中导入可重用变量，并使用`useAnimation()`如下所示的方法重用它。    

```js
import { Component } from '@angular/core';
import { useAnimation, transition, trigger, style, animate } from '@angular/animations';
import { transAnimation } from './animations';

@Component({
    trigger('openClose', [
      transition('open => closed', [
        useAnimation(transAnimation, {	// 这里使用并传递参数
          params: {
            height: 0,
            opacity: 1,
            backgroundColor: 'red',
            time: '1s'
          }
        })
      ])
    ])
  ],
})
```

  

# 6. 路由动画  

[点击跳转](./router_animation.md)