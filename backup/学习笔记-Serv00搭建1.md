
Serv00的免费主机的性能一般，但免费十年，还是值得申请使用的。这个主机有些特殊，系统是FreeBSD，在搭建应用过程中也有很多问题，记录一些使用经验。，

## 搭建Alist

具体的搭建过程可以参考Yutube的视频，这里记录一些重要的和容易错的事宜，还有我自己的操作方法。

- Alist运行程序需要使用FreeBSD编译的程序，Github上的[项目地址](https://github.com/uubulb/alist-freebsd)。也可以使用视频里up主的程序。
  
- 通过curl指令下载文件不好使，好像受限了。直接项目里下载最新release文件，通过Serv00的面板上传文件到服务器。
  
- 进入存放alist文件目录，并授予alist文件执行权限，并运行
  

```
chmod +x alist
./alist server
```

- 启动后会有password生成，一定要记录下来，后面不会再显示。
  
- 在Data目录下会生成一个config.json文件，打开它，修改配置参数。
  
- "database"的选项修改，把Serv00上新建的MySQL数据库信息填入，<mark>xxx</mark>内容替换为自己的。
  
  - "type": "mysql",
    
  - "host": "mysql7.serv00.com",
    
  - "port": <mark>3306</mark>,
    
  - "user": "m2853_<mark>xxx</mark>",
    
  - "password": "<mark>xxx</mark>",
    
  - "name": "m2853_<mark>xxx</mark>",
    
- “Scheme“的选项修改，<mark>xxx</mark>内容替换为自己的；address监听本地就好。
  
  - "address": "127.0.0.1",
    
  - "http_port": <mark>xxx</mark>,
    
- 配置文件修改完成，进入alist程序的目录下，启动alist并放入后台运行，可以使用nohup指令，也可以使用screen指令建一个shell窗口一直运行着，指令如下：
  

```
# nohup运行
nohup ./alist server &

# screen运行
screen -d -m ./alist server
```

- 使用自定义域名访问的话，请参考搭建视频操作，注意域名DNS解析，proxy代理设置，alist存放目录等等......
  
- 如果不用自定义域名，可以搭建Cloudflared隧道，然后用隧道域名访问，速度更快一些，二选一。