layout: '[post]'
title: OC基础
date: 2016-01-06 17:55:15
tags:
- 基础
- iOS 
category:
- Objective-C
---

###使用迭代器对元素进行删除
使用 [arr objectEnumerator]这种方式会报错,必须用下面这种block的方式
{% codeblock Objective-C lang:objc %}
    NSMutableArray *arr = [NSMutableArray array];
    for (int i = 0; i < 100; i ++) {
        [arr addObject:@(i)];
    }
    [arr enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
    if((arc4random() % 2))
    {
        [arr removeObject:obj];
    }
    }];
    for (NSNumber *num in arr) {
        NSLog(@"%@",num);
    }
{% endcodeblock %}
