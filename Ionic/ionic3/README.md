# [Ionic3](https://ionicframework.com/docs/v3)



## 安装

所需环境：   

- nodejs
- npm
- git

安装： 

```bash
npm install -g ionic cordova
```



## 创建项目



```bash
ionic start <projectName> <template> --type=ionic-angular
```

其中：

`<projectName` - 代表你的项目名字  

`<template>` -  代表需要创建的模板  



ionic 提供以下官方模板：  

- `tabs`: 简单的3选项卡布局
- `sidemenu`: 侧面有可能转换菜单的布局
- `blank`：一个单页的裸机
- `super`：具有超过14个即用型页面设计的入门项目
- `tutorial`：一个引导入门项目
- `conference`：一个演示真实世界应用程序的项目
- `aws`： AWS Mobile Hub starter
- `maps`： 一个包含谷歌地图的项目



## 部署

如果已经有一个有效的安卓环境，那么已经准备好了   



**要求**

- [Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
- [Android Studio](https://developer.android.com/studio/index.html)

- 更新了Android SDK工具，平台和组件依赖项。可通过Android Studio的[SDK Manager获得](https://developer.android.com/studio/intro/update.html)  



**运行你的应用程序**  

要运行您的应用程序，您只需在Android设备上启用USB调试和开发模式，然后从命令行运行 `ionic cordova run android --device`。  



## 生产部署  

要运行或构建您的应用程序以进行生产，请运行   

```bash
ionic cordova run android --prod --release
#or
ionic cordova build android --prod --release
```

这将使您的应用程序代码缩小为ionic的源代码，并从APK中删除任何调试功能。 



## IOS设备  

这里不做IOS设备环境部署，请移步[官方文档](https://ionicframework.com/docs/v3/intro/deploying/)

  



## 浏览器支持  



Ionic专注于通过Cordova构建本机/混合应用程序，以及为[Progressive Web Apps](https://ionicframework.com/docs/resources/progressive-web-apps/)和 [Electron](http://electron.atom.io/)添加功能   

支持以下操作系统和浏览器：    



移动操作系统：  

- IOS 8+ 
- Windows 10 通用应用程序
- Android 4.4+  



浏览器： 

- safari
- chrome
- edge