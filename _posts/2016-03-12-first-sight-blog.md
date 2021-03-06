---
layout: post
category: 阿西巴
tags: [日记]
title: 吾家博客初长成
description: 用Github Pages + Jekyll搭建博客
header: no
show: true
---

<a href="#" data-reveal-id="videoModal">![](../images/21-041223_215.jpg)</a>

终于有自己的博客了

Designed by [XiNGRZ](http://xingrz.me). Proudly built with [Jekyll](http://jekyllrb.com/) and hosted by [GitHub Pages](https://pages.github.com/).

## 初次见面

年初，想找一个使用Dragger、RxJava、RxAndroid、MVP/MVVM（Databinding）、lambda、Retrofit的开源项目，偶然的机会在GitHub上发现了（[Mr.Mantou](https://github.com/oxoooo/mr-mantou-android)），果断Clone下来仔细研读。后来看了作者的个人博客并Mark在了自己的Evernote中（说来惭愧，题主做Android开发近四年了，竟然没有自己的技术博客）。渐渐发现Evernote已经满足不了我的需求。就在昨晚，静下心来想把这件事情搞定，以此开启我人生新阶段的大门。

还好，功夫不负有心人，今早终于搞定了。

## 小试牛刀

起初对此类博客一无所知，更别说如何搭建自己的博客了。

一切未知的东西，在Google面前都是纸老虎！

Google之后发现此类的教程非常多，也都可以达到搭建博客的目的，在这些中我选择了一个容易上手的——[Jekyll](http://jekyllrb.com/) + [GitHub Pages](https://pages.github.com/)，具体步骤可以参考[10分钟搭建一个基于github和jekyll的免费博客](http://cenalulu.github.io/jekyll/how-to-build-a-blog-using-jekyll-markdown/)。搭建完成后，还需要加入评论模块，比较受欢迎的有[Disqus](https://disqus.com/)和[多说](http://duoshuo.com/)，本来想用Disqus的，因为它是社会化评论系统的鼻祖，无论是用户量还是使用体验，都是一级棒。无奈今天死活打不开它的官网，挂了VPN也不管用，所以只得作罢，无奈选择了多说。当然他们各有优缺点，具体可以参考[Disqus和多说在Jekyll的恩怨情仇](http://blog.fooleap.org/talk-about-duoshuo.html#disqus_thread)这篇文章，作者从Disqus迁移到了多说，当然也有从多说迁移到Disqus的，例如：[告别多说，拥抱 Disqus](https://blog.jamespan.me/2015/04/18/goodbye-duoshuo/)，其中还有迁移过来迁移过去的，例如：[Disqus，我又回来了！](https://imququ.com/post/back-to-disqus.html)

## 遇到的「坑」

在从Jekyll的默认风格切换成目前这个风格时，本地可以正常浏览，但是push到GitHub之后，不晓得什么情况，界面更新不了。网上说GitHub Pages默认有10分钟的缓存，但我都等了几个小时了好嘛！！！有的说GitHub在编译时可能会出现错误，可是我也没见它给我发邮件啊。

最后无奈之下，新建了一个分支master（默认为gh-pages），并且切换到master，push后把gh-pages分支删掉就好了。它就好了，它竟然好了。终于可以松一口气了。

不见得每个人都会遇到这样的问题，可能是因为我某个不恰当的操作吧。

> 那些奇怪的和不奇怪的「坑」总是跟我们不期而遇
>
> 感谢一路上有你相伴
>
> 才让我的旅途更有激情和挑战
