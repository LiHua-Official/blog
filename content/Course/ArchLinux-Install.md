---
title: "ArchLinux单系统安装"
date: 2020-09-08T15:28:15+08:00

description: "有点脱离WIKI的安装方式，并附带一点点维护技巧"

tags: ["Arch","Linux"]

draft: true
---

# 准备  

## 下载ISO镜像

[清华大学镜像站](https://mirrors.tuna.tsinghua.edu.cn/archlinux/iso)  
![下载](https://cdn.jsdelivr.net/gh/LiHua-Official/pic/2020-09-08_18-44.png)

## 检验镜像文件[可选]  

[下载用来验证签名的程序](https://www.gnupg.org/download/index.html#sec-1-2)  
![](https://cdn.jsdelivr.net/gh/LiHua-Official/pic/2020-09-08_18-55.png)
> 使用应该不要说明吧。

# 安装

## 进入安装模式

![](https://cdn.jsdelivr.net/gh/LiHua-Official/pic/Arch%20Linux-2020-09-09-07-36-12.png)
> 本文以`<Tab>`表示Tab键。

## 验证启动模式

如已知启动模式，该命令可忽略。

```bash
ls /sys/fi<Tab><Tab>
```

若命令显示存在efi，则系统以UEFI模式启动。若不存在，则系统可以BIOS（或CSM）模式启动。

## 联网

### 以太网

插入网线。

### Wifi

执行 `iwctl` 进入交互模式  
然后显示交互式提示，前缀为`[iwd]#`  
通过 `station wl<Tab> connect <Tab>` 选择SSID连接。

### 静态IP分配  

检查是否已通过DHCP分配ip地址。

```bash
ip addr show
```

若已分配IP地址，可跳转至[连通测试](#连通测试)；若无，手动分配ip地址。  

```bash
ip addr add dev <Tab>
```

选择设备，`<Tab>` 输入IP地址，回车。  
> 注意：IP地址应与网关处于同一子网且不得与现有设备的IP地址冲突。

### 连通测试

ping[某个测试网络连通专用网站](https://baidu.com)。

```bash
ping -c 3 baidu.com
```

如果正常，继续。  

### 更新系统时钟

```bash
timedatectl set-ntp true
```

## 分区  

### 查看物理总内存（知道你设备物理内存的可以忽略这一步）

```bash
echo `free -m | awk 'NR==2,NF=2'`MiB
```

> 记住数值，后面用得到  

### 查看分区表

```bash
fdisk -l
```

当由实时系统识别，磁盘被分配给一个块设备如/dev/sda，/dev/nvme0n1或/dev/mmcblk0
比如：
![](https://cdn.jsdelivr.net/gh/LiHua-Official/pic/Arch%20Linux-2020-09-09-09-46-40.jpg)
如果存在EFI System分区，建议备份

#### 备份efi方法示例（git）

##### 注册帐号（已有Gihub，Gitlab，Gitee等平台帐号的童鞋可忽略）

[Gitee](https://gitee.com/signup)
为什么这里选Gitee，因为国内平台，没经过墙，搬东西比较快。
就四个空，而且还是中文提示，姓名那个空可以随便填，应该会填吧！

##### 创建仓库

[Github](https://github.com/new)
[Gitlab](https://gitlab.com/projects/new)
[Gitee](https://gitee.com/projects/new)

##### 备份操作

```bash
# 安装git
yes | pacman -Sy git
# 挂载efi分区
# 请将"/dev/sda1"改为你的EFI分区
mount /dev/sda1 /mnt
# 切换到efi分区
cd /mnt
# git信息配置
git config --global user.email "你的电子邮箱"
git config --global user.name "你的用户名"
# 备份
git add .
git commit -m "Add EFI"
git remote add origin 你的仓库链接
git push -u origin master
# 卸载分区
cd
umount /mnt
```

#### 分区示例布局（新手建议布局，老手请自己分区）

| 挂载点                | 划分                      | 分区类型              | 建议大小     |
| :-------------------- | ------------------------- | --------------------- | ------------ |
| /mnt/boot OR /mnt/efi | /dev/efi_system_partition | EFI system partition  | 大于200 MiB  |
| [SWAP]                | /dev/swap_partition       | Linux swap            | 总内存的一半 |
| /mnt                  | /dev/root_partition       | Linux x86-64 root (/) | 剩余空间     |

### 开始分区
#### 进入分区模式
请将"/dev/sda"改为你要安装系统的硬盘
```bash
fdisk /dev/sda
```
然后，你可以看到
![](https://cdn.jsdelivr.net/gh/LiHua-Official/pic/Arch%20Linux-2020-09-10-20-06-29.png)
#### 删除所有分区
`d`，`<Enter>`，`<Enter>`，`d`，`<Enter>`，`<Enter>`...
#### 新建分区
`n`