# 使用[switchMap](https://rxjs-cn.github.io/learn-rxjs-operators/operators/transformation/switchmap.html)避免循环嵌套  
  

**下面是不推荐的方法**:  

```js
class AppComponent {
    user;

    constructor(
        private route: ActivateRoute,
        private userService: UserService
    ){
        this.route.params.pipe(map(v => v.id))
        .subscribe(id => {
            this.userService.fetchById(id).subscribe(user => this.user = user);   // 引发多层嵌套
        })
    }
}
```  

**使用下面的方法会更好**:  

```js
class AppComponent {
    user;

    constructor(
        private route: ActivateRoute,
        private userService: UserService
    ){
        this.route.params.pipe(
            map(v => v.id),
            switchMap(id => this.userService.fetchById(id)) // 使用switchMao
        ).subscribe(user => this.user = user);
    }
}
```