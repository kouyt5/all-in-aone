+ frp版本: 0.37.0
# 介绍
内网因为没有公网ip，所以搭建的服务外网不能直接访问。frp就是解决这个问题的，原理就是通过一个公网服务器，和内网建立一个通道，访问公网服务器的端口相当于访问内网服务器设置的端口一样。以此来达到内网穿透的目的。下面包含开箱即用的docker启动方式。只需要填写配置文件配置相应服务端口即可。
# 启动
## 启动服务端

```
./frps -c ./frps.ini
```
or 使用docker
```
docker-compose up -d
```
其中frps.ini文件如下
```
[common]
bind_port = 7000  # 客户端连接的端口
vhost_http_port = 7600  # 用于配置http
dashboard_port = 7500  # 管理界面
# dashboard的用户名和密码
dashboard_user = user
dashboard_pwd = pass
```

## 启动客户端

```
./frp -c ./frp.ini
```
需要注意的是必须替换配置文件中的server_addr为你的服务器
or 使用docker
```
docker-compose -f docker-compose-client.yml up -d
```
配置文件如下
```
[common]
server_addr = 127.0.0.1  # 公网服务器ip地址
server_port = 7000  # 公网服务器连接端口

[ssh]
type = tcp
local_port = 22
remote_port = 11406

[squid]
type = tcp
local_port = 3128
remote_port = 13128
```