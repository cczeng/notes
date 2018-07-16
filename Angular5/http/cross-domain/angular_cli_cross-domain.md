# Angular cli 发开过程中实现跨域  

> 最近在做项目的时候,碰见了跨域问题  

 
**1.只需在项目根目录下新建一个文件: `proxy.config.json`**

```javascript
{
    "/norm-webapp": {           // 这里为要匹配的url中api请求的开头
        "target": "http://192.168.1.118:80",    // 需要转发的目的地址
        "secure": false,
        "logLevel": "debug",
        "changeOrigin": true               // 如果后端启用了一些代理的时候必须设置为true
    }
}
```  


**2.修改`package.json`**
```javascript
{
  "name": "h5",
  "version": "0.0.0",
  "scripts": {
    "ng": "ng",
    "start": "ng serve --proxy-config proxy.conf.json",     // 此处修改,加入--proxy-config
    "build": "ng build",
    "test": "ng test",
    "lint": "ng lint",
    "e2e": "ng e2e"
  },
  ...
}
```


**3.启动**

```npm
npm start
```  

这里不再是使用`ng serve` 而是使用 `npm start` 来启动项目,即可看到运行时候出现以下:   

```node
> h5@0.0.0 start C:\Users\15017\Desktop\project\hengyin_h5
> ng serve --proxy-config proxy.conf.json
```