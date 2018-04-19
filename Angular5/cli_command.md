# Angular Cli 脚手架 常用命令  

## 项目:   

```javascript
ng new project-name - 创建一个新项目，置为默认设置
ng build - 构建/编译应用
ng test - 运行单元测试
ng e2e - 运行端到端（end-to-end）测试
ng serve - 启动一个小型web服务器，用于托管应用
ng deploy - 即开即用，部署到Github Pages或者Firebase
```

## 创建:  

```javascript
组件| ng g component home/component/my-new-component     
//相对生成组件生成位置在项目的根目录的  src/app/home/component（指令其他等等都可以用该方式生成）

指令| ng g directive my-new-directive 
    
管道| ng g pipe my-new-pipe 
    
服务| ng g service my-new-service 
    
类  | ng g class my-new-class 
    
接口| ng g interface my-new-interface 
    
枚举| ng g enum my-new-enum 
    
模块| ng g module my-module

```