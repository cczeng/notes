# 库的原理图  

> 当创建Angular库时，你可以同事为它打包进一组原理图，并把它与Angular CLI集成在一起。借助原理图，用户可以用  `ng add`来安装你这个库的初始版本，可以用`ng generate`来创建你在库中定义的一些工件    



## 创建一个原理图集合   

要开始一个集合，你需要创建一些原理图文件。    

1. 在库的根文件夹中，创建一个 `schematics/` 文件夹
2. 在`schematics/`文件夹中,为你的第一个原理图创建一个 `ng-add/` 文件夹
3. 在`schematics/`文件夹的根级，创建一个`collection.json`文件
4. 编辑 `collection.json`文件来定义你的集合的初始模式定义



```json
{
    "$schema": "../../../node_modules/@angular-devkit/schematics/collection-schema.json",
    "schematics": {
        "ng-add": {
            "description": "Add my library to the project",
            "factory": "./ng-add/index#ngAdd"
        }
    }
}
```

- `$schema`路径是相对于`Angular Devkit`集合模式定义的
- `schematics`对象描述了该集合中的命名原理图
- 第一个条目是名为`ng-add`的原理图。它包含了描述，并指向执行此原理图时要调用的工厂函数



**在库项目`package.json`文件夹中，添加一个`schematics`的条目，里面带有你的模式定义文件的路径。当Angular CLI 运行命令时，会根据这个条目在你的集合中查找指定名字的原理图**：    

```json
{
    "name": "my-lib",
    "version":"0.0.1",
    "schematics": "./schematics/collection.json"
}
```

你所创建的初始模式告诉 CLI 在哪里可以找到支持 ng add 命令的原理图。    



## 提供安装支持    

`ng add` 命令的原理图可以增强用户的初始安装过程。可以按如下步骤定义这种原理图：   

1. 进入`/schematics/ng-add/`目录
2. 创建主文件 `index.ts`
3. 打开`index.ts`并添加原理图工厂函数的源代码



```typescript
import { Rule, SchematicContext, Tree } from '@angular-devkit/schematics';
import { NodePackageInstallTask } from '@angular-devkit/schematics/tasks';


// just return the tree
export function ngAdd(_options: any): Rule {
    return (tree: Tree, _context: SchematicContext) => {
        _context.addTask(new NodePackageInstallTask());
        return tree;
    }
}
```

提供初始 `ng add` 支持所需的唯一步骤是使用 `SchematicContext` 来触发安装任务。该任务会借助用户首选的包管理器将该库添加到宿主项目的 `package.json` 配置文件中，并将其安装到该项目的 `node_module` 目录下。    

在这个例子中，该函数会接收当前的 Tree 并返回它而不做修改。   



## 构建你的原理图    

要把你的原理图和库打包到一起，就必须把这个库配置成单独构建原理图，然后再把它们添加到发布包中。   

你必须**先构建库**在构建原理图，这样才能把它们放到正确的目录下。   

- 你的库需要一个自定义的Typescript配置文件，里面带有如何把原理图编译进库的发布版的一些指令
- 要把这些原理图添加到库的发布包中，就要把这些脚本添加到该库的`package.json`文件中



假设你在 Angular 工作区中有一个库项目 my-lib。想要告诉库如何构建原理图，就要在生成的 `tsconfig.lib.json` 库配置文件旁添加一个 `tsconfig.schematics.json` 文件

