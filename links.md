---
layout: page
title: 链接
group: navigation
---

## 联系我

- Weibo: [@daishiwen](http://weibo.com/u/2823408445)
- GitHub: [@daishiwen](https://github.com/daishiwen)
- E-mail: [daishiwen1212@gmail.com](daishiwen1212@gmail.com)

## 友情链接

- [A]() - 友情链接一
- [B]() - 友情链接二
- [C]() - 友情链接三
- [D]() - 友情链接四
- [E]() - 友情链接五

## 个人项目

> 做一个让自己满意的APP —— "毕生"追求

- [A]() - 个人项目一
- [B]() - 个人项目二

## 其它有用的资源

- [Android Developers](https://developer.android.com/develop) - Android 开发者官网，官方的各种参考资料
- [Square](http://square.github.io/) - 一个很屌的组织，开源了 [Retrofit](/tags.html#Retrofit-ref) / Picasso / Otto 等很屌的 Java / Android 库

## 本站

{% for node in site.pages %}{% if node.title != null and (node.group == null or node.group == 'navigation') %}
  - [{{node.title}}]({{node.url}}){% endif %}{% endfor %}
