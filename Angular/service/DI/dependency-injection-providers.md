# 依赖注入方式  

## Provider 对象字面量



```typescript
providers: [Logger],
```

```typescript
providers: [ { provide: Logger, useClass: Logger } ]
```

上面两个等价，也是最基本的用法



## 替代类提供者

> 不同的类可以提供相同的服务。当组件请求使用Logger服务注入的时候，返回一个 BetterLogger 实例

```typescript
providers: [ { provide: Logger, useClass: BetterLogger } ]
```

这个用来更新以前的旧类，但是又不想去动组件代码的时候方便有效。



## 带依赖的类提供者

> 另一个类 EvenBetterLogger 需要在日志里面显示用户名，这个logger需要从注入的 UserService 实例中来获取该用户

```typescript
@Inject()
export class EventBetterLogger extends Logger {
    constructor(private userService: UserService) { 
    	super();
    }
    
    log(message: string){
        let name = this.userService.user.name;
        super.log(`Message to ${name}: ${message}`);
    }
}
```

使用 EventBetterLogger 来替代 Logger 服务。

```typescript
providers: [
    UserService,
    { provide: Logger, useClass: EventBeeterLogger }
]
```



## 类别名提供者

> 假设老的组件依赖于 OldLogger 类。OldLogger 类和 NewLogger 的接口相同，但是由于某种原因，我们没办法修改老组件使用 NewLogger。
>
> 当老组件要使用 OldLogger 记录信息时，你希望改用 NewLogger 的**单例**来处理它。这个时候无论某个组件请求老的 OldLogger 还是新的 NewLogger ，依赖注入器都应该注入新的服务。也就是说 OldLogger 应该是新的 NewLogger **别名**

使用 `useClass` 为 OldLogger 指定一个别名的话，就会在**应用程序中得到两个 NewLogger 的实例**。

```typescript
// 下面方法会得到两个实例
providers: [
    NewLogger,
    { provide: OldLogger, useClass: NewLogger }
]
```

如果只需要一个实例，则使用 `useExisting` 来为 OldLogger 指定别名。

```typescript
// 下面只会得到一个实例
provider: [
    NewLogger,
    { provide: OldLogger, useExisting: NewLogger },
]
```



## 值提供者

> 有时候 ，提供一个现成的对象会要比要求注入器从类中去创建更简单一些。如果要注入一个你已经创过的对象，请使用 `useValue` 选项来配置该注入器。

```typescript
export function SilentLoggerFn() {}

const silentLogger = {
    logs: ['Silent logger says "Shhhhh!". Provided via "useValue"'],
    log: SilentLoggerFn
}
```

下面的提供者定义对象使用`useValue`作为key来把该变量与 Logger 令牌关联起来。

```typescript
providers: [
    { provide: Logger, useValue: silentLogger }
]
```



## 非类依赖

> 并非所有的依赖都是类。有时候你会希望注入字符串、函数或对象

`app.config.ts`: 

```typescript
export const HERO_DI_CONFIG: AppConfig = {
    apiEndpoint: 'api.heroes.com',
    title: 'Dependency Injection'
};
```

TypeScript 接口不是有效的令牌。

`HERO_DI_CONFIG` 常量满足 `AppConfig` 接口的要求。 不幸的是，你不能用 TypeScript 的接口作为令牌。 在 TypeScript 中，接口是一个设计期的概念，无法用作 DI 框架在运行期所需的令牌。

```
// FAIL! Can't use interface as provider token
// 失败！不能使用接口作为提供者令牌
 providers: [
 	{provide: AppConfig, useValue: HERO_DI_CONFIG }
 ]
```

```typescript
// FAIL! Can't inject using the interface as the parameter type
//失败！无法使用接口作为参数类型进行注入
constructor(private config: AppConfig){}
```

> 如果你曾经在强类型语言中使用过依赖注入功能，这一点可能看起来有点奇怪，那些语言都优先使用接口作为查找依赖的 key。 不过，JavaScript 没有接口，所以，当 TypeScript 转译成 JavaScript 时，接口也就消失了。 在运行期间，没有留下任何可供 Angular 进行查找的接口类型信息。

替代方案之一是以类似于 AppModule 的方式，在NgModule中提供并注入这个配置对象。



`app.module.ts`: 

```typescript
providers: [
    UserService,
    { provide: APP_CONFIG, useValue: HERO_DI_CONFIG }
]
```



另一个为非类依赖选择提供者令牌的解决方案是**定义并使用InjectionToken**对象。

`app.config.ts` :

```typescript
import { InjectionToken } from '@angular/core';

export const APP_CONFIG = new InjectionToken<AppConfig>('app.config');
```

虽然类型参数在这里是可选的，不过还是能把此依赖的类型信息传达给开发人员和开发工具。 这个令牌的描述则是开发人员的另一个助力。

使用 `InjectionToken` 对象注册依赖提供者：

```typescript
providers: [ 
	{ provide: APP_CONFIG, useValue: HERO_DI_CONFIG }
]
```

现在，借助参数装饰器 `@Inject()`，你可以把这个配置对象注入到任何需要它的构造函数中。

```typescript
constructor(@Inject(APP_CONFIG) config: AppConfig){
    this.title = config.title;
}
```

