# MCU Learning

## 使用 CMake + Ninja + OpenOCD 开发 STM32


### 依赖

#### 下载
- `gcc-arm-none-eabi`：<https://developer.arm.com/downloads/-/gnu-rm>
- `MinGW`：<https://winlibs.com>
- `CMake`：winlibs 已包含
- `Ninja`：winlibs 已包含
- `Stm32CubeMX`
- `OpenOCD`

#### 检查环境变量
- `MinGW`：`gcc -v`
- `gcc-arm-none-eabi`：`arm-none-eabi-gcc -v`
- `CMake`：`cmake --version`
- `Ninja`：`ninja --version`

#### 编译步骤
1. 使用 Stm32CubeMX 生成 CMake 工具链，进入工程目录打开终端（PowerShell）
2. `cmake -B build -G "Ninja"` 命令生成 Ninja 配置文件
3. `ninja -C build` 命令生成 ELF 文件
4. 生成二进制文件：
    - `arm-none-eabi-objcopy ./build/*.elf -O ihex xxx.hex`
    - `arm-none-eabi-objcopy ./build/*.elf -O binary xxx.bin`

#### 烧录

OpenOCD 配置文件
```
# stm32f103c8t6.cfg
# 文件存放在项目根目录

set FLASH_SIZE 0x20000

# 使用 DAP-Link 烧录
adapter driver cmsis-dap
transport select swd
# 在 OpenOCD 的安装根目录下的 openocd\scripts\target 复制 stm32f1x.cfg 文件到项目根目录
source [find ./stm32f1x.cfg]
```


#### troubleshoot
1. Stm32CubeMX 生成的工程默认配置中不开启 SWD 调试
    - 解决：`Pinout & Configuration` 页面下，修改 `System Core` $\rightarrow$ `SYS` $\rightarrow$ `Debug` 中的选项，`SWD` = `Serial Wrie`，`SWD + JTAG` = `JTAG (5pins)`
    - 检查是否生效：查找源文件中 `stm32f1xx_hal_msp.c` 中是否含有 `__HAL_AFIO_REMAP_SWJ_ENABLE();`



## 使用 SDCC + CMake + Ninja 开发 51 单片机

### clangd 补全
clangd 各种奇怪报错：
1. `8052.h` 头文件中的 `<8051.h>` 改为 `"8051.h"`
2. 在 `8051.h` 头文件中新增 `#include<lint.h>` （`lint.h` 含有 sdcc 中特有的类型定义）
3. 在 `lint.h` 头文件中新增 `#include<stdbool.h>`

clangd 项目配置：（在项目根目录新建 `.clangd` 文件，输入以下内容）
```yaml
CompileFlags:
  Add: [
    -xc,
    -I/usr/share/sdcc/include/,
    -I/usr/share/sdcc/include/mcs51,
    -ferror-limit=0,
  ]

Diagnostics:
   Suppress:
     - unused-includes
```
> `-I` 参数选择 sdcc 头文件的所在路径

### 编译配置

#### CMakeLists 配置
```cmake
# 指定 CMake 最低版本
cmake_minimum_required(VERSION 3.29)


# 指定开发平台，单片机（裸机）为 Generic
set(CMAKE_SYSTEM_NAME Generic)
# 指定编译器
set(CMAKE_C_COMPILER sdcc)
# 自动生成 compile_commands.json 方便 clangd 调用
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)


# 定义项目名称
set(PROJECT_NAME light)

# 项目指定语言 C、汇编
project(${PROJECT_NAME} C ASM)

# 包含所有项目头文件
INCLUDE_DIRECTORIES(
    ${PROJECT_SOURCE_DIR}/include
)

# 包含所有项目源文件
aux_source_directory(${PROJECT_SOURCE_DIR}/src SOURCES)

# 生成目标文件
add_executable(${PROJECT_NAME})
# 添加源文件到工程中
target_sources(${PROJECT_NAME}
     PRIVATE
     ${SOURCES}
)

# 自动生成二进制文件到项目根目录
add_custom_command(TARGET ${PROJECT_NAME}
    POST_BUILD
    # 生成 .hex 文件
    COMMAND packihx ARGS ${PROJECT_NAME}.ihx > ../${PROJECT_NAME}.hex
    # 生成 .bin 文件
    COMMAND makebin ARGS ${PROJECT_NAME}.ihx > ../${PROJECT_NAME}.bin
)
```

#### 编译命令
生成 Ninja 配置：`cmake -B build -G "Ninja"`
编译：`Ninja -C build`

#### 其他
1. 当头文件修改时无法重新编译，需要进入 `build` 文件夹使用 `ninja` 清除编译（源文件修改则可以使用编译命令）
