# 从tabs下面跳转到Root   

![1565230415394](C:\Users\youhaosoft\Desktop\github\notes\Ionic\ionic3\images\setting.png)

  

例如从`tabs` 下面的`setting`中点击退出按钮:   

```js
import {App} from 'ionic-angular';

constructor(
    private app: App
){}

this.app.getRootNav().setRoot('WelcomePage',{},{
    animate: true
});
```

​    

**解析**：  

默认在组件中使用的都是 `NavController` 服务，这个只会在当前的`Root` 也就是 `tabs`对应页面跳转，而不是真的`AppRoot`下跳转。 所以应该使用 `App` 服务，首先获取到`RootNav` 然后在`RootNav`上`setRoot()` 即可。   



**另外思考**：   

因为根`Root`是由 `appComponent` 接管，所以也可以使用 `event` 方式，通知 `appComponent` 替换当前 `rootPage`     

```js
@Component({
  template: `<ion-menu [content]="content" type="overlay">
    <ion-header>
      <ion-toolbar>
        <ion-title>Pages</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content>
      <ion-list>
        <button menuClose ion-item *ngFor="let p of pages" (click)="openPage(p)">
          {{p.title}}
        </button>
      </ion-list>
    </ion-content>

  </ion-menu>
  <ion-nav #content [root]="rootPage"></ion-nav>`
})
export class MyApp {
  @ViewChild(Nav) nav: Nav;
  rootPage;
  // ... 
    
  // 假设此处为响应
  setRoot(page: string){
      this.nav.setRoot(page);
  }
}
```

