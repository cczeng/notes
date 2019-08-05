# 配置文件  

配置值存储在`JSON`文件中。  

- 全局配置文件(`~/.ionic/config.json`)：用于全局CLI配置和身份验证
- 项目配置文件(`ionic.config.json`)：用于离子项目配置

CLI提供了用于从项目配置文件和全局CLI配置文件设置和打印配置值的命令。查看`ionic config set --help`和`ionic config get --help`使用。   





## 集成  

检测到Cordova等集成时会自动激活，但可以轻松禁用。   



集成挂钩到CLI事件。例如，启用Cordova集成 `ionic cordova prepare` 后，将在运行后 `ionic build` 运行。   