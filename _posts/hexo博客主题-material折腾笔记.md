title: hexo博客主题--material折腾笔记
date: 2016-11-20 20:56:17
categories: hexo
tags: [hexo,原创]
---

&nbsp;&nbsp;好久没怎么照看博客了，昨天突然发现，以前用的`Next`主题挂了。因为用的别人的东西，不知道问题。本着折腾的想法，重新换了一个主题`material`。该主题算是满足了我大部分的需求吧。但还是有吐槽的地方……（虽然不会写js，不妨碍我吐槽。。原作者看到估计会气的吐血）

#### 添加谷歌和百度统计代码
按照[使用文档](https://material.vss.im/expert/)可以自行添加代码，但我添加百度统计代码时报错，所以直接把百度统计代码写到`layout/_partial/head.ejs`文件中了。

#### 在文章详情页添加回主页的方法
- 可以直接点击缩略图返回首页
- 分享按钮下增加回主页

#### 此主题的一些想法
- 好的方面
  + 背景支持bing图片，这个想法点赞
  + 每篇文章都提供缩略图`thumbnail`的设置，很棒
- 可以改进的地方
  + 侧边栏很不完美。包括样式和调出方式（鼠标点击改为鼠标滑过会好很多，尝试过更改，然而太菜，没有找到正确修改的方法）
  + 缺少文章分类页，看github上的issue，感觉作者根本没想到这个需求，但我之前用的主题都有，强迫症伤不起

突然想到，改了作者的文件，以后`pull`的时候又有得折腾了。anyway，自己菜就得忍受这些问题了。

---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>
