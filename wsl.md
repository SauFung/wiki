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
> 默认安装位置（`.vhdx`）：`$HOME/AppData/Local/Packages/xxx.<dist>.xxx/LocalState/rootfs`

### 安装到自定义位置

#### 所需工具
- `7z`：<https://7-zip.org/>
- `curl`

#### Microsoft Store 版本
- 获取安装包：`curl -L -o <name>.appx <url>`
- 下载目录：<https://learn.microsoft.com/zh-cn/windows/wsl/install-manual#downloading-distributions>
- 解压 `.appx` 文件：`7z *.appx -o<name>`
- 再解压 `_x64.appx` 文件：`7z x *_x64.appx -o<name>`
- 运行 `<dist>.exe` 文件

#### Arch WSL
- 下载`Arch.zip`：<https://github.com/yuk7/ArchWSL/releases/latest/>
- 解压`Arch.zip`：`7z x Arch.zip -oArch`
- 进入文件夹运行：`Arch.exe`

> ArchWSL：<https://wsldl-pg.github.io/ArchW-docs/How-to-Setup/>  
> 安装文件夹：运行 `.exe` 文件所在的目录即为安装目录




### Reference
- [WSL 手动安装](https://learn.microsoft.com/zh-cn/windows/wsl/install-manual)
- <https://learn.microsoft.com/zh-cn/windows/wsl/install>

