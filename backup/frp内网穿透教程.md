# frp内网穿透教程

家里NAS上搭建的本地服务，也会需要从公网上访问，比如immich照片库，talebook书库，就需要内网穿透了。frp能方便的实现内网穿透，还免费，唯一的要求是需要一台云服务器。
在内网穿透的同时，使用自定义域名加SSL证书进行HTTPS的访问是最安全的。测试了很多次，有两种设置方式可以完成上面的要求。

## 方法一：官方文档推荐的“Enable HTTPS for Local HTTP Service”
### frp服务端操作步骤
- 在云服务器上搭建，我用的是docker安装frps，指令如下：
```
docker run --restart=always --network host -d -v /etc/frp/frps.toml:/etc/frp/frps.toml --name frps snowdreamtech/frps
```
- 编辑服务端的配置文件frps.toml
指令`vi /etc/frp/frps.toml`，写入下面内容，按注释修改。
**服务端配置**
```
bindPort = 7000
vhostHTTPSPort = 443
auth.method = "token"
auth.token = "xxxxxxx" # 自己定义密码
```

- 完成配置文件编辑后，docker重启frps，服务端就运行起来了。
- 准备一个自己的域名，解析云服务器的IP地址，推荐在cloudflare上完成。
- 给域名申请SSL证书，这个需要在云服务器上操作，推荐使用acme脚本进行，github上有很多一键脚本。
- 下载申请完成的证书文件，cer文件和key文件即可，在本地搭建需要使用。
-
### frp客户端操作步骤
- 在本地NAS上搭建，docker安装frpc，指令如下：
```
docker run --restart=always --network host -d -v /etc/frp:/etc/frp --name frpc snowdreamtech/frpc
```
- 编辑客户端的配置文件frpc.toml
指令`vi /etc/frp/frpc.toml`，写入下面内容，按注释修改。
**客户端配置**
```
serverAddr = "x.x.x.x" # 云服务器地址
serverPort = 7000
auth.method = "token"
auth.token = "xxxxxxx" # 自己定义密码，和服务端一致
[[proxies]]
name = "test_htts2http" # 修改名称
type = "https"
customDomains = ["test.yourdomain.com"] # 自己的域名
[proxies.plugin]
type = "https2http"
localAddr = "127.0.0.1:80" #需要挂到公网的本地服务地址和端口
#HTTPS certificate related configuration
crtPath = "/etc/frp/server.crt" # 存放cer文件的路径，修改文件名为自己的证书名称
keyPath = "/etc/frp/server.key" # 存放key文件的路径，修改文件名为自己的证书名称
hostHeaderRewrite = "127.0.0.1"
requestHeaders.set.x-from-where = "frp"
```

- 把下载的两个证书文件保存到指定路径，参照配置文件里写的。
- 完成上述步骤后，docker重启frpc，客户端启动。
- 正常就连上了服务端，可以查看log文件，检查连接状态。
- 在浏览器输入"https://自己的域名", 可以正常访问。

## 方式二：需要Nginx反向代理frp的连接的本地服务
### frp服务端操作步骤
- 在云服务器上搭建，docker安装frps，指令如下：
```
docker run --restart=always --network host -d -v /etc/frp/frps.toml:/etc/frp/frps.toml --name frps snowdreamtech/frps
```
- 编辑服务端的配置文件frps.toml
指令`vi /etc/frp/frps.toml`，写入下面内容，按注释修改。  
**服务端配置**
```
bindPort = 7000
vhostHTTPPort = 8084 # 与本地服务的端口一致
auth.method = "token"
auth.token = "xxxxxxx" # 自己定义密码
```

- 完成配置文件编辑后，docker重启frps，服务端就运行起来了。
- 准备一个自己的域名，解析云服务器的IP地址，推荐在cloudflare上完成。
- 给域名申请SSL证书并设置反向代理(nginx)，指向frps在http的端口（见上面配置文件里）。可以安装宝塔面板或者1panel面板来完成证书和反向代理，会比较方便。
- nginx配置文件参考如下，注释的地方会需要修改
```
server {
    listen 80;
    listen [::]:80;
    listen 443 ssl;
    listen [::]:443 ssl;
    server_name yuming.com; # 自己的域名
	   # SSL证书存放路径
    ssl_certificate /etc/nginx/certs/yuming.com_cert.pem; 
    ssl_certificate_key /etc/nginx/certs/yuming.com_key.pem;
    # HTTP 重定向到 HTTPS
    if ($scheme = http) {
        return 301 https://$host$request_uri;
    }

    location / {
        proxy_set_header   X-Real-IP $remote_addr;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   Host $host;
        proxy_pass         http://0.0.0.0:0000; # 云服务器地址:端口号
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection "upgrade";
        add_header Alt-Svc 'h3=":443"; ma=86400';
    }

    location ~* \.(js|css|png|jpg|jpeg|gif|ico|bmp|swf|eot|svg|ttf|woff|woff2|webp)$ {
        proxy_pass http://0.0.0.0:0000; # 云服务器地址:端口号
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_http_version 1.1;
        proxy_cache my_proxy_cache;
        proxy_set_header Accept-Encoding "";
        aio threads;
        # add_header Cache-Control $add_cache_control;
        add_header Alt-Svc 'h3=":443"; ma=86400';
        log_not_found off;
        access_log off;
    }
    client_max_body_size 1000m;
}
```
- 浏览器输入"https://自己的域名", 能够打开并显示nginx的内容，说明反向代理成功。

### frp客户端操作步骤
- 在本地NAS上搭建，docker安装frpc，指令如下：
```
docker run --restart=always --network host -d -v /etc/frp:/etc/frp --name frpc snowdreamtech/frpc
```
- 编辑客户端的配置文件frpc.toml
指令`vi /etc/frp/frpc.toml`，写入下面内容，按注释修改。
**客户端配置**
```
serverAddr = "x.x.x.x" # 云服务器地址
serverPort = 7000
auth.method = "token"
auth.token = "xxxxxxx" # 自己定义密码，与服务端一致
[[proxies]]
name = "web" # 修改为自己的名称
type = "http"
localIP = "127.0.0.1" # 本地服务的地址，默认127.0.0.1
localPort = 8084 # 本地服务的端口
customDomains = ["test.yourdomain.com"] # 自己的域名
```
- docker重启frpc，客户端启动。
- 正常就连上了服务端，可以查看log文件，检查连接状态。
- 再次浏览器输入"https://自己的域名", 可以正常访问本地的服务内容。

## 对比两种方法差异

1. frp隧道连接不同，传输内容也不同
	1. 方法一：本地服务连接到服务端的https的443端口，按https协议传输，SSL证书保存在本地NAS上，因为需要把http转成https。
	2. 方法二：本地服务连接到服务端的http端口(例如8084)，按http协议传输，服务端通过反向代理完成https访问，SSL证书在服务端上。
	
2. 优缺点
	1. 方法一：安装较为方便，但需要手动申请证书更新证书，并拷贝的本地，对长期使用不方便。
	2. 方法二：除frp外需要安装反向代理工具，如nginx，步骤较多。但一次搞定后，自动续签证书，可长期使用。

## 相关搭建工具
- 申请SSL证书一般使用acme脚本，自制了一个ACME证书管理的脚本，方法一搭建可以使用。
```
bash <(curl -Ls https://raw.githubusercontent.com/sunbonoy/vps-scipts/master/myacme.sh)
```
- 为了方便完成反向代理和证书申请，自制了nginx的搭建脚本，方法二搭建可以使用。
```
bash <(curl -Ls https://raw.githubusercontent.com/sunbonoy/vps-scipts/master/test1.sh)
```

	

