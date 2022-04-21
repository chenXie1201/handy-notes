查看 Nginx 安装的目录

```bash
rpm -ql nginx

# /etc/nginx/nginx.conf
```     

进入到文件

```bash
cd /etc/nginx

ls 

vim nginx.conf
```

## 配置文件简单认识

```bash
user nginx; # 登录的用户
worker_processes auto; # 进程数
error_log /var/log/nginx/error.log; # 错误日志存放位置
pid /run/nginx.pid; # 存放pid位置

# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024; # 后台容许最大的并发数
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main; # 访问日志位置

    sendfile            on; # 高速传输模式
    tcp_nopush          on; # 减少网络报文
    tcp_nodelay         on; # 
    keepalive_timeout   65; # 超时时间
    types_hash_max_size 2048;

    include             /etc/nginx/mime.types; # 文件扩展名映射表
    default_type        application/ # 默认文件类型
    octet-stream;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf; # 子配置项

    server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  _;
        root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
            root /usr/share/nginx/html;
            index index.html index.htm;
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
      }
     # Settings for a TLS enabled server.
     #
     #    server {
     #        listen       443 ssl http2 default_server;
     #        listen       [::]:443 ssl http2 default_server;
     #        server_name  _;
     #        root         /usr/share/nginx/html;
     #
     #        ssl_certificate "/etc/pki/nginx/server.crt";
     #        ssl_certificate_key "/etc/pki/nginx/private/server.key";
     #        ssl_session_cache shared:SSL:1m;#        ssl_session_timeout  10m;
     #        ssl_ciphers PROFILE=SYSTEM;
     #        ssl_prefer_server_ciphers on;
     #
     #        # Load configuration files for the default server block.
     #        include /etc/nginx/default.d/*.conf;
     #
     #        location / {
     #        }
     #
     #        error_page 404 /404.html;#            
     #          location = /40x.html {}
     #
     #        error_page 500 502 503 504 /50x.html;
     #            location = /50x.html {
     #        }
     #    }

}

```

## 更改安全组配置

这个时候还不能通过 ip 访问静态页面，还需要我们打开安全组配置

云服务器列表 --》选择你要更改的服务器 --》更多 --》网络和安全组 --》安全组配置 --》规则配置 --》快速添加 --》勾选 HTTP 80 然后确认

然后就能访问页面了