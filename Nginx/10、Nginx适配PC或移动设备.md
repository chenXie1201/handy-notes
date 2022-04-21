
现在很多网站都是有了PC端和H5站点的，因为这样就可以根据客户设备的不同，显示出体验更好的，不同的页面了。

Nginx 判断当前设备是移动端还是PC端，然后跳转到相应的文件

先创建相应的文件

```bash
cd /usr/share/nginx
mkdir pc
mkdir mobile
```

然后新建 index.html 文件

```html
<!--pc/index.html-->
<h1>I am pc!</h1>

<!--mobile/index.html-->
<h1>I am mobile!</h1>
```

进入 nginx.conf 

```bash
server{
     listen 80;
     server_name nginx2.jspang.com;
     location / {
      root /usr/share/nginx/pc;
      # $http_user_agent 是个全局的变量
      # 判断设备跳转页面
      if ($http_user_agent ~* '(Android|webOS|iPhone|iPod|BlackBerry)') {
         root /usr/share/nginx/mobile;
      }
      index index.html;
     }
}
```

然后刷新浏览器或者在浏览器调试工具为手机就可以看到相应的页面了