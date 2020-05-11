---
title: nginx
category: computer
date: 2020-02-08 16:42:15
tags:
toc: true
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
	server_name example.com;	#注：填写自己的域名
	return		301 https://$host$request_uri;
}

server {
	#要开启 HTTP/2 注意nginx版本 
	#可以使用 nginx -V 检查
	listen  0.0.0.0:443 ssl http2 backlog=1024 so_keepalive=120s:60s:10 reuseport;	# backlog是nginx 监听队列 默认是511 使用命令 ss -tnl查看(Send-Q);
	listen  [::]:443 ssl http2 backlog=1024 so_keepalive=120s:60s:10 reuseport;
	#设置编码
	charset         utf-8;

	#证书配置
	ssl_certificate		/etc/letsencrypt/live/example.com/fullchain.pem;	#注：填写自己证书路径
	ssl_certificate_key	/etc/letsencrypt/live/example.com/privkey.pem;	#注：填写密钥路径

	ssl_session_timeout	1d;
	ssl_session_cache	shared:MozSSL:10m;
	ssl_session_tickets	off;
	
	#注：懒人配置	https://ssl-config.mozilla.org/
	ssl_protocols	TLSv1.3;
	ssl_prefer_server_ciphers off;
	
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
	index index.html index.htm index.php;

	server_name example.com;	#注： 将domain.Name 替换成你的域名

	location /minecraft {
		alias /usr/share/nginx/html/minecraft/;
		autoindex off;
	}

	location /stream {	#注：修改路径
	    if ($http_upgrade != "websocket") { # WebSocket协商失败时返回404
			return 404;
		}
		proxy_redirect off;
		proxy_pass http://127.0.0.1:10000;
		proxy_http_version 1.1;
		proxy_set_header Upgrade $http_upgrade;
		proxy_set_header Connection "upgrade";
		proxy_set_header Host $http_host;
		# Show real IP in v2ray access.log
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

		proxy_read_timeout 300s;
	}
}
```