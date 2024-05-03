# WSL 使用手册

## 安装 WSL

### 安装所需组件
- `wsl --install --no-distribution`
- 重启 Windows

### 安装发行版
#### 安装到默认位置：
- `wsl -l -o`：查看可以发行版列表
- `wsl install -d <dist_name>`：安装发行版

> `wsl --install` 命令会默认安装 ubuntu 到默认位置  
> 确保当前网络能访问 githubusercontent.com  
> 这种安装方式适用于 Windows 10(2004+) 或 Windows11，版本号不满足则要手动安装
> 默认安装位置（`.vhdx`）：`$HOME/AppData/Local/Packages/xxx.<distro>.xxx/LocalState/rootfs`

### 安装到自定义位置

#### 所需工具
- `7z`：<https://7-zip.org/>
- `curl`

#### Microsoft Store 版本
- 获取安装包：`curl -L -o <name>.appx <url>`
- 下载目录：<https://learn.microsoft.com/zh-cn/windows/wsl/install-manual#downloading-distributions>
- 解压 `.appx` 文件：`7z *.appx -o<name>`
- 再解压 `_x64.appx` 文件：`7z x *_x64.appx -o<name>`
- 运行 `<Distro>.exe` 文件

#### Arch WSL
- 下载`Arch.zip`：<https://github.com/yuk7/ArchWSL/releases/latest/>
- 解压`Arch.zip`：`7z x Arch.zip -oArch`
- 进入文件夹运行：`Arch.exe`

> ArchWSL：<https://wsldl-pg.github.io/ArchW-docs/How-to-Setup/>  
> 安装文件夹：运行 `.exe` 文件所在的目录即为安装目录

#### Gentoo
- 下载 stage3 文件：
    - 下载地址：<https://distfiles.gentoo.org/releases/amd64/autobuilds/current-stage3-amd64-openrc/>
    - 下载 `.tar.xz` 文件
    - 可选，验证文件哈希值下载 `.tar.xz.sha256`
- 解压 `.xz` 文件：`7z x *.tar.xz`
- 导入 Gentoo：
    - 语法：`wsl --import <Distro> <InstallLocation> <FileName>.tar --version 2`
- 启动：`wsl -d <Distro>`（`<Distro>` 为 `Gentoo`）

## WSL 操作

### 重启 WSL
方法1：管理员权限运行 PowerShell 命令：
    - `Get-Service LxssManager | Restart-Service`
    - 或 `net stop lxssmanager && net start lxssmanager`
方法2：`wsl --shutdown` 再重新打开 WSL
重启指定发行版：`wsl -t <Distro>` 再重新打开发行版

- WSL 是 Windows 下的一个服务 LxssManager


