# [Ionic](https://ionicframework.com/)



## 安装

- 需要安装nodejs
- 需要安装npm



```bash
npm install -g ionic cordova
```





## 创建



```bash
ionic start
# 或者直接指定要创建的项目类型
ionic start <project name> tabs
# 还可以指定使用Angular，仅限Ionic4
ionic start <project name> tabs --type=angular
```

可以创建为三种模板: 

- `tabs` : 底部栏
- `sidemenu` : 侧边栏
- `blank` : 空的，只有一个页面



## 启动

首先需要进入项目目录: 



```bash
ionic serve
```





## 打包

 需要使用到`cordova`:  

```bash
ionic cordova platform add borwser		# 加入borwser环境
ionic cordova build browser --prod		# 打包
```



如需打包成 `Android` && `IOS` 自行阅读官方文档。