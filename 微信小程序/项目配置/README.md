# 小程序项目配置 app.json

[可参考官方链接](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html)  

以下是一个包含了所有配置选项的 `app.json` :   

```javascript
{
  "pages": [                                    // 设置页面路径
    "pages/index/index",                        // 第一项代表小程序的初始页面, 不需要提供文件后缀
    "pages/logs/index"
  ],
  "window": {                                   // 设置默认页面的窗口表现
    "navigationBarBackgroundColor": "#000",             // 当行蓝背景颜色, 类型为 HexColor
    "navigationBarTextStyle": "white",                  // 导航栏标题颜色,仅支持 black/white, 类型为 String
    "navigationBarTitleText": "小程序开发",               // 导航栏标题文字内容， 类型为 String
    "navigationStyle": "default",                       // 导航栏样式,仅支持 default/custom. custom 模式可自定义导航栏，只保留右上角胶囊装按钮. 类型 String
    "backgroundColor": "#fff",                          // 窗口的背景色， 类型 HexColor
    "backgroundTextStyle": "dark",                      // 下拉 loadding 的样式，仅支持 dark/light, 类型 String
    "backgroundColorTop": "#fff",                       // 窗口的背景色，仅ios支持, 类型 HexColor
    "backgroundColorBottom": "#fff",                    // 底部窗口的背景色，仅 ios 支持. 类型 String
    "enablePullDownRefresh": false,                     // 是否开启下拉刷新, 类型 Boolean
    "onReachBottomDistance": 50,                        // 页面上拉触底事件触发时页面底部距离，单位为px, 类型 Number
  },
  /* 
    配置: tabBar
    设置底部tab的表现, 如果小程序是一个多tab应用(客户端窗口的底部或顶部有tab栏可以切换画面，可以通过tabBar配置项指定tab栏的表现，以及tab切换时显示的对应页面)
  */
  "tabBar": {
    /*
        提示: 
            1. 当设置position 为 top 时, 将不会显示 icon.
            2. tabBar 中的list是一个数组，只能配置最少2个、最多5个tab,tab按数组的顺序排序。
    */
    "color": "#000",                                    // tab 上的文字默认颜色
    "selectedColor": "#fff",                            // tab 上的文字选中时的颜色
    "backgroundColor": "#ccc",                          // tab 的背景色
    "borderStyle": "black",                             // tabbar 上边框的颜色，仅支持 black/white
    "list": [{                                          // tab 的列表,详见list属性说明,最少2个，最多5个tab
    /* 
        list数组选项:
            pagePath                //页面路径
            text                    //tab 上按钮文字
            iconPath                //图片路径，icon大小限制为40kb,建议尺寸为 81px * 81px,当position为top时，此参数无效，不支持网络图片
            selectedIconPath        // 选中时的图片路径，icon大小限制为40kb,建议尺寸为 81px * 81px,当postion为top时，此参数无效
    */
      "pagePath": "pages/index/index",
      "text": "首页",
    }, {
      "pagePath": "pages/logs/logs",
      "text": "日志"
    }],
    "position": "bottom"                                // 可选值 buttom top
  },
  "networkTimeout": {           // 设置网络超时时间
    "request": 10000,               // wx.request 的超市时间，单位毫秒，默认为: 60000
    "connectSocket": 60000,         // wx.connectSocket 的超时时间,单位毫秒,默认为: 60000
    "uploadFilt": 60000,            // wx.uploadFilt 的超时时间，单位毫秒，默认为: 60000
    "downloadFile": 10000           // wx.downloadFile 的超时时间,单位毫秒, 默认为: 60000
  }, 
  "debug": true                 // 设置是否开启debug模式,在开发者工具的控制台面板，调试信息以info的形式给出,其信息有page的注册,页面路由,数据更新,事件触发
}
```   
  
    

<a id="layout"></a>

## 微信小程序模块示意图

![微信小程序模块示意图](./images/layout.jpg)

  


<a id="tabbar"></a>

## 微信小程序模块示意图

![微信小程序模块示意图](./images/tabBar.png)