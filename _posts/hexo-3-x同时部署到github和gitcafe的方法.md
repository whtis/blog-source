title: hexo 3.x同时部署到github和gitcafe的方法
date: 2015-11-03 11:24:55
categories: hexo
tags: [hexo,原创]
---

详细的hexo博客搭建教程可以参考[这里](http://ibruce.info/2013/11/22/hexo-your-blog/)

在我按照教程搭建博客的过程中，无意中看到了可以同时将博客部署到`github`和`gitcafe`上，以提高国内外的访问速度，具体操作过程可以参考[这里](http://opiece.me/2015/04/28/push-hexo-to-github-and-gitcafe/?utm_source=tuicool&utm_medium=referral)

当按照教程修改hexo的配置文件`_config.yml`后，第二个repo里并没有更新代码，经过查阅[官方文档](https://hexo.io/docs/deployment.html)，`_config.yml`正确的配置如下：

```
deploy:
- type: git
  repo: git@github.com:yourname/yourname.github.io.git,master
- type: git
  repo: git@gitcafe.com:yourname/yourname.git,gitcafe-pages
```

现在使用`hexo d`，应该就没问题了。

**注意，type前面的`-`不能省略。**

---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>