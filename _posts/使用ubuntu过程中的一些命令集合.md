title: 使用ubuntu过程中的一些命令集合
date: 2017-06-19 09:57:27
categories: ubuntu
tags: [ubuntu,收集整理]

---

# ubuntu命令备忘
用ubuntu时，经常会用到一些有用的命令，但回头好久不用又忘了。而我又是那种不喜欢查手册的人，百度一下又得费时，所以干脆开篇博客记下来，以后直接来这查好了。

## `ubuntu`使用`tar`命令备份系统命令合集
这里的备份和恢复都是针对同一硬盘下的备份与恢复。

### 备份
- `tar cvpzf backup.tgz --exclude=/proc --exclude=/lost+found --exclude=/backup.tgz --exclude=/mnt --exclude=/sys /`
- 或者是 `tar cvpjf backup.tgz.gz2 --exclude=/proc --exclude=/lost+found --exclude=/backup.tgz --exclude=/mnt --exclude=/sys /`
两条命令的区别在于第二条的压缩率高于第一条。

### 恢复
- `tar xvpfz backup.tgz -C /` or `tar xvpfj backup.tar.bz2 -C /`

## `ubuntu`使用`dd`命令备份硬盘命令合集
这里的备份和恢复可以在不同硬盘下进行，但是要注意硬盘大小的问题，即恢复的硬盘大小不得小于备份的硬盘大小。这些操作请在`live CD`环境下进行。

### 备份
- `fdisk -u -l` 查看磁盘信息
- `dd if=/dev/sda of=/ubuntu/media/xxx/ghost.img` 备份到外接u盘中
- ```kill -USR1 `pgrep ^dd` ```显示上一条命令的进度（请在另外的终端中运行）
- `dd if=/ubuntu/media/xxx/ghost.img of=/dev/sda` 恢复到指定的硬盘中
<tab> kill -USR1 `pgrep ^dd` </tab>

---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>
