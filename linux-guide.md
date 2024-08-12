# Linux Guide

## 初始化系统
- `Systemd`
- `OpenRC`

### Systemd 使用



## Tips

### 检查文件完整性
1. 验证文件签名：`gpg --auto-key-retrieve --verify xxx.sig xxx`
2. 验证哈希值：






## Atrix Linux Install

### 启动
Atrix ISO 会自动检测：
- 键盘布局
- 是否为 UEFI / BIOS

可选择从 ISO/HDD 启动

进入 live system后：
- 用户名：`artix`，密码：`artix`（有提示）
- 使用 `su` 登录 root 方便操作


### 准备硬盘
```bash
# 分区
# 建议 EFI 分区大约 512MiB
cfdisk /dev/sda


# 格式化
mkfs.fat -F32 /dev/sda
mount -t btrfs -o compress=zstd /dev/sd(xn) /mnt

df -h
btrfs subvolume create /mnt/@
btrfs subvolume create /mnt/@home
btrfs subvolume list -p /mnt
umount /mnt
df -h

# 挂载
# 先挂载根分区再挂载其他分区
mount -t btrfs -o subvol=/@,compress=zstd /dev/sda2 /mnt
mkdir /mnt/home
mount -t btrfs -o subvol=/@home,compress=zstd /dev/sda2 /mnt/home
mkdir /mnt/boot
mount /dev/sda1 /mnt/boot
df -h
```

### 准备网络
```
# 测试网络连通性
ping huawei.com

# 下载编辑器（可选）
pacman -Sy neovim

# 换源加速（可选）
# 编辑 /etc/pacman.d/mirrorlist 将 China 中的一项放在文件最开头
```

### 更新系统时间
- `rc-service ntpd start`


### 安装系统
```
# 安装基本系统
basestrap /mnt base base-devel openrc elogind-openrc
# 安装内核
basestrap /mnt linux linux-firmware
# 安装其他工具
basestrap /mnt networkmanager-openrc neovim sudo zsh zsh-completions btrfs-progs
```


### 安装完成后检查
1. `/etc/fstab` 文件中是否有内容（关乎能否进入系统）
2. `/boot/EFI` 中是否有内容（关乎能否启动）
3. 使用 `rc-update` 查看 `NetworkManager` 是否为 `default` （关乎是否能联网）
