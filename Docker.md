# Docker

## 基本架构

- Docker 是一个构建、发布、运行分布式应用的平台，Docker 平台由 Docker Engine（运行环境 + 打包工具）、Docker Hub（API + 生态系统）组成
- Docker 的底层是各种 OS 以及云计算基础设施，而上层是各种应用程序和管理工具，每层之间都是通过 API 来通信
- Docker 将应用程序与该程序的依赖，打包在一个文件里面。运行这个文件，就会生成一个虚拟容器。程序在这个虚拟容器里运行，就好像在真实的物理机上运行一样

### Docker Client

- Docker 引擎可以理解为 Docker 程序，实际上是一个 C/S 结构的软件，有一个后台守护进程在运行，每次运行 docker 命令的时候实际上都是通过 RESTful Remote API 来和守护进程进行交互的

### Docker daemon

- daemon 是一个守护进程，它实际上就是驱动整个 Docker 的核心引擎

### Docker 镜像

- Docker 镜像是 Docker 系统中的构建模块（Build Componment），是启动一个 Docker 容器的基础
- Docker 镜像采用分层的结构构建，最底层是 bootfs，这是一个引导文件系统，一般很少会直接与其交互，在容器启动之后会自动卸载 bootfs
- bootfs 之上是 rootfs 是 Docker 容器在启动时内部可见的文件系统，就是 “/” 目录
- Docker 镜像使用到了联合挂载和写时复制，可以只在文件系统发生变化时才会把文件写道可读写层，一层层叠加，有利于版本管理和储存管理

### Docker 容器

- 在 Docker 中，容器是一个核心，容器是一个基于 Docker 镜像创建，包含为了运行某一特定程序所有需要的 OS、软件、配置文件和数据，是一个可移植的运行单元
- 在宿主机看来，它只是一个简单的用户进程

## 安装

