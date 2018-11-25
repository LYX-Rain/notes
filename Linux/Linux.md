# Linux笔记

## 系统分区

### 硬件设备文件名：

| 硬件     | 设备文件名 |
| -------- | --------- |
| IDE硬盘  | /dev/hd[a-d] |
| SCSI/SATA/USB 硬盘 | /dev/sd[a-p] |
| 光驱 | /dev/cdrom 或 /dev/sr0 |
| 软盘 | /dev/fd[0-1] |
| 打印机（25针） | /dev/1p[0-2] \n |
| 打印机（USB） | /dev/usb/1p[0-15] |
| 鼠标 | /dev/mouse |

- 5一定是第一个逻辑分区，或者说逻辑分区一定时从5开始的

### 分配盘符（Linux中称为“挂载点”）

- 必须分区：
1. "/"根分区
1. "swap"分区（交换分区，虚拟内存，理论上是内存的2倍，但不超过2GB）
- 推荐分区：
1. "boot"（启动分区，200MB）保存启动时需要的数据，写完之后不在写入任何数据，保证Linux系统的启动

- 从Linux系统上看，/boot、/home 目录都是根目录的子目录，但是从硬盘上看它们可以每一个目录都有自己独立的硬盘空间

### 总结

- 分区：把大硬盘分为小的逻辑分区
- 格式化：写入文件系统
- 分区设备文件名：给每个分区定义设备文件名
- 挂载：给每个分区分配挂载点

## VM 安装 Linux

### CentOS

1. 创建新的虚拟机 > 选择典型 > 选择映像文件 > 选择位置 > 磁盘大小默认即可、选择将虚拟磁盘储存为单个文件
1. 自定义硬件 > 删除 USB 控制器、打印机，网络连接选择桥接模式（这样就可以使局域网中的计算机都能访问到虚拟机） > 完成
1. 光盘引导后选择 Install CentOS 7 （因为是虚拟机，不需要测试磁盘）
1. 进入图形安装界面 > 选择默认英文（选中文会导致有的文件夹是中文名） > 可以先选中文查看后面的配置选项
1. 选择英文后 更改日期和时间 > 选择安装位置（直接点 done） > 选择网络和主机名（打开即可） > 开始安装 > 设置 root 密码 > 创建其他用户（可选） > 安装完成后重启
1. 初始化配置：
    1. 换源：
    ```bash
    # 1.备份原来的数据源配置文件
    sudo mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
    # 2.编辑数据源配置文件
    sudo vi /etc/yum.repos.d/CentOS-Base.repo
    #在(http://mirrors.ustc.edu.cn/)这里找到最新的数据源进行替换
    #tip: vim中输入 :%d 快速删除所有
    # 3.生成缓存
    sudo yum makecache
    ```
    1. 安装 vim (有代码高亮)
    > yun install vim -y
    1. 安装 lrzsz （传输文件）
    > yum install lrzsz -y

### 磁盘分区

## 常用命令

### 命令格式：

- 命令 [-选项] [参数]

### 目录处理命令：ls

- ls -a  显示所有文件，包括隐藏文件
- ls -l  详细信息显示
- ls -d  查看目录属性

```bash
ls -l
total 0
drwxr-xr-x 1 Rain Rain 4096 Jan 26 14:39 Rain
```

| drwxr-xr-x | 1 | Rain | Rain | 4096 | Jan 26 14:39 | Rain |
| ---------- | - | ---- | ---- | ---- | ------------ | ---- |
| 权限所属 | 引用计数(该文件被调用了几次) | u所有者 | g所属组 | 文件大小(默认以字节为单位) | 最后修改时间 | 文件名 |

- drwxr-xr-xr-
- 开头的 “d” 代表文件类型：“-”二进制文件，“d”目录(文件夹)，“l”软连接文件

| rwx- | xr- | xr- |
| --- | --- | --- |
| u所有者 | g所属组 | o其他人 |
| r读 | w写 | x执行 |

## 初始配置

## 更换数据源

- 在Ubuntu下我们可以通过 apt-get命令 很方便的安装/卸载软件，由于默认的软件包仓库是位于国外的，安装软件的时候就可能遇到各种网络问题或者下载到的一些资源不完整，因此就需要切换数据源为国内的镜像站点来改善。

```bash
# 1.备份原来的数据源配置文件
sudo cp /etc/apt/sources.list /etc/apt/sources.list_backup
# 2.编辑数据源配置文件
sudo vim /etc/apt/sources.list
#在(http://mirrors.ustc.edu.cn/)这里找到最新的数据源进行替换
#tip: vim中输入 :%d 快速删除所有
#或者直接替换字段
sudo sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
# 3.更新配置
sudo apt-get update
```

### 删除无用软件

