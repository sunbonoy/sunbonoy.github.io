继续分享一些搭建和使用的经验。

> [!Note]
> 
> - 目前这些项目的代码已经很成熟了，更改很少，建议把代码直接下载下来，然后在CF上进行部署。如果用Wokers部署，就拷贝代码进去；如果用Pages部署就上传代码压缩包，很方便。
>   
> - 使用Workers部署，修改变量或者代码后直接保存就能完成重新部署，很方便，优先推荐。但需要套上自定义域名来使用，因为Workers域名被阻断，Workers套域名必须使用CF内托管的域名。
>   
> - 如果没有域名，建议使用Pages部署，只是每次修改代码或者变量后需要重新上传代码数据包来部署，稍微麻烦一些。Pages也可以加自定义域名，并可以使用没有托管CF的外部域名。
>   
> - Vless节点有带tls节点和不带tls节点两种，Workers部署后这两种节点都可以使用，如果加了自定义域名则只有tls节点可使用；Pages部署的只能使用带tls的节点。
>   
> - Trojan节点只有带tls的，更简单方便一些。
>   
> - 如果使用优选IP的话，请使用订阅链接地址在客户端订阅。


> [!Tip]
> 
> - 优选IP：分为优选官方IP和优选反代IP。官方IP是CF家的IP地址，有很多，还包括官方域名（用CF家DNS解析的网站）；反代IP是指反向代理了CF CDN的IP端口，这些iP地址能直接把访问CF的数据直接转到CF CDN，包括一些反代域名。优选IP可以使用大佬们提供的，也可以自建测速项目，用自己优选出来的IP地址。
>   
> - ProxyIP：在CF的节点项目里ProxyIP是很重要的，因为在访问CF自家的网站时候会出现问题，而用ProxyIP转发访问CF的请求，可以解决这个问题。ProxyIP的获得是比较麻烦的，建议使用大佬们提供的，只是高峰时段会比较拥堵。
>   
> - 优选IP端口：不带TLS的节点可以使用七个端口，80 8080 8880 2052 2082 2086 2095；带了TLS的节点可以使用六个端口，443 2053 2083 2087 2096 8443。这些是常规端口，还有一些特殊的端口号正常是获取不了的。
>   
> - 优选订阅器：cmliu大佬的这个是比较好用的，既可以给CF上的vless节点使用，还可以给其它VPS上自建的vless节点使用，前提是套了CF域名。推荐搭建一个，通过API参数来更新优选IP。
>   
> - TLS：http的加密协议，加密后就变成https。不带tls的vless节点速度会比带tls的快一点，但因为数据不加密，安全性差。要是有不带tls节点，CF域名设置里把强制https的选项需要关闭。
> 
> - Socks5：Socks5代理可以替代ProxyIP，cmliu大神的项目都有说明。公用的Socks5地址不好使，用免费小鸡搭建一个会比较好用，比如Serv00提供的容器型VPS。
> 