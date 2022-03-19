[下载地址](https://nginx.org/en/download.html)：https://nginx.org/en/download.html

<font color=red>注</font>：（指令都需要在nginx的安装目录下——> cmd里面进行）

①启动指令：

```bash
start nginx
```

②地址：http://localhost:80

③停止指令：

```bash
nginx.exe -s stop
```

# 一、nginx解决跨域

```bash
worker_processes 1;


events {
    worker_connections 1024;
}


http {
    include mime.types;
    default_type application/octet-stream;
    sendfile on;
    keepalive_timeout 65;


    server {
        listen 8090;
        server_name localhost;

        location /api/ {
            proxy_pass 服务端IP地址;
        }
    }
    include servers/*;
}
```

在页面的使用

```javascript
axios({
    url: "http://localhost:8090/api/login",
    method: "post",
    data: {
        PlatformName: "test",
        AuthPassword: "123456"
    }
}).then(res => {
    console.log(res);
})
```







