# Shell Cheatsheet

## Bash

### Basic

Shell 提示符：
- bash：`$`
- zsh：`%`
- 其他信息：`<username>@<hostname>`

修改默认 shell 程序：
- `Bash`：`chsh -s /bin/bash`
- `Zsh`：`chsh -s /bin/zsh`
- `Fish`：`chsh -s /bin/fish`
> 程序路径为绝对路径，以 `/etc/shells` 文件的内容为准，否则设置失败


#### 后台程序

进程睡眠：`sleep <n>`，单位秒

后台进程：命令后 + `&`

查看后台作业：`jobs`

协程：在后台生成子 shell，并在子 shell 中执行命令
- 命令：`coproc + <command>`

#### 命令

外部命令（非内建命令）：
- 存储在
    - `/bin`：系统启动时的基本命令
    - `/usr/bin`：系统常用命令
    - `/usr/local/bin`：自行编译的程序
    - `/sbin`：管理员使用的系统管理程序，需要 root 权限
    - `/usr/sbin`
    - `/usr/local/sbin`
    - `~/.local/bin`（用户程序）

查看命令所处目录：
- `type -a <command>`
- `which -a <command>`
- `command -v <command>`：判断命令是否为别名或输出命令所在目录
    - `command -v <command> &> /dev/null` 可判断命令是否存在

查看命令类型：
- bash：`type -t <command>`
- zsh：`whence -w <command>`


搜索包与命令：
- Gentoo：
    - 搜索命令所在包：`equery belongs <command>`（需要安装 `gentoolkit`）
    - 搜索包名：`eix <package>`（代替 `emerge -S`，需要安装 `eix`）
- Arch：
    - 搜索命令所在包：`pacman -F <command>`
    - 搜索包名：`pacman -Ss <package>`
- Debian：
    - 搜索命令所在包：`apt-file search <command>`
    - 搜索包名：`apt search <package>`


衍生（forking）：执行外部命令时会创建一个子进程

内建命令：
- 与外部命令的区别：内建命令不需要使用子进程来执行
- 内建命令执行速度快，效率高


#### 环境变量
查看全局环境变量：`printenv` / `env`

查看指定环境变量：`printenv <env>` / `echo $<env>`

查看所有环境变量（包括局部、全局、用户自定义）：`set`

> 系统环境变量通常使用大写字母，自定义变量和脚本中使用小写字母

定义变量：`<var>=<value>`，不能有空格


引用变量：`$<var>`

在字符串中引用环境变量：
- `"$<var>"`（双引号包围）：引用 var 的值
- `'$<var>'`（单引号包围）：不引用 var 的值，直接是 `$<var>`

设置全局环境变量：`export <var>=<value>`


删除变量：`unset <var>`

常见环境变量：
- `TERM`：当前当前终端环境
- `EDITOR`：默认编辑器
- `NAME`：主机名
- `USER`：当前用户名
- `SHELL`：当前 shell
- `PWD`：当前目录
- `PATH`：程序路径
    - 添加 `PATH` 项：`export PATH="$PATH:/path/to/bin`

#### 语句结构
循环
```bash
# ----- for 语句 -----
# 操作列表
for var in list
do
    commands
done
# 操作多值
for var in item1 item2 ... itemN
do
    commands
done
# 迭代，类似 C 语言
for (( i=1; i<=5; i++))
do
    commands
done

# ----- while 语句 ------

```


## PowerShell