编辑`tsconfig.schematics.json`：   

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "lib": [
      "es2018",
      "dom"
    ],
    "declaration": true,
    "module": "commonjs",
    "moduleResolution": "node",
    "noEmitOnError": true,
    "noFallthroughCasesInSwitch": true,
    "noImplicitAny": true,
    "noImplicitThis": true,
    "noUnusedParameters": true,
    "noUnusedLocals": true,
    "rootDir": "schematics",
    "outDir": "../../dist/my-lib/schematics",
    "skipDefaultLibCheck": true,
    "skipLibCheck": true,
    "sourceMap": true,
    "strictNullChecks": true,
    "target": "es6",
    "types": [
      "jasmine",
      "node"
    ]
  },
  "include": [
    "schematics/**/*"
  ],
  "exclude": [
    "schematics/*/files/**/*"
  ]
}
```

- `rootDir` 指出在你的 `schematics/` 文件夹中包含要编译的输入文件 
- `outDir` 映射到了库中的输出目录下。默认情况下，这是工作空间根目录下的 dist/my-lib 文件夹

要确保你的原理图源文件会被编译进库包中，请把下列脚本添加到库项目的根文件夹(project/my-lib)下的 `package.json` 文件中。   

```json
{
  "name": "my-lib",
  "version": "0.0.1",
  "scripts": {
    "build": "../../node_modules/.bin/tsc -p tsconfig.schematics.json",
    "copy:schemas": "cp --parents schematics/*/schema.json ../../dist/my-lib/",
    "copy:files": "cp --parents -p schematics/*/files/** ../../dist/my-lib/",
    "copy:collection": "cp schematics/collection.json ../../dist/my-lib/schematics/collection.json",
    "postbuild": "npm run copy:schemas && npm run copy:files && npm run copy:collection"
  },
  "peerDependencies": {
    "@angular/common": "^7.2.0",
    "@angular/core": "^7.2.0"
  },
  "schematics": "./schematics/collection.json"
}
```

- build 脚本使用自定义的 `tsconfig.schematics.json ` 文件来编译你的原理图
- copy:* 语句将已编译的原理图文件复制到库的输出目录下的正确位置，以保持目录的结构
- postbuild 脚本会在 build 脚本完成后复制原理图文件



## 提供生成器支持   

假设要给库定义一项某些设置的服务 my-service，你希望用户能够用下面的CLI 命令来生成它。   

```node
ng generate my-lib:my-service
```

首先，在`schematics/`文件夹中新建一个子文件夹 `my-service` 



**配置新的原理图**    

当需要把一个原理图添加到集合中时，就必须在该集合的模式中指向它，并提供一些配置文件来定义用户以传给该命令的选项。    

1. 编辑一下 `schematics/collection.json` 文件，指向新的原理图子文件夹，并附上一个指向模式文件的指针，该文件将会指定新原理图的输入。    

```json
{
  "$schema": "../../../node_modules/@angular-devkit/schematics/collection-schema.json",
  "schematics": {
    "ng-add": {
      "description": "Add my library to the project.",
      "factory": "./ng-add/index#ngAdd"
    },
    "my-service": {
      "description": "Generate a service in the project.",
      "factory": "./my-service/index#myService",
      "schema": "./my-service/schema.json"
    }
  }
}
```

2. 进入<lib-root>/schematics/my-service/ 目录。
3. 创建一个 schema.json 文件并定义该原理图的可用选项。

```json
{
  "$schema": "http://json-schema.org/schema",
  "id": "SchematicsMyService",
  "title": "My Service Schema",
  "type": "object",
  "properties": {
    "name": {
      "description": "The name of the service.",
      "type": "string"
    },
    "path": {
      "type": "string",
      "format": "path",
      "description": "The path to create the service.",
      "visible": false
    },
    "project": {
      "type": "string",
      "description": "The name of the project.",
      "$default": {
        "$source": "projectName"
      }
    }
   },
  "required": [
    "name"
  ]
}
```

- **id**: 这个模式定义在集合中唯一的id.
- **title**: 一个人类可读的模式描述
- **type**: 由这些属性提供的类型描述符
- **properties**: 一个定义该原理图可用选项的对象

每个选项都会把key与类型、描述和一个可选的别名关联起来。该类型定义了你所期望的值的形态，并在用户请求你的原理图给出用法帮助时显示这份描述。    



关于原理图的更多自定义选项，请参阅工作区的模式定义。   

1. 创建一个 schema.json 文件，并定义一个接口，用于存放 schema.json 文件中定义的各个选项的值。   

```typescript
export interface Schema {
    name: string;
    
    path?: string;
    
