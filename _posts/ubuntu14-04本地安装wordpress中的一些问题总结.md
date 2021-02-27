title: ubuntu14.04本地安装wordpress中的一些问题总结
date: 2015-10-29 18:13:35
categories: wordpress
tags: [wordpress,ubuntu,apache2,收集整理]
---
1、安装apache2的问题
===
当我们使用如下语句安装apache2时：

```
sudo apt-get install apache2
```

会发现启动报错：

```
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'ServerName' directive globally to suppress this message
                                                                         [ OK ]
```

解决方法：

编辑`sudo nano /etc/apache2/apache2.conf`，在最后加上

```
ServerName localhost：80
```

再试试，apache2服务器就可以正常启动了。

_**请注意：apache2的默认配置文件不再是apache的httpd，而是apache2的apache2.conf**_

2、安装wordpress时需要注意的问题
===
wordpress的下载以及配置，需要注意的只有一点：

_wordpress的默认根目录是 **/var/www/html** ，而不是 **/var/www**_

因此，假如你想通过localhost访问时，你的目录结构应该是 **/var/www/html/\***，同理，如果你要通过localhost/wordpress访问时，你的目录结构应该是 **/var/www/html/wordpress/\***



---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>
