# [Angular Libraries](https://angular.io/guide/libraries)

> 许多应用都需要解决一些同样的常见问题，例如提供统一的用户界面、呈现数据，以及允许数据输入。开发人员可以为特定的领域创建一些通用解决方案，以便在不同的应用中重复使用。像这样的解决方案就可以构建成 Angular *库*，这些库可以作为 *npm 包*进行发布和共享。    



## 创建库   



```bash
ng generate library my-lib
```



这会在当前项目工作区创建  `project/my-lib` 文件夹，里面包含  `NgModule` 中的一个组件和一个服务。该工作区的配置文件`angular.json`中也添加了一个`library`类型的项目。    



```js
"projects": {
  ...
  "my-lib": {
    "root": "projects/my-lib",
    "sourceRoot": "projects/my-lib/src",
    "projectType": "library",
    "prefix": "lib",
    "architect": {
      "build": {
        "builder": "@angular-devkit/build-ng-packagr:build",
        ...
```



可以使用 CLI 命令构建、测试和lint这个项目：  

```bash
ng build my-lib
ng test my-lib
ng lint my-lib
```

**注意：** 该项目配置的构建器与应用类项目的默认构造器不同。此构造器可以确保永远使用 AOT 编译器构建，而不必再指定 --prod 标志。    



要让库代码可以服用，必须为它定义一个公共的API。这个"用户层"定义了库中消费者的可用内容。该库用户应该可以通过单个的导入路径来访问公共功能。   

库的功能API是在库文件夹下的 `public-api.ts`文件中维护的。当你的库被导入应用时，从改文件导出的所有内容都会被公开。请使用 NgModule 来暴露这些服务和组件。   



## 把应用中的部分内容重构成一个库   

为了让你的解决方案可供服用，需要对它进行调整，以免它依赖应用特有的代码。在将应用的功能迁移到库中时，需要注意以下几点：   

- 组件和管道之类的可声明对象应该设计成无状态的，这意味着他们不依赖或修改外部变量。如果确实依赖于状态，就需要对每种情况进行评估，以决定它是应用的状态还是库要管理的状态。
- 组件内部订阅的所有可观察对象都应该在这些组件的生命周期内进行清理和释放
- 组件对外暴露交互方式时，应该通过输入参数来提供上下文，通过输出参数来将事件传递给其他组件
- 服务声明自己的提供商(而不是在NgModule或组建中声明供应商)，这样他们才是**可摇树优化**的。这样，如果该服务从未被注入或导入到应用中，编译器就会把该服务从发布包中删除。
- 如果你在多个NgModule中注册全局服务提供商或共享提供商，请使用 `RooterModule`提供的`forRoot()`和`forChild()`模式
- 检查所有的内部依赖
  - 对于在组件或服务中使用的自定义或接口，检查它们是否依赖于其他类或接口，它们也需要一起迁移。
  - 同样，如果你的库代码依赖于某个服务，则需要迁移该服务
  - 如果你的库代码或其模板依赖于其他库(比如Angular Material),你就必须把它们配置为改库的依赖   



## 可复用的代码和 schematics   

库通常都包含*可复用的代码*，用于定义组件、服务，以及你刚才导入到项目中的其它 Angular 工件（管道，指令等）。库被打包在一个 npm 包中，用于发布和共享，这个包还可以包含一些 [schematics](https://angular.cn/guide/glossary#schematic)，用于提供直接在项目中生成或转换代码的指令，就像 CLI 用 `ng generate component` 创建一个通用的骨架应用一样。例如，与库配套的 schematics 可以为 Angular CLI 提供生成该库中定义的特定组件所需的信息。

你在库中所包含的内容取决于你要完成的任务类型。例如，如果你想用一个带有预置数据的下拉列表来展示如何把它添加到应用中，你的库中就可以定义一个 schematic 来创建它。对于像下拉列表那样每次都要传入不同值的组件，你可以把它作为共享库中的组件提供出来。

假设你要读取配置文件，然后根据该配置生成表单。如果该表单需要用户进行额外的自定义，它可能最适合用作 schematic。但是，如果这些表单总是一样的，开发人员不需要做太多自定义工作，那么你就可以创建一个动态的组件来获取配置并生成表单。通常，自定义越复杂， schematic 方式就越有用。  





## 与CLI 集成  

库中可以包含那些能与 Angular CLI 集成的 [schematics](https://angular.cn/guide/glossary#schematic) 。

- 包含一个安装型 schematic，以便 `ng add` 可以把你的库添加到项目中。
- 包含一些生成型 schematic ，以便 `ng generate` 可以为项目中的已定义工件（组件，服务，测试等）生成脚手架。
- 包含一个更新（update）原理图 ，以便 `ng update` 可以更新此库的依赖，并针对新版本中的破坏性变更提供辅助迁移。

要了解更多信息，参见 [原理图概览](https://angular.cn/guide/schematics) 和 [供库使用的原理图](https://angular.cn/guide/schematics-for-libraries)。   



## 发布库

使用Angular CLI和npm包管理器来把你的库构建并发布成 npm 包。默认情况下，库是在AOT模式下构建的，因此在构建发布时你不需要指定 --pord 标志。    

```bash
ng build my-lib
cd dist/my-lib
npm publish
```

如果你之前从未在 npm 中发布过包，就必须创建一个用户帐号。[点此阅读发布 npm 包](https://docs.npmjs.com/getting-started/publishing-npm-packages)的更多信息。   



**更多内容移步官网**