- [列表](https://store.docker.com/search?type=edition&offering=community)
- [Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows)
- [Mac](https://store.docker.com/editions/community/docker-ce-desktop-mac)
- [Ubuntu](https://store.docker.com/editions/community/docker-ce-server-ubuntu)
- [CentOS](https://store.docker.com/editions/community/docker-ce-server-centos)

### Ubuntu

- 卸载旧版本
> sudo apt-get remove docker docker-engine docker.io

#### 安装 Docker CE

##### 使用镜像仓库进行安装

- 首次在新的主机上安装 Docker CE 之前，需要设置 Docker 镜像仓库。然后，就可以从此镜像仓库安装和更新 Docker

```bash
# 更新 apt 软件包索引：
sudo apt-get update

# 安装软件包，以允许 apt 通过 HTTPS 使用镜像仓库：
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common

# 添加 Docker 的官方 GPG 密钥：
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# 验证密钥指纹是否为 9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88。
$ sudo apt-key fingerprint 0EBFCD88

pub 4096R/0EBFCD88 2017-02-22
    9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid Docker Release (CE deb) <docker@docker.com>
sub 4096R/F273FCD8 2017-02-22

# 设置 stable 镜像仓库
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"

# 安装 Docker CE
# 更新 apt 软件包索引
sudo apt-get update

#安装最新版本的 Docker CE，或者转至下一步以安装特定版本。将替换任何现有的 Docker 安装版本。
sudo apt-get install docker-ce
```

#### 卸载 Docker CE

```bash
# 卸载 Docker CE 软件包：
sudo apt-get purge docker-ce

# 主机上的镜像、容器、存储卷、或定制配置文件不会自动删除。如需删除所有镜像、容器和存储卷，请运行下列命令：
sudo rm -rf /var/lib/docker
```

- 必须手动删除任何已编辑的配置文件

### Linux 的安装后步骤

#### 以非 root 用户身份管理 Docker

- docker 守护进程绑定至 Unix 套接字，而不是 TCP 端口。默认情况下，该 Unix 套接字由用户 root 所有，而其他用户只能使用 sudo 访问它。docker 守护进程始终以 root 用户身份运行
- 在使用 docker 命令时，如果您不想使用 sudo，请创建名为 docker 的 Unix 组并向其中添加用户。docker 守护进程启动时，它将使 Unix 套接字的所有权可由 docker 组进行读取/写入
- 如需创建 docker 组并添加您的用户

```bash
# 创建 docker 组
sudo groupadd docker

# 向 docker 组中添加用户。
sudo usermod -aG docker $USER

# 注销并重新登录，以便对您的组成员资格进行重新评估。
# 如果在虚拟机上进行测试，可能必须重启此虚拟机才能使更改生效。
# 在桌面 Linux 环境（例如，X Windows）中，彻底从您的会话中注销，然后重新登录。
# 验证是否可以在不使用 sudo 的情况下运行 docker 命令。
docker run hello-world
```

#### 将 Docker 配置为在启动时启动

- 大多数最新的 Linux 分发版（RHEL、CentOS、Fedora、Ubuntu 16.04 及更高版本）都使用 systemd 来管理在系统启动时启动的服务
> sudo systemctl enable docker
- 如需禁用此性能
> sudo systemctl disable docker
- 如果您需要添加 HTTP 代理，请为 Docker 运行时文件设置另一个目录或分区

## Docker 基本操作

### docker attach

- docker attach 作用是进入容器内部，可以查看容器内部的持续输出，或以交互方式控制容器
- docker attach 主要功能是查看信息，容器内部操作一般使用更方便的 docker exec 命令

### docker build

- docker build 是构建镜像的命令，常用的命令参数有：
- -c：控制 CPU 使用
- -f：选择 Dockerfile 名称
- -m：设置构建内存上限
- -q：不显示构建过程的一些信息
- -t：为构建的镜像打上标签

### docker commit

- docker commit 命令的功能是把当前容器提交打包为镜像，主要参数：
- -a：添加作者信息
- -c：修改 Dockerfile 指令
- -m：类似 git commit -m，提交修改信息
- -p：暂停正在 commit 的操作
- 但一般情况下，更推荐使用 Dockerfile 构建镜像

### docker cp

- docker cp 用于从容器内部复制文件到宿主机
> docker cp ubuntu:/home/test.txt ~/test.txt

### docker create

- Docker 容器状态中有一种是 Created，表示容器已经创建，但没有启动
- 和 Stop 不同，Stop 通常是手动或外部操作容器停止的，Create 是手动创建但是没有成功启动
- 例如两个容器都要使用 80 端口，第二个容器创建时就会启动失败，显示 Create 状态
- Create 状态的容器不占用内存和 CPU 资源
- 通常使用 docker create 命令为容器启动做前置准备

### docker diff

- docker diff 可以列出容器内发生变化的文件和目录
- 变化包括添加（A-add）、删除（D-delete）、修改（C-change）
> docker diff containerID
- docker diff 的运行与容器的状态无关，只显示文件差异

### docker events

- docker events 命令实时输出 Docker 服务器端的事件
- docker events 涵盖了几乎全部 Docker 事件，通过 -f 指定参数，可以过滤不必要的事件
> docker events -f container=<name or id>
- 通过指定容器 ID 可以过滤和容器相关的事件：attach，commit，copy，create，destroy，detach，die，exec_create，exec_detach，exec_start，export，kill，oom，pause，rename，resize，restart，start，stop，top，unpause，update
> docker events -f image=<tag or id>
- 通过指定镜像 ID 可以过滤和镜像相关的事件：delete，import，load，pull，push，save，tag，untag
> docker events -f volume=<mane or id>
- 通过指定 volume ID 可以过滤和 volume 相关的事件：create，mount，unmount，destory
> docker events -f network=<name or id>
- 通过指定网络 ID 可以过滤和网路相关的事件：create，connect，disconnect，destroy
> docker events -f daemon=reload
- 用于监控记录 Docker 守护进程的状态

### docker exec

- 通过 docker exec 进入服务器可以像使用 SSH 登陆一样操作服务器
- docker exec 的常用参数有：
- -d 分离模式：在后台运行的命令
- -i 交互模式
- -t 分配一个 TTY
- -u 指定用户和用户组，格式：<name|id>[:<group|gid>]