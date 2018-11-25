# ArchLinux

## Installation（安装）

### 检查引导方式

- 目前的引导方式主要分为EFI引导+GPT分区表与BIOS(LEGACY)引导+MBR分区表两种，几乎比较新的机器都采用了EFI/GPT引导的方式
- 输入：
> ls /sys/firmware/efi/efivars
- 如果提示:
> ls: cannot access '/sys/firmware/efi/efivars': No such file or directory
- 表明你是以BIOS方式引导，否则为以EFI方式引导

### 联网

- arch并不能离线安装，因为我们需要联网来下载需要的组件，所以我们首先要连接网络。
- 如果是有线网并且路由器支持 DHCP 的话插上网线后先执行以下命令获取 IP 地址：
> dhcpcd
- 如果是无线网，连接 WIFI:
> wifi-menu
- 检查
> ping -c 4 www.baidu.com

### 更新系统时间

> timedatectl set-ntp true

### 硬盘分区与格式化

- 查看目前的分区情况
> fdisk -l

#### 建立分区

##### 用 cfdisk 来分区

- 选择第一个（gpt）
- UEFI系统需要有 ESP 分区，建议将 ESP 挂载到 /boot，使用 UEFI 时，需要至少 512 MiB 空间
- 在 BIOS 系统上使用 GPT 进行分区后，安装 GRUB 时会需要一个额外的 BIOS 启动分区
- 如果是安装双系统则不需要分 EFI 分区（安装 Windows 时会有，只需要将其挂载在 boot 目录下）
- ArchLinux 不建议使用 swap 分区 推荐使用 swap 文件

#### 格式化分区

- 格式化主分区
> mkfs.ext4 /dev/sda1
- 格式化 EFI 分区
> mkfs.vfat /dev/sda2

#### 挂载分区

- 首先将根分区挂载到 /mnt
> mount /dev/sda1 /mnt
- 如果使用多个分区，还需要为其他分区创建目录并挂载它们（/mnt/boot、/mnt/home、……）
- 如果是 EFI/GPT 引导方式

```bash
mkdir /mnt/boot
mount /dev/sdxY /mnt/boot （将sdxY替换为之前创建或是已经存在的引导分区）
```

- BIOS/MBR引导方式无需进行这步。
- 接下来 genfstab 将会自动检测挂载的文件系统和 swap 分区

### 安装系统

#### 更换国内源

- 编辑下文件
> vim /etc/pacman.d/mirrorlist
- 将列表中的国内源放在最前面（优先使用）
- 按 “2dd” 剪切下两行，按 gg 回到文件首，按 p 粘贴
- :wp保存退出

#### 安装基本包

- 使用pacstrap 脚本，安装 base 组
> pacstrap /mnt base
- 这个组并没有包含全部 live 环境中的程序，有些需要额外安装
> pacstrap -i /mnt base base-devel
- 这个包中包含一些开发所需组件

#### 配置Fstab

- 用以下命令生成自动挂载分区的 fstab 文件 (用 -U 或 -L 选项设置UUID 或卷标)：
> genfstab -U /mnt >> /mnt/etc/fstab
- 在执行完以上命令后，后检查一下生成的文件是否正确
> cat /mnt/etc/fstab

#### Chroot

- Change root 到新安装的系统：
> arch-chroot /mnt
- 如果以后的系统出现了问题，只要插入U盘并启动， 将系统根分区挂载到了/mnt下（如果有efi分区也要挂载到/mnt/boot下），再通过这条命令就可以进入系统进行修复操作

#### 设置时区

> ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
- 运行 hwclock 以生成 /etc/adjtime
> hwclock --systohc

#### 设置 local

> nano /etc/locale.gen
- 取消下列两个的注释
- en_US.UTF-8 UTF-8
- zh_CN.UTF-8 UTF-8
- 接着执行locale-gen以生成locale讯息
> locale-gen
- 创建 locale.conf 并编辑：LANG 变量
> echo LANG=en_US.UTF-8 > /etc/locale.conf
- 不推荐在此设置任何中文locale，或导致tty乱码

