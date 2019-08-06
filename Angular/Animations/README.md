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



