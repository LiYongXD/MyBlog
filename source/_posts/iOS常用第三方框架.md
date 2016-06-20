layout: '[draft]'
title: iOS常用第三方框架
date: 2015-12-16 11:18:19
tags:
- 框架
- iOS
category:
- iOS
---

## 获取本机IP
{% codeblock HYBIPHelper.h lang:objc %}
//
//  HYBIPHelper.h
//  XiaoYaoUser
//
//  Created by 黄仪标 on 14/12/9.
//  Copyright (c) 2014年 xiaoyaor. All rights reserved.
//

#import <Foundation/Foundation.h>

@interface HYBIPHelper : NSObject

/*!
* get device ip address
*/
+ (NSString *)deviceIPAdress;

@end

{% endcodeblock %}

{% codeblock HYBIPHelper.m lang:objc %}
//
//  HYBIPHelper.m
//  XiaoYaoUser
//
//  Created by 黄仪标 on 14/12/9.
//  Copyright (c) 2014年 xiaoyaor. All rights reserved.
//

#import "HYBIPHelper.h"

#include <ifaddrs.h>
#include <arpa/inet.h>


@implementation HYBIPHelper

+ (NSString *)deviceIPAdress {
NSString *address = @"an error occurred when obtaining ip address";
struct ifaddrs *interfaces = NULL;
struct ifaddrs *temp_addr = NULL;
int success = 0;

success = getifaddrs(&interfaces);

if (success == 0) { // 0 表示获取成功

temp_addr = interfaces;
while (temp_addr != NULL) {
if( temp_addr->ifa_addr->sa_family == AF_INET) {
// Check if interface is en0 which is the wifi connection on the iPhone
if ([[NSString stringWithUTF8String:temp_addr->ifa_name] isEqualToString:@"en0"]) {
// Get NSString from C String
address = [NSString stringWithUTF8String:inet_ntoa(((struct sockaddr_in *)temp_addr->ifa_addr)->sin_addr)];
}
}

temp_addr = temp_addr->ifa_next;
}
}

freeifaddrs(interfaces);
return address;
}
@end
{% endcodeblock %}

## SvUDID
苹果禁用了获取UDID的API,之后又禁用了获取Mac地址的API,所以大神们研究出来的使用KeyChain方法.
https://github.com/smileEvday/SvUDID

## FTPManager


#### 第一步 初始化GRRequestsManager
{% codeblock HYBIPHelper.m lang:objc %}
self.requestsManager = [[GRRequestsManager alloc] initWithHostname:self.ipString user:self.userNameString password:self.passwordString];
{% endcodeblock %}
#### 第二步 设置代理
{% codeblock HYBIPHelper.m lang:objc %}
[self.requestsManager setDelegate:self];
{% endcodeblock %}
#### 第三步 不知道..
{% codeblock HYBIPHelper.m lang:objc %}
[self.requestsManager addRequestForListDirectoryAtPath:@"/"];
{% endcodeblock %}
#### 第三步 linkstart!!
{% codeblock HYBIPHelper.m lang:objc %}
[self.requestsManager startProcessingRequests];
{% endcodeblock %}

### 要重写代理方法
{% codeblock HYBIPHelper.m lang:objc %}

- (void)requestsManager:(id<GRRequestsManagerProtocol>)requestsManager didStartRequest:(id<GRRequestProtocol>)request
{
    NSLog(@"requestsManager:didStartRequest:");
}

- (void)requestsManager:(id<GRRequestsManagerProtocol>)requestsManager didCompleteListingRequest:(id<GRRequestProtocol>)request listing:(NSArray *)listing
{
    NSString *title = getString(@"LoginSuccess");
    [HUDProgress showWithTitle:title type:HUDProgressTypeMessage afterDelay:[HUDProgress timeWithTitle:title] callback:^{
        [self.emptyView removeFromSuperview];
    }];
    NSLog(@"requestsManager:didCompleteListingRequest:listing: \n%@", listing);
    [self onRefresh:nil];
}

- (void)requestsManager:(id<GRRequestsManagerProtocol>)requestsManager didCompleteUploadRequest:(id<GRDataExchangeRequestProtocol>)request
{
    NSLog(@"requestsManager:didCompleteUploadRequest:");
    [HUDProgress showWithTitle:getString(@"UPS") type:HUDProgressTypeMessage];
    dispatch_async(dispatch_get_global_queue(0, 0), ^{
    //        for (int i = 0; i < self.udtSockets.count; i++) {
    //            BOOL isSuccess = NO;
    //            UdpManager *udtManager = self.udtSockets[i];
    //            if ([udtManager getsockstate]<6 && [udtManager sendWithMsg:self.tempDic]) {
    //                isSuccess = YES;
    //            }
    //            dispatch_async(dispatch_get_main_queue(), ^{
    //                if (isSuccess) {
    //                    [HUDProgress showWithTitle:[NSString stringWithFormat:@"%@ 屏幕通知成功", udtManager.ip] type:HUDProgressTypeMessage];
    //                } else {
    ////                    [HUDProgress showWithTitle:[NSString stringWithFormat:@"%@ 屏幕通知失败", udtManager.ip] type:HUDProgressTypeMessage];
    //                }
    //            });
    //        }
});

}

- (void)requestsManager:(id<GRRequestsManagerProtocol>)requestsManager didFailRequest:(id<GRRequestProtocol>)request withError:(NSError *)error
{
    NSLog(@"requestsManager:didFailRequest:withError: \n %@", error);
    if ([requestsManager isMemberOfClass:[GRUploadRequest class]]) {
        [HUDProgress showWithTitle:getString(@"UPF") type:HUDProgressTypeMessage];
    } else {
        NSString *title = getString(@"ListController.didFailRequest");
        [HUDProgress showWithTitle:title type:HUDProgressTypeMessage afterDelay:[HUDProgress timeWithTitle:title] callback:^{
        [self back:nil];
        }];
    }
}
{% endcodeblock %}




