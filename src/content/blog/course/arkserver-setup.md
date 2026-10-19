---
title: 【方舟:生存进化】ARK服务器搭建
description: 网上的教程我觉得不够细节，所以自己来写一个。
pubDate: 2024-11-09 20:21:52
updateDate:
hearImage: "https://s21.ax1x.com/2024/11/10/pA6folt.jpg"
tags:
- 方舟：生存进化
- ARK服务器
- 教程
categories: 教程
cover: https://s21.ax1x.com/2024/11/10/pA6fISI.jpg
---

# 引言

最近和朋友一块玩方舟有点上头，用的 **非专用主机** 联机的，但是有很多问题。再加上官服停服，也没找到自己满意的私服，所以还是打算开个服。正好看到之前搭MC服务器用的 **MCSM面板** 也可以用来开Steam游戏，就打算用这个来试一试。

{% note success no-icon %}
环境介绍：
　　Linux系统：Debian 12.7.0
{% endnote %}

本教程参考于：
　　SteamCMD官方Wiki：https://developer.valvesoftware.com/wiki/SteamCMD
　　ARK专用服务器搭建官方Wiki：https://ark.wiki.gg/wiki/Dedicated_server_setup

---

# 游戏服务端安装

{% note info no-icon %}

因为服务器是Debian系统，所以接下来的命令操作都是基于Debian的，虽然各个Linux系统的操作大差不差，但还是有细微差别的。可以跟着我的步骤，再对照着官方文档对于你所使用的系统的命令是否不同，一步一步走。

{% endnote %}

## 1.安装依赖

```bash
sudo apt update && sudo apt upgrade -y              #更新系统
sudo apt install software-properties-common         #安装管理软件源和仓库配置的工具包
sudo apt-add-repository non-free                    #添加non-free仓库
sudo dpkg --add-architecture i386                   #添加32位架构支持
sudo apt update                                     #更新软件源
sudo apt install lib32gcc-s1                        #安装依赖项lib32gcc-s1软件包
```

## 2.创建新用户

创建新用户的目的是为了系统安全考虑，以下是官方原话

{% note warning %}
Warning: Do not run steamcmd while operating as the root user. Doing so is a security risk.
{% endnote %}

```bash
sudo adduser steam                                  #创建一个名叫steam的用户，跟着提示完成创建
sudo mkdir -p /opt/steam/steamcmd                   #创建存放steamcmd软件的目录(我喜欢把软件放在`opt`里面，根据自己喜欢选择路径)
sudo chown -R steam:steam /opt/steam                #给steam用户提权，让它可以对该路径文件操作
su - steam                                          #切换到新用户
```

## 3.安装steamcmd

下载[steamcmd(点击下载)](https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz)压缩包，上传到steam用户目录下

```bash
tar -zxvf steamcmd_linux.tar.gz -C /opt/steam/steamcmd      #解压文件到steamcmd目录
```

## 4.安装ARK游戏

>tips：如果在steamcmd中输错了，发现退格键没用，变成了^H，此时按住ctrl再按退格就能删除了

```bash
mkdir /opt/steam/ark                                #创建文件夹，用于存放ARK服务端文件
cd /opt/steam/steamcmd                              #切换到steamcmd的目录
./steamcmd.sh                                       #启动SteamCMD
force_install_dir /opt/steam/ark                    #指定ARK服务端安装路径
login anonymous                                     #匿名登录
app_update 376030 validate                          #安装ARK，"376030"是ARK专用服务器的AppID，"validate"选项用于确保文件完整性
exit                                                #退出
```

以上就是游戏服务端的安装了，接下来就是要使用MCSM面板对游戏进行很方便的配置管理工作

---

# 面板安装及使用

{% note info no-icon %}

首先先来回答一个问题，为什么要用这个面板。换句话来说用它有什么好处，我来说一下好处。配置好了之后每次只需要访问web页面就能对游戏进行方便的管理，不用每次都用ssh连接，对着命令行敲指令。无论是上传文件还是编辑文件都会很方便直观。

{%endnote%}

## 1.MCSM面板安装

十分简单，一键安装  {% hideInline 要是安装都能像这样这么简单多好🤯, 悄悄话 %}

推荐切换成root用户安装，普通用户的话容易出现权限问题。由于上面还是steam用户，而steam用户不具备sudo权限，所以需要先切回之前的用户，再执行下面的命令才能成功。

```bash
sudo su -                                           #切换成root用户（可选）
sudo su -c "wget -qO- https://script.mcsmanager.com/setup_cn.sh | bash"
```

如果服务器上有防火墙，需要开放 **23333** 和 **24444** 这两个端口。如果是云服务器，去云服务器的管理页面把这两个端口打开就行了。如果是家用服务器，不仅开服的这个主机的防火墙的端口要打开，路由器上的防火墙的端口也要打开。

## 2.面板配置

在浏览器地址栏输入：{% label 服务器ip地址:23333 green %} 回车，就能进入MCSM的欢迎页面了，创建一个用户，接下来就可以使用了。

1. 进入面板之后，按照图片提示，点击应用实例，然后新建应用

