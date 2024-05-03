# 常用命令

## ls
命令：`ls <dir>`
作用：列出当前目录下所有文件和文件夹
- `-1`：一列显示所有目录
- `-a`：显示所有文件或文件夹，包括隐藏目录
- `-h`：方便人类阅读，单位变为（KiB，MiB，GiB）
- `-l`：以列表显示，权限、拥有权、大小、修改日期

## pwd
作用：显示当前绝对路径

## cd
命令：
```bash
# 进入指定目录
cd {dir}
# 返回上级目录
cd ..
# 返回上次目录
cd -
# 返回家目录
cd
# 进入指定用户家目录
cd ~{username}
# 进入根目录
cd root
```



## cp
命令：`cp <source> <target>`
作用：复制文件或目录
参数：
- `-r`：递归复制，复制文件目录
- `-v`：显示详细信息
- `-t`：复制多个文件到一个目录

## mv：
命令：`mv <source> <target>`
作用：重命名和移动文件或目录
参数：
- `-f`：强制移动
- `-i`：提示确认
- `-v`：显示详细信息
- 移动多个文件到一个目录时不用参数


## mkdir
命令：`mkdir <dir_name>`
作用：新建目录
参数：
- `-p`：创建的目录无父目录是使用
- `-m`：指定目录权限

## touch
命令：`touch <file_name>`
作用：创建新的空白文件

## rm
命令：`rm <file>/<dir>`
作用：删除文件或目录
参数：
- `-r`：递归删除，删除目录
- `-f`：强制删除
- `-v`：显示详细信息
- `-i`：交互模式


## wc
命令：`wc <file>`
作用：打印文件参数
参数：
- `-l`：打印行数
- `-c`：打印字节数
- `-m`：打印字符数
- `-w`：打印单词数量


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
- 用法同 `cp` 命令

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

### Access Github

#### Hosts
- <https://gitlab.com/ineo6/hosts/-/raw/master/hosts>
- <https://hosts.gitcdn.top/hosts.txt>
- <https://raw.githubusercontent.com/maxiaof/github-hosts/master/hosts>
- <https://raw.hellogithub.com/hosts>



#### Mirrors



## Modern Unix

### Fish


### 
