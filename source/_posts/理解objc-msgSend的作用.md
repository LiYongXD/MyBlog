title: 理解objc_msgSend的作用
date: 2016-06-21 10:26:46
tags:
---

## 理解objc_msgSend的作用
### 静态绑定与动态绑定
#### 静态绑定代码示例
{% codeblock c lang:c %}
#import <stdio.h>

void printHello()
{
    printf("Hello World!\n");
}

void printGoodbye()
{
    printf("GoodBye World!\n");
}

void doTheThing(int type)
{
    if(type == 0)
    {
        printHello(); //在编译代码时已经知道程序此时需要调用printHello这个函数,于是在这会生成调用这些函数的指令.printHello这个函数地址被硬编码在指令之中,这就叫静态绑定.
    }else
    {
        printGoodbye();
    }
    return 0;
}
{% endcodeblock %}

#### 动态绑定代码示例
{% codeblock c lang:c %}
#import <stdio.h>

void printHello()
{
    printf("Hello World!\n");
}

void printGoodbye()
{
    printf("GoodBye World!\n");
}

void doTheThing(int type)
{
    void (*fnc)();
    if(type == 0)
    {
        fnc = printHello;//与静态绑定不同,将函数地址赋值给一个变量,并不是将地址硬编码到指令中去.
    }else
    {
        fnc = printGoodbye;
    }
    fnc();//动态绑定,因为fnc指向的函数地址是不确定的,无法硬编码到指令中去,需要到运行期才能确定.
    return 0;
}
{% endcodeblock %}
#### 