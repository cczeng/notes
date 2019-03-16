# Angular2+ 项目部署到服务器刷新404   

> 因为Angular route 的问题，所以在网页刷新的时候,默认是使用服务器后台的路由进行导航,在没有配置后台的时候就会导致404.我们可以使用在URL中添加一个 `#` 来告诉服务器后台使用web进行导航.   

**App.module**

```javascript
import {HashLocationStrategy,LocationStrategy} from '@angular/common';

@NgModule({
    declarations: [AppCmp],
    bootstrap: [AppCmp],
    imports: [BrowserModule,routes],
    providers: [{provide:LocationStrategy,useClass:HashLocationStrategy}]
});

```

主要添加代码:   

```providers: [{provide:LocationStrategy,useClass:HashLocationStrategy}]```