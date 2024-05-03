# Lua CheatSheet

## 环境安装

### 编译安装
源码下载：<https://www.lua.org/ftp/>

```
# 解压
tar vxf *.tar.gz
# 进入源码目录
cd lua-*
# Windows mingw 编译
mingw32-make mingw
# Linux 编译
make linux
# Linux 安装
make install
```

### Windows 安装
将下列文件放入同一文件夹并加入 PATH 环境变量
- `lua.exe`
- `luac.exe`
- `lua54.dll`


### 二进制安装
下载地址：<https://luabinaries.sourceforge.net/download.html>


## Basic

### 语句
```lua
-- 循环语句

-- while
while condition do
    statements
end

-- repeat-until
repeat
    statements
until condition

-- for
-- i=1 为初始化，即初始化变量
-- 10 为最大或最小值
-- 2 为迭代增量
for i=1,10,2 do
    statements
end


-- 条件语句

-- if
if condition then
    statements
end

-- if-else
if condition1 then
    statements1
else
    statements2
end

-- if-elseif-else
if condition1 then
    statements1
elseif condition2 then
    statements2
    xxx
else
    statements3
end

-- 函数
function name(vars)
    statements
end
-- 或
name = function(vars)
    statements
end
```

### 运算符

#### 算术运算符
`+`：加法
`-`：减法
`*`：乘法
`/`：除法
`%`：取余
`^`：幂运算
`//`：整除

#### 关系运算符
`==`：等于
`~=`：不等于
`>`：大于
`<`：小于
`>=`：大于等于
`<=`：小于等于

#### 逻辑运算符
`and`：与
`or`：或
`not`：非

#### 其他
`-`：负号
`..`：连接字符串
`#`：返回表或字符串的长度

### 字符串

#### 定义
单行字符串：单引号（`'`）或双引号（`"`）包围
多行字符串：`[[]]` 包围
> 字符串中有单引号则可用双引号包围  
> 字符串中有双引号则可用单引号包围