    project?: string;
}
```

- **name**: 你要为创建的这个服务指定的名称
- **path**: 覆盖为原理图提供的路径，默认情况下，路径是基于当前工作目录的。
- **project**: 提供一个具体项目来运行原理图，在原理图中，如果用户没有给出该选项，你可以提供一个默认值



### **添加模板文件**    

要把工件添加到项目中，你的原理图就需要自己的模板文件。原理图模板支持特殊的语法来执行代码和变量替换。    

1. 在 `schematics/my-service/` 目录下创建一个 `files/` 文件夹
2. 创建一个名叫 `__name@dasherize__.service.ts.template`的文件，它定义了一个可以用来生成文件的模板。这里的模板会生成一个已把 Angular 的 HttpClient 注入到其构造函数中的服务。   

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Injectable({
    provideIn: 'root'
})
export class <%= classify(name) %>Service {
    constructor(private http: HttpClient){}
}
```

- classify 和 dasherize 方法是实用函数，你的原理图会用它们来转换你的模板源码和文件名。
- name 是工厂函数提供的一个属性。它与你在模式中定义的name是一样的。



**添加工厂函数**

Schematics 框架提供了一个文件模板系统，它支持路径和内容模板。操作系统会操作在这个输入文件树(Tree)中加载的文件内或路径中定义的占位符，用传给 Rule 的值来填充它们。   

1. 创建主文件 `index.ts `并为你的原理图工厂函数添加源代码
2. 导入你需要的原理图定义。Schematics 框架提供了许多实用函数来创建规则或在执行原理图时使用规则    

```typescript
import { 
	Rule, Tree, SchematicsException,
    appley, url, applyTemplates, move,
    chain, mergeWith
} from '@angular-devkit/schematics';

import { strings, normalize, experimental } from '@angular-devkit/core';

import { Schema as MyServiceSchema } from './schema';	// 导入已定义的模式接口，它会为你的原理图选项提供类型信息

// 要构建“生成器原理图”，我们从一个空白的规则工厂开始
export function myService(options: MyServiceSchema): Rule {
    return (tree: Tree) => {
       	return tree;
    }
}
```

这个简单的规则工厂返回树而不做任何修改。这些选项都是从 `ng generate` 命令传过来的选项值。   



### 定义一个生成器规则



**获取项目配置**   

要确定目标项目，可以使用 Tree.read() 方法在工作空间的根目录下读取工作空间配置文件 `angular.json` 的内容。将以下代码添加到工厂函数中    

```typescript
import {
  Rule, Tree, SchematicsException,
  apply, url, applyTemplates, move,
  chain, mergeWith
} from '@angular-devkit/schematics';

import { strings, normalize, experimental } from '@angular-devkit/core';

import { Schema as MyServiceSchema } from './schema';

export function myService(options: MyServiceSchema): Rule {
  return (tree: Tree) => {
    const workspaceConfig = tree.read('/angular.json');
    if (!workspaceConfig) {
      throw new SchematicsException('Could not find Angular workspace configuration');
    }

    // convert workspace to string
    const workspaceContent = workspaceConfig.toString();

    // parse workspace string into JSON object
    const workspace: experimental.workspace.WorkspaceSchema = JSON.parse(workspaceContent);
  };
}
```

- 一定要检查此上下文是否存在，并抛出相应的错误
- 把这些内容读入成字符串后，把配置解析成一个JSON对象，把它的类型设置为 WorkSpaceSchema

`WorkspaceSchema` 包含工作空间配置的所有属性，包括一个 `defaultProject`值，用来确定如果没有提供该参数，要使用哪个项目。如果 `ng generate` 命令中没有明确指定任何项目，我们就会把它作为后备值。   

```typescript
if(!options.project){
    options.project = workspace.defaultProject;
}
```

有了项目名称，用它来检索指定项目的配置信息。   

```typescript
const projectName = options.project as string;

const project = workspace.projects[projectName];

const projectType = project.projectType === 'application' ? 'app' : 'lib';
```

workspace projects 对象包含指定项目的全部配置信息。   

1. `options.path`决定了应用原理图之后，要把原理图模板文件移动到的位置   

