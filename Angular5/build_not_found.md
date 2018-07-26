# build 编译之后放在网站二级目录下会找不到资源   


## 一、 需要注意 img url的引入


使用相对路径,因为在最后 `build` 之后cli会自动导出到正确目录.   

```css
background-image: url(../assets/images/header_bg.png);
```   
  
## 二、 ng build --prod --base-href ./   

> ng build --prod --base-href ./  
  
使用 `ng build --prod --base-href ./` 这条 `cli` 指令来进行编译。也可以把他写在 `package.json` 里面，然后使用 `npm run build` 执行。   

   
```javascript
 "scripts": {
    "ng": "ng",
    "start": "ng serve",
    "build": "ng build --prod --base-href ./",              // 修改这条
    "test": "ng test",
    "lint": "ng lint",
    "e2e": "ng e2e"
  },
```

```cmd
npm run build
```

打包出来的就可以了