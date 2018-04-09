# 路由跳转(router)

Angular2+ 的路由跳转大致有两种方式:  

1. [routerLink](#routerlink)
1. [navigate](#navigate)


<span id="routerlink"></span>
## routerLink   

例:  

```
    <a routerLink="/home" routerLinkActive="active-class">
        Home
    </a>
```

其中 `routerLink` 代表要跳转的路由, `routerLinkActive` 当前路由时要添加的class   


<span id="navigate"></span>
## navigate

无参数跳转:  

```
    import { Router } from '@angular/router';
    ...

    gotoRoute(route:string){
        this.router.navigate(['/home']); 
    }
```

带参数跳转:   

```
    import { Router } from '@angular/router';
    
    ...

    gotoRoute(route:string,user){
        this.router.navigate(['/home', { id: user.id, age: user.age }]);
    }
```


接收:   

```
    import { Router, ActivatedRoute, ParamMap } from '@angular/router';
    import 'rxjs/add/operator/switchMap'; 

    ...

    // 使用快照，component永远不重复使用
    let id = this.route.snapshot.paramMap.get('id');


    // Rxjs map, component 多次复用而采用这个方法
    ngOnInit() {
        this.hero$ = this.route.paramMap
            .switchMap((params: ParamMap) =>
            this.service.getHero(params.get('id')));
    }
```