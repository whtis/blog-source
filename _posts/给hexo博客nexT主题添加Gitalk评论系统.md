title: 给hexo博客nexT主题添加Gitalk评论系统
date: 2017-10-19 14:58:47
categories: hexo
tags: [hexo，Gitalk，nexT]

---

## 写在前面

**注意，此插件基于github开发，授权阶段需要获取的权限不局限于`issues`，还包括编辑你代码的权限，因此对这应用授权时需要十分小心**
![authorization](authorization.jpg)

前段时间因为`hexo`的`nexT`主题崩了（载入博客时显示全白，[原因在这](https://www.zhihu.com/question/52455581)），换了`material`主题，这主题很有安卓的风格，我也写过对它的一些折腾：[hexo博客主题--material折腾笔记](hexo博客主题--material折腾笔记)。
这些都不是重点，重点的是今年6月份，多说离开了。为此我专门注册了`disqus`，搭配着`material`主题进行使用。使用期间，让我很郁闷的是连黄图哥都懒得评论QAQ，再加上前几天博客莫名的多出来了很多有规律的访问（没错，是爬虫），所以为了防止攻击，我又回归了`nexT`的怀抱。
下面主要记录下我将评论系统`Gitalk`集成到`nexT`的过程。

### 为什么选择`Gitalk`

多说消失后，原先与多说竞争最激烈的`disqus`看起来似乎是最大赢家。然而在天朝的环境中，它没有像想象中那样一跃成为主流的评论系统。而在国内，随着多说的消失，`畅言`、`网易云跟帖`等等产品都跃跃欲试，想取代多说。然而现实是，网易云跟帖活了不到两个月官方就宣布不再为第三方博客提供服务，畅言虽然很活跃，但鉴于国内的大环境，我还是决定寻找一款国外的评论系统。
`nexT`主题集成了很多第三方评论系统，其中有一款很和我意：`gitment`。它由国内大神编写，基于`GitHub Issues`编写。详细的内容可以[看这里](https://imsun.net/posts/gitment-introduction/)。然而不幸的是，作者似乎弃坑了，不再进行维护，而且查看`issues`发现`gitment`在移动端貌似使用不了。
然后，我就发现了`Gitalk`，这个评论系统和`gitment`类似，且有人一直在维护，移动端也可以使用。因此我决定模仿`gitment`将`Gitalk`集成到`nexT`主题中。

### `Gitalk`和`nexT`主题目录的简要介绍

在实际修改之前，我们先了解下Gitalk和nexT项目的目录文件，这方便我们理解后续集成的思路和使用的方法。

#### Gitalk 简介

> Gitalk 是一个基于 Github Issue 和 Preact 开发的评论插件。
- 特性：
  + 使用 Github 登录
  + 支持多语言 [en, zh-CN, zh-TW, es-ES, fr]
  + 无干扰模式（设置 distractionFreeMode 为 true 开启）
  + 快捷键提交评论 （cmd|ctrl + enter）

以上内容摘自github，详细的内容请查看[Gitalk中文文档](https://github.com/gitalk/gitalk/blob/master/readme-cn.md)。

#### nexT 目录说明

nexT 主题的源码结构非常清晰，下面对目录树进行简要说明。**注意：主题迭代频繁，博主使用的版本是5.1.3，开发前请确认自己使用的版本目录树是否与本文所写一致**

```
├── languages            博客语言配置
├── layout               博客的整体布局
│   ├── _custom          用户自定义配置文件(仅支持对主界面布局进行修改)
│   ├── _macro    
│   ├── _partials        博客组件的配置
│   ├── _scripts
│   └── _third-party     第三方插件
├── scripts
│   └── tags
├── source               博客使用的样式目录
│   ├── css              
│   ├── fonts
│   ├── images
│   ├── js
│   └── lib
└── test
```

### 集成的步骤

如果要达到我们最初的目的，需要修改以下内容：
- `layout/_partials/comments.swig`
- `layout/_third-party/comments/index.swig`
- `_config.yml`
还需要在`layout/_third-party/comments/`路径下，添加我们自己的文件`gitalk.swig`。下面我们一步一步来。

#### 编写我们自己的插件文件`gitalk.swig`

```
{% if not (theme.duoshuo and theme.duoshuo.shortname) and not theme.duoshuo_shortname %}
{% if theme.gitalk.enable and theme.gitalk.client_id %}
<!-- LOCAL: You can save these files to your site and update links -->
  {% set CommentsClass = "Gitalk" %}
  <link rel="stylesheet" href="https://unpkg.com/gitalk/dist/gitalk.css">
  <script src="https://unpkg.com/gitalk/dist/gitalk.min.js"></script>
<!-- END LOCAL -->

    {% if page.comments %}
      <script type="text/javascript">
      function renderGitalk(){
        var gitalk = new {{CommentsClass}}({
            owner: '{{ theme.gitalk.github_user }}',
            repo: '{{ theme.gitalk.github_repo }}',
            clientID: '{{ theme.gitalk.client_id }}',
            clientSecret: '{{ theme.gitalk.client_secret }}',
            admin: '{{ theme.gitalk.admin }}',
            {% if theme.gitalk.distractionFreeMode %}
              distractionFreeMode: '{{ theme.gitalk.distractionFreeMode }}'
            {% endif %}
            });
        gitalk.render('gitalk-container');
      }
      renderGitalk();
      </script>
    {% endif %}

{% endif %}
{% endif %}
```

#### 把编写好的插件添加到hexo主题中

在`layout/_third-party/comments/index.swig`中添加下面语句：
```
{% include 'gitalk.swig' %}
```

#### 根据条件判断是否启用该插件

在`layout/_partials/comments.swig`中相应的地方加入如下语句：
```
{% elseif theme.gitalk.enable %}
    <div class="comments" id="comments">
      <div id="gitalk-container"></div>
    </div>
```

#### 在主题配置文件中配置有关`Gitalk`的信息

编辑`_config.yml`文件，在需要配置`gitalk`的地方加入如下内容：

```
gitalk:
    enable: true
    github_user: # owner
    github_repo: # repo
    client_id: # clientID
    client_secret: # clientSecret     
    admin: # Github repository collaborators. (Ensure having write access to this repository)
    distractionFreeMode: # Facebook-like distraction free mode
```

至此，大功告成。**但需要注意的是：** 当使用`hexo s -d`本地调试时你可能会看到

```
未找到相关的 Issues 进行评论
请联系 @whtis 初始化创建
```

出现了这个提示不要紧，你只要部署后，使用github登录就好了。

## 博客的其他修改

- 因为很喜欢`material`主题同步`bing`每日壁纸的功能，所以也把hexo的背景图片与bing壁纸进行同步。实现这个功能，只需要编辑`source/css/_custom/custom.styl`文件，加入如下内容即可：
```css
html, body {
  height: 100%;
  background-image: url("http://api.dujin.org/bing/1920.php");
  background-repeat: no-repeat;
  background-attachment:fixed;
  background-position:50% 50%;
  background-size: cover;
}
```

其中，image的url是其他博主提供的[api接口](https://www.dujin.org/fenxiang/jiaocheng/3618.html)，目前看来还是很稳定的。后续也可以考虑自己抓取bing的壁纸，有关这部分内容，可以参考这篇博文:[获取Bing每日图片API接口](http://www.cnblogs.com/liujianshe1990-/archive/2017/10/07/7635339.html)

- 将`post`、`sidebar`和`header-inner`透明度调成80%，这是为了较好的突出背景。实现方法：编辑`source/css/_custom/custom.styl`文件，加入如下内容即可：

```css
.header-inner {
  opacity: 0.8;
}

.main-inner {
  opacity: 0.8;
}

.sidebar {
  opacity: 0.8;
}
```

- 其他修改。参考文章见文末。

## 后记
看了`next`的`issues`发现已经有人在两个月前`Pull requests`了，但还在`open`状态，怪不得我`clone`的时候找不到这个插件。

~~最后，有同学如果也研究过hexo的，欢迎教我怎么把**文章那块（`post-block`）调成半透明的**。。。楞是没有找到修改哪里才能生效⊙﹏⊙~~

最后的最后，附一个比较全面的hexo折腾总结：[hexo的next主题个性化教程：打造炫酷网站](http://blog.csdn.net/qq_33699981/article/details/72716951)


---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>
