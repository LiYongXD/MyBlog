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


## 使用Masonry,AutoLayout明明设置了Top = 0,还是被Nav或者Tab覆盖.
原文链接:http://www.jianshu.com/p/ab6d6f278e6d
1.
{% codeblock Objc lang:objc%}

#ifdef __IPHONE_7_0
- (UIRectEdge)edgesForExtendedLayout {
return UIRectEdgeNone;
}
#endif

{% endcodeblock %}

2.
这个办法有几个控件就会调用几次edgesForExtendedLayout函数。
第二种就是修改导航栏的translucent为NO，iOS6(包括)前导航栏的translucent默认都是NO的，但是iOS7后translucent默认是YES的，我们修改一下透明度即可

{% codeblock Objc lang:objc%}
- (void)loadView {
self.view = self.viewClass.new;
self.view.backgroundColor = [UIColor whiteColor];
self.navigationController.navigationBar.translucent = NO;
}
{% endcodeblock %}