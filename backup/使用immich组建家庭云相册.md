# 使用immich组建家庭云相册
家庭NAS系统可以方便的存储照片，并实现共享，方便各个终端的访问。这方面的方案和工具很多，最终我选择了immich，不管是搭建，使用和app都很让人满意，十分推荐使用👍。
主要介绍一下如何搭建和使用，以及一些需要避开的坑和注意事宜。

## **Immich**
Immich是一款基于开源技术的私有云相册管理工具，旨在提供快速、自动化的照片和视频备份功能。
- 开源地址：[https://github.com/immich-app/immich](https://cloud.tencent.com/developer/tools/blog-entry?target=https%3A%2F%2Fgithub.com%2Fimmich-app%2Fimmich&objectId=2483137&objectType=1&isNewArticle=undefined) 
- 官方地址：[https://immich.app/](https://cloud.tencent.com/developer/tools/blog-entry?target=https%3A%2F%2Fimmich.app%2F&objectId=2483137&objectType=1&isNewArticle=undefined)

## **Docker-compose安装 Immich**
1. 建立目录，下载官方的配置文件（文件目录并不需要创建在root下，可以在用户目录下）
```
mkdir immich
cd immich
wget -O docker-compose.yml https://github.com/immich-app/immich/releases/latest/download/docker-compose.yml
wget -O .env https://github.com/immich-app/immich/releases/latest/download/example.env
```

2. 编辑.env文件
执行`sudo vim .env`，参照如下的配置内容
UPLOAD_LOCATION=./library
TZ=Asia/Shanghai
DB_USERNAME=postgres # 用户名可修改
DB_PASSWORD=postgres # 密码可修改

3. 修改docker-compose.yml
执行`sudo vim docker-compose.yml`，示例内容，外部图片文件目录需要设置。
```
immich-server:
  volumes:
    - ${UPLOAD_LOCATION}:/usr/src/app/upload # 这个不修改
    - /home/user/Pic:/home/user/Pic:ro # 映射地址改为自己的图片存储文件夹，有多个文件夹地址可增加多行
```

4. 配置文件修改保存后，运行指令启动
```
sudo docker-compose up -d
```

5. 登录页面
- 默认地址端口：http://[your_ip]:2283

![image-20241231161436485](https://developer.qcloudimg.com/http-save/8492898/26ecccf6cabcb6cab720f02c3729a51e.png)
- 会需要创建用户名和密码
> [!Tip]
>
> - immich的docker镜像库是在github，pull的时候会很慢，如果有加速地址请自行加上。
> - .env和docker-compose.yml文件内的其他内容不要修改，都是官方默认设置就好。
> - 
  

## **加载自己的图片相册并识别**
- 配置文件内写入的图片存储目录`/home/user/Pic`就是外部图库目录，在immich界面右上角点击用户图标，选择“系统设置”，再选择“外部图库”栏，设置外部图库的地址为`/home/user/Pic`。
- 然后拷贝自己的图片到这个目录即可，整个图片文件夹拷贝也可以，immich可以扫描子目录的。
- 界面上选择“任务”栏，找到“外部图库”任务，点击重新扫描，马上会开始扫描目录内的图片了。
- 正常扫描图片后，智能搜索和人脸检测的功能也会开始，如果没开始，在任务栏这里，找到这些任务，点击启动。
- 任务执行完成，图片就完全被加载了，在首页可以看到自己的图片了。

> [!Tip]
> - 智能搜索和人脸识别的功能需要下载AI模型，默认是从huggingface下载，但国内访问不佳，是无法正常下载模型的，导致功能失效。
> - 解决办法是，需要科学上网，然后到这个网址[huggingface.co/immich-app](https://huggingface.co/immich-app)，找到需要的AI模型。
> 	- 智能搜索的模型名称是：ViT-B-32__openai
> 	- 人脸识别的模型名称是：buffalo_l
> - AI模型的页面，拷贝网页地址，用`git clone`指令下载模型到本地，两个模型都需要，可以直接用下面指令。
> ```
> git clone https://huggingface.co/immich-app/ViT-B-32__openai
> git clone https://huggingface.co/immich-app/buffalo_l
> ```
> - 下载完成后，得到两个模型名称命名的文件夹。
> - 到immich文件夹（即docker运行前创建的），里面会有个model-cache的文件夹，进入该文件夹，在里面创建两个新的文件夹
> 	- clip：存放搜索AI模型，把整个ViT-B-32__openai模型文件夹拷贝进去
> 	- facial-recognition：存放人脸识别AI模型，把整个buffalo_l模型文件夹拷贝进去
> - 完成后，可以重启动一下docker运行，这样智能搜索和人脸识别功能就能正常使用了。

## **Immich App**
- Iphone手机在Apple store里下载，安卓手机在Google Player商店里下载
- 安装后，打开App会要求输入访问地址，用户名和密码。
- 登录成功后，就能看到云服务上的相片了，选中相片可以下载，还可以分享给不同用户。
- 手机本地照片可以选择同步备份，马上就能上传到家庭NAS的硬盘上，保存在immich/library文件夹下，会按用户名分别存储。

