title: ubuntu16.04删除当前用户密码文件/etc/shadow后无法进入系统的解决方法
date: 2017-04-08 15:48:02
categories: ubuntu
tags: [ubuntu,搜集整理]
---

### 写在前面
最近项目用到了dspace，在windows中向dspace中导入item时会报解压错误的`error`,后来经过排查，确定是windows平台编码问题导致。于是转战ubuntu，在安装了16.04版本，配置了`java`环境、安装编译器等等一系列工作、差不多可以进行开发的时候，我手贱删除了这个文件`/etc/shadow`，为什么会删除它，这就涉及到安装shadowsocks了，这里不详说。反正是在编辑`/etc/shadowsocks.json`文件时，我觉得`/etc/shadow`可能是个临时文件，就用root权限删除了。这样一删不要紧，重启机器以后傻眼了，输入正确的用户名密码后，无法登陆到图形界面，提示也是说密码不对。类似于下图：
![ubuntu密码错误](http://7xnttb.com1.z0.glb.clouddn.com//558d1f9e0e1dcda9c9001b15c0b4d63d.png)

#### 原因
解决这个问题之前，需要了解下ubuntu登陆图形界面时的用户认证过程。以下是个人理解，可能会有错误，还请指正。
- ubuntu会在`/etc`目录下保存用户的登陆信息，当然是加密后的，但是不像我们认为的那样只有一个文件，而是包含两个文件：`/etc/passwd`、`/etc/shadow`，登陆时ubuntu会自动比对两个文件的信息，如果一直则允许用户登陆，否则就会拒绝。我因为删除了`/etc/shadow`这个文件，所以在比对时会出错。

#### 解决
找到了原因，就很好办了。网上有很多遗忘了ubuntu密码后找回的教程，但是很遗憾，我所能找到的没有一个教程是完全正确的，因为大多数都是互相抄，这种现象确实可悲。以下解决思路是我本人亲测，转载时请附带ubuntu版本信息及出处。
- ~~第一种解决方法：既然我是误删了`/etc/shadow`，那么我想办法在命令行登陆，拷贝`/etc/passwd`文件为`/etc/shadow`不就可以绕过俩文件校验了么？（事实证明这个想法行不通，如果你查看两个文件的内容，你会发现实际上两个文件的内容是不同的，但登陆时具体怎么校验的，没有深入研究过）~~
- ~~第二种方法：想办法在命令行登陆，使用命令行更改用户密码，然后就可以使用原用户名+新密码登陆了（这种方法看似可行，而且命令行中更改密码时也会提示更改成功，但仍然无法登陆图形界面。ps：此时可以在shell中登陆）~~
- 第三种方法：想办法在命令行登陆，然后创建一个新的用户，将老用户文件拷贝到新用户目录下，重启后就可以使用新用户登陆了。（事实证明这种方法可行，唯一的问题是之前用户的某些配置会丢失，这也没办法了。。）

##### 实际操作
可能有人注意到，上面的解决方法都有一点是相同的，**想办法在命令行登陆**。但当机器出现这问题时，`CTRL+ALT+F2~F6`都已经无法进入命令行了。下面说下正确的做法，截图来自网络。
1、开机按ESC，出现如下界面
![ubuntu开机画面](http://7xnttb.com1.z0.glb.clouddn.com//ef0ff36421090d6e1227c8c3cdfabefb.png)

2、按回车键进入如下界面，然后选中有recovery mode的选项
![ubuntu-recovery-mode](http://7xnttb.com1.z0.glb.clouddn.com//5ce0970a7d9d6cc2bcd68b4ae256fb47.png)

3、这一步不要按回车，按下`e`进行编辑状态，找到图中红色框的`recovery nomodeset`并删除，修改`ro`为`rw`（不改的话进入命令行无法输入命令）,并在后面输入`quiet splash rw init=/bin/bash`后，按`F10`或`CTRL+X`重启
![edit-grub](http://7xnttb.com1.z0.glb.clouddn.com//e512c666c1780ade758e424890e8973c.png)
**注意，网上大多数教程就是这一步操作不对，导致重启后要么无法进入命令行，要么无法编辑**

4、重启后如果看到下图，则说明我们可以开始尝试上面提到的三种方法了。
![root-temp](http://7xnttb.com1.z0.glb.clouddn.com//4c37c3c29de17d562411db499f3a3bd1.png)

#### 后记
- 请注意本文全程的解决是基于ubuntu没有开启`root`用户图形界面登陆的前提，关于如何开启root用户登陆，可以自行`google`。如果开启了`root`用户，则以上的讨论都可以省略，因为以`root`用户登陆后，更改用户等操作都不会遇到任何困难。因此强烈建议：`使用ubuntu桌面版时请开启root用户登陆，方便遇到问题后能及时解决。`
- 附上我在测试以上三种方法时，使用的一些`ubuntu`指令：
  + 递归拷贝用户目录：`sudo cp -r sourceFold_path targetFold_path`
  + 递归指定用户目录所有者：`sudo chown user usergroup`
  + 增加用户并且在`/home`目录下生成相应文件夹：`sudo adduser user`，该命令会自动创建用户主目录，创建用户同名的组
  + 修改密码：`sudo passwd`，然后输入两次密码即可。如果要修改特定用户的密码，可以在后面加上用户名 `sudo passwd user`

---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>
