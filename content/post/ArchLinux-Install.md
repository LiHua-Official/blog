---
title: "ArchLinux安装"
date: 2020-09-08T15:28:15+08:00

description: "有点脱离WIKI的安装方式"

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
##  进入安装模式
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
若无，手动分配ip地址。  
```bash
ip addr add dev <Tab> 
```
ping某个测试网络连通专用网站。
```bash
ping -c 3baidu.com
```
如果正常，继续。  
更新系统时钟。
```bash
timedatectl set-ntp true
```
