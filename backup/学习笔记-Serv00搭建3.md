
这次介绍网址搭建，Serv00已经配置网站运行环境，数据库，安装Wordpress就可以方便的创建网站了。

## 安装Wordpress

- 按照官方文档指导，可以方便的安装部署wordpress，[文档地址](https://docs.serv00.com/WordPress/)。
  
- 安装前需要先创建一个自定义域名，在域名的服务商处添加DNS地址解析，比如域名是CF上托管，就在CF里操作。DNS记录里添加A记录，指向Serv00提供的IP地址，小黄云不用开。
  
- 再到Serv00提供的服务面板里，添加自定义域名，选择php，如图示。
  ![fig-1](https://img.761226.xyz/file/5c5dbd081e28cf43b5f74.png)

- 继续在面板里找到SSL证书服务，选择一个IP地址，给刚添加的域名申请SSL证书，使用生成证书的选项，一会儿就会完成，如图示。
  ![fig-2](https://img.761226.xyz/file/123a65ac61b35b431a530.png)

- 创建数据库，在MySQL里面，添加名称，用户名和密码，需要记录下来，后面用到，如图示。
  ![fig-3](https://img.761226.xyz/file/5811611a90ecdcf01fb11.png)

- 下载wordpress安装包到服务器里，可以使用官方文档的指令，进入域名生成的目录内再执行指令。中文版可以改个下载地址，代码示例：
  

```
fetch https://cn.wordpress.org/latest-zh_CN.zip
```

- 继续按照官方文档操作，解压，移动文件，安装完毕。
  
- Web浏览器访问域名地址，就会进入wordpress界面，需要输入数据库信息，把之前记录的数据库名称，用户名，密码等等...填入。并新建wordpress管理员名称和密码
  
- 至此，部署完成，进入wordpress管理面板，可以开始网站搭建了。如何使用wordpress做网站，网上教程很多，不多介绍了。