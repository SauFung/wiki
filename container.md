# 容器使用

## 安装
- `Docker`：
```bash
export DOWNLOAD_URL="https://mirrors.tuna.tsinghua.edu.cn/docker-ce"
curl -fsSL https://get.docker.com/ | sh
# 或
wget -O- https://get.docker.com/ | sh
```

- `Podman`：
```
# Debian 系
sudo apt install podman
# Arch 系
sudo pacman -S podman
```


## Docker Hub
- 主页：<https://hub.docker.com/>
- 镜像：
    - <https://dockerproxy.com>
    - <https://docker.m.daocloud.io>
    - <https://docker.nju.edu.cn/>

## docker
### Hello World
`docker run hello-world`

### 命令
```bash
# 查看正在运行的容器
docker ps
# 查看所有的容器（包括已停止的容器）
docker ps -a
# 启动容器
docker start {container}
# 停止容器
docker stop <container>
# 重启容器
docker restart <container>
# 强制停止容器
docker kill <container>
# 删除容器
docker rm <container>
# 查看容器日志
docker logs -f <container>

# 拉取镜像
docker pull <user>/<image>:<tag>
# 运行镜像
docker run <user>/<image>:<tag>
# 搜索镜像
docker search <image>
# 查看已下载镜像
docker image ls
docker images
# 删除镜像
docker rmi <image>
# 进入容器
docker exec -it <container> /bin/bash

# 运行容器命令后退出
docker --rm -t <user>/<image> <command>
```
### 容器运行参数（`docker run`）
- `-v <host>:<container>`：挂载数据卷
- `-p <host>:<container>`：端口映射
- `--name="<name>"`：容器名
- `-i`：交互模式
- `-t`：分配一个终端
- `-d`：后台运行
- `-e <ENV>="<var>"`：设置环境变量


### 镜像配置
- 编辑：`/etc/docker/daemon.json`
- 内容：
```json
{
    "registry-mirrors":[
        "https://dockerproxy.com/"
            "https://docker.m.daocloud.io/"
            "https://docker.nju.edu.cn/"
    ]
}
```
- 重启 docker：`sudo systemctl restart docker`






## podman

### 配置文件
- 用户级：`~/.config/containers/registries.conf`
- 系统级：`/etc/containers/registries.conf`

#### 配置内容
```
[[registry]]
prefix = "docker.io"
insecure = false
blocked = false
location = "docker.io"

[[registry.mirror]]
location = "https://docker.m.daocloud.io/"
```


## 运行多容器

### 安装
`Docker Compose`：自带
`Podman Compose`：`pip3 install podman-compose` 或 `pipx install podman-compose`

### `docker-compose.yml`
```yaml
# 版本号
version: "3.8"

# 网络配置示例
networks:
  my_network:
    driver: bridge

services:
  # 服务名，随意
  webapp:
    # 镜像
    image: <url>/<user>/<image>:<tag>
    # 容器名
    container_name: "<name>"
    # 重启
    restart: always
    # 环境变量
    enveronment:
      - USER_UID=1000
      - USER_GID=1000
      - <ENV>=<var>
    # 网络设置
    networks:
      - my_network
    # 挂载数据卷
    volumes:
      - <host1>:<container1>
      - <host2>:<container2>
    # 端口映射
    ports:
      - <host1>:<container1>
      - <host2>:<container2>
```


### Podman Compose

## 交叉编译
```bash
sudo podman run --rm --privileged multiarch/qemu-user-static --reset -p yes
podman run --rm -t arm64v8/debian uname -m
```
> WSL 下也适用

### x86_64 下编译 ARM64 版本 Neovim（示例）
目录结构：
- `nvim`：为源码目录
- `sources.list`：为软件镜像源文件
```bash
.
├── docker-compose.yml
└── nvim
    ├── compile.sh
    └── sources.list
```

`docker-compose.yml`：
```yaml
services:
  nvim:
    image: docker.io/arm64v8/debian:buster
    container_name: neovim
    volumes:
      - ./nvim:/mnt
    command: /bin/bash /mnt/compile.sh
```

`compile.sh`：
```bash
#!/bin/bash
cp /mnt/sources.list /etc/apt/sources.list
apt update
apt install -y ninja-build gettext cmake unzip curl build-essential git pkg-config doxygen libtool-bin libtool
cd /mnt
make CMAKE_BUILD_TYPE=RelWithDebInfo
cd build
cpack -G DEB
```

操作：
```
sudo podman run --rm --privileged docker.io/multiarch/qemu-user-static --reset -p yes
mkdir nvim/build
podman-compose up
```

### Reference
- <https://github.com/multiarch/qemu-user-static>


## 问题处理
`WARN[0000] Error validating CNI config file`：CNI 版本过旧
- 手动下载 CNI 插件：<https://github.com/containernetworking/plugins/releases>
- 创建目录：`sudo mkdir /usr/local/lib/cni`
- 解压缩到 `/usr/local/lib/cni` 中：`sudo tar xvf cni-*.tgz -C /usr/local/lib/cni`


hello <world>
