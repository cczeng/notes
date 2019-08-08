# Ionic3 使用自定义字体     

> Ionic CLI 不会自动将字体打包到 assets 下   

鉴于这个问题，所以需要手动在`assets`下面建一个`fonts`文件夹 ：   

`src/assets/fonts/` 这里如果需要多种字体,则可以继续往下新建文件夹   

```css
@font-face {
    font-family: 'NotoSansCJK';
    src: url('../assets/fonts/NotoSansCJK/NotoSansCJK-Bold.otf');
    font-weight: 400;
    font-style: normal;
}

page-list-master {
    p {
        font-family: 'NotoSansCJK'
    }
}
```

  

项目目录结构：  

![1565235655216](C:\Users\youhaosoft\Desktop\github\notes\Ionic\ionic3\images\assets_fonts.png)

  

www目录结构：  

![1565235802328](C:\Users\youhaosoft\Desktop\github\notes\Ionic\ionic3\images\www_fonts.png)