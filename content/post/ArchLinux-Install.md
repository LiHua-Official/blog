---
title: "ArchLinux安装"
date: 2020-09-08T15:28:15+08:00

description: "有点脱离WIKI的安装方式，并附带一点点维护技巧"

categories: ["教程"]

tags: ["Arch"]

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

## 查看分区表

```bash
fdisk -l
```

当由实时系统识别，磁盘被分配给一个块设备如/dev/sda，/dev/nvme0n1或/dev/mmcblk0
比如：
![](https://cdn.jsdelivr.net/gh/LiHua-Official/pic/Arch%20Linux-2020-09-09-09-46-40.jpg)
