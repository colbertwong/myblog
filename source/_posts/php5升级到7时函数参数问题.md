---
title: php5升级到7时函数参数问题
date: 2020-02-06 22:51:05
categories:
- 技术
tags:
- php
- 升级
---

## 从php5.2升级到php7.1时函数的参数问题

最近在做一个系统的ID管理系统php升级，从php5.2升级到php7.1,经常会遇到一些问题。今天遇到一个小问题，拿出来分享一下。废话少说，上示例代码。

- SC.php

``` php
<?php
class SC {
        static function out($str) {
            echo 'input str:' . $str;
        }
    }

    // 有参数调用
    SC::out('one param');
    // 无参数调用
    SC::out();
```

- php5.2中的实行结果：

```
input str:one param
Warning: Missing argument 1 for SC::out(), called in C:\Users\colbe\Documents\SC.php on line 11 and defined in C:\Users\colbe\Documents\SC.php on line 4
input str:
```

- php7.1中的实行结果：

```
input str:one param
Fatal error: Uncaught ArgumentCountError: Too few arguments to function SC::out(), 0 passed in C:\Users\colbe\Documents\SC.php on line 11 and exactly 1 expected in C:\Users\colbe\Documents\SC.php on line 4

ArgumentCountError: Too few arguments to function SC::out(), 0 passed in C:\Users\colbe\Documents\SC.php on line 11 and exactly 1 expected in C:\Users\colbe\Documents\SC.php on line 4

Call Stack:
0.0069 348536 1. {main}() C:\Users\colbe\Documents\SC.php:0
0.0088 348568 2. SC::out() C:\Users\colbe\Documents\SC.php:11
```

Warning变Fatal，直接就崩了。修改倒是很简单，给函数的参数加个默认值就可以了。修改一下方法的签名部分为`[static function out($str = null) {]`就可以了。

- 再次运行，结果如下：

```
input str:one paraminput str:
```

PS:来自TimJuly【技术小组长 @ 阿里巴巴】的重要提示：

    说白了就是代码不规范。Warning 是错误的一种类型，
    并不是说你这个没有错，可以这样用。就算是 Notice
    的错误也应该处理完再上线。

以上