#### 设置主机名

```bash
echo ArchLinux > /etc/hostname
nano /etc/hosts
# 输入
127.0.0.1	localhost
::1		    localhost
127.0.1.1	ArchLinux.localdomain	ArchLinux
```

### Root密码

> passwd

### 添加登录用户

- 这里添加wheel用户组是为了能够使用sudo提权
> useradd -m -g users -G wheel -s /bin/bash rain
- 设置密码
> passwd rain
- 安装 vim
> pacman -S vim
- 最后设置wheel组的用户能用sudo获取root权限:
> visudo
- 去掉 #%wheel ALL=(ALL) ALL 的注释后保存

### 网络配置

> pacman -S iw wpa_supplicant dialog networkmanager

### 安装Intel-ucode（非IntelCPU可以跳过此步骤）
> pacman -S intel-ucode

### 安装引导程序 Grub2

- 首先安装os-prober这个包，它可以配合Grub检测已经存在的系统，自动设置启动选项
> pacman -S os-prober
- 单系统可以不安

#### 如果为BIOS/MBR引导方式：

- 安装grub包：
> pacman -S grub
- 部署grub：
> grub-install --target=i386-pc /dev/sdx （将sdx换成安装的硬盘）
- 注意这里的sdx应该为硬盘（例如/dev/sda），而不是形如/dev/sda1这样的分区。
- 生成配置文件：
> grub-mkconfig -o /boot/grub/grub.cfg
- 如果报warning failed to connect to lvmetad，falling back to device scanning.错误
- 编辑/etc/lvm/lvm.conf这个文件，找到use_lvmetad = 1将1修改为0，保存，重新配置grub

#### 如果为EFI/GPT引导方式：


- 安装grub与efibootmgr两个包
> pacman -S grub efibootmgr
- 设置启动目录，并设置引导程序ID。挂载 ESP 分区，例如挂载到 /boot 或 /boot/efi。将下面命令中的 esp_mount 修改为挂载点(通常为 /boot)
> grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=grub
- 生成主配置文件
> grub-mkconfig -o /boot/grub/grub.cfg
- 如果报warning failed to connect to lvmetad，falling back to device scanning.错误
- 编辑/etc/lvm/lvm.conf这个文件，找到use_lvmetad = 1将1修改为0，保存，重新配置grub
- 如果报grub-probe: error: cannot find a GRUB drive for /dev/sdb1, check your device.map类似错误，并且sdb1这个地方是你的u盘，这是u盘uefi分区造成的错误，对我们的正常安装没有影响，可以不用理会这条错误

#### 检查

- 检查是否成功生成各系统的入口
> cat /boot/grub/grub.cfg
- 检查接近末尾的menuentry部分是否有windows或其他系统名入口
- 如果你没有看到Arch Linux系统入口或者该文件不存在，请先检查/boot目录是否正确部署linux内核：
> ls /boot
- 查看是否有initramfs-linux-fallback.img initramfs-linux.img intel-ucode.img vmlinuz-linux这几个文件
- 如果都没有，说明linux内核没有被正确部署，很有可能是/boot目录没有被正确挂载导致的，确认/boot目录无误后，可以重新部署linux内核：
> pacman -S linux
- 再重新生成配置文件，就可以找到系统入口
- 目前处的U盘安装环境下有可能无法检测到其他系统的入口，请在下一步中重启登陆之后重新运行:
> grub-mkconfig -o /boot/grub/grub.cfg

### 重启

> systemctl enable dhcpcd
- 退出 chroot 环境
> exit
- 手动卸载被挂载的分区
> umount -R /mnt
- 重启
> reboot

### 联网

