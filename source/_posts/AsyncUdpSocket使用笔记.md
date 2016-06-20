title: AsyncUdpSocket使用笔记
date: 2016-06-07 12:20:35
tags:
- Runloop
- Socket
- 多线程
---

## AsyncUdpSocket使用笔记

AsyncUdpSocket与GCDAsyncUdpSocket不同,前者是基于Runloop的,Runloop我不懂,但我歪打正着就用上了,下面就说说
1. AsyncUdpSocket默认只在主线程中生效,因为主线程的Runloop默认是开启的,如果AsyncUdpSocket的代码写在子线程就不会生效,不会发出收到任何消息.
2. 如果想在子线程中生效需要开启子线程的Runloop,下面是示例代码:
{% codeblock Objc lang:objc %}
dispatch_async(dispatch_get_global_queue(0, 0), ^{
        self.socket = [[AsyncUdpSocket alloc]initWithDelegate:self];
        Byte bytes[] = {'a','b'};
        [self.socket sendData:[NSData dataWithBytes:bytes length:2] toHost:@"127.0.0.1" port:55056 withTimeout:5 tag:0];
        [self.socket bindToPort:55123 error:nil];
        [self.socket receiveWithTimeout:30 tag:0];
        [[NSRunLoop currentRunLoop] run];//开启Runloop
    });
{% endcodeblock %}
3. 该Socket在子线程A中运行,用它发送消息的时候必须也在该线程发消息,如果你在线程B中使用该Socket发送,也会发不出去.解决方法:用线程之间通信.
{% codeblock Objc lang:objc %}
#import "ViewController.h"
#import "AsyncUdpSocket.h"

@interface ViewController ()<AsyncUdpSocketDelegate>

@property (nonatomic,strong) AsyncUdpSocket * socket;

@property (weak,nonatomic) NSThread *socketThread;

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    dispatch_async(dispatch_get_global_queue(0, 0), ^{//线程A
        self.socket = [[AsyncUdpSocket alloc]initWithDelegate:self];
        Byte bytes[] = {'a','b'};
        [self.socket sendData:[NSData dataWithBytes:bytes length:2] toHost:@"127.0.0.1" port:55056 withTimeout:5 tag:0];
        [self.socket bindToPort:55123 error:nil];
        [self.socket receiveWithTimeout:30 tag:0];
        [[NSRunLoop currentRunLoop] run];
        self.socketThread = [NSThread currentThread];//将线程保存
    });
    [NSThread sleepForTimeInterval:1];
    dispatch_async(dispatch_get_global_queue(0, 0), ^{//线程B
        //使用该方法进行线程间通信,传入Socket,可以在这里做下判断
        if([NSThread currentThread] != self.thread)
        {
            [self performSelector:@selector(socketsend) onThread:self.socketThread withObject:nil waitUntilDone:YES];
        }
    });
    
    
}
- (void)socketsend
{
    Byte bytes[] = {'C','D'};
    [self.socket sendData:[NSData dataWithBytes:bytes length:2] toHost:@"127.0.0.1" port:55056 withTimeout:5 tag:0];
}
{% endcodeblock %}