# 常用命令

## ssh
- 连接远程主机
- `~/.ssh`：存放密钥和配置文件

### 命令
```bash
# 使用主机名或 IP 访问远程主机
ssh <user>@<host>
# 使用用户名访问主机，需要设置
ssh <name>
# 执行远程命令
ssh <host> "<command>"
```
- `<host>` 为 `<user>@<ip>` 或 `<user>@<host_name>`


### 参数
- `-p <port>`：指定端口
- `-i ~/.ssh/<private_key>`：指定验证私钥
- `-v`：启用调试，显示连接信息，验证私钥等
- `-vv`：启用更加详细的调试

### 配置
- 配置文件：`~/.ssh/config`

#### 配置示例
```bash
# <name> 任意
# <host> 远程主机名或 IP
# <user> 远程用户
Host <name>
    HostName <host>
    User <user>
    Port 22
    # IdentityFile ~/.ssh/<private_key>
    IdentityFile ~/.ssh/id_ed25519

```

###  上传公钥
- 使用公钥访问更安全，并且可实现免密登陆
```bash
# Linux 适用
ssh-copy-id <host>
# 上传指定公钥到远程主机
ssh-copy-id -i ~/.ssh/<public_key> <host>

# 通用
cat ~/.ssh/<public_key> | ssh <host> "mkdir ~/.ssh && cat >> ~/.ssh/authorized_keys"

# 手动上传（通用）
# 确保远程主机有 .ssh 文件夹
ssh <host> "mkdir ~/.ssh"
scp ~/.ssh/<public_key> ~/
ssh <host>
cat ~/<public_key> >> ~/.ssh/authorized_keys
rm ~/<public_key>
```

### 注意事项
- 命令行中，Linux 下的家目录为 `~`，Windows 下的家目录为 `$HOME`
- `.pub` 文件为公钥，无后缀为私钥，上传时注意


## ssh-keygen
- 创建 ssh 密钥对
### 参数
- `-t ed25519/rsa`：指定加密算法
- `-C "<string>"`：备注
- `-f ~/.ssh/<key_name>`：指定写入文件

## scp
- 使用 SSH 复制文件

### 命令
```bash
# <host> 可以是 SSH 配置文件中设置的 name，也可以 <user>@<ip> 或 <user>@<host_name>
# 复制远程文件到本地
scp <host>:remote_file local_file
# 复制本地文件到远程主机
scp local_file <host>:remote_file
```

### 参数
- `-r`：复制文件夹
- `-i ~/.ssh/<private_key>`：指定验证私钥
- `-P <port>`：指定端口号


## git

### 配置文件
* 设置编辑器：`git config --global core.editor nvim`
* 打开配置文件（全局）：`git config --global -e`

```bash
# 常用配置
[core]
    # 指定 ssh 验证私钥和端口
    sshCommand = ssh -i $HOME/.ssh/<private_key> -p <port>
```