关于`InjectionToen`接口可点击 [这里](https://angular.io/api/core/InjectionToken)





## 工厂提供者

> 有时候你需要动态创建依赖值，创建时需要的信息你要等运行期间才能拿到。 比如，你可能需要某个在浏览器会话过程中会被反复修改的信息，而且这个可注入服务还不能独立访问这个信息的源头。
>
> 这种情况下，你可以使用*工厂提供者*。 当需要从第三方库创建依赖项实例时，工厂提供者也很有用，因为第三方库不是为 DI 而设计的。
>
> 比如，假设 `HeroService` 必须对普通用户隐藏*秘密*英雄，只有得到授权的用户才能看到他们。
>
> 像 `EvenBetterLogger` 一样，`HeroService` 需要知道该用户是否有权查看秘密英雄。 而认证信息可能会在应用的单个会话中发生变化，比如你改用另一个用户登录。
>
> 假设你不希望直接把 `UserService` 注入到 `HeroService` 中，因为你不希望把这个服务与那些高度敏感的信息牵扯到一起。 这样 `HeroService` 就无法直接访问到用户信息，来决定谁有权访问，谁没有。
>
> 要解决这个问题，我们给 `HeroService` 的构造函数一个逻辑型标志，以控制是否显示秘密英雄。

简单来说这个功能就是在程序运行时注入这个服务的时候判断是否允许注入或者允许某些权限。

例如当切换登录的角色的时候，需要把某些角色特有的配置信息不给予显示，或者在切换到低权限的角色之后，返回新的一个服务，这个服务提供了或者不提供那么多接口（这里建议提供以免没有对应方法报错，可以把原先接口方法直接写返回空值）。



`hero.service.ts`: 

```typescript
constructor(
	private logger: Logger,
    private isAuthorized: boolean){}
	
	getHeroes(){
        let auth = this.isAuthorized ? 'authorized' : 'unauthorized';
        this.logger.log(`Getting heroes for ${auth} user.`);
        return HEROS.filter(hero => this.isAuthorized || !hero.isSecret);
    }
)
```

这里实际上是无效的，在服务`hero.service.ts`中可以注入 Logger 服务，但是不能注入 isAuthorized 标志。不过可以改为工厂提供者来为HeroService 创建一个新的 Logger 实例。

`hero.service.provider.ts`: 

```typescript
let heroServiceFactory = (logger: Logger, userService: UserService) => {
    return new HeroService(logger, userService.user.isAuthorized);
}
```

虽然 HeroService 不能访问 UserService服务，但是工厂函数可以。

`hero.service.provider.ts`: 

```typescript
export let heroServiceProvider = {
    provide: HeroService,
    useFactory: heroServiceFactory,
    deps: [Logger, UserService]
}
```

- useFactory 字段告诉Angular该提供者是一个工厂函数，该函数的实现代码是 heroServiceFactory
- deps 属性是一个 [提供者令牌](https://angular.cn/guide/dependency-injection#token)数组。 Logger 和 UserService 类作为它们自己的类提供者令牌使用。注入器解析这些令牌，并把与之对应的服务注入到相应的工厂函数函数表中。





## 可摇树优化的提供者

> 摇树优化是指一个编译器选项，意思是把应用中未引用过的代码从最终生成的包中移除。 如果提供者是可摇树优化的，Angular 编译器就会从最终的输出内容中移除应用代码中从未用过的服务。 这会显著减小你的打包体积。

> 理想情况下，如果应用没有注入服务，它就不应该包含在最终输出中。 不过，Angular 要能在构建期间识别出该服务是否需要。 由于还可能用 `injector.get(Service)` 的形式直接注入服务，所以 Angular 无法准确识别出代码中可能发生此注入的全部位置，因此为保险起见，只能把服务包含在注入器中。 因此，在 NgModule 或 组件级别提供的服务是无法被摇树优化掉的。

下面是一个不可摇树优化的例子：

`service-and-module.ts`: 

```typescript
import { Injectable, NgModule } from '@angular/core';

@Injectable()
export class Service{
    doSomething():void {}
}

@NgModule({
    providers: [ Service],
})
export calss ServiceModule{}
```

可以把该模块导入到应用模块中，以便该服务可注入到你的应用中。

`app.module.ts`: 

```typescript
@NgModule({
    imports: [
        BrowserModule,
        RouterModule.forRoot([]),
        ServiceModule,
    ]
})
export class AppModule{}
```

当运行 `ngc` 时，它会把 `AppModule` 编译到模块工厂中，工厂包含该模块及其导入的所有模块中声明的所有提供者。在运行时，该工厂会变成负责实例化所有这些服务的注入器。

这里摇树优化不起作用，因为 Angular 无法根据是否用到了其它代码块（服务类），来决定是否能排除这块代码（模块工厂中的服务提供者定义）。要让服务可以被摇树优化，关于如何构建该服务实例的信息（即提供者定义），就应该是服务类本身的一部分。



### 创建可摇树优化的提供者（service）

> **只要在服务本身的 `@Injectable()` 装饰器中指定，而不是在依赖该服务的 NgModule 或组件的元数据中指定，你就可以制作一个可摇树优化的提供者。**



`service.ts`: 

```typescript
@Injectable({
    providedIn: 'root',
})
export class Service{
    constructor(private dep: string){}
}
```

