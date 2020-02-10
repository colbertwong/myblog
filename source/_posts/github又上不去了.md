---
title: github又上不去了
categories:
  - 技术
tags:
  - github
date: 2020-02-08 10:08:54
---

### github又上不去了

昨天写的Hexo博文，推不到github上了。以为是github又间歇性抽风了，结果今天还是上不去。使用站长[DNS查询工具](http://tool.chinaz.com/dns/)检测了一下`github.com`，几个结果。其中【新加坡 Amazon数据中心】的ip地址ping了一下，毫无意外的timeout了。而【美国 弗吉尼亚州阿什本GitHub】的ip是可以ping通的。
这就好办了，打开本地`hosts`文件，加入映射信息。当然也不能忘了【github.global.ssl.fastly.net】(解决国内访问速度慢的问题)的映射信息。使用[https://www.ipaddress.com](https://www.ipaddress.com)来查询IP地址，应该更准确一些吧。

- hosts文件例

``` ini
#github hosts
192.30.253.113 github.com
199.232.5.194 github.global.ssl.fastly.net
```

- 刷新dns

``` bash
ipconfig /flushdns
```
如果没生效重启电脑

以上
