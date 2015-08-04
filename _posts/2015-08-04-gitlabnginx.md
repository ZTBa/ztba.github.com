---
layout: post
title: "利用Gitlab内置nginx添加新网站"
description: ""
category: lessons
tags: [linux,debian,tutorial]
---
{% include JB/setup %}

Gitlab整合了仿制Github所需要的几乎所有服务器端程序,例如内置了nginx. 作为静态网站服务的首选服务, nginx强大灵活的分流配置一直为广大程序员喜爱.


##目标
利用现成的Gitlab内置nginx服务器, 添加新的静态网站到指定端口

##改菜谱
Gitlab利用一种叫做菜谱cookbook的方法管理不同模板template的配置文件,每次gitlab-ctl reconfigure都是重新生成配置文件的过程. 换言之, 原先直接对配置文件的修改在这里是行不通的, 因为只要重启服务就会被覆盖.

修改nginx的菜谱
  
		sudo vi /opt/gitlab/embedded/cookbooks/gitlab/templates/default/nginx.conf.erb 

找到 **include <%= @gitlab_http_config %>;** 这一行, 在其上方添加客制配置文件路径 **nginx['custom_nginx_config'] = "include /etc/nginx/conf.d/*.conf;"** 

修改后完整的菜谱为

		# This file is managed by gitlab-ctl. Manual changes will be
		# erased! To change the contents below, edit /etc/gitlab/gitlab.rb
		# and run `sudo gitlab-ctl reconfigure`.

		user <%= node['gitlab']['web-server']['username'] %> <%= node['gitlab']['web-server']['group']%>;
		worker_processes <%= @worker_processes %>;
		error_log stderr;
		pid nginx.pid;

		daemon off;

		events {
		  worker_connections <%= @worker_connections %>;
		}

		http {
		  sendfile <%= @sendfile %>;
		  tcp_nopush <%= @tcp_nopush %>;
		  tcp_nodelay <%= @tcp_nodelay %>;

		  keepalive_timeout <%= @keepalive_timeout %>;

		  gzip <%= @gzip %>;
		  gzip_http_version <%= @gzip_http_version %>;
		  gzip_comp_level <%= @gzip_comp_level %>;
		  gzip_proxied <%= @gzip_proxied %>;
		  gzip_types <%= @gzip_types.join(' ') %>;

		  include /opt/gitlab/embedded/conf/mime.types;
		  
		  <%= @custom_nginx_config %>
		  include <%= @gitlab_http_config %>;
		  <% if @gitlab_ci_http_config %>
		  include <%= @gitlab_ci_http_config %>;
		  <% end %>

		}

##添路径
因为所有Gitlab的行为是由Ruby脚本定制的, 所以下一步要告知执行脚本客制网站的配置文件路径.

		sudo vi /etc/gitlab/gitlab.rb
  
找到 **nginx['custom_nginx_config']** 一行取消注释, 并填写其后的配置文件路径, 修改后的内容为

		nginx['custom_nginx_config'] = "include /etc/nginx/conf.d/*.conf;"

##正常配置nginx
如果路径不存在,创建之

		sudo mkdir -p /etc/nginx/conf.d
		sudo vi /etc/nginx/conf.d/nginx.conf
  
这里使用14344端口绑定到客制网站, 完整的配置文件如下

		server {
			listen   14344; ## listen for ipv4; this line is default and implied
			#listen   [::]:80 default_server ipv6only=on; ## listen for ipv6

			root /usr/share/nginx/www;
			index index.html index.htm;

			# Make site accessible from http://localhost/
			server_name 192.168.107.201;

			location ~* \.(eot|ttf|woff)$ {
				add_header Access-Control-Allow-Origin *;
			}

			location / {				
				
				 if ($request_method = 'OPTIONS') {
					add_header 'Access-Control-Allow-Origin' '*';
					#
					# Om nom nom cookies
					#
					add_header 'Access-Control-Allow-Credentials' 'true';
					add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
					#
					# Custom headers and headers various browsers *should* be OK with but aren't
					#
					add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
					#
					# Tell client that this pre-flight info is valid for 20 days
					#
					add_header 'Access-Control-Max-Age' 1728000;
					add_header 'Content-Type' 'text/plain charset=UTF-8';
					add_header 'Content-Length' 0;
					return 204;
				}
				if ($request_method = 'POST') {
					add_header 'Access-Control-Allow-Origin' '*';
					add_header 'Access-Control-Allow-Credentials' 'true';
					add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
					add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
				}
				if ($request_method = 'GET') {
					add_header 'Access-Control-Allow-Origin' '*';
					add_header 'Access-Control-Allow-Credentials' 'true';
					add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
					add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
				}
			}
			
		}

然后在指定路径上添加静态网页

		sudo mkdir -p /usr/share/nginx/www
		sudo chmod 0777 -R /usr/share/nginx/www
		vi /usr/share/nginx/www/index.html

最后重启Gitlab服务

		gitlab-ctl reconfigure
  
收工.