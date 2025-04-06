# 家庭NAS系统搭建

十几年前的旧笔记本一直闲置，是戴尔的E6530，配置还行，酷睿i5三代，8G内存，256G的SSD硬盘，扔掉可惜，利用一下搭建家庭NAS系统

## 第一步：安装Ubuntu系统

选用Linux系统是因为对老电脑友好，功耗也低，Ubuntu 24.04.2 LTS版提供稳定的长期服务支持，还是桌面版，方便使用。

- 官网下载地址：[Ubuntu 24.04.2 LTS](https://ubuntu.com/download/desktop)， ISO镜像文件有5.9GB.

- 烧录软件选择Rufus，小巧，不用安装，选择[rufus-4.6_x86.exe](https://github.com/pbatard/rufus/releases/download/v4.6/rufus-4.6_x86.exe)

- 一个U盘插入电脑，打开rufus工具，选择下载的Ubuntu的ISO镜像文件，文件系统选择NTFS，点击开始，一会烧录完成。

- U盘插入旧电脑，开机启动，按F12，出现启动菜单，选择U盘引导。

- U盘启动后，进入Ubuntu安装界面，创建用户名和密码，其他都默认选择，就可以开始安装了。

- 安装完，电脑重启，进入Ubuntu特有的深红色桌面，一切就绪，运行速度还是很快了。

## 第二部：安装OpenSSH服务和设置远程桌面

OpenSSH服务方便使用SSH登录，远程桌面也是方便操作这个旧电脑。

- 在Ubuntu桌面上，鼠标右键后选择打开终端。

- 终端窗口内，输入下面指令，即可安装完成OpenSSH服务

```
sudo apt update
sudp apt install openssh-server
```

- 用别的电脑试一下，IP地址正确就能ssh登录了。

- Ubuntu桌面找到“设置”菜单，选择“系统”--“远程桌面”，进入设置菜单。

- 把“桌面共享”和“远程连接”选项都打开，登录用户名和密码设置一下，就可以使用远程桌了。

- 测试一下，windows电脑打开“远程桌面连接”的窗口，输入计算机名，用户名，点击连接，需要输入密码；输入后会有警告窗口，同意即可，连接成功。

有了OpenSSH和远程桌面，旧电脑就可以不要操作了，连根网线，放到一边静静的运行吧。

> [!Tip]:
> 
> 1. 连接网线后，给旧电脑固定一下IP地址，这个在无线路由里面设置一下就好，方便访问。
> 
> 2. 我的戴尔笔记本把盖子合上后系统就挂起了，无法选项登录，需要取消挂起，保持一直运行状态。ubuntu桌面设置内找不到对应的电源设置选项，一时难住了。
> 
> 3. AI搜索问询了一下，给出了解决办法，在终端窗口内输入下面指令，编辑logind.conf文件
>    
>    `sudo vi /etc/systemd/logind.conf`
> 
> 4. 找到下面行，去掉前面的#, 将 `HandleLidSwitch` 的值改为 `ignore` 
>    HandleLidSwitch=ignore  
>    HandleLidSwitchDocked=ignore
> 
> 5. 保存退出，重启服务
> 
>   `sudo systemctl restart systemd-logind`
> 
> 6. 这下关上笔记本屏幕，系统就不会挂起了，一直会运行，当然屏幕也不熄屏，可以把亮度调到最低。
>
> 7. 远程桌面设置里面会有安全密钥设置要求，需要设置一下，并且每次系统重启后，远程桌面连接的密码会新生成，需要去查看和再次改成自己的，否则无法用原密码远程登录，这点比较麻烦

## 第三步：SMB文件共享

这个比较简单，安装samba协议，就能实现文件共享。

- 用别的电脑的终端ssh登录笔记本的ubuntu系统，输入安装指令

```
sudo apt install samba
```

- 安装完成后，使用vim编辑配置文件

```
sudo vi /etc/samba/smb.conf
```

- 在打开的配置文件里写入需要共享的文件夹配置信息，参考如下，path是文件夹的路径，user, group用自己的。

```
[my_docu]
path = /home/user/Documents
available = yes
browseable = yes
public = yes
writable = yes
create mask = 0755
directory mask = 0775
security = share
force user = xxxx
force group = xxxx
```

- 保存并退出，需要重启一下smb服务，指令如下

```
sudo service smbd restart
```

- 浏览器通过IP地址就可以远程访问共享的文件夹了，成功

- 需要多共享文件夹，在smb.conf里再添加新的共享配置设置就好。

- 把我的存电影的移动硬盘外挂在笔记本电脑上，设置共享后，能够远程访问看电影了 😄