![](https://s21.ax1x.com/2024/11/13/pAgGKPJ.png)

2. 选择Steam游戏服务器

![](https://s21.ax1x.com/2024/11/13/pAgGn54.png)

3. 选择localhost主机

![](https://s21.ax1x.com/2024/11/13/pAgGmaF.png)

4. 选择现有目录

![](https://s21.ax1x.com/2024/11/13/pAgGVbT.png)

5. 实例名称自己发挥，服务端文件目录跟着图片填写

![](https://s21.ax1x.com/2024/11/13/pAgGeVU.png)

6. 点击进入创建好的应用实例

![](https://s21.ax1x.com/2024/11/14/pAgUzV0.png)

7. 页面往下翻，找到文件管理

![](https://s21.ax1x.com/2024/11/14/pAgUjrn.png)

8. 如果出现和下图一样的文件列表，那就说明配置完成了

![](https://s21.ax1x.com/2024/11/14/pAgUvbq.png)

## 3.启动文件

{% note info no-icon %}

此时万事俱备，只欠东风。就差一个启动文件，来启动服务器就行了

{% endnote %}

1. 我们只需要在Windows桌面上新建一个文本文档，将其文件名改为`start.sh`，再在其中写入如下启动参数（示例）

```bash
#! /bin/bash
./ShooterGame/Binaries/Linux/ShooterGameServer TheIsland?SessionName=ARK?Port=7777?QueryPort=27015?MaxPlayers=50?ServerPassword=123456789?ServerAdminPassword=123456789?ServerCrosshair=true?AllowThirdPersonPlayer=true?ShowMapPlayerLocation=true -log -crossplay
```

<details>
<summary>启动参数详情</summary>

- TheIsland：孤岛地图。
- SessionName=ARK：设置游戏会话的名称为"ARK"。
- Port=7777：设置游戏服务器的端口为7777。
- QueryPort=27015：设置查询端口为27015，用于获取服务器信息。
- MaxPlayers=50：设置最大玩家数为50。
- ServerPassword=123456789：设置玩家加入服务器时需要的密码。
- ServerAdminPassword=123456789：设置管理员密码，用于管理服务器。
- ServerCrosshair=true：启用屏幕上的准星。
- AllowThirdPersonPlayer=true：允许玩家使用第三人称视角。
- ShowMapPlayerLocation=true：在地图上显示玩家的位置。
- ~~-server：启动为服务器模式。~~(已弃用)
- -log：启用服务器日志记录。
- -crossplay：允许跨平台游戏，支持与其他版本（例如主机版）联机。

</br>

<center><b>地图及内存占用</b></center>

</br>

|Map            |Level name       |Memory usage on an empty map |
|:-------------:|:---------------:|:---------------------------:|
|The Island     |TheIsland        |3.5-4.5 GiB                  |
|The Center     |TheCenter        |3-4 GiB                      |
|Scorched Earth |ScorchedEarth_P  |3-4 GiB                      |
|Ragnarok       |Ragnarok         |4-5 GiB                      |
|Aberration     |Aberration_P     |3-4 GiB                      |
|Extinction     |Extinction       |3-4 GiB                      |
|Valguero       |Valguero_P       |3-4 GiB                      |
|Genesis: Part 1|Genesis          |5-6 GiB                      |
|Crystal Isles  |CrystalIsles     |5.5-6.5 GiB                  |
|Genesis: Part 2|Gen2             |10-12.5 GiB                  |
|Lost Island    |LostIsland       |5.5-7 GiB                    |
|Fjordur        |Fjordur          |4-5 GiB                      |

</details>

2. 然后根据图片提示，将其上传到实例的`/`目录下，上传完成后，可以在这个界面找到这个文件，此时的文件可能是没有执行权限的，右侧有更改文件权限的设置，将执行权限勾上即可

![](https://s21.ax1x.com/2024/11/14/pAgbc0s.png)

3. 回到实例界面，点击应用实例设置

![](https://s21.ax1x.com/2024/11/14/pAgbsXQ.png)

4. 再将`sudo -u steam ./start.sh`填入启动命令（下图仅供参考，根据本行填写）

![](https://s21.ax1x.com/2024/11/14/pAgb6mj.png)

5. 最后回到实例界面，点击开启，即会启动服务器

![](https://s21.ax1x.com/2024/11/14/pAgqoPP.png)

</br>

之后对于服务器的开关操作都可以通过这个面板来操作，甚至修改一些采集、孵化倍率也可以通过面板的文件管理，找到对应文件之后在线编辑直接修改，多摸索摸索就会慢慢熟练了。

---

# 联机

{% note success no-icon %}

记得服务器的启动文件里有这两个参数`Port=7777`、`QueryPort=27015`吗，这是我们需要开启的两个端口，最好再加上一个`7778`端口。这几个端口都是`udp`协议，开启之后才能联机，记得在防火墙中打开。

{% endnote %}

实现联机的方式有很多种，具体教程我就不做详细介绍了。因为每个人的网络环境、所拥有的条件都不同，有的人有动态公网ipv4，有的有动态ipv6，有的两个都没有，总之会遇到各种各样的问题。网上也有很多关于这个的教程，可以根据我的提示，来看看自己是属于哪种情况，可以去找相应的教程。

村长（指作者本人）实现联机的方法是wireguard + 公网ipv6。

- 有公网ip
    - 购买云服务器的用户可以直接通过服务商给的公网ip:端口号的方式联机
    - 如果家里能申请到动态公网ipv4，只需要路由器做个DDNS解析，通过域名:端口号来联机
    - 只要是办理了宽带一般都是会有动态公网ipv6的，但是这个不知道是steam还是ark这个游戏对这个支持不太好，不能直接联机，可以通过wireguard、zerotier等来组建虚拟局域网来实现联机

- 无公网ip
    - 可以通过一些提供frp服务的服务商来做内网穿透实现联机。

</br>
</br>

<center><b><font size=5>至此，本篇ARK服务器搭建教程就结束了。接下来，希望开好服务器的大家，可以好好享受游戏带来的快乐。(*^︹^*)</font></b></center>
