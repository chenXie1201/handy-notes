
## 直接启动

```bash
nginx # 直接启动

systemctl start nginx.service # 启动任何linux服务

ps aux | grep nginx # 查看服务
```

![](../imgs/nginx1.png)

## 停止服务

```bash
nginx -s quit # 从容的停止

nginx -s stop # 强制停止

killall nginx # 强制停止

systemctl stop nginx.service # 停止

ps aux | grep nginx # 查看服务
```

![](../imgs/nginx2.png)

## 重启

```bash
systemctl restart nginx.service # 重启

nginx -s reload # 重新加载配置文件（更改完nginx配置文件后执行）

netstat -tlnp # 查看服务器开启的端口

nginx -t # 检查nginx配置文件
```