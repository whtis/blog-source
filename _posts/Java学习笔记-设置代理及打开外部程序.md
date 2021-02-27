title: Java学习笔记--设置代理及打开外部程序
date: 2016-05-29 20:14:59
categories: java
tags: [java,原创,java学习笔记]
---

### 设置系统代理

- 系统设置全局代理
```java
   System.getProperties().setProperty("proxySet", "true");
   System.getProperties().setProperty("http.proxyHost", "ip");
   System.getProperties().setProperty("http.proxyPort", port);
```
- 使用了selenium的chrome驱动，webdriver设置代理
```java
    System.setProperty("webdriver.chrome.driver", "filepath");
    String proxyIpAndPort = ip + ":" + port;

    // 代理配置
    DesiredCapabilities cap = new DesiredCapabilities();
    org.openqa.selenium.Proxy proxy = new org.openqa.selenium.Proxy();

    // 配置http、ftp、ssl代理（注：当前版本只支持所有的协议公用http协议，下述代码等同于只配置http）
    proxy.setHttpProxy(proxyIpAndPort)
            .setFtpProxy(proxyIpAndPort)
            .setSslProxy(proxyIpAndPort);

    // 以下三行是为了避免localhost和selenium driver的也使用代理，务必要加，否则无法与iedriver通讯
    cap.setCapability(CapabilityType.ForSeleniumServer.AVOIDING_PROXY, true);
    cap.setCapability(CapabilityType.ForSeleniumServer.ONLY_PROXYING_SELENIUM_TRAFFIC, true);
    System.setProperty("http.nonProxyHosts", "localhost");

    cap.setCapability(CapabilityType.PROXY, proxy);

    WebDriver webDriver = new ChromeDriver(cap);
```

### 打开外部程序
```java
   Process p = Runtime.getRuntime().exec("cmd command");
```
**如果外部程序放在一个单独的线程中执行，需要注意的是：该线程执行完并不代表着外部程序也执行完相应的任务。**

---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>