```bash
sudo apt remove thunderbird totem rhythmbox empathy brasero simple-scan gnome-mahjongg aisleriot gnome-mines cheese transmission-common gnome-orca webbrowser-app gnome-sudoku landscape-client-ui-install onboard deja-dup libreoffice*

thunderbird         #自带邮件
totem               #自带视频播放器
rhythmbox           #音乐
empathy             #聊天软件
brasero             #光盘刻录工具
simple-scan         #扫描器
gnome-mahjongg      #游戏
aisleriot           #纸牌
gnome-mines         #扫雷
cheese              #相机
transmission-common #自带的bt下载客户端
gnome-orca          #屏幕阅读
webbrowser-app      #浏览器
gnome-sudoku        #数独
landscape-client-ui-install #管理服务
onboard             #屏幕键盘
deja-dup            #备份

sudo apt-get remove libreoffice* #自带office,WPS代替
sudo apt-get remove yelp #帮助
sudo apt-get remove blue* #蓝牙
sudo apt-get remove gnome-software #软件中心 apt够用
sudo apt-get remove unity #换gnome
sudo apt autoremove
```

### 安装必备软件

```bash
sudo apt-get install unrar #安装unrar程序
sudo apt-get install exfat-fuse #Ubuntu默认不支持exFat，需要手动安装exfat的支持
sudo apt-get install openssh-server #安装之后，就可以在Win下用ssh工具远程登陆了，当然也多了一个安全隐患，如果不想远程登陆本机的话，可以不装openssh-server
sudo apt-get install axel #多线程下载神器
sudo apt-get install apt-fast #用 axel 来加速 apt-get 软件安装的脚本,由于是多线程下载，所以加速效果还是很明显的。使用apt-fast 命令替代原apt-get 命令即可
sudo apt-get install git #git
sudo apt-get install vim #神器
sudo apt-get install pip #pip
sudo apt-get install shutter #截图
sudo apt-get install gnome-tweak-tool #配置主题
sudo apt-get install wps-office
sudo apt-get install gnome-mpv #视频播放
```

### 安装Arc GTK主题

```bash
sudo apt install unity-tweak-tool
sudo sh -c "echo 'deb http://download.opensuse.org/repositories/home:/Horst3180/xUbuntu_16.04/ /' >> /etc/apt/sources.list.d/arc-theme.list"

wget http://download.opensuse.org/repositories/home:Horst3180/xUbuntu_16.04/Release.key
sudo apt-key add - < Release.key
```

### 安装 MAC OS 主题

```bash
sudo apt-get install unity-tweak-tool

# 载并安装MacOS主题
sudo add-apt-repository ppa:noobslab/macbuntu
sudo apt-get update
sudo apt-get install macbuntu-os-icons-lts-v7
sudo apt-get install macbuntu-os-ithemes-lts-v7

# 安装 Slicecold 启动器（用于启动板）
sudo add-apt-repository ppa:noobslab/macbuntu
sudo apt-get update
sudo apt-get install slingscold

# 安装 Plank Dock 和主题
sudo apt-get install plank
# 打开 plank 按住 Ctrl + 右键可以设置

# 安装完 plank dock 后安装 macbuntu plank 主题
sudo add-apt-repository ppa：noobslab / macbuntu
sudo apt-get update
sudo apt-get install macbuntu-os-plank-theme-lts-v7

# 用面板上的“Mac”替换“Ubuntu桌面”文本
cd && wget -O Mac.po http://drive.noobslab.com/data/Mac/change-name-on-panel/mac.po
cd / usr / share / locale / en / LC_MESSAGES; sudo msgfmt -o unity.mo〜/ Mac.po; rm〜/ Mac.po; cd

# 修改启动界面：
sudo apt-get install macbuntu-os-bscreen-lts-v7
```

### 安装配置 Shadowsocks 服务端

```bash
sudo apt-get install python-gevent python-pip python-m2crypto python-wheel python-setuptools
sudo pip install shadowsocks

sudo vim /etc/shadowsocks.json
# 写入如下内容：注意替换my_server_ip和my_password
{
    "server":"my_server_ip",
    "server_port":8000,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"my_password",
    "timeout":300,
    "method":"rc4-md5"
}

# 下载libsodium：
wget https://github.com/jedisct1/libsodium/releases/download/1.0.10/libsodium-1.0.10.tar.gz
tar xf libsodium-1.0.10.tar.gz && cd libsodium-1.0.10
apt-get install build-essential
./configure && make && make install
ldconfig

# 创建配置文件（/etc/shadowsocks.json），写入如下内容：
{
    "server":"my_server_ip",
     "server_port":8000,
     "local_port":1080,
     "password":"my_password",
     "timeout":600,
     "method":"chacha20"
}

# 启动和停止
$ sudo ssserver -c /etc/shadowsocks.json -d start

$ sudo ssserver -c /etc/shadowsocks.json -d stop
```

