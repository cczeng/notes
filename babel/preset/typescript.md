# @babel/preset-typescript



babel 7 已经支持对 `typescript` 等进行编译了，如需编译 `typescript` 文件则只需安装标题的包。



## 安装 



```bash
npm install --save-dev @babel/preset-typescript
```



## 配置



在你的babel配置文件中添加一项即可，例如这里是 `babel.config.js`

```javascript
const presets = [
    [
        "@babel/env", 
        {
            targets: {
                chrome: 52,
                ie: 9
            },
            useBuiltIns: "usage",
        }
    ],
    "@babel/typescript", // 只需添加这一行即可
];

module.exports = { presets };
```