### Reference
- [WSL 手动安装](https://learn.microsoft.com/zh-cn/windows/wsl/install-manual)
- <https://learn.microsoft.com/zh-cn/windows/wsl/install>


## ArchWSL 设置

### 设置用户
```bash
# 设置 root 密码
passwd

# 设置非 root 用户
# 设置 sudoers 文件，或使用 export EDITOR=nvim visudo
echo "%wheel ALL=(ALL) ALL" > /etc/sudoers.d/wheel
# 添加新用户
useradd -m -G wheel {username}
# 设置新用户密码
passwd {username}
# 设置默认用户
printf "[user]\ndefault={username}\n" >> /etc/wsl.conf
```
重启 WSL 发行版生效



### 设置软件源

设置密钥环
```bash
sudo pacman-key --init
sudo pacman-key --populate
sudo pacman -Syy archlinux-keyring
```

编辑文件：`/etc/pacman.conf`

```ini
# 开启 32 位支持
[multilib]
Include = /etc/pacman.d/mirrorlist

# 开启 archlinuxcn
[archlinuxcn]
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
```

在本地信任 farseerfc 的 GPG key：`pacman-key --lsign-key "farseerfc@archlinux.org"`

导入 GPG key：`pacman -Sy archlinuxcn-keyring`

更新系统：`pacman -Syyu`


## Gentoo 设置

### 查看硬件信息

查看是否支持 `x86-64-v3`：`ld.so --help`

查看 `CPU_FLAGS_X86` 参数：
- 安装：`emerge -av cpuid2cpuflags`
- 执行：`cpuid2cpuflags`

### 设置编译参数

编辑文件：`/etc/portage/make.conf`

```ini
# 通用选项
COMMON_FLAGS="-march=native -O2 -pipe"
# x86-64-v3 配置
# COMMON_FLAGS="-O2 -pipe -march=x86-64-v3 -mtune=native"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
FCFLAGS="${COMMON_FLAGS}"
FFLAGS="${COMMON_FLAGS}"
MAKEOPTS="-j4"
LC_MESSAGES=C.utf8
# 镜像
GENTOO_MIRRORS="https://mirrors.ustc.edu.cn/gentoo/"
ACCEPT_LICENSE="*"
L10N="zh-CN en-US"
EMERGE_DEFAULT_OPTS = "--job 4"
# 二进制配置
FEATURES="${FEATURES} getbinpkg binpkg-request-signature"
BINPKG_FORMAT="gpkg"
```

### 设置 Portage

新建 `repos.conf` 目录：`mkdir -p /etc/portage/repos.conf`  
编辑文件：`/etc/portage/repos.conf/gentoo.conf`

```ini
[DEFAULT]
main-repo = gentoo

[gentoo]
location = /var/db/repos/gentoo
sync-type = rsync
sync-uri = rsync://rsync.mirrors.ustc.edu.cn/gentoo-portage
auto-sync = yes
sync-rsync-verify-jobs = 1
sync-rsync-verify-metamanifest = yes
sync-rsync-verify-max-age = 3
sync-openpgp-key-path = /usr/share/openpgp-keys/gentoo-release.asc
sync-openpgp-keyserver = hkps://keys.gentoo.org
sync-openpgp-key-refresh-retry-count = 40
sync-openpgp-key-refresh-retry-overall-timeout = 1200
sync-openpgp-key-refresh-retry-delay-exp-base = 2
sync-openpgp-key-refresh-retry-delay-max = 60
sync-openpgp-key-refresh-retry-delay-mult = 4
sync-webrsync-verify-signature = yes
```

### 二进制配置

编辑文件：`/etc/portage/binrepos.conf/gentoobinhost.conf`

```ini
[binhost]
priority = 9999
sync-uri = https://mirrors.aliyun.com/gentoo/releases/amd64/binpackages/23.0/x86-64/
# option，支持 x86-64-v3 使用
# sync-uri = https://mirrors.aliyun.com/gentoo/releases/amd64/binpackages/23.0/x86-64-v3/
```

### 本地化

编辑文件：`/etc/locale.gen`，添加内容：
- `en_US.UTF-8 UTF-8`
- `zh_CN.UTF-8 UTF-8`

```bash
# 生成 locale
locale-gen
# 选择 locale
eselect locale list
eselect locale set <n>
```

### 更新软件源
```bash
# 获取 Gentoo ebuild 数据库，报错可跳过这一步
emerge-webrsync
# 更新 Portage ebuild 数据库
emerge --sync

# 选择配置文件
# 选择 default/linux/amd64/23.0 (stable)
eselect profile list
eselect profile set <n>

# 再次更新 Portage ebuild 数据库
emerge --sync

# 看新闻
eselect news list
eselect news read new
```

### 安装必要软件
```bash
# 更新系统软件包
emerge -uDNav @world
# 安装 sudo 工具
emerge -av sudo
# 安装 neovim 编辑器
emerge -av neovim
# 设置默认编辑器为 nvim
eselect editor set /usr/bin/nvim
# 使设置生效
. /etc/profile
# 查看设置是否生效
echo $EDITOR
# 编辑 /etc/sudoers 文件
# 取消注释：%wheel ALL=(ALL:ALL) ALL
visudo
# 修改 /etc/portage/make.conf 后使其生效
emerge --oneshot sys-apps/portage
```


### 设置用户
```bash
# 设置 root 密码
passwd
# 设置非 root 用户
useradd -m -G wheel username
passwd username
```

编辑文件：`/etc/wsl.conf`
```ini
# Openrc
[boot]
command = "/sbin/openrc default"

[user]
default = username

# 是否与 Windows 隔离，false 为隔离
[interop]
enabled = true
appendWindowsPath = false
```

重启子系统：
```powershell
# 关闭所有 WSL
wsl --shutdown
# 关闭单个子系统
wsl -t <Distro>
# 启动子系统
wsl -d <Distro>
```








