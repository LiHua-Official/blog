---
title: "ArchLinux安装"
date: 2020-09-08T15:28:15+08:00

description: ""

categories: ["教程"]

tags: ["Arch"]

draft: true
---

# 准备
## 下载ISO镜像
[清华大学镜像站](https://mirrors.tuna.tsinghua.edu.cn/archlinux/iso)

## 检验镜像文件

# 安装
> 本文以`<Tab>`表示Tab键
## 验证启动模式
```bash
ls /sys/firmware/efi/efivars
```
如果命令显示的目录没有错误，则系统以UEFI模式启动。如果该目录不存在，则系统可以BIOS（或CSM）模式启动。
## 联网
### 以太网
插入网线
### Wifi
执行 `iwctl` 进入交互模式  
然后显示交互式提示，前缀为`[iwd]#`  
```
station wl<Tab> connect <Tab>
```