- 如果你是有线网并且路由器支持DHCP的话插上网线后先执行以下命令获取IP地址：
> dhcpcd
- 无线网
> wifi-menu
- 或者
> systemctl enable dhcpcd
- 查看网卡
> ip link
- 使用 ens32
> ip link set ens32 up
- 重启

### 创建交换文件

- 之前通常采用单独一个分区的方式作为交换分区，现在更推荐采用交换文件的方式，更便于管理
- 分配一块空间用于交换文件：
> fallocate -l 2G /swapfile （请将 2G 换成需要的大小，只能以M或G为单位）
- 交换文件的大小可以自己决定，推荐4G以下的物理内存，交换文件与物理内存一致，4G以上的物理内存，交换文件4-8G
- 更改权限
> chmod 600 /swapfile
- 设置交换文件
> mkswap /swapfile
- 启用交换文件
> swapon /swapfile
- 最后需要编辑 /etc/fstab 为交换文件设置一个入口
> nano /etc/fstab
- 直接在最后新起一行加入以下内容：
> /swapfile none swap defaults 0 0

#### 删除交换文件

- 如果要删除一个交换文件，必须先停用它
> swapoff -a
- 然后即可删除它
> rm -rf /swapfile
- 最后从 /etc/fstab 中删除相关条目

### 提前配置网络

- 之前使用的一直都是netctl这个自带的网络服务，而桌面环境使用的是NetworkManager这个网络服务，所以我们需要禁用netctl并启用NetworkManager：
```bash
sudo systemctl disable netctl
sudo systemctl enable NetworkManager （注意大小写）
```

### 安装桌面环境

#### 安装驱动

- 输入下面命令，查看所有显卡开源驱动
> pacman -Ss xf86-video
- 安装对应的驱动
- Nvidia的独显驱动如非必要，建议只装集成显卡的驱动（省电，如果同时装也会默认使用集成显卡），不容易出现冲突问题
> pacman -S nvidia
- 触摸板驱动
> pacman -S xf86-input-synaptics

#### 安装 Xorg

- Xorg 是 Linux 下的一个著名的开源图形服务，有的桌面环境需要 Xorg 的支持
> sudo pacman -S xorg

#### Xfce

> sudo pacman -S xfce4 xfce4-goodies

#### KDE(Plasma)

> sudo pacman -S plasma kde-applications kde-l10n-zh_cn

#### 安装桌面管理器

- 安装好了桌面环境包以后，我们需要安装一个图形化的桌面管理器来帮助我们登录并且选择我们使用的桌面环境，推荐使用 sddm
> sudo pacman -S sddm
- 设置开机启动sddm服务
> sudo systemctl enable sddm

#### Deepin

##### 安装

> pacman -S deepin deepin-extra

##### 启动Deepin 桌面环境

- 使用登录管理器
- deepin默认lightdm greeter是lightdm-deepin-greeter，可通过pacman安装
- 安装后需编辑/etc/lightdm/lightdm.conf，修改：
> greeter-session=lightdm-deepin-greeter
- 启动
> systemctl enable lightdm.service

##### 添加源

> sudo vim /etc/pacman.conf 
- 首先去掉multilib中两行的注释
- 在文档结尾处加入下面的文字：
```conf
[archlinuxcn]    
SigLevel = Optional TrustAll
Server = http://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
# 这是清华的，也可用下面中科大的
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
```

- 保存退出，刷新 pacman 数据库
> sudo pacman -Syy

##### 安装常用软件

- 中文字体
> sudo pacman -S adobe-source-han-serif-cn-fonts
- 声卡
> sudo pacman -S alsa-utils
- 命令行补全
> sudo pacman -S bash-completion
- chrome
> sudo pacman -S google-chrome
- 网易云音乐
> sudo pacman -S netease-cloud-music
- 搜狗输入法
> sudo pacman -S fcitx-sogoupinyin fcitx-configtool
- 配置输入法
> vim .xprofile
- 输入
```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
```
- 保存、重启
- WPS
> sudo pacman -S wps-office ttf-wps-fonts