---
title: OpenWRT编译及使用教程
description: 每次觉得路由器可能要更新了就得到处找教程，索性自己写一个免得到处找。
pubDate: 2025-08-31 00:33:51
heroImage: "https://s21.ax1x.com/2024/09/05/pAZwac9.jpg"
updateDate:
tags:
- openwrt
- 路由器
- 教程
categories: 教程
cover: 
---

# 引言

最初是因为家里的nas不是经常使用，长时间开着觉得浪费电，于是想着能不能搞个远程开关机，然后就了解到了**网络唤醒**这个功能，确实不错。不过这是一个在路由器上的功能，我登录我家老路由器的后台一看，果然没有。然后在找路由器的过程中，我发现了openwrt这个系统，开源、功能丰富、社区庞大......一下子就被吸引了，虽然需要折腾一下，但这个过程也是一个十分好的学习的机会，于是经过多方考量，直接购买了一个x86平台的小主机用来当家庭网络的主路由，等弄好之后就把剩下的路由器当AP来用，说干就干。

# 配置介绍

我的设备是畅网微控的X86-P2小主机
               
``` TXT
CPU：J4125
显卡：Intel UHD Graphics 600
网卡：Intel i226-V
内存：三星 4G 2400MHz
硬盘：三星 NVME 128G
```

# 环境配置

首先，我们需要一个Linux环境来进行编译，在Windows上用VMware Workstation Pro（个人使用是免费的）为我们创建一个Linux的虚拟环境就很方便，推荐使用Ubuntu作为我们的编译环境。

1. 我们得先去Ubuntu的官网去下载一个它的系统镜像

2. 将其导入到VMware中像安装windows系统一样安装它

3. 





# Ubuntu操作

1. 拉取openwrt官方源码

2. 更改分支

3. 



# menuconfig

- [x] target system -> x86
- [x] subtarget -> x86_64
- [x] target profile -> generic x86/64
- [x] target images
    - -> kernel partition size(64)MB
    - -> root filesystem partition size(512)MB


## Base System

- [x] block-mount
- [x] ca-bundle【默认勾选】
- [ ] dnsmasq（取消勾选，选下面这个）
- [x] dnsmasq-full
- [ ] dropbear（取消勾选）

## Administration

- [x] btop
- [ ] netdata
- [x] sudo


## Firmware

- [x] i915-firmware
- [x] intel-microcode


## Kernel Modules

### Block Devices

- [x] kmod-nvme

### Filesystems

- [x] kmod-fs-exfat
- [x] kmod-fs-ext4
- [x] kmod-fs-f2fs
- [x] kmod-fs-ntfs3

### GPIO support

- [ ] kmod-gpio-button-hotplug

### Netfilter Extensions

- [x] kmod-nft-bridge
- [x] kmod-nft-offload【默认勾选】
- [x] kmod-nft-queue
- [x] kmod-nft-socket
- [x] kmod-nft-tproxy（nft透明代理）

### Network Devices

- [x] kmod-igc【默认勾选】

### Network Support

- [x] kmod-inet-diag
- [x] kmod-netlink-diag
- [x] kmod-sched-core
- [x] kmod-sched-fq
- [x] kmod-tcp-bbr
- [x] kmod-tls
- [x] kmod-tun
- [x] kmod-veth

### Other modules

- [ ] kmod-button-hotplug

### USB Support

- [x] kmod-usb-storage
- [x] kmod-usb-storage-uas
- [x] kmod-usb2
- [x] kmod-usb3

### Video Support

- [x] kmod-drm-i915


## Libraries

- [ ] libuci-lua


## luci

### 1. Collections

- [x] luci-nginx
- [ ] luci-ssl-openssl

### 2. Modules

- [x] Translations -> Simplified Chinese
- [ ] luci-compat
- [x] luci-mod-dashboard

### 3. Applications（优先勾选这个，有些依赖项会自动勾选）

