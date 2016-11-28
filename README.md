# GDHNetwork
GDHNetwork is is a high level request util based on AFNetworking

此网络二次开发是基于AFNetworking3.0以上版本封装的网络层。
使用常见的block、代理及SEL方式进行回调数据。
提供常用的GET/POST接口、上传下载图片、文件接口、支持缓存等。
增加无网络时提示框提醒,还增加了网络监听，MBProgressHUD菊花等。

#下载安装

pod 'GDHNetwork'

github网址：
https://github.com/gdhGaoFei/GDHNetwork.git

QQ联系：964195787

Demo中已有参考代码，谢谢支持！

#方法
# 在使用时直接将其文件中的 #import "GDHNetworkHeader.h" 写到pch文件中，直接可以调用基类(#import "GDHNetworkObject.h")和管理类(#import "GDHNetworkManager.h")的方法及菊花(MBProgressHUD)

# 基类 #import "GDHNetworkObject.h" 中涵盖了 
1.创建网络请求的方法
2.图片上传、文件上传及下载
3.取消全部及某个网络请求
4.网络请求的缓存大小及清理缓存的方法等

# 监听网络状态
[GDHNetworkObject StartMonitoringNetworkStatus:^(GDHNetworkStatus status) {
switch (status) {
case GDHNetworkStatusUnknown://未知网络
DTLog(@"未知网络");
break;
case GDHNetworkStatusNotReachable://没有网络
{
DTLog(@"网络无连接");
SHOW_ALERT(@"网络连接断开,请检查网络!");
}
break;
case GDHNetworkStatusReachableViaWWAN:
DTLog(@"2、3、4G网络");
break;
case GDHNetworkStatusReachableViaWiFi:
DTLog(@"WiFi网络");
break;
default:
break;
}
}];
# 设置网络请求的基础网址
[GDHNetworkObject updateBaseUrl:baseURL];

# 修改网络请求的缓存路径
[GDHNetworkObject updateBaseCacheDocuments:@"GDH_123"];

# 获取网络请求的缓存路径
[GDHNetworkObject baseCache]

# 开启或关闭接口打印信息
[GDHNetworkObject enableInterfaceDebug:YES];

# 设置请求超时时间
[GDHNetworkObject setTimeout:15];

# 配置请求和响应类型，由于部分伙伴们的服务器不接收JSON传过去，现在默认值改成了plainText
[GDHNetworkObject configRequestType:GDHRequestTypeJSON
responseType:GDHResponseTypeJSON
shouldAutoEncodeUrl:YES
callbackOnCancelRequest:NO];

# 设置GET、POST请求都缓存
[GDHNetworkObject cacheGetRequest:YES shoulCachePost:YES];


# 管理类 #import "GDHNetworkManager.h" 中涵盖了   1.GET请求的block、delegate、SEL三个方法的请求  1.POST请求的block、delegate、SEL三个方法的请求  根据自己的需求自行调用
















本文参考了CocoaChina上的 “超强 AFN 封装” 和HYBNetworking两个版本的网络封装，此次封装是将其两个的方法及思路综合在了一起。

超强 AFN 封装: 提供了三种回调数据的方式 
HYBNetworking:提供了无网络时获取缓存数据(经测试能够存储缓存数据，但是无法获取到缓存数据，问题已在此版本中修改)

在缓存数据时 HYBNetworking 是使用以下方法
if (sg_cacheGet) {
[self cacheResponseObject:responseObject request:task.currentRequest parameters:params];
}
应该更换成：
if (sg_cachePost) {
[self cacheResponseObject:responseObject request:absolute  parameters:params];
}
