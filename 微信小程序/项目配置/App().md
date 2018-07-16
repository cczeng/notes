# App()  


+ [生命周期](#life)
+ [onLaunch,onShow](#on)
+ [场景值](#scene)
+ [onPageNotFound](#notfound)
+ [getApp()](#getapp)
+ [注意](#warring)


<a id="life"></a>
## 生命周期


`App()` 函数用来注册一个小程序。接受一个object参数，其指定小程序的生命周期函数等。   
  

| 属性              | 类型             | 描述                        | 触发时机                                  |
| ---------------- | ---------------- | -------------------------- | ---------------------------------------  |
| `onLaunch`       | Function   | 生命周期函数--监听小程序初始化 | 当小程序初始化完成时，会触发 onLaunch(全局只触发一次)  |
| `onShow`         | Function   | 生命周期函数--监听小程序显示 | 当小程序启动，或从后台进入前台显示,会触发 onShow        |
| `onHide`         | Function   | 生命周期函数--监听小程序隐藏 | 当小程序从前台进入后台,会触发 onHide                  |
| `onError`        | Function   | 错误监听函数 | 当小程序发生脚本错误，或者 api 调用失败时,会触发onError并带上错误信息   |
| `onPageNotFound` | Function   | 页面不存在监听函数 | 当小程序出现要打开的页面不存在的情况，会带上页面信息回调该函数      |
| 其他              | Any       |                  | 开发者可以添加任意的函数或数据到Object参数中,用 `this` 可以访问   |
  

`前台、后台定义` : 当用户点击左上角关闭，或者按了设备 Home 键离开微信，小程序并没有直接销毁，而是进入了后台;当再次进入微信或再次打开小程序，又会从后台进入前台。需要注意的是： 只有当小程序进入后台一定时间，或者系统资源占用过高，才会被真正的销毁。   
  
关闭小程序(基础库版本 1.1.0 开始支持): 当用户从扫一扫、转发等入口(场景值为: 1007,1008,1011,1025)进入小程序,且没有置顶小小程序的情况下退出，小程序会被销毁。   

**小程序运行机制在基础库版本 1.4.0 有所改变**: 上一条关闭逻辑在新版本已不适用。 [详情](https://developers.weixin.qq.com/miniprogram/dev/framework/operating-mechanism.html)   
    

实例代码: 

```javascript
App({
  onLaunch: function(options) {
    // Do something initial when launch.
  },
  onShow: function(options) {
      // Do something when show.
  },
  onHide: function() {
      // Do something when hide.
  },
  onError: function(msg) {
    console.log(msg)
  },
  globalData: 'I am global data'
})
```   
<a id="on"></a>
## onLaunch, onShow 参数

| 字段                      | 类型      | 说明                                                      |
| ----------------          | ---------| ---------------------------------------                  |
| `path`                    | String   | 打开小程序的路径                                            |
| `query`                   | Object   | 打开小程序的query                                          |
| `scene`                   | Number   | 打开小程序的场景值                                          |
| `shareTicket`             | String   | shareTicket，详见 获取更多转发信息                           |
| `referrerInfo`            | Object   | 当场景为由从另一个小程序或公众号或App打开时，返回此字段           |
| `referrerInfo.appId`      | String   | 来源小程序或公众号或App的 appId，详见下方说明                  |
| `referrerInfo.extraData`  | Object   | 来源小程序传过来的数据，scene=1037或1038时支持                 |  

<a id="scene"></a>  

## [场景值](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/scene.html)   


| 场景值 |	场景                          |	appId 信息含义              |
| ----- | ----------------------------- | -------------------------  |
| 1020  |	公众号 profile 页相关小程序列表 |	返回来源公众号 appId       |
| 1035  |	公众号自定义菜单               |	返回来源公众号 appId      |
| 1036  |	App 分享消息卡片              |	   返回来源应用 appId        |
| 1037  |	小程序打开小程序               |	返回来源小程序 appId      |
| 1038  |	从另一个小程序返回             |	返回来源小程序 appId      |
| 1043  |	公众号模板消息                 |	返回来源公众号 appId      |  
  

<a id="notfound"></a>  

## onPageNotFound  
当要打开的页面并不存在时，会回调这个监听器，并带上一下信息:    


| 字段          |	类型     |	说明                                                                          |
| -----        | -------- | -------------------------                                                     |
| path         |	String  |	不存在页面的路径                                                                 |
| query        |	Object  |	打开不存在页面的 query                                                           |
| isEntryPage  |	Boolean |	是否本次启动的首个页面（例如从分享等入口进来，首个页面是开发者配置的分享页面）             |
  

开发者可以在 onPageNotFound 回调中进行重定向处理，但必须在回调中同步处理，异步处理（例如 setTimeout 异步执行）无效。   

实例代码:   


```javascript
App({
  onPageNotFound(res) {
    wx.redirectTo({
      url: 'pages/...'
    }) // 如果是 tabbar 页面，请使用 wx.switchTab
  }
})
```   

  
**注意:**

  1. 如果开发者没有添加 `onPageNotFound` 监听，当跳转页面不存在时，将推入微信客户端原生的页面不存在提示页面
  1. 如果 `onPageNotFound` 回调中又重定向到另一个不存在的页面，将推入微信客户端原生的页面不存在提示页面，并且不在回调 `onPageNotFound`
  

<a id="getapp"></a>  

## getApp()
全局的 getApp() 函数可以用来获取到小程序实例。

```javascript
// other.js
var appInstance = getApp()
console.log(appInstance.globalData) // I am global data
```

<a id="warring"></a>  

**注意:**   

1. `App()` 必须在 `app.js` 中注册，且`不能注册多个`。
1. `不要在`定义于 `App()` 内的函数中调用 `getApp()` ，使用 `this` 就可以拿到 `app` 实例。
1. 不要在 `onLaunch` 的时候调用 `getCurrentPages()`，此时 `page` 还`没有生成`。
1. 通过 `getApp()` 获取实例之后，`不要私自调用生命周期函数`。