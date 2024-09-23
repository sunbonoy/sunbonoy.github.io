WARP是Cloudflare提供一个代理软件，基于Wireguard协议的隧道连接，可以被用来科学上网。自2024年6月WARP的IP地址被大量封禁，基本就无法连接使用了。

目前WARP使用新协议MASQUE，又能够使用了，免费又方便的的工具还是值得收藏的。

## Windows上WARP使用

- 到[WARP官网](https://one.one.one.one/)下载windows的客户端，并安装。
  
- 在"C:\ProgramData\Cloudflare"目录下新建一个**mdm.xml**文件，写入以下内容，并保存。
  

```
<dict>
    <key>warp_tunnel_protocol</key>
    <string>masque</string>
</dict>
```

- 重启WARP，点击连接按钮，即可连接上，十分简单。

## Android手机使用WARP

相比windows，安卓手机使用WARP就复杂很多，基本操作步骤如下。

- 手机先连接能够科学上网的网络（比如已经连接外网的电脑共享网络），不要用手机开代理上外网。
  
- 进入Google Player商店，下载“1.1.1.1”的WARP客户端，目前需要加入Beta计划，因为试用版才支持MASQUE协议。
  
- 安装后，打开软件，会提示更新软件，更新后就是试用版的客户端，版本号：6.35。
  
- 继续打开软件完成提示的设置，需要在连接外网条件下完成客户端初始注册。
  
- 进入菜单 >- 高级 >- 连接选项 >- Tunnel protocol，选择`MASQUE`，完成设置。
  
- 断开外网连接，WARP客户端上点击连接按钮，即可连接成功。
  

> [!Note]
> 
> 1. IOS手机没有使用，所以没有测试，网上教程不少，很简单。
>   
> 2. 需要WARP+的，可以添加密钥到账户内，就有用不完的流量。密钥的生成请自行网络搜索。
>   
> 3. Zero Trust也是可以用的，需要登陆CF网站，在Zero Trust设置里开启"MASQUE`"，客户端直接登录Zero Trust账号即可。
>