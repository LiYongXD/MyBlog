title: Objective-C中的属性修饰符
date: 2016-06-21 09:08:33
tags:
- 属性修饰符
- Objective-C
---

## 属性修饰符(属性特质)
### 属性修饰符包括是否为原子性、读写权限、内存管理语义、方法名
----------
### 原子性:nonatomic和atomic

atomic：默认是有该属性的，这个属性是为了保证程序在多线程情况下，编译器会自动生成一些互斥加锁代码，避免该变量的读写不同步问题。
nonatomic：如果该对象无需考虑多线程的情况，请加入这个属性，这样会让编译器少生成一些互斥加锁代码，可以提高效率。

atomic的意思就是setter/getter这个函数，是一个原语操作。如果有多个线程同时调用setter的话，不会出现某一个线程执行完setter全部语句之前，另一个线程开始执行setter情况，相当于函数头尾加了锁一样，可以保证数据的完整性。nonatomic不保证setter/getter的原语行，所以你可能会取到不完整的东西。因此，在多线程的环境下原子操作是非常必要的，否则有可能会引起错误的结果。

下面是载录的网上一段加了atomic的例子：
{% codeblock Objc lang:objc %}
{lock}
if (property != newValue) { 
[property release]; 
property = [newValue retain]; 
}
{unlock}
{% endcodeblock %}
可以看出来，用atomic会在多线程的设值取值时加锁，中间的执行层是处于被保护的一种状态，atomic是oc使用的一种线程保护技术，基本上来讲，就是防止在写入未完成的时候被另外一个线程读取，造成数据错误。而这种机制是耗费系统资源的，所以在iPhone这种小型设备上，如果没有使用多线程间的通讯编程，那么nonatomic是一个非常好的选择。

### 读/写权限:readwrite和readonly
readwrite会自动生成getter和setter方法.
readonly是只读的,只会生成getter方法.

### 内存管理语义(此特质会影响setter方法的生成,编译器在合成setter方法时,需要根据此特质来决定所生成的代码)
> 下面出现的代码,该代码是系统生成属性时的原理复现代码

#### assign(纯量类型)
纯量类型:该属性为CGFloat,NSInteger,int,float,double等基础数据类型时,setter方法只会生成简单的赋值操作
{% codeblock Objc lang:objc %}
if (property != newValue) {
property = newValue;
}
{% endcodeblock %}
#### strong(拥有关系)
拥有关系:先retain新值,release旧值,然后设置新值上去.
{% codeblock Objc lang:objc %}
if (property != newValue) { 
[property release]; 
property = [newValue retain]; 
}
{% endcodeblock %}
#### weak (非拥有关系)
非拥有关系:同assign类似,即不retain新值,也不release旧值,只做简单的赋值操作.
但属性所指的对象被销毁时,属性值会被清空 = nil.通常用于对象.assign用于基本类型
#### unsafe_unretained (非拥有关系,保留属性值)
非拥有关系:跟weak一样,非拥有关系,区别就是当新值被销毁后,该属性值还会保留.也就是对象的指针啦,容易野指针,不安全.
#### copy (拷贝)
拷贝:拷贝新值,释放(release)旧值.常见于NSString类型的属性中.因为新值有可能是可变的(mutable),比如,NSMutableString类的实例赋值给NSString类型的属性.按照Strong类型的方法的话,就不太科学了.因为NSString指向的是NSMutableString的,也就是说在指不定在哪里这个属性被别的属性给改了,你却不知道.所以我们应该赋值的时候进行拷贝一份不可改变的,这样就有效的避免了.
{% codeblock Objc lang:objc %}
if (property != newValue) { 
[property release]; 
property = [newValue copy]; 
}
{% endcodeblock %}
#### 方法名(getter和setter)可通过getter和setter特质来指定存取的方法名
{% codeblock Objc lang:objc %}
@property (nonatomic,getter = isOn) BOOL on;//getter方法的名字被改为isOn.
{% endcodeblock %}
setter=<name>这种不太常见.

## 注意事项
> 若是自己实现存取方法,必须要保证其具备相关属性的特质,否则会误导调用者,也可能会出现Bug.
> 不应该在init方法中调用存取方法,而是要遵守标示的语义,自行初始化.
> 即使属性中使用了readonly特质,也应该要标示内存管理语义,因为这样可以让本类的调用者知道在init方法中怎么初始化.如果每次初始化都用copy语义,难免有点低效.
> 应该尽量使用nonatomic属性,因为iOS中atomic属性会严重影响性能,而且也不能完全保证线程是安全的,若要实现线程安全,还需要更深层次的东西才行.
> 不通过setter方法而直接使用实例变量进行修改时,一定遵从该属性所声明的语义.


