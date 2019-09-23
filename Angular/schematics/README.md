# schematics 原理图    

> 原理图是一个基于模板的支持复杂逻辑的代码生成器。它是一组通过生成代码或修改代码来转换软件项目的指令。    
>
> 原理图的集合可以作为一个强大的工具，以创建、修改和维护任何软件项目，特别是当要自定义Angular项目以满足你自己组织的特定需求时。例如，你可以借助原理图来用预定义的模板或布局生成常用的UI模式或特定的组件。你也可以使用原理图来强制执行架构规则和约定，让你的项目保持一致性和互操作性。    



## Angular CLI 中的原理图    

> Angular CLI 使用原理图对 Web 应用项目进行转换。你可以修改这些原理图，并定义新的原理图，比如更新代码以修复依赖中的重大变更，或者把新的配置项或框架添加到现有的项目中。   



`@schematics/angular` 集合中的原理图是`ng generate` 和 `ng add` 命令的默认原理图。   

```bash
ng generate my-schematic-collection:my-schematic-name
```

或者   

```bash
ng generate my-schematic-name --collection collection-name
```



编写库的原理图:   

- **添加(Add)原理图**允许开发人员使用`ng add`在Angular工作空间中安装你的库 
- **生成(Generation)原理图**可以告诉`ng generate`子命令如何修改项目、添加配置和脚本，以及为库中定义的工件提供脚手架  
- **更新(Update)原理图**可以告诉`ng update`命令，如何更新库的依赖，并在发布新版本时调整其中的破坏性变更   

要了解细节请查看：   

- [创作原理图](./authoring.md)
- [库的原理图](./libraries.md)      



### 添加(Add)原理图   

> 库中通常会提供一个添加原理图，以便通过`ng add`把这个库添加到现有项目中。add 命令会运行包管理器来下载新的依赖，并调用一个原理图形式的安装脚本    



合作伙伴和第三方库也可以通过添加原理图来支持 Angular CLI。例如，@ng-bootstrap/schematics 会把 ng-bootstrap 添加到应用中，@clr/angular 会安装并设置 VMWare 的 Clarity.   



“添加原理图”还可以通过更改配置、添加额外依赖（比如腻子脚本），或者添加程序包特有的初始化代码来修改项目。例如@angular/pwa原理图会通过添加一个应用清单（manifest）和Service Worker，来把你的应用编程一个PWA，@angular/elements 原理图添加了 document-register-element.js腻子脚本和Angular Elements的依赖项。    





### 生成(Generation)原理图    

> 生成器原理图是  `ng generate` 的操作指令。那些已经有文档的子命令会使用默认的Angular生成器原理图，但你可以在子命令中指定另一个原理图来生成你的库中定义的那些工件   



例如Angular Material为它定义的一些UI组件提供了生成器原理图。下面的命令会使用其中一个原理图来渲染一个Angular Material的  `<mat-table>`组件，它预先配置了一个用于排序和分页的数据源。   

```node
ng genrate @angular/material:table
```



### 更新(Update)原理图    

> ng update 命令可以用来更新工作空间的库依赖。如果你没有提供任何选项或使用了help选项，该命令会检查你的工作空间并建议要更新哪些库    



```node 
ng update
    We analyzed your package.json, there are some packages to update:

      Name                               Version                  Command to update
     --------------------------------------------------------------------------------
      @angular/cdk                       7.2.2 -> 7.3.1           ng update @angular/cdk
      @angular/cli                       7.2.3 -> 7.3.0           ng update @angular/cli
      @angular/core                      7.2.2 -> 7.2.3           ng update @angular/core
      @angular/material                  7.2.2 -> 7.3.1           ng update @angular/material
      rxjs                               6.3.3 -> 6.4.0           ng update rxjs


    There might be additional packages that are outdated.
    Run "ng update --all" to try to update all at the same time.
```



例如，你需要更新  `Angular Material`库   

```node
ng update @angular/material
```



该命令会在你的工作空间的`package.json`中更新`@angular/material`及其依赖项`@angular/cdk`。如果任何一个包含了涵盖从现有版本到新版本的迁移规则的更新原理图，那么该命令就会在你的工作区中运行这个原理图。   