- [x] luci-app-acme
- [x] luci-app-argon-config
- [ ] luci-app-banip（恶意IP屏蔽）
- [x] luci-app-ddns
- [x] luci-app-firewall
- [ ] luci-app-dockerman（由于docker会使用旧版的iptables规则，和nftables混用会报错，决定去除docker）
- [ ] luci-app-https-dns-proxy（DoH）
- [ ] luci-app-irqbalance（中断负载均衡，把硬件产生的中断请求（IRQ）均匀地分配到路由器 CPU 的各个核心上）
- [ ] luci-app-ksmbd
- [x] luci-app-nlbwmon
- [x] luci-app-package-manager
- [x] luci-app-pbr（策略路由）
- [x] luci-app-smartdns
- [x] luci-app-statistics
- [ ] luci-app-transmission
- [x] luci-app-ttyd
- [x] luci-app-upnp
- [x] luci-app-wol

### 4. Themes

- [x] luci-theme-argon

### 5. Protocols

- [x] luci-proto-wireguard

### 6. Libraries

- [ ] luci-lib-jsonc


## Network

### BitTorrent

- [ ] transmission-cli
- [ ] transmission-remote
- [ ] transmission-web-control

### IP Addresses and Names

- [x] acme-acmesh-dnsapi
- [x] ddns-scripts-aliyun
- [x] ddns-scripts-cloudflare
- [x] ddns-scripts-dnspod-v3
- [x] ddns-scripts-utils

### SSH

- [x] openssh-server
- [x] openssh-sftp-server


## Utilities

- [ ] dockerd
    - [ ] Installs check-config.sh with dependencies
    - [ ] Enable available kernel support for CGroupsV1
    - [ ] Enable optional kernel support for Docker
    - [ ] Network
        - [ ] Includes the Overlay network feature
        - [ ] Includes macvlan kernel modules
    - [ ] Storage
        - [ ] Enables support for ext3 or ext4 as the backing filesystem
        - [ ] Enables support for btrfs as the backing filesystem
- [ ] irqbalance
- [x] acpid（高级配置与电源接口）

### Disc

- [x] fdisk
- [x] lsblk
- [x] partx-utils

### Editors

- [x] vim-full
- [x] vim-runtime

### Filesystem

- [x] e2fsprogs
- [x] mkf2fs

### Shells

- [x] bash
- [x] zsh


# 外网访问配置

因为运营商会封锁80、443端口，所以通过监听高位端口来实现外网访问。但是这样访问的话需要在地址栏后面加上监听的端口号，如下方的12443端口。</br>示例访问域名：www.yourdomain.com:12443

本人使用的是nginx作为web服务器，与openwrt默认的web服务器uhttpd配置会稍有差异。

通过添加一个server块来实现外网访问（以下是nginx的配置）

```uci
config server '_wan'
	list listen '12443 ssl'
	list listen '[::]:12443 ssl'
	option server_name '_wan'
	list include 'conf.d/*.locations'
	option uci_manage_ssl 'none'
	option ssl_certificate '/etc/ssl/acme/yourdomain.com.fullchain.crt'
	option ssl_certificate_key '/etc/ssl/acme/yourdomain.com.key'
	option ssl_session_cache 'shared:SSL:32k'
	option ssl_session_timeout '64m'

##  如果想要客户端CA证书锁（mTLS 双向认证），把下面两行加上
#	option ssl_client_certificate '/etc/nginx/certs/ca.crt'
#	option ssl_verify_client 'on'
```

# DDNS动态域名解析

相信大部分人家里都是没有公网ipv4地址的，但随着ipv6的推广越来越普及，大部分地区已经可以使用ipv6了。所以，本教程使用的就是通过ipv6+DDNS来实现域名访问。

因为运营商下发的ipv6的地址前缀是会变化的，也就是动态ipv6地址，所以需要DDNS来配合访问。




# SSL证书配置（通过ACME自动证书安装）

将内网服务暴露在公网中是很危险的一种行为，所以，在不得不暴露的情况下就需要一些安全措施来保障访问的私密性。由于HTTP是明文传输的（十分危险），那么使用SSL证书来加密访问就显得必不可少。

