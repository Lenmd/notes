
``` bash

# 测试配置文件是否正确
nginx -t

#重新加载配置
nginx -s reload

```


```nginx
location /app/{
	proxy_pass http://localhost;
}

/app/user      http://localhost/app/user


location /app/{
	proxy_pass http://localhost/;
}
/app/user      http://localhost/user

```

- 如果你希望**去掉前缀**：使用 `proxy_pass http://backend/;`
    
- 如果你希望**保留前缀**：使用 `proxy_pass http://backend;`


## 多站点

```nginx
server {
	listen 80;
	server_name a.com;
	location / {
		root   /var/www/a;
	}
}
server {
	listen 80;
	server_name b.com;
	location / {
		root   /var/www/b;
	}
}
```

## 负载均衡配置

```nginx
http {
    upstream backend_servers {
        server 192.168.0.101:8080;
        server 192.168.0.102:8080;
        server 192.168.0.103:8080;
    }

    server {
        listen 80;
        server_name yourdomain.com;

        location / {
            proxy_pass http://backend_servers;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
        }
    }
}

```

负载均衡策略
round-robin（默认） 轮询方式分发请求
weight  权重
ip_hash 同一客户端ip分发同一后端
least_conn  最少连接优先

```nginx
# 权重方式
upstream backend_servers {
	server 192.168.0.101:8080 weight=3;
	server 192.168.0.102:8080 weight=1;
}

upstream backend_servers {
	ip_hash
	server 192.168.0.101:8080;
	server 192.168.0.102:8080;
}

upstream backend_servers {
	least_conn
	server 192.168.0.101:8080;
	server 192.168.0.102:8080;
}

```

## 完整配置

```nginx

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
            root   html; #网站根目录
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


```