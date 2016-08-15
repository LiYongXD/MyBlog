title: CocoaPods使用教程
date: 2016-01-11 17:09:15
tags:
- iOS
- OSX
category:
- 工具
---

## 常见错误
{% codeblock %}
- `Masonry (~> 0.6.4)` required by `Podfile`
{% endcodeblock %}
有的第三方框架说明文件上写着 `pod 'Masonry'` 但就是安装失败,是因为你的PodFile文件里没有指定平台最低版本.
{% codeblock %}
platform :ios, '6.0'
pod 'JSONKit',       '~> 1.4'
pod 'Masonry'
{% endcodeblock %}
指定上版本就好了,我个项目最低支持6.0.
后来仔细想了想,PodFile里指定项目版本是必须的,方便人家框架判断适不适合用在你的项目上,不应该是第三方框架的问题,所以这个锅肯定不能给第三方框架,而是给唐巧boy,就是因为看了他的教程,这么重要的东西怎么能不写呢????
坚决好好学习英语,自己看官方的文档,能把人气死,每天都浪费这么多时间~~


## CocoaPods更新慢的问题
最近使用CocoaPods来添加第三方类库，无论是执行pod install还是pod update都卡在了Analyzing dependencies不动了，令人甚是DT。

查了好多的资料，原因在于当执行以上两个命令的时候会升级CocoaPods的spec仓库，加一个参数可以省略这一步，然后速度就会提升不少。加参数的命令如下：

pod install --verbose --no-repo-update

pod update --verbose --no-repo-update
原文:http://www.cnblogs.com/yiqiedejuanlian/p/3698788.html



