title: 将百度搜索结果的长url链接转换为真实的url地址
date: 2016-09-19 00:37:15
categories: java
tags: [java，原创]
---
### 写在前面
最近写爬虫的时候，想调用百度的搜索结果，于是有了这个问题。需要注意的是，本篇内容转化是仅针对百度跳转到非百度链接的，百度内部链接的跳转，如百度搜索主入口跳转到百度知道、百度文库等情况不再讨论之列。

### 问题描述
- 百度搜索入口：`https://www.baidu.com/s?wd=`,后面加上需要的搜索内容即可。如需要进行更加精确的搜索，可以使用百度的[高级搜索](http://imgs.xinhuanet.com/js/baidu/gj.htm)功能。
- 百度搜索结果示例：`https://www.baidu.com/link?url=NsJrxOylR5il9kO2p7IJZu9RdvqdWgobSo1W7n29FIu&amp;wd=&amp;eqid=e4c01daf0006800b0000000357debb6c`,实际的url为`www.whtis.com`。
- 原因：百度为什么要实行加密跳转，网上有很多有意思的文章，感兴趣可以看看。这里仅从技术角度进行说明。</br>
百度的这种跳转采用的是302重定向。虽然有时候状态码不是302，但原理差不多一样：当你点击了百度的加密链接后，会向百度内部的服务器发送请求，这个时候服务器会根据你提交的百度链接使用重定向技术自动跳转到相应的真实url。

### 拿到真实的URL链接
网上有很多博客都提到过这事，既然知道了302跳转的原理，那么可以采用这种思路拿到真实的URL：模拟一个请求，在百度服务器进行重定向的时候获得重定向的地址即可。</br>
原理很简单，但是我在网上找了很多，基本都是互相抄，抄的代码怎么说呢，用的工具都是httpclient，但是都是httpclient已经废弃的方法，虽然不报错，但是没法实现这功能了。所以这也告诉我们写博客的人，一定要注明自己的代码基于的是哪个版本的工具，虽然这只是一点小事，但却能让自己和他人节约很多时间。</br>
下面上代码：

**使用版本：httpclient 4.3.3**
```java
public static String getRealUrl(String url) {

        String realUrl = "";

        HttpClient client = HttpClientBuilder.create().build();
        RequestConfig config = RequestConfig.custom()
        //禁止自动重定向
                .setRedirectsEnabled(false)
                .build();
//302跳转
        HttpGet get = new HttpGet(url);
        get.setConfig(config);
        try {
          //对url进行utf-8解码
            url = URLDecoder.decode(url, "utf-8");
            HttpResponse rs = client.execute(get);
            Header h = rs.getFirstHeader("Location");
            realUrl = h.getValue().trim();
        } catch (IOException e) {
            e.printstacktrace();
            return url;
        }
        if (realUrl.equals("")) {
            return url;
        } else {
            return realUrl;
        }
    }
```





---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>
