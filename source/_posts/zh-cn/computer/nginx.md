---
title: nginx
category: computer
date: 2020-02-08 16:42:15
tags:
---

# Nginx 配置改动

```conf /etc/nginx/nginx.conf
http {
    # 分段配置
    include /etc/nginx/conf.d/*.conf;
}
```

梗概:

1. 配置v2ray的WebSocket+TLS+Web, [模板在此](https://github.com/KiriKira/vTemplate/blob/master/websocket%2BNginx%2BTLS/nginx_Domain.Name.conf)
2. 配置hexo目录 `/var/www/blog/public`
3. 配置网站目录 `/minecraft` 的目录为 `/usr/share/nginx/html/minecraft/` 以存放mc相关的资源文件

```conf /etc/nginx/conf.d/nginx_v2ray.conf
##
# You should look at the following URL's in order to grasp a solid understanding
# of Nginx configuration files in order to fully unleash the power of Nginx.
# https://www.nginx.com/resources/wiki/start/
# https://www.nginx.com/resources/wiki/start/topics/tutorials/config_pitfalls/
# https://wiki.debian.org/Nginx/DirectoryStructure
#
# In most cases, administrators will remove this file from sites-enabled/ and
# leave it as reference inside of sites-available where it will continue to be
# updated by the nginx packaging team.
#
# This file will automatically load configuration files provided by other
# applications, such as Drupal or Wordpress. These applications will be made
# available underneath a path with that package name, such as /drupal8.
#
# Please see /usr/share/doc/nginx-doc/examples/ for more detailed examples.
##

# Default server configuration
#
#####本配置使用正常环境 debian9_x64 nginx_1.10.3 openssl_1.1.0f v2ray_4.2
#####兼容客户端Firefox 27, Chrome 30, IE 11 on Windows 7, Edge, Opera 17, Safari 9, Android 5.0, and Java 8
#####注：切勿修改<nginx.conf>中的内容，但<该文件>与<nginx.conf>中的<参数重叠>那么会<遵从前者>

server {
	# 禁用不需要的请求方式 以下只允许 get、post
        if ($request_method  !~ ^(POST|GET)$) {
                return	444;
        }

	listen		0.0.0.0:80;
	listen		[::]:80;
	server_name	www.example.com blog.example.com;	#注：填写自己的域名
	return		301 https://$host/;
}

upstream v2ray {
        server		127.0.0.1:8891;	#注：v2ray后端监听地址、端口
        keepalive	2176;   # 链接池空闲链接数
}

map $http_upgrade $connection_upgrade {
        default		upgrade;
        ''		close;
}


server {
	#要开启 HTTP/2 注意nginx版本 
	#可以使用 nginx -V 检查
	listen  0.0.0.0:443 ssl http2 backlog=1024 so_keepalive=120s:60s:10 reuseport;	# backlog是nginx 监听队列 默认是511 使用命令 ss -tnl查看(Send-Q);
	listen  [::]:443 ssl http2 backlog=1024 so_keepalive=120s:60s:10 reuseport;
	#设置编码
	charset         utf-8;

	#证书配置
	ssl_certificate		/etc/v2ray/v2ray.crt;	#注：填写自己证书路径
	ssl_certificate_key	/etc/v2ray/v2ray.key;	#注：填写密钥路径

	ssl_session_cache	shared:SSL:50m;
	ssl_session_timeout	1d;
	ssl_session_tickets	off;
	
	# https://nginx.org/en/docs/http/ngx_http_ssl_module.html
	ssl_protocols	TLSv1.2 TLSv1.3;
	#openssl ciphers
	#注：懒人配置	https://mozilla.github.io/server-side-tls/ssl-config-generator/
	ssl_ciphers	TLS-CHACHA20-POLY1305-SHA256:TLS-AES-256-GCM-SHA384:TLS-AES-128-GCM-SHA256:EECDH+CHACHA20:EECDH+AESGCM:EECDH+AES;
	ssl_prefer_server_ciphers on;
	
	#安全设定
	#屏蔽请求类型
        if ($request_method  !~ ^(POST|GET)$) {
                return  444;
        }
	add_header      X-Frame-Options         DENY;
	add_header      X-XSS-Protection        "1; mode=block";
	add_header      X-Content-Type-Options  nosniff;
	# HSTS (ngx_http_headers_module is required) (15768000 seconds = 6 months)
	###测试前请使用较少的时间
	### https://www.nginx.com/blog/http-strict-transport-security-hsts-and-nginx/
	add_header	Strict-Transport-Security max-age=15 always;
	
	#openssl dhparam -out dhparam.pem 2048
	#openssl dhparam -out dhparam.pem 4096
	#ssl_dhparam		/home/dhparam.pem;
	#ssl_ecdh_curve		secp384r1;

	# OCSP Stapling ---
	# fetch OCSP records from URL in ssl_certificate and cache them
	ssl_stapling		on;
	ssl_stapling_verify	on;
	resolver_timeout	10s;
	#resolver	[去掉括号并将文字改成你希望的dns服务器ip地址]	valid=300s;
			#范例 resolver	2.2.2.2		valid=300s;
	resolver	8.8.8.8		valid=300s;

	root	/var/www/blog/public;

	# Add index.php to the list if you are using PHP
	index index.html index.htm  index.php ;

	server_name	www.example.com blog.example.com;	#注： 将domain.Name 替换成你的域名

	location /minecraft {
		alias /usr/share/nginx/html/minecraft/;
		autoindex off;
	}

	location /projectv/ {	#注：修改路径
		proxy_http_version	1.1;
		proxy_set_header	Upgrade $http_upgrade;
		proxy_set_header	Connection $connection_upgrade;	#此处与<map>对应
		proxy_set_header	Host $http_host;
		
		# 向后端传递访客ip
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		
		
		sendfile                on;
		tcp_nopush              on;
		tcp_nodelay             on;
		keepalive_requests      25600;
		keepalive_timeout	300 300;
		proxy_buffering         off;
		proxy_buffer_size       8k;
		
		#后端错误重定向
		proxy_intercept_errors on;
                error_page 400 = https://github.com/;		# url是一个网站地址。例如:https://www.xxxx.com/
		if ($http_host = "www.example.com") {	#注： 修改 domain.Name 为自己的域名
			#v2ray 后端 查看上面"upstream"字段
			proxy_pass      http://v2ray;
		}
	}
}
```