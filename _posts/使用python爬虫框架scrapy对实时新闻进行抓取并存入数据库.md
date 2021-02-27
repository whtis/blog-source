title: 使用python爬虫框架scrapy对实时新闻进行抓取并存入数据库
date: 2017-07-28 22:15:20
categories: python
tags: [python，scrapy，mysql，新闻抓取]
---

### 写在前面
每天的新闻更新很快，如果要全面了解非常困难，更可恶的是一些门户网站还经常取一些乱七八糟的标题，点进去是文不对题。所以萌生了一个想法：自己抓取不同门户网站的新闻更新信息，然后将这些内容进行整合，推送一些当日的热点新闻。
想的很简单，真的做起来，发现自己还是太年轻，到写这篇博客为止，我也就是完成了一个基本的抓取框架，连内容都没有获得多少。不过本来就是抱着学习`python`的想法使用`scrapy`来抓取新闻，既然已经有了个差不多的框架，也该写写使用`scrapy`过程中的遇到的一些问题了。

### 明确需求和技术路线
- 需求：很简单，抓取每天最新的新闻内容（文本），按照一定的格式存入数据库`mysql`中。
- 技术路线：之前一直在使用`java`语言开发的`webmagic`开源爬虫框架进行爬虫开发。但一直听说`python`下有个大名鼎鼎的爬虫框架`scrapy`,所以抱着学习的态度，使用`scrapy`进行爬虫的开发。

