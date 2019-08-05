# [CLI 命令](https://ionicframework.com/docs/v3/cli)  

可以使用 `ionic --help` 列出所有可用命令  



## 创建项目



```bash
ionic start <projectName> <template> --type=ionic-angular
```



## 启动  

```bash
cd ./myproject
ionic serve
```



## 使用Cordova  

```bash
npm install -g cordova
ionic cordova --help
ionic cordova run ios
```

  

| Command                                                      | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [build](https://ionicframework.com/docs/v3/cli/build/)       | 构建Web资产并为任何平台目标准备应用程序                      |
| [docs](https://ionicframework.com/docs/v3/cli/docs/)         | 打开Ionic文档网站                                            |
| [generate](https://ionicframework.com/docs/v3/cli/generate/) | 生成管道，组件，页面，指令，提供程序和选项卡（ionic-angular> = 3.0.0） |
| [info](https://ionicframework.com/docs/v3/cli/info/)         | 打印系统/环境信息                                            |
| [link](https://ionicframework.com/docs/v3/cli/link/)         | 将您的本地应用程序连接到Ionic                                |
| [login](https://ionicframework.com/docs/v3/cli/login/)       | 使用您的Ionic ID登录                                         |
| [serve](https://ionicframework.com/docs/v3/cli/serve/)       | 为app dev / testing启动本地开发服务器                        |
| [signup](https://ionicframework.com/docs/v3/cli/signup/)     | 创建一个Ionic帐户                                            |
| [start](https://ionicframework.com/docs/v3/cli/start/)       | 创建一个新项目                                               |
| [telemetry](https://ionicframework.com/docs/v3/cli/telemetry/) | （已弃用）选择加入和停用遥测                                 |
| [upload](https://ionicframework.com/docs/v3/cli/upload/)     | （已弃用）上传应用的新快照                                   |
| [config get](https://ionicframework.com/docs/v3/cli/config/get/) | 打印配置值                                                   |
| [config set](https://ionicframework.com/docs/v3/cli/config/set/) | 设置配置值                                                   |
| [cordova build](https://ionicframework.com/docs/v3/cli/cordova/build/) | 构建（准备+编译）给定平台的离子项目                          |
| [cordova compile](https://ionicframework.com/docs/v3/cli/cordova/compile/) | 编译本机平台代码                                             |
| [cordova emulate](https://ionicframework.com/docs/v3/cli/cordova/emulate/) | 在模拟器或模拟器上模拟离子项目                               |
| [cordova platform](https://ionicframework.com/docs/v3/cli/cordova/platform/) | 管理Cordova平台目标                                          |
| [cordova plugin](https://ionicframework.com/docs/v3/cli/cordova/plugin/) | 管理Cordova插件                                              |
| [cordova prepare](https://ionicframework.com/docs/v3/cli/cordova/prepare/) | 将资产复制到Cordova平台，为本机构建做好准备                  |
| [cordova requirements](https://ionicframework.com/docs/v3/cli/cordova/requirements/) | 检查并打印出平台的所有要求                                   |
| [cordova resources](https://ionicframework.com/docs/v3/cli/cordova/resources/) | 自动创建图标和启动屏幕资源                                   |
| [cordova run](https://ionicframework.com/docs/v3/cli/cordova/run/) | 在连接的设备上运行Ionic项目                                  |
| [doctor check](https://ionicframework.com/docs/v3/cli/doctor/check/) | 检查您的Ionic项目的健康状况                                  |
| [doctor ignore](https://ionicframework.com/docs/v3/cli/doctor/ignore/) | 忽略特定问题                                                 |
| [doctor list](https://ionicframework.com/docs/v3/cli/doctor/list/) | 列出所有问题标识符                                           |
| [git remote](https://ionicframework.com/docs/v3/cli/git/remote/) | 将Ionic git remote添加/更新到您当地的Ionic应用程序库         |
| [integrations disable](https://ionicframework.com/docs/v3/cli/integrations/disable/) | 禁用集成                                                     |
| [integrations enable](https://ionicframework.com/docs/v3/cli/integrations/enable/) | 添加各种集成到您的应用程序                                   |
| [monitoring syncmaps](https://ionicframework.com/docs/v3/cli/monitoring/syncmaps/) | 将源映射同步到Ionic Pro错误监控服务                          |
| [package build](https://ionicframework.com/docs/v3/cli/package/build/) | （不建议使用）启动包构建                                     |
| [package download](https://ionicframework.com/docs/v3/cli/package/download/) | （已弃用）下载您的打包应用                                   |
| [package info](https://ionicframework.com/docs/v3/cli/package/info/) | （不建议使用）获取有关构建的信息                             |
| [package list](https://ionicframework.com/docs/v3/cli/package/list/) | （已弃用）列出您的云构建                                     |
| [ssh add](https://ionicframework.com/docs/v3/cli/ssh/add/)   | 向Ionic添加SSH公钥                                           |
| [ssh delete](https://ionicframework.com/docs/v3/cli/ssh/delete/) | 从Ionic删除SSH公钥                                           |
| [ssh generate](https://ionicframework.com/docs/v3/cli/ssh/generate/) | 生成私钥和公钥SSH密钥对                                      |
| [ssh list](https://ionicframework.com/docs/v3/cli/ssh/list/) | 在Ionic上列出您的SSH公钥                                     |
| [ssh setup](https://ionicframework.com/docs/v3/cli/ssh/setup/) | 自动设置您的Ionic SSH密钥                                    |
| [ssh use](https://ionicframework.com/docs/v3/cli/ssh/use/)   | 设置活动的Ionic SSH密钥                                      |