# [可重复使用的动画](<https://angular.io/guide/reusable-animations>)  

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

