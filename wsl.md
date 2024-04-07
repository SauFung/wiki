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
- 启动：`wsl -d Gentoo`（`<Distro>` 为 `Gentoo`）

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

### 用户设置
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

### 设置密钥环
```bash
sudo pacman-key --init
sudo pacman-key --populate
sudo pacman -Syy archlinux-keyring
```

## Gentoo 设置
```bash
# 设置 root 密码
passwd
# 设置非 root 用户
useradd -m -G wheel {username}
passwd {username}
printf "[user]\ndefault={username}\n" >> /etc/wsl.conf
```
- 使用 root 用户启动：`wsl -u root -d Gentoo`

设置 `/etc/wsl.conf`：
```ini
# Openrc
[boot]
command = "/sbin/openrc default"

# Systemd
[boot]
systemd=true

# 是否与 Windows 隔离，false 为隔离
[interop]
enabled = true
appendWindowsPath = true
```


