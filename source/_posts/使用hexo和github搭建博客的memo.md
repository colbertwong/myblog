---
title: 使用hexo和github搭建博客的memo
date: 2020-02-07 13:17:06
categories:
- 备忘
tags:
- blog
- hexo
- github
---

## 使用hexo和github搭建博客的memo

### 感谢

搭建本博客时，基本是照搬这篇文章[hexo+github搭建博客(超级详细版，精细入微)](https://m.aliyun.com/yunqi/articles/742964?spm=5176.11156470.0.0.11c916075ld9TP)来的，感谢作者[YangAir](https://yafine-blog.cn)。

### MEMO

#### 环境搭建

Node.js和git是常用软件，基本电脑上都安装过了。

- Node.js不要使用旧版本。否则在开启 gitalk 评论模块时，会发生 `Error: Not Found.` 异常。
- npm包全局命令目录也要加入`path`环境变量。可参考[jyjin的node环境变量配置，npm环境变量配置](https://blog.csdn.net/jianleking/article/details/79130667)
- npm镜像源改为淘宝的

#### Github Pages创建

先注册GitHub，再创建Github Pages

- Github Pages的仓库：username.github.io 我的 username 是 colbertwong，所以我的仓库名就是 colbertwong.github.io

#### 安装hexo静态博客框架

[Hexo中文官网](https://hexo.io/zh-cn/)

``` bash
# hexo框架的安装
npm install -g hexo-cli
# 等上一个命令完成后，在输入下面的命令
hexo init <新建文件夹的名称>  #初始化文件夹
cd <新建文件夹的名称>
npm install  # 安装博客所需要的依赖文件
# 运行监测
hexo g
hexo s
```
浏览器中打开http://localhost:4000/，可以看到Hexo博客网页

#### 本地博客和Github Pages的关联

- 安装发布用插件

``` bash
npm install hexo-deployer-git --save
```

- 将本地目录与GitHub关联起来

``` bash
ssh-keygen -t rsa -C "你的邮箱地址"
```
生成的公钥（id_rsa.pub）与Github（点击右上角的头像 Settings 选择SSH and GPG keys）关联。

- 测试一下是否与GitHub连接成功

``` bash
ssh -T git@github.com
```

#### 本地博客设置

- 基本设置（/_config.yml）

``` yml
# Site
title: 半岛愚人
subtitle: 半岛有愚人，不惑仍不知。
description: ''
keywords:
author: ColbertWong
language: zh-CN
timezone: ''

# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: https://colbertwong.github.io
root: /
permalink: :year/:month/:day-:hour:minute.html
permalink_defaults:
pretty_urls:
  trailing_index: true # Set to false to remove trailing 'index.html' from permalinks
  trailing_html: true # Set to false to remove trailing '.html' from permalinks
```

- deploy设置（/_config.yml）

``` yml
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: git@github.com:colbertwong/colbertwong.github.io.git
  brabch: master
```

- 生成页面，并发布至Github Pages，执行如下命令：

``` bash
# Hexo会根据配置文件渲染出一套静态页面
hexo g
# 将上一步渲染出的一系列文件上传至至Github Pages
hexo d
# 也可以直接输入此命令，直接完成渲染和上传
hexo g -d
```
上传完成后，在浏览器中打开https://<用户名>.github.io，查看上传的网页。

#### hexo博客主题安装

应用 [matery](https://blinkfox.github.io/2018/09/28/qian-duan/hexo-bo-ke-zhu-ti-zhi-hexo-theme-matery-de-jie-shao/) 主题

- 切换主题 ok
- 新建分类 categories 页 ok
- 新建标签 tags 页 ok
- 新建关于我 about 页 ok
- 新建友情连接 friends 页（可选的）启用
- 代码高亮 ok
- 搜索 ok
- 中文链接转拼音（可选的） 未启用
- 文章字数统计插件（可选的） 未启用
- 添加 RSS 订阅支持（可选的） 未启用
- 修改页脚 ok
- 修改社交链接 ok
- 修改打赏的二维码图片 未启用
- 配置音乐播放器（可选的） 未启用
- 更换favicon 和 Logo ok
- 开启 GitTalk 评论模块（留言板也依赖该模块） ok
- 在github上创建Gitalk用仓库后，一定要初始化issues,就是写一条issues。

#### 其他一些DIY

- 修改原有相册[传送门](https://liyangzone.com/2019/07/22/hexo博客添加一级分类相册/) 未修改
- 添加天气小插件 未添加
```
首先去中国天气官网：https://cj.weather.com.cn/plugin/pc，配置自己的插件，
选择自定义插件—>自定义样式——>生成代码，然后会生成一段代码，
复制粘贴到 themes/matery/layout/layout.ejs即可。
```
- 自定义404页面 未修改
首先再站点根目录下的source文件夹下新建404.md文件，里面内容如下：
```
---
title: 404
date: 2019-10-28 16:41:10
type: "404"
layout: "404"
description: "Oops～，我崩溃了！找不到你想要的页面了"
---
```
紧接着再新建主题文件夹的layout目录下新建404.ejs文件，添加内容如下：
``` js
<style type="text/css">
    /* don't remove. */
    .about-cover {
        height: 90.2vh;
    }
</style>
<div class="bg-cover pd-header about-cover">
    <div class="container">
        <div class="row">
            <div class="col s10 offset-s1 m8 offset-m2 l8 offset-l2">
                <div class="brand">
                    <div class="title center-align">
                        404
                    </div>
                    <div class="description center-align">
                        <%= page.description %>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<script>
    // 每天切换 banner 图.  Switch banner image every day.
    $('.bg-cover').css('background-image', 'url(https://cdn.jsdelivr.net/gh/Yafine/cdn@2.6/source/medias/banner/' + new Date().getDay() + '.jpg)');
</script>
```

- 添加Gitalk评论模块

#### 博客优化

- 图片懒加载 ok
使用图片懒加载需要安装插件：hexo-lazyload-image
在站点根目录执行下面的命令：
``` bash
npm install hexo-lazyload-image --save
```
之后在站点配置文件(/_config.yml)下添加下面的代码：
``` yml
lazyload:
  enable: true  # 是否开启图片懒加载
  onlypost: false  # 是否只对文章的图片做懒加载
  loadingImg: # eg ./images/loading.gif
```

- 代码压缩 未启用
- CDN加速 未启用
- 图床 开启
本图床是基于Github的，采用jsdelivr cdn进行加速，上传工具采用的是PicGo。
   1. 新建一个GitHub仓库 `colbertwong-imgs`
   2. 生成token `setting -> Developer settings -> Persongal Access tokens -> Generate new token -> Note:picgo -> repo选中 -> Generate token`
   3. 复制token值粘贴到文本文档中，先保存下来，后面配置PicGo要用到。
   4. 下载安装PicGO[下载地址：](https://github.com/Molunerfinn/PicGo/releases)
   5. 运行PicGO，开始设置。
       1. 设定仓库名： `colbertwong-imgs`
       2. 设定分支名： `master`
       3. 设定Token： 之前生成的token
       4. 指定存储路径：自定义，例：`images/`
       5. 设定自定义域名：格式：`https://cdn.jsdelivr.net/gh/colbertwong/colbertwong-imgs`，colbertwong是我的GitHub用户名，colbertwong-imgs为新建的仓库，用于存储图片

#### SEO优化
未优化，可参照[博文](https://blog.sky03.cn/posts/42790.html#toc-heading-18)

以上
