# Sass

> 根据阮一峰的 [SASS用法指南](http://www.ruanyifeng.com/blog/2012/06/sass.html)记录

# 安装

需要先安装 `[Ruby](http://www.ruby-lang.org/zh_cn/downloads/)`, 自行查询安装方法   

安装完成 `Ruby` 之后,接着在命令行输入:  
```
gem install sass
```


# 使用

将 `.scss` 文件转换成 `.css` :

```bash
sass test.scss test.css
```

`SASS` 提供`四个编译风格的选项`:   

```json
nested: 嵌套缩进的css代码，他是默认值
expanded: 没有缩进的、扩展的css代码
compact: 简洁格式的css代码
compressed: 压缩后的css代码
```

生产环境中一版使用最后一个选项:  

```
sass --style compressed test.sass test.css      // 这里 .sass .scss 均可
```

也可以让`SASS`监听某个文件或目录，一旦源文件有变动，就自动生成编译后的版本.   

```json
// watch a file 
sass --watch input.scss:output.css

// 示例
sass --watch sass/:css/
```

> 此篇不做语法介绍



- [Compass](./Compass.md) 