frp 具体的使用说明，移步 fatedier/frp。

# 服务端使用方法
` docker run --restart always -d --name frps -p 9080:80 -p 9443:443 -p 9000:7000 -p 9500:7500 -v /srv/docker/frp:/etc/frp  zhangruhong/frp:server-latest ` 

修改服务端配置文件 /srv/docker/frp/frps.ini 后，可通过以下命令重新加载

` docker exec frps frps -c /etc/frp/frps.ini --reload ` 
或者直接重启容器

` docker restart frps` 
服务端配置模板

此处z输入代码


>[common]
 bind_addr = 0.0.0.0
 bind_port = 7000
 vhost_http_port = 80
 vhost_https_port = 443
 dashboard_port = 7500
 #log_file = ./frps.log
 # debug, info, warn, error
 #log_level = info
 #log_max_days = 3
 #auth_token = 123
 #privilege_mode = true
 #privilege_token = 456
 #privilege_allow_ports = 60000-60009
 max_pool_count = 100
 [www]
 #tcp | http | https
 type = http
 auth_token = 123
 #bind_addr = 0.0.0.0
 #listen_port = 6000
 custom_domains = www.doname.com

 
# 客户端使用方法
` docker run --restart always -d --name frpc -v /srv/docker/frp:/etc/frp zhangruhong/frp:client-latest` 
客户端配置文件位于 /srv/docker/frp/frpc.ini，修改后需要重启 frpc

` docker restart frpc` 
客户端配置模板

>[common]
 server_addr = x.x.x.x
 server_port = 7000
 #log_file = ./frpc.log
 # debug, info, warn, error
 #log_level = info
 #log_max_days = 3
 auth_token = 123
 #privilege_token = 456
 [www]
 # tcp | http | https
 type = http
 local_ip = n.n.n.n
 local_port = 80
 #remote_port = 54678
 use_gzip = true
 #use_encryption = true
 pool_count = 2
 #privilege_mode = true
 custom_domains = www.doname.com
 #host_header_rewrite = dev.doname.com
