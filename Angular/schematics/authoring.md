# 创作原理图    

> 库开发人员通常会把这些原理图与他们的库打包在一起，以便把它们与 Angular CLI 集成在一起。你也可以创建独立的原理图来操作Angular应用中的文件和目录结构，以便为你的开发环境定制它们，并让他们符合你的标准和约束。多个原理图还可以串联起来，通过运行其他原理图来完成复杂的操作。    



在应用程序中操作代码可能即强大又危险。例如，创建一个已存在的文件会出错，如果出现这种情况，就应该放弃已应用的所有其它更改。Angular原理图工具通过创建虚拟文件系统来防止副作用和错误。原理图描述了一个可应用于虚拟文件系统的转换管道。当原理图运行时，转换就会记录在内存中，只有当这些更改被确认有效时，才会应用到实际的文件系统中。   



## 原理图的概念   

原理图的公共 API 定义了其表达其基本概念的类。

- 虚拟文件系统用Tree(树)表示。Tree 数据结构包含在一个**基础状态base**(一组已经存在的文件)和一个**暂存区staging**(需要应用到base的更改列表)。在进行修改的过程中，你并没有真正改变它的base, 而是把那些修改添加到了暂存区 

- Rule(规则) 对象定义了一个函数，它接受 Tree ，进行变换，并返回一个新的Tree。原理图的主文件index.ts定义了一组实现原理图逻辑的规则。

- 变换由Action(动作)表示。有四种动作类型： Create、Rename、Overwrite和Delete

- 每个原理图都在一个上下文中运性，上下文由一个SchematicContext对象表示     

  ​	

### 定义规则和动作   

当使用 [Schematics CLI](https://angular.cn/guide/schematics-authoring#cli) 创建一个新的空白原理图时，它所生成的入口函数就是一个**规则工厂**。 `RuleFactory`对象定义了一个用于创建Rule的高阶函数。    

```js
import { Rule, SchematicContext, Tree } from '@angular-devkit/schematics';


export function helloWorld(_options: any): Rule {
    return (tree: Tree, _context: SchematicContext) => {
        return tree;
    }
}
```

你的这些规则可以通过调用外部工具和实现逻辑来修改你的项目。比如，你需要一个规则来定义如何将原理图中的模板合并到宿主项目中。

规则可以利用 `@schematics/angular` 包提供的实用工具。寻求辅助函数来处理模块、依赖、TypeScript、AST、JSON、Angular CLI 工作空间和项目等等。    

```js
import {
    JsonAstObject,
    JsonObject,
    JsonValue,
    Path,
    normalize,
    parseJsonAst,
    strings
} from '@angular-devkit/core';
```



###  利用模式和接口来定义输入选项   

规则可以从调用者那里收集选项值，并把它们注入到模板中。规则可用的选项及其允许的值和默认值是在原理图的JSON模式文件<schematic>/schema.json中定义的。你可以使用 TypeScript 接口来为这个模式定义变量或枚举的数据类型。    



### Schematics CLI    

原理图有自己的命令行工具。使用  `Node 6.9` 或以上版本，全局安装Schematic命令行工具：    

```node
npm install -g @angular-devkit/schematics-cli
```

这将安装可执行文件`schematics`, 你可以用它在自己的项目文件夹中创建一个新的原理图集合、把一个新的原理图添加到一个现有的集合中，或者扩展一个现有的原理图。    



但是，原理图的最常见用途是将 Angualr 库与 Angular CLI 集成在一起。你可以直接在 Angular 工作空间的库项目中创建原理图文件，而无需使用Schematics CLI。    



### 创建一个原理图的集合   

下面命令用来在同名的项目文件夹中创建一个名为 hello-world的新原理图   

```node
schematics blank --name=hello-world
```

blank 原理图是由 Schematics CLI提供的。该命令用于创建一个新的项目文件夹(该集合的根文件夹)，并在该集合中创建一个最初的命名原理图。    



转到 `collection` 文件夹，安装依赖：   

```bash
cd hello-world
npm install
npm run build
code .
```



### 运行原理图    

使用schematics命令运行一个命名原理图。按以下格式提供项目文件夹的路径、原理图名称和所有必选项。    

```bash
schematics <path-to-schematics-project>:<shcematics-name> --<required-option>=<value>
```

改路径可以是绝对路径，也可以是相对路径。

```bash
schematics .:hello-world
```



### 把原理图添加到集合中    

要把一个原理图添加到现有的集合中，请使用和新建原理图相同的命令，不要改为在该项目的文件夹下运行该命令。    

```bash
cd hellow-world
schematics blank --name=goodbye-world
```

该命令会在你的集合中生成一个新的命名原理图，它包含一个主文件  `index.ts` 及其他相关的测试规约。它还会把这个新原理图的名字(name)、说明(description)和工厂函数(factory function)添加到  `collection.json` 文件中此集合的 JSON 模式中。    



### 集合的内容    

集合的根文件夹中包含一些配置文件、node_modules文件夹和 src/ 文件夹。 src/ 文件夹包含该集合中各个命名原理图的子文件夹，以及一个模式文件(collection.json)，它是集合中各个原理图的模式定义。每个原理图都是用名称，描述和工厂函数创建的。   

```json
{
  "$schema":
     "../node_modules/@angular-devkit/schematics/collection-schema.json",
  "schematics": {
    "hello-world": {
      "description": "A blank schematic.",
      "factory": "./hello-world/index#helloWorld"
    }
  }
}
```

- `$schema` 属性指定了 CLI 进行验证时所用的模式。
- `schematics` 属性列出了属于这个集合的各个命名原理图。每个原理图都有一个纯文本格式的描述，以及指向主文件中自动生成的那个入口函数。
- `factory` 属性指向自动生成的那个入口函数。在这个例子中，你会通过调用 `helloWorld()` 工厂函数来调用 `hello-world` 原理图。
- 可选属性 `schema` 是一个 JSON 模式文件，它定义了本原理图中可用的命令行参数。
- 可选数组属性 `aliases` 指定了一个或多个可用来调用此原理图的字符串。比如，Angular CLI “generate” 命令的原理图有一个别名 “g”，这就可以让你使用命令 `ng g` 。



### 命名原理图    

当你使用 Schematics CLI 创建空白原理图项目时，该集合的第一个成员是一张与该集合同名的空白原理图。当你把这个新的命名原理图添加到本集合中时，它会自动添加到 `collection.json` 模式中。

除了名称和描述外，每个原理图还有一个 `factory` 属性，用于标识此原理图的入口点。在本例中，你通过在主文件 `hello-world/index.ts` 中调用 `helloWorld()` 函数来调用此原理图中定义的功能。

![overview](https://angular.cn/generated/images/guide/schematics/collection-files.gif)



该集合中每个命名原理图都有以下主要部分。

|               |                                  |
| :------------ | :------------------------------- |
| `index.ts`    | 定义命名原理图中转换逻辑的代码。 |
| `schema.json` | 原理图变量定义。                 |
| `schema.d.ts` | 原理图变量。                     |
| `files/`      | 要复制的可选组件/模板文件。      |

原理图可以在 `index.ts` 文件中提供它全部的逻辑，不需要额外的模板。你也可以在 `files/` 文件夹中提供组件和模板来为 Angular 创建动态原理图，比如那些独立的 Angular 项目。这个 index 文件中的逻辑会通过定义一些用来注入数据和修改变量的规则来配置这些模板。