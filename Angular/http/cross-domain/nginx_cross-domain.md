# Nginx 实现反向代理解决跨域问题  

> Nginx（发音同engine x）是一个异步框架的 Web服务器，也可以用作反向代理，负载平衡器 和 HTTP缓存。     —— wiki  

上面是关于`维基百科`对nginx的定义  

在 `nginx.conf`中只需添加一个`location`即可:


```javascript
// 实现负载均衡,也可用于转发,可以省略此处,直接在location中直接指定url
upstream tomcat_client {
        server 192.168.1.118:8080 weight=1;
        # server t02:8080 weight=1;
} 

server {
    listen 80;      // 监听80端口
    sendfile on;
    default_type application/octet-stream;
    gzip on;
    gzip_http_version 1.1;
    gzip_disable      "MSIE [1-6]\.";
    gzip_min_length   1100;
    gzip_vary         on;
    gzip_proxied      expired no-cache no-store private auth;
    gzip_types        text/plain text/css application/json application/javascript application/x-javascript text/xml application/xml application/xml+rss text/javascript;
    gzip_comp_level   9;

    location / {    // 这里为配置Angular router 404 问题
        root /usr/share/nginx/html;
        try_files $uri $uri/ /index.html =404;
    }

    location /norm-webapp {             // 拦截 /norm-webapp开头的api请求
        proxy_pass http://tomcat_client;       // 这里为转发
    }

}

```