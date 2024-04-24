# 服务器 WIKI


## Basic

### 网卡名称格式：
旧标准：`eth{N}`，如 `eth0`
- `{N}`：为网卡端口索引
新标准：`enp{N1}s{N2}`，如 `enp0s0`
- `{N1}`：表示 PCI 槽号
- `{N2}`：表示网卡端口索引
- 别名：
    - `ens{N}`
    - `eno{N}`
    - `{N}`：为网卡端口索引



## PVE

### 关闭网页订阅提示
```
cd /usr/share/javascript/proxmox-widget-toolkit/
cp proxmoxlib.js proxmoxlib.js.bak
# 将 Ext.Msg.show 改为 Ex.Msg.noshow
vim proxmoxlib.js
# 修改完后，重启服务刷新网页生效
systemctl restart pveproxy
```

### 更换软件源
```
# 

```


### Debian 12 修改静态 IP （netplan）
- 所有基于 Debian 12 的系统同样适用

```bash
# 查看网卡名，如 ens18
ip a

# 备份配置文件
cp /etc/netplan/xxx.yaml /etc/netplan/xxx.yaml.bak
# 编辑配置文件
vim /etc/netplan/xxx.yaml

# 查看设置
netplan get
# 测试
netplan try
# 应用
netplan apply
```

`/etc/netplan/xxx.yaml` 内容：
```yaml
network:
  ethernets:
    # 网卡名
    enp6s0:
      # 关闭 dhcp
      dhcp4: false
      dhcp6: false
      link-local: []
      # 设置静态 IP
      addresses:
        - 172.16.0.1/24
      # 设置网关
      routes:
        - to: default
          via: 172.16.0.254
      # 设置 DNS
      nameservers:
        addresses:
          - 172.16.0.254
          - 172.16.0.253
```

配置文件命名格式：`{num.}-{name}-{interface}`
- `{num.}`：优先级
- `{name}`：文件名
- `{interface}`：网卡名
- 例如：OMV 中 `20-openmediavault-ens18.yaml`

警告：
- `WARNING even when openvswitch-switch not installed`
    - 安装 `Open vSwitch`：`apt install openvswitch-switch`


### 网络唤醒
```bash
# 查看网卡名称
ip a

# 查看网卡硬件是否已开启网络唤醒，需要在 BIOS 中设置
# Support Wake-on: pumbag 表示硬件开启
ethtool enp4s0 | grep Wake-on

# 设置网络唤醒，非永久设置
ethtool -s enp4s0 wol g
# 查看网络唤醒是否开启成功
# Wake-on: g 表示已开启网络唤醒
ethtool enp4s0 | grep Wake-on

# 永久设置网络唤醒
# 新建 wol 服务
vim /etc/systemd/system/wol.service
# 设置开机自启
systemctl enable wol.service
```

`/etc/systemd/system/wol.service` 内容：
```
[Unit]
Description=Wake-on-LAN for enp4s0
Requires=network.target
After=network.target

[Service]
ExecStart=/usr/bin/ethtool -s enp4s0 wol g
Type=oneshot

[Install]
WantedBy=multi-user.target
```
- 网卡名称一般为 `enp{N}s0`，例如 `enp4s0`
- `enp4s0` 替换为设置的物理网卡名称

