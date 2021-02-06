# Nginx

## 简介

- nginx 是一个高性能的HTTP和反向代理web服务器，同时也提供了IMAP／ＰＯＰ３　／SMTP服务。

- 特点　： 占用内存少，并发能力强（5000个并发连接），专为性能优化开发。



## 安装



- window 安装 官网 ： http://nginx.org/en/download.html

  - 默认启动的是80端口，启动nginx.exe,浏览器输入 http://localhost:80

    

- linux 安装 官网 ： http://nginx.org/en/download.html



## 常用命令

~~~bash
# 启动nginx ：  /usr/local/nginx/sbin   
./nginx 启动 

#查看nginx.conf是否生效
./nginx -t

# 关闭nginx： 
./nginx -s stop

# 查看nginx是否启动： 
ps -aux|grep nginx 

# 安全退出
./nginx -s quit

# 重新加载配置文件
./nginx -s reload

~~~



## 正向代理

正向代理：  在客户端（浏览器）配置服务器，通过代理服务器进行互联网访问(  想成VPN  ，通过 VPN可以访问国外网站)。



## 反向代理



## 负载均衡





## 实战

![nginx215324](D:\typora\JAVA-MD\nginx\nginx215324.png)

~~~bash
# 原生nginx.conf 
# 可以用这个修改为原来的conf

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}

~~~







