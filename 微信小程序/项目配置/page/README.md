# page 页面

page.json:  

每一个小程序页面也可以使用 `.json` 文件来对本页面的窗口表现进行配置。 页面的配置比 `app.json` 全局配置简单得多，只是设置 `app.json` 中的 `window` 配置项的内容，页面中配置项会覆盖 `app.json` 的 `window` 中相同的配置项。

页面的 `.json` 只能设置 `window` 相关的配置项，以决定本页面的窗口表现，所以无需写 `window` 这个键，如:   
  


```javascript
"navigationBarBackgroundColor": "#000",             // 当行蓝背景颜色, 类型为 HexColor
"navigationBarTextStyle": "white",                  // 导航栏标题颜色,仅支持 black/white, 类型为 String
"navigationBarTitleText": "小程序开发",               // 导航栏标题文字内容， 类型为 String
"backgroundColor": "#fff",                          // 窗口的背景色， 类型 HexColor
"backgroundTextStyle": "dark",                      // 下拉 loadding 的样式，仅支持 dark/light, 类型 String
"enablePullDownRefresh": false,                     // 是否开启下拉刷新, 类型 Boolean
"disableScroll": false,                             // 设置为ture则页面整体不能上下滚动; 只在page.json中有效,无法在app.json中设置该项, 类型 Boolean
"onReachBottomDistance": 50,                        // 页面上拉触底事件触发时页面底部距离，单位为px, 类型 Number
```   

  

