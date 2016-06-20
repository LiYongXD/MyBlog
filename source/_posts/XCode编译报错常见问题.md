title: XCode编译报错常见问题
date: 2015-12-16 10:06:16
tags:
- XCode
- 错误处理
category:
- iOS
---

## Expression is not an integer constant expression
如果在一个文件中只声明常量的话,可以在.h中直接赋值.因为有的时候编译器判断用不到.m文件时,它会不编译,导致不识别你这是个常量,比如:
{% codeblock Util.h lang:objc %}
NSInteger const MSG_TYPE_INVITE = 10; //
{% endcodeblock %}
但是!!这种写法是不对的,这会造成重复引用的问题:duplicate symbols.我还特意在github搜索了下NSInteger const关键字,没看到过在.h中直接赋值的,都是在.h中声明,在.m中赋值.
最稳妥的解决办法,应该是把变量定义成宏或者写成枚举.
{% codeblock Util.h lang:objc %}
NSInteger const MSG_TYPE_INVITE;
{% endcodeblock %}
{% codeblock Util.m lang:objc %}
NSInteger const MSG_TYPE_INVITE = 10;
{% endcodeblock %}
参考链接:
http://stackoverflow.com/questions/6585276/why-can-i-not-use-my-constant-in-the-switch-case-statement-in-objective-c-e
## duplicate symbols for architecture x86_64
出现变量重名了,你可以找找引入的头文件看看有没有跟本类有重名的变量
## iOS沙盒路径问题
iOS每次启动App的时候,App的沙盒名字是会变的,比如上一次启动时沙盒的路径是:
`/Users/liyong/Library/Developer/CoreSimulator/Devices/D930CF62-5F91-4CCE-86DE-3C52B7556DFC/data/Containers/Data/Application/27BF3BA8-4CA9-4E72-924B-F6749D828A82/Documents/`
再次打开的时候就变成:
`/Users/liyong/Library/Developer/CoreSimulator/Devices/D930CF62-5F91-4CCE-86DE-3C52B7556DFC/data/Containers/Data/Application/CEC5EE1D-F79E-441C-9686-FFED73CE2482/Documents/`
模拟器是这样,真机测试也是如此.所以我们还是要使用相对路径.
通过系统给的API获取沙盒路径再拼接自己的相对路径,如果你保存绝对路径,下次启动可就找不到喽.

