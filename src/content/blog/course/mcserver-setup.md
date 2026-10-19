---
title: 【我的世界】MC服务器搭建
description: 哦？未来的大腐竹，啥时候带我去你的服务器里转转呢*_*
pubDate: 2023-01-11 12:45:23
updateDate:
heroImage: "https://s21.ax1x.com/2024/09/05/pAZwU1J.jpg"
tags: 
- 我的世界
- MC服务器
- 教程
categories: 教程
cover: https://s21.ax1x.com/2024/09/05/pAZwac9.jpg
---

# 引言

{% note default no-icon%}
可以实现 **远程联机** 需求的方式有很多，比如在家中用本机或其他计算机运行服务端，配合端口转发实现，或者进入游戏开启 **对局域网开放** 配合端口映射实现，再或者购买云服务器，在云服务器中搭建等。每个方法各有优缺点，看大家自己喜欢哪种方式。
{% endnote %}

本教程是教大家如何在**Linux**系统中通过安装 <td><font color=green>**MCSM管理面板**</font></td> 来快速便捷地搭建**MC服务器**。

![MC服务器搭建](https://s1.ax1x.com/2023/01/15/pSQdkjJ.png)

**前情提要**：本教程比较偏向纯小白，有些步骤可能过于细致，或者说啰嗦，相信就算你再纯，跟着这篇教程来走也能顺利地把服务器开起来。其中我已经尽量将一些比较细致的部分进行了折叠，也方便有些基础的朋友浏览。

# 一、环境准备

>要搭建MC服务器，你需要拥有一台服务器，无论是自己家里的，还是购买的云服务器都可以，这里比较推荐云服务器。因为服务器不出意外的话应该是24小时一直开着的，如果在家中自己搭建的话需要考虑到 <u>噪音，电费，公网ip或内网穿透等</u> 比较麻烦的事。但以上都可以克服，加上家里的服务器性能（主要看CPU和内存）也还可以的话，在家中开服各方面应该都是要优于云端开服的。

## 1.1 服务器选购
如果你只是想要开一个小型服务器，能够满足和身边的几个小伙伴一起玩耍，偶尔加几个插件或者mod来玩，那么 **2C-4G-6M** 的配置已经可以满足六七个小伙伴一起愉快地玩耍了，要只是开原版服的话，估计15个人以内应该也不是问题。

首先是服务器推荐，新人可以去看看阿里云和腾讯云的新用户首单优惠，力度非常大，上面提到的 **2C4G6M** 的服务器一年大概只要**100块**左右(我当时买的时候是正正好好100块，腾讯云)，两家的价格应该都差不多，足够你与小伙伴畅玩很长一段时间了。两边都有新用户的试用选项，可以选择 **试用1个月** 先试试，如果不符合你的要求再进行更换。当然云服务器的厂商有很多，如果你有更好的选择肯定可以，我暂时就只提国内用户比较了解的两个了。

下面是提到的腾讯云的 <font color = red>新用户试用</font> 和 <font color = red>首单优惠</font> 的详情图片。(阿里是老用户了，显示不了新用户的优惠，感兴趣的可以自己去看看)

[腾讯云新用户试用](https://cloud.tencent.com/act/free?from=19067)   [腾讯云服务器购买](https://cloud.tencent.com/act/pro/developer_business-scenario?from=18693)（链接我就贴在这边了）
<center class="half">
<img src="https://s1.ax1x.com/2023/01/15/pSQwAG8.png" width = "350"><img src="https://s1.ax1x.com/2023/01/15/pSQwERS.png" width = "350">
</center>
<center>（写这篇时看2C4G6M的价格是在159块）</center>

---

>选这个基本没什么讲究，有一个说一下就是服务器的地址（像上海，广州，北京...），买的离你住的地址越近应该是越好的,其实就算远也没啥太大影响。系统这里推荐选择 **centos 7.6**，因为相较于其他Linux系统这个系统更加的轻量，对内存的占用更加少，所以在我们开服的过程中能分配更多的内存给到游戏，让游戏更加稳定。

## 1.2 服务器配置
拿到服务器之后，我们先进入控制台进行 <font color = #3B88DF><u>**重置密码**</u></font> ，因为接下来要使用 <font color = red>SSH工具</font> 进行远程连接，更加方便安全地进行配置和传输文件。

### 1.2.1 SSH工具的安装与配置
如果有自己在用的ssh工具或使用网页端ssh工具就不必展开查看此步骤了。

<details>
<summary>要不，点开康康~</summary>

**(1)选择与安装**

这边博主只推荐我用过的两款SSH工具，Xshell和FinalShell，都是十分好用的工具。

Xshell虽然是付费软件，但其面向家庭和学生是有免费版的，只需填写一下姓名及邮件，勾选一下需要的软件，你就会收到软件的下载地址。为了更加方便地传输文件，通常需要搭配Xftp使用，所以在勾选时一般勾选两者。申请链接贴在这里 [https://www.xshell.com/zh/free-for-home-school](https://www.xshell.com/zh/free-for-home-school/)

![](https://s1.ax1x.com/2023/04/02/ppfKCIf.png)

\
FinalShell是一款国产免费的软件，其中有些功能很方便，就不具体说明了。但因为其不开源的原因，而且网上有传闻说有时使用它时CPU会被大量占用，疑似被用来挖矿，导致很多用户出于安全考虑而不敢使用。博主在使用时确实发现FinalShell在使用过程中要比Xshell卡顿一些。如果你没有安全顾虑，那么这应该也是一款不错的工具。下载链接贴在这里[http://www.hostbuf.com/downloads/finalshell_install.exe](http://www.hostbuf.com/downloads/finalshell_install.exe)

![](https://s1.ax1x.com/2023/04/02/ppfQADs.jpg)

下载完成之后，一路同意安装即可。

**(2)远程连接与配置**

接下来以 Xshell 作为示例进行后面的操作。

打开软件之后，我们先点击图片中左上角的新建会话（图中红框）
![](https://s1.ax1x.com/2023/05/09/p90aAxK.png)

然后在这个界面中给你要远程连接的主机命名，以及将远程主机的 **公网ip** 填入（图中红框）。其他不用动，点击确定即可。
![](https://s1.ax1x.com/2023/05/09/p90ak26.png)

在图中左边的 **所有会话** 中，会出现的你所命名的主机，双击它进行连接。
![](https://s1.ax1x.com/2023/05/09/p90TXkj.png)

随后会跳出登录界面，我们输入用户名 **root** ，并勾选记住用户名，这样下次登录时就不用重复输入了。
![](https://s1.ax1x.com/2023/05/09/p90bFY9.png)

再下一步是输入密码，密码就是你<u>刚拿到服务器时重置的密码</u>，同时勾选记住密码，也是方便下次登录时不用重复输入。
![](https://s1.ax1x.com/2023/05/09/p90biFJ.png)

>不出意外的话，此时已经顺利连接到远程主机了，如果出现没连接成功的情况，可以去你购买的云服务器控制台看看，在防火墙那一页，是否是因为22端口未开放所导致的。

Xftp 的连接同上面 Xshell 配置连接的方式一样，连接成功即可方便地传输文件。
![](https://s1.ax1x.com/2023/05/11/p9rRReJ.png)

</details>

### 1.2.2 JAVA环境部署

我们都知道游玩MC时，JAVA环境是必不可少的（基岩版除外），因为MC本身就是由JAVA语言所编写的，所以接下来要在远程主机上部署java运行环境。

因为Minecraft不同的游戏版本所需要的java版本不相同,所以在安装java前，首先要知道我们需要哪个版本的java。

<details>
<summary>要不，再康康~</summary>

{% note success no-icon %}
**推荐**：  
　　<font color=red> 1.16.5 </font> 及之前的版本使用 <font color=red> java 8 </font>  
　　<font color=green> 1.17 </font> 及之后的版本使用 <font color=green> java 17 </font>  
　　(如果不行，根据报错提示来选择合适的java版本)o(*￣▽￣*)ブ
{% endnote %}

</details>

---

<font size=4>选择好要安装的java版本后，下一步来进行安装部署(以 java 17 来举例)：</font>


#### (1) 下载

下载java 17，这里把官网[^1]下载链接🔗贴在这：[jdk-17_linux-x64_bin.tar.gz](https://download.oracle.com/java/17/latest/jdk-17_linux-x64_bin.tar.gz)

#### (2) 解压

将下载好的压缩包上传到远程主机，并解压到 /usr/local/java/
   
1. 我们先打开Xftp，连接到远程主机，然后进入根目录，将压缩包上传过去，此时压缩包存放在根目录里。

<details>
<summary>哦？你还想看~</summary>

<center> 双击下图带两点的文件夹，其作用就是返回上一级 </center>

![](https://s1.ax1x.com/2023/05/11/p9rW4BQ.png)

<center> 一直返回，直到看见地址栏只有一条" <b> / </b> "或者如下图的文件夹类型 </center>

![](https://s1.ax1x.com/2023/05/11/p9rWWjS.png)

<center>此时右击下载好的压缩包，选择传输</center>

![](https://s1.ax1x.com/2023/05/11/p9r5i3q.png)

<center>传输完成后，就可以在右边的根目录列表中找到了（如下图）</center>

![](https://s1.ax1x.com/2023/05/11/p9rIPde.png)

</details><br/>

2. 接下来，将传输完成的压缩包解压到 **/usr/local/java/** 

<center>我们先打开Xshell，连接好远程主机，然后键入下面的命令</center>

```bash
cd /                            //切换到根目录
mkdir /usr/local/java/          //创建名为 java 的目录
ls                              //列出当前目录的内容（如下图）
```
![](https://s1.ax1x.com/2023/05/11/p9rzp9K.png)
<center>找到传好的java的压缩包，然后复制下来它的名字，填到 <font color=#0B63C7> -zxvf </font> 和 <font color=#0B63C7> -C </font> 的中间（如下行命令）</center>

```bash
tar -zxvf jdk-17_linux-x64_bin.tar.gz -C /usr/local/java/     //解压文件 jdk-17... 到 /usr/local/java/ 路径里
rm -rf jdk-17_linux-x64_bin.tar.gz                            //删除 jdk-17... 文件
```
<center>可以在根目录下再次使用 <font color=#0B63C7> ls </font> 命令查看 <font color=#D13D2C> jdk-17... </font> 文件是否被删除</center>

#### (3) 配置

配置环境变量使其生效，并添加软连接

```bash
vi etc/profile                  //编辑 profile 文件，配置环境变量
```
1. 打开进入文件后，使用键盘方向键，向下移动到文件底部,然后按键盘 o 键，进入编辑模式。  
2. 复制下面四行代码，将它们粘贴进去。

```bash
#Java
export JAVA_HOME=/usr/local/java/jdk-17.0.7
#export CLASSPATH=.:${JAVA_HOME}/lib        #JDK1.5之后，可以不配置这个变量
export PATH=${JAVA_HOME}/bin:$PATH
```
![](https://s1.ax1x.com/2023/05/12/p9s6omd.png)
3. 然后按键盘ESC键，退出编辑模式。  
4. 最后按 <font color=green>:键</font>加上 <font color=green>w键</font> 和 <font color=green>q键</font>，回车结束。

{% note success no-icon %}
***tips***：如果中间出现误操作，导致文件被意外修改，只需将第四步的 w键和 q键换成 q键和 !键。表示不保存已修改文件，并强制退出。然后重新按照1-2-3-4来操作就可以了。
{% endnote %}

配置好环境变量后再键入以下指令

```bash
source /etc/profile             //重新执行刚修改的初始化文件,使之立即生效
ln -s /usr/local/java/jdk-17.0.7/bin/java /usr/bin/java       //添加软连接，相当于Windows中的快捷方式
java -version                   //查看Java版本
```
<center>不出意外的话，java环境就已经配置好了（如下图）</center>

![](https://s1.ax1x.com/2023/05/12/p9s65OH.png)
<center>o(*￣▽￣*)o</center>
<br></br>

# 二、<font color=green>MCSM</font> 面板

前面所有的准备都做好了之后，就可以安装这个极其方便的MC服务器的管理面板了。以后想开服，启动或者停止服务器，后台管理配置服务器文件等，都可以通过这个面板来操作，十分方便。

这是一个完全免费且开源的项目，大家可以去他们的[官网](https://mcsmanager.com/)看看，觉得好用有实力的也可以赞助一波~

## 2.1 安装

十分朴实无华的安装过程，只需键入以下命令，然后安静等待进度跑完就行。
```bash
wget -qO- https://raw.githubusercontent.com/mcsmanager/Script/master/setup_en.sh | bash
```
<center>安装完成后应该是像下图一样，如果安装不成功，可以<font color=red>多试几次</font>上面的命令。</center>

![](https://s1.ax1x.com/2023/05/14/p9ctajI.png)

<center>我将需要用到的信息截到了下图</center>

![](https://s1.ax1x.com/2023/05/14/p9gi62j.png)

可以看到上图中红色的字提示我们要将 <font color=red>“23333”</font> 和 <font color=red>“24444”</font> 两个端口打开才能正常使用网页服务。

所以我们需要去到你购买服务器的云服务商那里，将防火墙中的两个端口打开。
![](https://s1.ax1x.com/2023/05/14/p9gEpJU.png)

<center>如下图一样添加两个tcp的端口即可</center>

<center class="half">
<img src="https://s1.ax1x.com/2023/05/14/p9gE4X9.png" width = "360">
<img src="https://s1.ax1x.com/2023/05/15/p9gZ1at.png" width = "360">
</center>

然后我们去浏览器的地址栏输入：<font color=red>服务器的公网ip:23333</font>　　然后回车。

<center>这样就进入mcsm的web管理界面了。（如下图）</center>

![mcsm](https://s1.ax1x.com/2023/08/01/pPCQywt.png)

## 2.2 使用

1. 点击开始使用，我们根据提示创建用户，设置密码。（若是觉得开放的这两个端口不太安全，可以去**面板设置**和**节点管理**中修改，但是别忘记在防火墙中开放你修改后的两个端口号）

2. 在网页左侧选项卡中，选择**应用实例**。（如下图）

![](https://s1.ax1x.com/2023/08/01/pPCR9tP.png)

3. 新建实例（下图演示根据红框所选）

（1）先点击新建实例。
![](https://s1.ax1x.com/2023/08/01/pPCWoRK.png)

（2）根据自己所游玩的MC版本，来选择是 *Java版* 还是 *基岩版* ，这里选Java版。
![](https://s1.ax1x.com/2023/08/01/pPCWIG6.png)

（3）此处选择第一个，上传单个服务端软件[^2]。
![](https://s1.ax1x.com/2023/08/01/pPCW5Px.png)

<details>
<summary>  >_<  都被你看光了，你还要看，哼！</summary></br>

官方服务端下载地址：[https://www.minecraft.net/zh-hans/download/server](https://www.minecraft.net/zh-hans/download/server)
![](https://s1.ax1x.com/2023/08/01/pPC4ACq.png)
<center>（一般纯净生存就用这个服务端）</center>
</details></br>

（4）填写实例名称，上传服务端软件，弹出最终确认，问你是否继续，选择确定。
![](https://s1.ax1x.com/2023/08/01/pPC4IGq.png)

4. 上传完成后，前往编辑实例具体参数。

![](https://s1.ax1x.com/2023/08/01/pPCo5id.png)

5. 回到控制台，开启实例，此时服务端就会成功启动了。

![](https://s1.ax1x.com/2023/08/01/pPCoxij.png)

6. 但是会遇到下图情况，这是每个服务器第一次开启时都会出现的。可以看到右下角被红框框住的地方，意思是你得同意它的协议，才能继续运行服务端。此时点击左边的特定配置。

![](https://s1.ax1x.com/2023/08/02/pPCbMyq.png)

7. 点击浏览eula.txt

![](https://s1.ax1x.com/2023/08/02/pPCbkef.png)

8. 将是否同意Minecraft eula协议，更改为是，然后保存。

![](https://s1.ax1x.com/2023/08/02/pPCbAw8.png)

9. 如果想让盗版玩家也可以进入的话，需要在实例特定配置中修改 ***server.properties*** 文件，将其中 <font color=green>正版验证</font> 给改为否，保存后，盗版玩家也可以进入服务器。里面的配置都有中文解释，根据自己需求更改即可。(<u>更改后别忘记保存</u>)

10. 返回控制台，再次开启实例，此时服务器就正式开起来了。

# 三、联机

* 可以选择直接通过直接在多人游戏中，添加服务器，在服务器地址栏中填写 <font color=red>公网ip:端口号[^3]</font> 的方式，来联机。

* 如果觉得一串数字记起来很不方便，可以购买域名，将域名解析到这个ip地址。然后在服务器地址栏中填写 <font color=red>你购买的域名:端口号</font> 的方式，来联机。

* 如果你使用**默认的端口号（25565）**的话，在服务器地址栏中只填写 <font color=red>公网ip</font> 或者 只填写  <font color=red>你购买的域名</font> 不填写端口号也是可以的。

>还有一种域名解析方式可以将你修改过的端口号也隐藏掉，只需输入域名即可联机。因为只是服务器的搭建教程，详细的域名购买及解析问题就不作具体教学了。

</br>
</br>

<font size="5"><center>感谢你能看到这里，(●'◡'●)</center></font>

---

[^1]:官网下载地址：https://www.oracle.com/cn/java/technologies/downloads/
[^2]:MC官方服务端：https://www.minecraft.net/zh-hans/download/server
[^3]:端口号：一般默认都是25565，如果觉得不安全，可以更换，但记得防火墙要开放此端口