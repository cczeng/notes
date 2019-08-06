# [复杂动画序列](<https://angular.io/guide/complex-animation-sequences>)

- 使用`query()`和`stagger()` 函数为多个元素设置动画
- 使用`group()`函数的并行动画
- 顺序动画与平行动画

  

控制复杂动画序列的函数如下：   

- `query()` 找到一个或多个内部HTML元素 
- `stagger()` 对多个元素的动画应用练级延迟
- `group()` 并行运行多个动画步骤
- `sequence()` 一个接一个地运行动画步骤  

 ## 使用`query()`和`stagger()`函数 设置多个元素动画  

```js
animations: [
    trigger('pageAnimations', [
        transtion(
            ':enter', [
                query('.hero, form',[	// 查找.hero 和 form 元素
                    style({	// 转场前的样式
                        opacity:0,
                         transform: 'translateY(-100px)'
                    }),
                    stagger(-30,[	// 将每个动画延迟30ms
                        animate('500ms cubic-bezier(0.35,0,0.25,1)', style({	// 转场后的样式
                            opacity: 1,
                            transform: 'none'
                        }))
                    ])
                ])
            ])
    ])
]

export class HeroListPageComponent implements OnInit {
    @HostBinding('@pageAnimations') public animatePage = true;
}
```

  

## 使用 `group()` 函数并行动画  

```js
animations: [
    trigger('flyInOut', [
        state('in',style({
            width: 120,
            transform: 'translateX(0)',
            opacity: 1
        })),
        transition('void => *', [
            style({
                width: 10,
                transform: 'translateX(50px)',
                opacity: 0
            }),
            group([
                // 两个动画并行执行
                animate('0.3s 0.1s ease', style({
                    transform: 'translateX(0)',
                    width: 120
                })),
                // 两个动画并行执行
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
    ]),
]
```

  

## 顺序与并行动画  

 ### 过滤动画示例  

```html
<ul class="heroes" [@filterAnimation]="heroTotal"></ul>
```

```js
@Component({
    animations: [
        trigger('filterAnimation',[
            transition(':enter, * => 0, * => -1', []),
            transition(':increment', [
                query(':enter', [
                    style({
                        opacity: 0,
                        width: '0px'
                    }),
                    stagger(50, [
                        animate('300ms ease-out', style({
                            opacity: 1,
                            width: '*'
                        }))
                    ])
                ],{optional: true})
            ]),
            transition(':decrement', [
                query(':leave', [
                    stagger(50, [
                        animate('300ms ease-out',style({
                            opacity: 0, 
                            width:'0px'
                        }))
                    ])
                ])
            ])
        ])
    ]
})

export class HeroListPageComponent implements OnInit {
    heroTotal = -1;
}
```