## 安装 nginx

### Ubuntu

```bash
# 如果已经安装，请先卸载
sudo apt-get remove nginx
# 安装方法：
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:nginx/stable
sudo apt-get update
sudo apt-get install nginx
# 查看nginx 版本
nginx -v
```

### CentOS

```bash
# 添加CentOS 7 EPEL仓库
sudo yum install epel-release

# 现在Nginx存储库已经安装在您的服务器上，使用以下yum命令安装Nginx
sudo yum install nginx

# 启动Nginx
sudo systemctl start nginx

# 如果您正在运行防火墙，请运行以下命令以允许HTTP和HTTPS通信：
sudo firewall-cmd --permanent --zone=public --add-service=http 
sudo firewall-cmd --permanent --zone=public --add-service=https
sudo firewall-cmd --reload

# 在系统启动时启用Nginx
sudo systemctl enable nginx
```

### 安装好的文件位置：

- /usr/sbin/nginx：主程序
- /etc/nginx：存放配置文件
- /usr/share/nginx：存放静态文件
- /var/log/nginx：存放日志
- 如果要更清楚Nginx的配置项放在什么地方，可以打开/etc/nginx/nginx.conf

### 常用命令

- sudo service nginx {start|stop|restart|reload|force-reload|status|configtest|rotate|upgrade}

### 配置静态服务器访问路径

- 打开 Nginx 的默认配置文件 /etc/nginx/nginx.conf ，修改 Nginx 配置，将默认的 root /usr/share/nginx/html; 修改为: root /data/www;，如下：

```conf
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    include /etc/nginx/conf.d/*.conf;

    server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  _;
        root         /data/www;

        include /etc/nginx/default.d/*.conf;

        location / {
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }

}
```

- 配置文件将 /data/www/static 作为所有静态资源请求的根路径，如访问: http://119.29.250.68/static/index.js，将会去 /data/www/static/ 目录下去查找 index.js。现在我们需要重启 Nginx 让新的配置生效

> nginx -s reload

- 重启后，现在我们应该已经可以使用我们的静态服务器了，现在让我们新建一个静态文件，查看服务是否运行正常。
首先让我们在 /data 目录 下创建 www 目录
> mkdir -p /data/www
- 在 /data/www 目录下创建我们的第一个静态文件 index.html

## 安装 MySQL

### 添加MySQL APT存储库

- 访问http://dev.mysql.com/downloads/repo/apt/，转到MySQL APT存储库的下载页面
- 选择并下载适用于您的Linux发行版的发行包
- 使用以下命令安装下载的发行包，替换 version-specific-package-name 为下载的包的名称（如果未在包所在的文件夹内运行该命令，则以其路径开头）：
> sudo dpkg -i /PATH/version-specific-package-name.deb
- 例如，对于w.x.y-z软件包的版本 ，该命令是：
> sudo dpkg -i mysql-apt-config_w.x.y-z_all.deb
- 相同的软件包可在所有支持的Debian和Ubuntu平台上运行
- 在安装软件包的过程中，系统会要求您选择要安装的MySQL服务器和其他组件的版本
- 如果您不想安装特定的组件，您也可以选择none
- 使用以下命令从MySQL APT存储库更新包信息
> sudo apt-get update
- 查看从MySQL APT存储库安装的软件包的名称
> dpkg -l | grep mysql | grep ii

### 用 APT 安装 MySQL

> sudo apt-get install mysql-server
- 这将安装MySQL服务器的软件包，以及客户端和数据库通用文件的软件包
- 在安装过程中，系统会要求您为root用户提供MySQL安装密码
- 安装好之后会创建如下目录：
- 数据库目录：/var/lib/mysql/ 
- 配置文件：/usr/share/mysql（命令及配置文件） ，/etc/mysql（如：my.cnf）
- 相关命令：/usr/bin(mysqladmin mysqldump等命令) 和/usr/sbin

### 启动和停止 MySQL 服务器

- MySQL服务器在安装后自动启动。您可以使用以下命令：

```bash
# 检查MySQL服务器的状态
sudo service mysql status

# 停止MySQL服务器
sudo service mysql stop

# 重新启动MySQL服务器
sudo service mysql start
```

## 设置中文

- 运行
> sudo dpkg-reconfigure locales
- 第一个界面是选择区域，找到后面的 zh_CN.UTF-8 UTF-8 ,回车确认
- 第二个界面是选择语言，选择 zh_CN.UTF-8 ,回车确认
- 重启

## 远程连接

```bash
#首先看看自己的Ubuntu是不是已经安装或启用了ssh服务
ps -e |grep ssh
#如果没有安装执行
sudo apt install openssh-server
```

- ubuntu add-apt-repository command not found解决方法
- apt-get install software-properties-common