原理图模式中的 path 选项默认会替换为当前工作目录。如果未定义 path ，就是用项目配置中的 `sourceRoot` 和 `projectType` 来确定。   

```typescript
if(options.path === undefined){
    options.path = `${project.sourceRoot}/${projectType}`;
}
```



### 定义规则    

Rule 可以使用外部模板文件，对它们进行转换，并使用转换后的模板返回另一个 Rule 对象。你可以使用模板来生成原理图所需的任意自定义文件。    

1. 添加以下代码到工厂函数

```typescript
const templateSource = apply(url('./files'), [
    applyTemplates({
        classify: strings.classfy,
        dasherize: strings.dasherize,
        name: options.name
    }),
    move(normalize(options.path as string))
])
```

- apply() 方法会把多个规则应用到源码中，并返回转换后的源代码。它需要两个参数，一个源代码和一个规则数组
- url() 方法会从文件系统中相对于原理图的路径下读取源文件
- applyTemplates() 方法接受一个参数，它的方法和属性可用在原理图和模板的原理图文件名上。它返回一条 Rule 。可以在这里定义 classify() 和 dasherize() 方法，以及name属性
- classify() 方法接受一个值，并返回标题格式(title case)的值。比如，如果提供的名字是MyService，它就会返回“my-service”的形式。
- 当应用了此原理图之后，move方法会把所提供的源文件移动到目的地

最后，规则工厂必须返回一条规则。   

```typescript
return chain([
    mergeWith(templateSource)
]);
```

`chain()`方法允许你把多个规则组合到一个规则中，这样就可以在一个原理图中执行多个操作。这里只是把模板规则和原理图要执行的代码合并在一起。   



一个原理图规则函数的完整例子：   

```typescript
import {
  Rule, Tree, SchematicsException,
  apply, url, applyTemplates, move,
  chain, mergeWith
} from '@angular-devkit/schematics';

import { strings, normalize, experimental } from '@angular-devkit/core';

import { Schema as MyServiceSchema } from './schema';

export function myService(options: MyServiceSchema): Rule {
  return (tree: Tree) => {
    const workspaceConfig = tree.read('/angular.json');
    if (!workspaceConfig) {
      throw new SchematicsException('Could not find Angular workspace configuration');
    }

    // convert workspace to string
    const workspaceContent = workspaceConfig.toString();

    // parse workspace string into JSON object
    const workspace: experimental.workspace.WorkspaceSchema = JSON.parse(workspaceContent);
    if (!options.project) {
      options.project = workspace.defaultProject;
    }

    const projectName = options.project as string;

    const project = workspace.projects[projectName];

    const projectType = project.projectType === 'application' ? 'app' : 'lib';

    if (options.path === undefined) {
      options.path = `${project.sourceRoot}/${projectType}`;
    }

    const templateSource = apply(url('./files'), [
      applyTemplates({
        classify: strings.classify,
        dasherize: strings.dasherize,
        name: options.name
      }),
      move(normalize(options.path as string))
    ]);

    return chain([
      mergeWith(templateSource)
    ]);
  };
}
```



###  运行你的库原理图  

在构建库和原理图之后，你就可以安装一个原理图集合来运行你的项目了。    

#### 构建你的库和原理图  

在工作区的根目录下，运行库的 `ng build` 命令   

```node
ng build my-lib
```

然后，进入库目录，构建原理图

```node
cd projects/my-lib
npm run build
```

**链接这个库**    

这些库和原理图都已打包好了，就放在你工作区根目录下的 `dist/my-lib` 文件夹中。要运行这个原理图，你需要把这个库链接到 `node_modules` 文件夹中。在工作空间的根目录下，运行 `npm link` 命令，并把你的可分发库的路径作为参数。      

```node
npm link dist/my-lib 
```

**运行原理图**    

现在你的库已经安装完毕，可以使用 `ng generate` 命令来运行原理图了。    

```node
ng generate my-lib:my-service --name my-data
```

在控制台中，你会看到原理图已经运行过了，my-data.service.ts 文件被创建在了你的app文件夹中。   

```node
CREATE src/app/my-data.services.ts
```