### 简单的技术介绍和入门
之前只是使用`python`写过一些简单的脚本，这次为了使用`scrapy`,看了一遍**廖雪峰大大**的`python3`[教程](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000)
看完后没记住多少，不过有个概念就是了。然后开始查看一些讲`scrapy`入门的博客，自己实现博客中的例子，算是对`scrapy`有个大概的了解。我复现的例子是这个：[【图文详解】scrapy安装与真的快速上手——爬取豆瓣9分榜单](http://www.jianshu.com/p/fa614bea98eb)

### 需求的再分析及让`scrapy`与需求相关
- 需求分析：新闻内容很多，获取的方式也不同。最笨的方式就是复现浏览器的行为：针对不同的信息源获取不同的页面进行解析；其次好一点的方法：由于某些新闻网站提供了rss订阅接口，因此可以解析此接口页面获得内容。下面分析下这两种方法的优缺点：
  + 模拟浏览器行为：
    * 优点：能够精准解析到需要的内容
    * 缺点：解析难度高，主流新闻网站都有一系列反爬虫机制；代码与页面结构高度相关，如页面改版，代码改动较大
  + rss接口：
    * 优点：不同网站的接口页面结构基本相同，可以使用一套解析逻辑进行解析
    * 缺点：并不是所有网站都会提供该接口，且该接口返回的数据，有些仅有正文摘要，还需要再进一次网站获取完整的正文内容
- 明确需求下的`scrapy`:上一节提到的入门内容如果看完后，应该对`scrapy`会有一个大概的整体认识。在我这需求下，似乎单独创建一个爬虫无法完成所有的需求。因此我创建了两个爬虫文件：`rss_news_spider.py`和`web_news_spider.py`，下面会对这两个文件进行详细解释。

### 开始写爬虫

#### 使用scrapy基础模板来爬取rss接口的新闻内容

终于，振奋人心的时刻到了。可以开始我们的爬虫了。首先我们创建`runRss.py`,然后写上如下启动爬虫的代码：

```python
# -*- coding: utf-8 -*-

# run rssSpider
from scrapy import cmdline

cmdline.execute("scrapy crawl rss".split())
```

是不是很熟悉，对，这条命令启动了名为`rss`的爬虫（**注意，在scrapy中创建的每一个爬虫必须有自己唯一的`name`，否则无法启动**）。我的`rss`爬虫位于`rss_news_spider.py`这个文件当中：
```python
class RssSpider(scrapy.Spider):
    name = 'rss'
    start_urls = [
        'http://www.wyzxwk.com/e/web/?type=rss2&classid=0',
    ]

    def parse(self, response):
        if response.url.find('rss') != -1:
            yield Request(response.url, callback=self.parse_rss)
        elif response.url.find('Article') != -1:
            yield Request(response.url, callback=self.parse_details)
        else:
            self.logger.info('start urls is not useful, please check it!')

    def parse_rss(self, response):
        selector = scrapy.Selector(response)
        contents = selector.xpath('//channel/item')
        for content in contents:
            title = content.xpath('title/text()').extract()[0]
            link = content.xpath('link/text()').extract()[0]
            ……
            article = ArticleItem()
            article['table_name'] = 'article_' + table_name.lower()
            article['title'] = title
            article['url'] = link
            if link is not None:
                yield scrapy.http.Request(url=link, meta={'item': article}, callback=self.parse_details)

    def parse_details(self, response):
        article = response.meta['item']
        selector = scrapy.Selector(response)
        content = selector.xpath('//div[@class="m-article s-shadow"]/article/p').extract().__str__()
        content = content.replace("', '", "").replace("\\u3000", "  ").replace("'", "").replace("\\xa0", "")
            .replace("▍", "")
        article['content'] = content
        return article
```

来认真看下这段代码：
- 设置`name`、`start_urls`和`parse`函数我们都很熟悉。那`parse_rss`和`parse_details`是干嘛用的？
  * 在本文的例子中，我们用了[乌有之乡](http://www.wyzxwk.com/)的`rss`接口作为例子。这个`rss`接口的特点是给出的正文内容是 摘要，如果要查看详细的正文内容需要访问相应的`url`,因此两个函数的作用就非常明显了：`parse_rss`函数是处理接口页面的信息并获取正文的链接，然后使用`parse_details`函数对正文做解析。
  * 这个地方比较重要的是函数之间如何传递参数？也就是我想把`parse_rss`中解析到的内容传递给`parse_details`。从代码中可以看出，我们在传递链接的时候使用的`scrapy.http.Request`中可以附加参数：`url`是链接；`meta`是一个字典，可以包含所有希望下一个函数获得的内容；`callback`是回调函数，用于指定这些内容由哪个函数进行处理。
- `start_urls`中注释的url是干啥用的？
  * 如我们在需求分析中所说，rss接口页面结构是相同的，因此理论上来说，在`parse_details`之前的逻辑，适用于任何实现了rss接口的网站。

现在有个问题？如果我爬取的网站比较多，那么`parse_details`的逻辑我是不是要根据不同的网站去分别实现？按道理来说是这样的，但`scrapy`还为我们提供了另外一种实现思路。下面的内容主要参考了如下几篇博客：
- [Python爬虫框架Scrapy教程(1)—入门](http://www.code123.cc/1431.html)
- [Python爬虫框架Scrapy教程(2)—动态可配置](http://www.code123.cc/1432.html)
- [Python爬虫框架Scrapy教程(3)—使用Redis和SQLAlchemy](http://www.code123.cc/1434.html)

#### 读取数据库配置动态生成爬虫
我们知道，爬虫的核心其实就是发送请求与接收请求。那么如果我们要从网络上获取到新闻内容，浏览器发送这两个请求就可以：
1、请求新闻的列表页，解析出每一篇新闻的详细链接
2、访问每一篇新闻的详细链接，然后解析返回的内容，就可以得到新闻的标题、发表时间、正文等内容了。
（**以上两步其实是爬虫领域很经典的列表+详情页爬取逻辑**）
在这个过程中，浏览器（或者代码）的执行步骤是相同的，不同的只是访问的链接，以及对不同链接内容的解析（主要是`xpath`和`正则表达式`）。因此假设访问的链接和解析方法都能够动态加载（从数据库中读取），那么我们就可以写一个爬虫来加载这些配置，实现多个网站内容的抓取了。
我们创建一个`runWeb.py`文件，写入下面内容：
```Python
# run webSpider
settings = Settings()
settings.set("ITEM_PIPELINES", {
    # 'pipelines.DuplicatesPipeline': 200,
    # 'pipelines.CountDropPipline': 100,
    'pipelines.DataBasePipeline': 300,
})
# crawl settings
settings.set("USER_AGENT",
             "Mozilla/5.0 (Windows NT 6.2; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/32.0.1667.0 Safari"
             "/537.36")

process = CrawlerProcess(settings)

db = DBSession()
rules = db.query(Rule).filter(Rule.enable == 1)
for rule in rules:
    process.crawl(WebSpider, rule)
process.start()
```

加载完自定义`settings`后，对每一个`rule`生成一个进程，运行`webSpider`。我的`webSpider`在`web_news_spider`中。可以看下这里面的代码：
```python
class WebSpider(CrawlSpider):
    name = "web"

    def __init__(self, rule):
        self.rule = rule
        self.name = rule.site_name
        self.allowed_domains = rule.allow_domains.split(",")
        self.start_urls = rule.start_urls.split(",")
        rule_list = []
        # 添加`下一页`的规则
        if rule.next_page:
            rule_list.append(Rule(LinkExtractor(restrict_xpaths=rule.next_page)))
        # 添加抽取文章链接的规则
        rule_list.append(Rule(LinkExtractor(
            allow=[rule.allow_url],
            restrict_xpaths=[rule.extract_from]),
            callback='parse_item', follow=True))
        self.rules = tuple(rule_list)
        super(WebSpider, self).__init__()

    def parse_item(self, response):
        self.log('Hi, this is a web page! %s' % response.url)

        content = infos.xpath(self.rule.content_xpath).extract()
        article["content"] = content[0] if content else ""

        publish_time = infos.xpath(self.rule.publish_time_xpath).extract()
        article["publish_time"] = publish_time[0] if publish_time else ""
        ……
```

其中`__init__`函数很关键，除了从数据库读取配置外，它使用`Rule(LinkExtractor(……))`指定了对新闻列表页的解析规则：`allow`指定了加入下载队列中url的格式；`restrict_xpaths`指定了寻找url的页面范围;`callback`指定了处理后续url（新闻详情页）的方法。
- `allow`和`restrict_xpaths`从`正则表达式`和`xpath`共同对后续帖子详情页url进行限制；
- `callback`指定的是解析详情页的方法，顾名思义，列表页的处理是由爬虫自己处理的（使用参数，这也是我没搞懂的地方，因为不是自己处理，在爬虫无法抽取详情页url时很难定位到哪里出错了）

如果你的`allow`和`restrict_xpaths`设置无误，且列表页返回的信息很完整，你就可以使用`parse_item`函数制定解析详情页的逻辑了。
**1、初始接触这种方法时，极其建议使用已经有的例子进行学习，上面提到的博客中的代码，在本文书写时仍能正常运行**
**2、这种方法对动态加载的列表页是无效的**

### 把抓取到的内容存入数据库

网上大部分使用的`mysql`驱动是`mysqlDb`，但是它不支持`python3.x`。因此本文使用的是`pymysql`,`orm`框架是[`SQLAlchemy`](https://www.sqlalchemy.org/)
本文仅对一些缺少资料地方进行说明：
- `SQLAlchemy`连接数据库的方法（参考自官方文档）：
```python
# 初始化数据库连接:
engine = create_engine('mysql+pymysql://user:passwd@localhost:3306/spider?charset=utf8mb4')

# 创建DBSession类型:
DBSession = sessionmaker(bind=engine)
```

需要注意的是，初始化`engine`时需要指定数据库。本文使用的代码，数据库信息存储在`scrapy`的默认`settings`中。
- SQLAlchemy存储信息时动态指定表名
我们有这个一个需求，不同的网站内容我们需要存储到不同的表当中。但学习`SQLAlchemy`用法的时候，我们可以看到，在创建`model`类时就必须指定`__tablename__`,否则无法存储。
```python
class ArticleModel(Base):
    __tablename__ = 'none'

    aid = Column(Integer, primary_key=True)
    title = Column(String)
    url = Column(String)
    ……

```
那我们总不能创建多个除了`__tablename__`不同其他都相同的`modle`类吧。还好，我们可以使用如下方法在向数据库插入数据的时候（pipeline中）动态指定表名：
```python
ArticleModel.__table__.name = item['table_name']
a = ArticleModel(title=item["title"].encode("utf-8"),
                 url=item["url"],
                 ……
                ）
```

### 其他
本来以为`mysql`中的`timestamp`对应于`python`中的`date`类型，今天向数据库中插入时间戳的时候，才发现对应的是`str`类型。
```python
pub_float = time.mktime(time.strptime(pubDate, '%a, %d %b %Y %X %z'))
timestamp = time.strftime('%Y-%m-%d %X', time.localtime(pub_float))
```
>>> type(timestamp) → str

### 后记
本文所写内容为这段时间学习成果之记录，开头所提到的需求离实际完成还很远。文中提到的第二种方法在具体到本文中需求中使用时并没有成功，具体原因由于本人水平所限，并未查出来。如果有对这方面感兴趣的可以一同研究学习。本文提到的完整代码可以在我的[github](https://github.com/whtis/news_scrapy)上找到。





---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>
