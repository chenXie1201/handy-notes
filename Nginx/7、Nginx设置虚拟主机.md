
什么是虚拟主机？

虚拟主机是指在一台物理主机服务器上划分出多个磁盘空间，每个磁盘空间都是一个虚拟主机，每台虚拟主机都可以对外提供Web服务，并且互不干扰。

在外界看来，虚拟主机就是一台独立的服务器主机，这意味着用户能够利用虚拟主机把多个不同域名的网站部署在同一台服务器上，而不必再为简历一个网站单独购买一台服务器，既解决了维护服务器技术的难题，同时又极大地节省了服务器硬件成本和相关的维护费用。

## 基于端口号配置虚拟主机

原理就是Nginx监听多个端口，根据不同的端口号，来区分不同的网站。

可以配置到主配置文件`/etc/nginx/nginx.conf`或者配置到子配置文件`/etc/nginx/conf.d/default.conf`当中

我这里配置到了`8001.conf`中

```bash
cd /etc/nginx/conf.d

ls 

# default.conf 8001.conf

vim 8001.conf
```

8001.conf 的内容

```bash
server{
    listen 8001;
    server_name localhost;
    root /usr/share/nginx/html/html8001; # 需要有对应的文件夹和文件
    index index.html;
}
```

然后重启 nginx ，浏览器访问 ip:8001 就能看到 `/usr/share/nginx/html/html8001/index.html` 的文件内容了s

## 基于IP的虚拟主机

基于IP和基于端口的配置几乎一样，只是把server_name选项，配置成IP就可以了。

```bash
server{
    listen 80;
    server_name 112.74.164.244;
    root /usr/share/nginx/html/html8001;
    index index.html;
}
```

这种配置需要多个IP的支持。