title: Masonry常见错误
date: 2016-01-13 12:22:46
tags:
- 错误处理
category:
- iOS
---

##  Attributes should be chained before defining the constraint relation
第一次Masonry第三方，遇到这种问题，百思不得其解，然后发现是把with写成了width 
##  'couldn't find a common superview for XXX
使用Masonry的View必须先要添加到父View上,才能进行布局.

