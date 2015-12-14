layout: '[post]'
title: 使用Requests实现单线程爬虫
date: 2015-12-14 21:49:16
tags:
- 爬虫
- Requests 
---

## Requests是什么?
Requests是Python的一个优秀的第三方库,python自带的urllib2十多行才能实现的功能,用Requests几行就可以实现.
## 如何安装?
Windows下控制台输入:
{% codeblock %}
pip install requests
{% endcodeblock %}
Linux下终端输入:
{% codeblock %}
sudo pip install requests
{% endcodeblock %}
#### 第三库安装使用技巧
少用easy_install.因为只能安装,卸载麻烦.需要到Python的lib目录下自行删除想卸载的模块.
多使用pip方式安装.pip install <模块名字> 进行安装 , pip uninstall <模块名字> 进行卸载.非常简单!

## 开始使用
#### 这是我写的一个单线程爬虫代码 
{% codeblock Python3 lang:python http://www.baidu.com 么么哒 %}
# -*- coding:utf-8 -*-
import requests #导入模块
import re
#针对有的网站有防爬虫机制,仿造用户代理.
headers = {'User-Agent	Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11) AppleWebKit/601.1.56 (KHTML, like Gecko) Version/9.0 Safari/601.1.56'}
i = 1
while(i):
    #获取网站源代码
    htmlStr = 'http://www.4493.com/siwameitui/28624/'+ str(i) +'.htm'
    i += 1
    #获取网页源代码
    html = requests.get(htmlStr)
    #用正则表达式匹配,findall三个参数:
    #第一个是匹配的正则
    #第二个是要正则的内容
    #第三个re.S表示多行匹配,比如不写re.S的话,内容中有\n,那么这行匹配不到的话,就换下一行开始匹配.re.S表示把所有内容看做一个整体进行匹配.
    link = re.findall('<div class="picsbox picsboxcenter"><p><img src="(.*?)" alt=',html.text,re.S)
    #判断下,如果匹配到了就循环输出匹配后得到的list,否则提示没有了,退出循环
    if len(link) != 0:
        for each in link:
            print(each)
    else:
        print("已经没有了^_^")
        break
{% endcodeblock %}
输出结果:
http://img.1985t.com/uploads/attaches/2015/01/28624-kPIq1j.jpg
http://img.1985t.com/uploads/attaches/2015/01/28624-95Ksqg.jpg
http://img.1985t.com/uploads/attaches/2015/01/28624-cJE7Lj.jpg
http://img.1985t.com/uploads/attaches/2015/01/28624-i9cTpA.jpg
http://img.1985t.com/uploads/attaches/2015/01/28624-oaPVY8.jpg
http://img.1985t.com/uploads/attaches/2015/01/28624-YPu5dM.jpg
http://img.1985t.com/uploads/attaches/2015/01/28624-UDj4Y7.jpg
http://img.1985t.com/uploads/attaches/2015/01/28624-flfR20.jpg
http://img.1985t.com/uploads/attaches/2015/01/28624-4Meb9V.jpg
http://img.1985t.com/uploads/attaches/2015/01/28624-tkFlzE.jpg
http://img.1985t.com/uploads/attaches/2015/01/28624-cvkFLw.jpg
已经没有了^_^

