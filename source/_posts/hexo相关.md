title: hexo相关
date: 2016-06-21 15:04:22
tags:
  - tagg
---
## Hexo 支持更使用特定的语法，插入指定大小的图片，如下:
{% codeblock lang:html %}
// 语法
{% img [class names] /path/to/image [width] [height] [title text [alt text]] %}

// 实例
{% img full-image /hexo-experiences/PL01.jpg 180 180 hello %}

// 生成的代码
<img src="/blog/hexo-experiences/PL01.jpg" class="full-image" width="180" height="180" title="hello">
{% endcodeblock %}

值的注意的几点：
* 路径名必须以 / 开始，否则会解析出错
* 路径是相对于 conifg 内的 root 的，这一点挺坑，可以在 source/ 下新建一个 uploads 文件夹用于专门放置这些图片资源 {\% img hi /uploads/images/test.jpg 100 100 hello hello %}
* 图片宽高只能使用数值，不能包含字符串，也不能是百分数
* 最后一个字段可以为图片添加标题

## 引入某个文件中的代码，使用 include_code
{% codeblock lang:html %}
// 语法
{% include_code [title] [lang:language] path/to/file %}

// 实例
{% include_code DOMUtil lang:javascript demo.js %}
{% endcodeblock %}

值得注意的是：code 代码所在的文件必须在 downloads/code/ 目录下，否则无法获取
## 引用其他文章的路径，基本功能不大
{% codeblock lang:html %}
// 语法
{% post_path slug %}

// 实例
{% post_path OS-Brief-Intro %} // >> /blog/2016/05/03/OS-Brief-Intro/
{% endcodeblock %}
## 引用其他文章的链接，用处很大，但是有一个小坑
{% codeblock lang:html %}
// 语法
{% post_link slug [title] %}

// 实例
{% post_link OS-Brief-Intro 操作系统 %}
{% endcodeblock %}
小坑：不能放在一段的段首，否则 md 文档或解析错误，出现莫名奇妙的 bug

## 引用文章的资源：获取到的是文章对应 asset 目录下的资源
{% codeblock lang:html %}
// 语法
{% asset_path slug %}
{% asset_img slug [title] %}
{% asset_link slug [title] %}

// 实例
{% asset_path 01.png %}
{% asset_img 01.png 图片 %}
{% asset_link 01.png 图片 %}
{% endcodeblock %}

原文地址:http://yangfch3.com/2016/05/08/hexo-experiences/
