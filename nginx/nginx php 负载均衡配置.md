# nginx代理节点
    http {
        include       mime.types;
        default_type  application/octet-stream;
    
        log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                          '$status $body_bytes_sent "$http_referer" '
                          '"$http_user_agent" "$http_x_forwarded_for"';
    
        #access_log  logs/access.log  main;
    
        sendfile        on;
        #tcp_nopush     on;
    
        #keepalive_timeout  0;
        keepalive_timeout  65;
    
        #gzip  on;
        upstream backend {
            #ip_hash;
            server 192.168.1.113;
            server 192.168.1.114;
        }
        server {
            listen       80;
            server_name   localhost ;
            charset utf-8;
            root   /data/www/default;
            location / {
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_pass http://backend;
            }
            access_log  /data/log/default.access.log;
            error_log   /data/log/default.error.log;
    
        }
    }

# 后端服务器的配置：
    http {
        include       mime.types;
        default_type  application/octet-stream;
    
        log_format  mylog  '$http_x_forwarded_for  ----- $remote_user [$time_local] "$request" '
                          '$status $body_bytes_sent "$http_referer" '
                          '"$http_user_agent"';
        #access_log  logs/access.log  mylog;
    
        sendfile        on;
        #tcp_nopush     on;
    
        #keepalive_timeout  0;
        keepalive_timeout  65;
    
        #gzip  on;
    
        server {
            listen       80;
            server_name   localhost ;
            charset utf-8;
            root   /data/www/default;
            location / { 
                index  index.html index.htm index.php;
                autoindex  off;
            }
            location  ~ .+\.php($|/)   {
                fastcgi_index index.php;
                fastcgi_pass 127.0.0.1:9000;
                include      fastcgi_params;
                set $path_info "";
                set $real_script_name $fastcgi_script_name;
                if ($fastcgi_script_name ~ "^(.+?\.php)(/.+)$") {
                    set $real_script_name $1;
                    set $path_info $2;
                }
                fastcgi_param SCRIPT_FILENAME $document_root$real_script_name;
                fastcgi_param SCRIPT_NAME $real_script_name;
                fastcgi_param PATH_INFO $path_info;
    
            }
            set_real_ip_from 192.168.1.110;
            real_ip_header X-Real-IP;
            access_log  /data/log/default.access.log mylog;
            error_log   /data/log/default.error.log;
    
        }
    }
# 唯一的区别是：
    set_real_ip_from 192.168.1.110;
    real_ip_header X-Real-IP;
## 主要是为了获取客户端的真实IP地址