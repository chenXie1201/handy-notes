
Nginx 官网：nginx.org

## 更改镜像源

查看服务器上 Nginx 的镜像源：

```bash
yum list | grep nginx
```

如果服务器上的镜像源比较落后，我们可以更改镜像源

## 安装 Nginx

```bash
yum install nginx # 安装

nginx -v # 验证是否安装完成

# nginx version: nginx/1.14.1
```
