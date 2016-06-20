layout: '[post]'
title: Python配置
date: 2015-12-14 22:57:12
tags:
- Python配置
- 心得
category:
- Python
---

## 如何找到Python的安装目录
1.在终端输入pyhton
2.引入sys系统库
3.打印sys系统库的位置
{% codeblock %}
Python
import sys
print sys.path
{% endcodeblock %}
