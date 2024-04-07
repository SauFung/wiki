# C cheatsheet



## C Basics

### Hello World
```c
#include <stdio.h>

int main(){
    printf("Hello World!\n");
    return 0;
}
```

### 标识符

#### 标识符命名规则
组成：
- 数字
- 下划线
- 大小写字母
- \u 或 \U 转义的 Unicode 字符（c99）
限制：
- 以非数字字符开头
- 严格区分大小写


#### 标识符作用
定义名字
- 变量名
- 函数名
- 结构体名及结构体成员名
- 联合体名及联合体成员名
- 数据类型别名
- 宏名
- 形式参数名

### 数据类型
`void`：空类型
`char`：字符型
`short`：短整型
`int`：整型
`long`：长整型
`long long`：更长的整型
`float`：单精度浮点型
`double`：双精度浮点型

#### 其他
`unsigned`：无符号前缀
`sizeof()`：
- 用于数据类型：返回数据类型大小
- 用于变量：返回变量所占内存大小（即变量的数据类型大小）
- 用于字符串：返回字符串长度，包括 `\0`
- 用于数组：返回数组总长度（类型 $\times$ 元素个数）
    - 返回数组长度：`sizeof(a)/sizeof(a[0])`
- 用于指针：返回指针大小
    - 32 位为 4 byte
    - 64 位为 8 byte


### 语句

#### 条件语句
```c
// if
if(<condition>){
    <statement>;
}

// if-else
if(<condition>){
    <statement>;
}else{
    <statement>;
}

// 可存在多个 elseif
// if-elseif-else
if(<condition>){
    <statement>;
}elseif(<condition>){
    <statement>;
}else{
    <statement>;
}

// switch
switch(<condition>){
    case <value1>: <statement>;
    case <value2>: <statement>;
    ...
    default: <statement>;
}
// switch 一般配合 break 使用
switch(<expression>){
    case <value1>: <statement>; break;
    case <value2>: <statement>; break;
    ...
    default: <statement>;
}
```

#### 循环语句
```c
// for
for(<init>;<condition>;<action>){
    <statement>;
}

// while
while(<condition>){
    <statement>;
}

// do-while
do{
    <statement>;
}while(<condition>)
```

### 其他语句
```c


```


## Misc

### 编译
C 语言编译流程
1. 预处理（Pre-processing）：
- `gcc -E main.c`
- 结果：产生 `main.i` 文件
- 对源码处理：
    - 删除注释
    - 替换宏
    - 插入头文件内容
    - 条件编译
2. 编译（Compiling）：
- `gcc -S main.c`
- 结果：产生 `main.s` 文件，汇编源码
3. 汇编（Assembling）：
- `gcc -c main.c`
- 结果：产生 `main.o` 文件，二进制文件
4. 链接（Linking）：
- `gcc main.c -o main.out`（Linux）
- `gcc main.c -o main.exe`（Windows）
- 结果：产生最终可执行文件

#### 编译选项
命令：`gcc`
选项：
- `-std=c99`：标准版本，还有c11等
- `-m32`/`-m64`：编译 32/64 位程序，默认为 64 位
- `-Wall`：显示所有警告

### C 常用标准库
`<math.h>`：数学常用库  
`<string.h>`：字符串处理库  
`<stdlib.h>`：通用工具库，如随机数  
`<time.h>`：时间相关库  

### 预处理指令
#### 宏定义
格式：`#define <str1> <str2>`
示例：
- 定义：`#define PI 3.1415926`
- 取消宏定义：`#undef PI`
- PI 代表 3.1415926



#### 头文件
防止头文件重复定义
```c
// 仅新编译器支持
#pragma once
// 或
// 通用，XXX 为头文件名，全为大写
#ifndef XXX_H_
#define XXX_H_
    <statement>;
#endif
```


## Reference
- [C 语言教程](https://wangdoc.com/clang/)
- [learn-c.org](https://learn-c.org/)
- [tutorialspoint](https://www.tutorialspoint.com/cprogramming/index.htm)
- [geeksforgeeks](https://www.geeksforgeeks.org/c-programming-language/)
- [菜鸟教程](https://www.runoob.com/cprogramming/c-tutorial.html)
