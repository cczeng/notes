# [babel](https://babeljs.io/)

> Babel是一个工具链，主要用于将ECMAScript 2015+代码转换为当前和旧版浏览器或环境中的向后兼容版本的JavaScript。以下是Babel可以为您做的主要事情



## 安装



在项目下:  

```bash
npm install --save-dev @babel/cli @babel/core @babel/preset-env
```



也可以安装到全局。  





## 建立配置文件



在项目根目录下创建配置文件，简单的项目可以创建 `.babelrc` ,复杂项目可创建 `babel.config.js` 也可以直接在 `package.json` 中添加babel配置。



### babel.config.js

```javascript
const presets = [
    [
        "@babel/env",//包含了es2015等
        {
            targets: {
                chrome: 52,
                ie: 9
            },
            useBuiltIns: "usage"
        }
    ]
];
const plugins = [];

module.exports = { presets, plugins };
```



## .babelrc

```json
{
  "presets": [...],
  "plugins": [...]
}
```



### package.json

```json
{
  "name": "my-package",
  "version": "1.0.0",
  "babel": {
    "presets": [ ... ],
    "plugins": [ ... ],
  }
}
```





## [Cli 用法](https://babeljs.io/docs/en/babel-cli#usage)



编译: 

```bash
npx babel script.js
```



指定输出文件: 

```bash
npx babel script.js --out-file script-compiled.js
npx babel script.js -o script-compiled.js  //缩写
```



每次更改文件时编译: 

```bash
npx babel script.js --watch -o script-compiled.js
```



编译文件夹: 

```bash
npx babel src --out-dir newSrc
npx babel src -d newSrc //缩写
```



使用源映射进行编译:  

```bash
npx babel src -d newSrc --source-maps
```



讲编译输出追加到另一个文件: 

```bash
npx babel --out-file script-compiled.js < script.js
```



**更多命令请查看 `babel —help`**

