前面的节点套CF优选IP需要使用端口80，往往VPS的80端口需要预留，不能被使用。使用别的端口也可以套CF优选IP，需要增加访问地址端口回源。

## DNS域名解析

参照前一篇。

## VPS新建节点（XUI面板）

1. 以vless节点为例，选择vless+ws。
  
2. snifiying不用，端口为任意。
  
3. uuid填入新生成的。
  
4. path可默认/。
  
5. 其它不动，保存启动。
  

## CF设置回源

1. 在CF内解析的域名页面内，左侧菜单栏内选择 >>"Rules" >>"Origin Rules"
  
2. 选择"Create Rule"，进入Rule编辑界面。
  
3. Field选择"hostname"，Operator选择"equals"，Value填入DNS解析过的域名。
  
4. 目标端口填入节点的端口，保存完成规则设置，如图示。
![pic-1](https://oss.761226.xyz/Pictures/web/cf-125034.png)
  
6. 左侧菜单栏选择 >>"SSL/TLS" >>"edge certificates"。
  
7. "Always Use HTTPS"选项需要关闭，如图示，如果需要使用优选IP的80端口连接。
![pic-1](https://oss.761226.xyz/Pictures/web/cf-102046.png)

## 套用CF优选IP

1. 拷贝上述vless节点的地址，在V2rayN中导入，测试是否连通。
  
2. 连通有效，把节点配置的ip地址更改为自己的域名（上面DNS解析的），再次测试连接延时。
  
3. 编辑节点，在地址中填入CF优选IP，端口改为80。
  
4. 然后把自己的域名写入伪装域名栏，编辑完成，测试连接。
  

## 套tls使用

1. 上述节点是不带tls的，拷贝一个有效节点。
  
2. 编辑节点，端口改成443（tls加密默认端口）。
  
3. tls协议选择打开，编辑完成，然后测试连接。
  
4. 这样客户端到CF的传输就有tls加密了，但cf到VPS是没有tls的。
  
5. 如果CF到VPS传输需要tls加密，则需要在VPS上建立带tls证书的节点。