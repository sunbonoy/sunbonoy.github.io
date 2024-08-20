
继续介绍在Serv00上Cloudflared隧道的搭建和使用。

## 搭建Cloudflared

- Cloudflared的程序不能使用CF的原始程序，需要FreeBSD适配的代码程序，直接去项目网站下载最新版的程序后再上传到Serv00里，[下载网址](https://cloudflared.bowring.uk/)。
  
- 在CF里创建tunnel，在Youtube上有很多相关视频，可以参照操作，记录下需要的token。
  
- 解压Cloudflared的程序，然后运行，指令示例如下，token替换为自己的。
  

```
./cloudflared tunnel --edge-ip-version auto --protocol http2 --heartbeat-interval 10s run --token ARGO_TOKEN
```

- Cloudflared的运行也需要放入后台运行，可以使用nohup指令，也可以使用screen指令建一个shell窗口一直运行着。我是使用screen指令，代码如下:

```
screen -d -m ./cloudflared tunnel --edge-ip-version auto --protocol http2 --heartbeat-interval 10s run --token ARGO_TOKEN
```

- 最后，需要在CF里完成隧道的域名设置，并定义http访问指向本地的访问端口（如之前alist的访问端口），如图示。
![](https://img.761226.xyz/file/6e74adeb99a75e40f47b9.png)

- 隧道正常工作后，显示绿色的HEALTHY。可以使用隧道域名访问了，比如alist。
  
- 不要使用pm2进程管理，很快就被清除，用screen是好的选择，也方便做自动进程监控并重启。