---
title: url被截断问题
date: 2020-02-06 22:44:27
categories:
- 技术
tags:
- url
- php
---

## 在URL中使用另一个url作为参数时会被`&`截断的问题

下午帮同事写一个url转二维码的小工具时，发生标题中描述的问题。比如：输入的url是`http://www.example.dev/name=user&code=2000`,转换为二维码后，扫描得到的url却是`http://www.example.dev/name=user`,`&`后的部分没有正确转换。问题很明显，作为参数的url中的`&`后的被解析为其他参数了。

前台请求的完整url是`"http://{domain}/tools/getQrCode.php?url=http://www.example.dev/name=user&code=2000`，后台从`$_GET['url']`中取得却是`http://www.example.dev/name=user`，另一部分成了`$_GET['code']`了。


解决方法其实很简单，给作为参数的url做一下urlencode就好了。js中可以直接使用`encodeURIComponent`函数为url编码。


#### 代码示例如下：

* 后台php

``` php
<?php

$url = isset($_GET['url']) ? $_GET['url'] : '';
if (empty($url)) {
    echo '<div class="alert alert-danger" role="alert">请输入url！</div>';
    exit(1);
}
echo genQrCode($url);
exit(0);

function genQrCode($url=''){
    require_once 'libs/phpqrcode.php';
    $value = $url;                  //二维码内容

    $errorCorrectionLevel = 'L';    //容错级别
    $matrixPointSize = 5;           //生成图片大小

    //生成二维码图片
    $filename =  '/../tmp/'.microtime().'.png';
    QRcode::png($value, dirname( __FILE__ ) . $filename, $errorCorrectionLevel, $matrixPointSize, 2);

    return '<img src="tmp'. $filename . '"><br><p>' . $url . '</p>';
}
```

* 前台html和js


``` html
之前的部分略
        <div class="col-md-6 col-lg-6">
            <div class="panel panel-warning">
                <div class="panel-heading">二维码转换</div>
                <div class="panel-body">
                    <form>
                        <div class="form-group">
                            <label for="url">url</label>
                            <input type="text" class="form-control" id="url" placeholder="url">
                        </div>
                        <button type="button" id="getQr" class="btn btn-primary">生成二维码</button>
                        <button type="reset" class="btn btn-success">清除url</button>
                    </form>
                </div>

                <div id="qrdiv" class="panel-body" style="word-break: break-all;">

                </div>
            </div>
        </div>
    </div>
</div>
<script type="text/javascript">
    $(
        $("#getQr").click(function () {
            $qrstr=$("#url").val();
            $url = "tools/getQrCode.php?url=" + encodeURIComponent($qrstr);
            $.get($url, function (result) {
                $("#qrdiv").html(result);
            });
        })
    );
</script>
</body>
</html>
```


PS：样式部分使用的bootstrap3
以上