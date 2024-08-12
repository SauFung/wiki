# Linux cheatsheet

## Little Tips

查看 Linux 发行版名（无需安装任何软件）：`cat /etc/os-release | grep -w NAME`
查看 CPU 支持的架构版本：`ld.so --help`
    - 对应位置显示 `supported` 则支持
进入 tty ：`Ctrl + Alt + F1 ~ F7`

## 无图形界面安装 Linux

### 远程访问

#### 开启 SSH
编辑 `/etc/ssh/sshd_config`
- 确保开启：
    - `PasswordAuthentication yes`
    - `PermitRootLogin yes`
```bash
# 修改 live 系统的 root 密码
passwd
# 开启 sshd 服务
# Systemd
systemctl start sshd
# OpenRC
rc-server sshd start
# 查看当前 IP
ip address
```

#### 访问远程主机
远程主机为虚拟机：
- 配置端口转发，虚拟机 22 端口转发到宿主机任意端口（可不同配置 IP）
- 访问：`ssh root@127.0.0.1 -p <port>`

远程主机为物理机：`ssh root@<ip>`


### 分区与挂载






## Arch Linux



## Gentoo Linux

### 配置

#### Portage 镜像
`/etc/portage/repos.conf/gentoo.conf`：
```
[DEFAULT]
main-repo = gentoo

[gentoo]
location = /usr/portage
sync-type = rsync
sync-uri = rsync://rsync.mirrors.ustc.edu.cn/gentoo-portage
auto-sync = yes
```

#### 编译配置
`/etc/portage/make.conf`：
```
# 设置编译标志
COMMON_FLAGS="-march=native -O2 -pipe"
# x86-64-v3 配置
COMMON_FLAGS="-O2 -pipe -march=x86-64-v3 -mtune=native"
# 为两个变量使用相同的设置
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
# 指定编译器执行任务数量
MAKEOPTS="-j4"
ACCEPT_KEYWORDS="~amd64"
CPU_FLAGS_X86 = ""

# 二进制配置
# getbinpkg 自动下载二进制包特性，相当于 EMERGE_DEFAULT_OPTS = "--getbinpkg"
# binpkg-request-signature 二进制包签名特性，无签名会拒绝安装
FEATURES="${FEATURES} getbinpkg binpkg-request-signature"
# 指定二进制包格式
BINPKG_FORMAT="gpkg"

# 配置 emerge
# --job 3 表示 3 个包并行构建
EMERGE_DEFAULT_OPTS = "--job 3"
# 配置 Gentoo 镜像
GENTOO_MIRRORS="https://mirrors.ustc.edu.cn/gentoo/"
# 允许所有软件
ACCEPT_LICENSE="*"
# 或，仅允许自由软件
# ACCEPT_LICENSE="-* @FREE"

# 本地化
L10N="zh-CN en-US"

# 添加软件支持
# USE="kde"
# 移除
# USE="-kde"
# 常用
USE = "qt5"


# 显卡驱动，不用图形界面可不设置
VIDEO_CARDS="virgl"
# grub 引导，不用 grub 引导不用设置
GRUB_PLATFORMS="efi-64
```

#### 二进制包配置
`/etc/portage/binrepos.conf/gentoobinhost.conf`：
```
[binhost]
priority = 9999
sync-uri = https://mirrors.aliyun.com/gentoo/releases/amd64/binpackages/17.1/x86-64/
# 可选，二选一
# sync-uri = https://mirrors.aliyun.com/gentoo/releases/amd64/binpackages/17.1/x86-64-v3/
```




## 快捷键
命令行输入模式：
- Emacs（默认）
- Vim
说明： `<C-x>` = `Ctrl + x`，`<A-x>` = `Alt + x`，`<S-x>` = `Shift + x`

### 光标移动
``

### 其他
`<C-c>`：取消操作
`<C-d>`：注销
`<C-l>`：清屏
`<C-z>`：后台运行，`fg` 恢复
`<Tab>`：补全命令



## 变量

查看变量的值：
- `echo $<var>`
- 例如，`echo $PATH`




### 常用环境变量
- `$SHELL`：默认 Shell 执程序
- `$PATH`：可执行程序
- `$HOME`：当前用户家目录
- `$TERM`：当前终端类型
    - 一般为 `xterm-256color`，支持 256 位颜色
- `$EDITOR`：默认编辑器







## 管理系统
修改主机名：`sudo echo "<hostname>" > /etc/hostname`
- systemd：`sudo hostnamectl set-hostname <hostname>`
- 查看主机名：`hostnamectl`
修改 hosts 文件：
- 文件：`/etc/hosts`
- 通用设置
```
127.0.0.1   localhost
127.0.0.1   <hostname>
```
- Arch Linux:
```
127.0.0.1   localhost
::1         localhost
127.0.0.1   <hostname>.localdomain  <hostname>
```


关闭休眠：`sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target`


## 其他

查找指定主机 IP：`ping -4 <hostname>`
