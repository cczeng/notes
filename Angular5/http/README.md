# HTTP 

> 一些关于http出现的问题  
  
##  跨域  
  
**最近项目开发中碰到了跨域问题,无法调试api。现在已经找到两个方法分别解决不同场景下的问题:**

1. 在开发阶段跨域(--proxy-config)
2. 在生产环境中跨域(Nginx 反向代理)

+ [1. Angular cli 在开发的时候实现跨域](cross-domain/angular_cli_cross-domain.md)

+ [2. Nginx 反向代理解决跨域](cross-domain/nginx_cross-domain.md)