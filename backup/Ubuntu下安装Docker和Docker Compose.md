# 安装Docker和Docker Compose的步骤

旧电脑上NAS文件共享已经搭建，要继续折腾，比如做一些家庭影院或者云相册共享，那就需要Docker容器了。

## Docker安装
1. 没有使用官方安装步骤，根据网络教程，选择了下面安装指令组合：

```
sudo apt-get install ca-certificates curl #安装SSL证书包，安装curl工具（
curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add - #从阿里云镜像站下载Docker的加密密钥，将密钥添加到APT可信密钥列表
sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable" #配置阿里云Docker镜像源
sudo apt-get install docker-ce docker-ce-cli containerd.io  #安装Docker核心组件
sudo docker version #验证版本
```
2. 安装完毕，添加国内可用的仓库加速网址到配置文件
```
sudo vim /etc/docker/daemon.json
```
3. 一般会新建这个配置，按Insert输入下面内容，找到的一些国内仓库加速网址，不一定都能用，有自己建的也可以加进去，我是使用自己在CF上建立的中转网址。
```
{
  "registry-mirrors": [
    "https://dockerhub.icu",
    "https://docker.chenby.cn",
    "https://docker.1panel.live",
    "https://docker.awsl9527.cn",
    "https://docker.anyhub.us.kg",
    "https://dhub.kubesre.xyz"
  ]
}

```
4. 写入后，按ESC退出编辑，保存并退出（键盘输入:wq!，再回车键）
5. 重启docker使配置文件生效
```
sudo systemctl daemon-reload
sudo systemctl restart docker
```
6. 后续下载docker镜像速度就很快了

## Docker-compose安装
安装了Docker，已经能够使用了，但运行的指令和参数太多，不方便使用，为了更方便管理和配置Docker运行，Docker-compose是需要的。
1. 使用官方的源进行安装，指令如下
```
sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-Linux-x86_64 > /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose #授予运行权限
sudo docker-compose version #验证版本
```
2. 安装完成，还可以增加一些配置，如开启容器的 IPv6 功能，以及限制日志文件大小，同样编辑daemon.json的配置文件，加入下面内容
```
{
    "log-driver": "json-file",
    "log-opts": {
        "max-size": "20m",
        "max-file": "3"
    },
    "ipv6": true,
    "fixed-cidr-v6": "fd00:dead:beef:c0::/80",
    "experimental":true,
    "ip6tables":true
}
```
3. 保存退出后，重启docker服务。

安装好docker和docker-compose后，就可以安装各种好用的软件了。:smile:

> [!Tip] 
> ### 常用的docker指令
> - docker info #查看docker的所有信息
> - docker ps #查看运行的容器，加-a查看所有容器
> - docker images #查看存在的镜像文件
> - docker pull <image_name> #下载镜像文件
> - docker run #建立并启动容器，需要运行参数
> - docker stop <container_id> #停止容器运行，也可使用<container_name>
> - docker start <container_id> #运行存在的容器，也可使用<container_name>
> - docker rm <container_id> #删除容器，也可使用<container_name>
> - docker logs <container_id> #查看容器的log日志，也可使用<container_name>
> - docker rmi <image_name> #删除镜像文件
>
>### docker-compose的指令
> - docker-compose up #创建并启动容器，-d后台运行
> - docker-compose down #停止并删除容器，-v同时删除容器的volume
> - docker-compose restart #重启容器服务
> - docker-compose start #启动存在的容器
> - docker-compose stop #停止容器运行
> 
