
在真实的上线环境中，一个网站是需要域名和公网IP才可以访问的。

域名需要注册！！！

域名虚拟主机就是访问不同的域名可以看到不同的项目或页面，减少了购买多台服务器的压力。

我们修改etc/nginx/conf.d目录下的default.conf 文件，把原来的80端口虚拟主机改为以域名划分的虚拟主机。

```bash
server {
    listen       80;
    server_name  nginx.jspang.com;
    ...
```

我们再把同目录下的8001.conf文件进行修改，改成如下：

```bash
server{
        listen 80;
        server_name nginx2.jspang.com;
        location / {
                root /usr/share/nginx/html/html8001;
                index index.html index.htm;
        }
}
```

然后重启 nginx

https://jspang.com/article/39#toc8