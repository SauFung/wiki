# Windows Package Manager

## PowerShell 脚本执行策略
列出执行策略：`Get-ExecutionPolicy -List`  
设置当前用户的执行策略：`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`

> 安装 scoop 确保 `CurrentUser` 的执行策略为 `RemoteSigned`

## 安装
默认安装：`irm get.scoop.sh | iex`
默认安装位置为：`$HOME\scoop`

> Windows 下的 `$HOME` 为 `C:\Users\<UserName>`

### Reference
- <https://github.com/ScoopInstaller/Install#readme>


## Scoop 加速
```
# 更换 repo 地址
scoop config SCOOP_REPO "https://gitee.com/glsnames/scoop-installer"
# scoop-proxy-cn 镜像
scoop bucket add spc https://ghproxy.org/https://github.com/lzwme/scoop-proxy-cn
# scoop-cn 镜像
scoop bucket add scoop-cn https://ghproxy.org/https://github.com/duzyn/scoop-cn
# 拉取新库地址
scoop update

# 列出已设置的仓库
scoop bucket list
# 显示 scoop 帮助
scoop help
# 列出已安装 App
scoop list
```

## 推荐应用
- `7zip`：命令行解压
- `aria2`：scoop 多线程下载支持
- `psutils`：提供 `sudo` 功能
- `scoop-completion`：scoop 命令补全
- `scoop-search`：scoop 搜索软件
