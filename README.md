CWShare 1.3
=======
### 更新说明
1.3版本更新（2013-11-07）
- 增加了QQ互联SSO授权方式。
- 更新所有支持库到最新版本，支持IOS7。

1.2版本更新（2013-04-26）
- 增加新浪微博SSO授权方式。
- 修改CWShare为单例模式。
- 完善了部分细节。

1.1版本更新（2013-04-10）
- 将腾讯微博更换为腾讯QQ互联，能同时分享到QQ空间，腾讯微博。
- 授权后自动获取用户第三方个人资料，方便填充账号信息。
- 优化授权回调函数，使用Block交互方式。

### 关于CWShare
CWShare是一个集成的国内分享平台的Object-C版本的SDK。

目前CWShare支持以下平台:
- 新浪微博
- 腾讯QQ

### 谁适合使用它
仅使用第三方登录和简单分享，降低用户注册账号的门槛。
使用第三方登录授权后自动获取用户个人信息，用来填充用户个人资料。

### 使用注意:
由于Demo里的分享AppKey都是刚申请的测试应用，不支持测试账号以外的其他账号授权，所以在测试Demo的时候，请将CWShareConfig文件里的配置信息更换为自己的AppKey，否则授权不通过。

### 如何使用:
CWShare里使用了两个很常用的第三方库，ASIHttpRequest和JsonFramework。
在Demo里大家可以将这两个第三方库一起拷贝到自己的项目里，当然引用这两个第三方库需要在项目里添加相应的Frameworks。
你需要添加如下framwork:
- CFNetwork.framework
- SystemConfiguration.framework
- MobileCoreServices.framework
- QuartzCore.framework
- libz.dylib

由于腾讯SSO授权不公开API接口，所以项目中为了引用腾讯的封包的接口，需要添加如下framework:
- CoreTelephony.framework
- Security.framework
- libstdc++.dylib
- libsqlite3.dylib
- libiconv.dylib
- TencentOpenAPI.framework（腾讯自己的封包接口，请从Demo内拷贝到自己的项目里）

由于增加了SSO授权方式，所以需要相应的修改项目配置文件。选中项目的TARGETS，选择Info选项，找到最下面的URL Types，添加新的URL Types。
新浪的URL Schemes填写sinaweibosso."your app key"。
腾讯的URL Schemes填写tencent"your app key"。
其他信息可以任意填写。可以参考Demo里的配置设置。

配置SSO授权最后一步，在项目的AppDelegate.h文件里按如下方式填充方法：
```objective-c

- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation
{
    if ([sourceApplication isEqualToString:@"com.sina.weibo"]) {
        [[CWShare shareObject] handleOpenURL:url];
    } else if ([sourceApplication isEqualToString:@"com.tencent.mqq"]) {
        [TencentOAuth HandleOpenURL:url];
    }
    return YES;
}

```

下面是使用CWShare的方法，非常简单。

使用的时候先在你要调用CWShare的.h头文件里申明要实现CWShareDelegate代理。
```objective-c
#import <UIKit/UIKit.h>
#import "CWShare.h"

@interface ViewController : UIViewController <CWShareDelegate>

@end
```
在分享的时候你需要设置它的代理和父视图控制器，调用如下代码。
```objective-c
#import "ViewController.h"

...

- (IBAction)sinaShareContent:(id)sender
{
	[[CWShare shareObject] setDelegate:self];
    [[CWShare shareObject] setParentViewController:self];
    [[CWShare shareObject] sinaShareWithContent:@"test cwshare"];
}

...
```

在你的类里实现如下代理，可以处理分享之后的操作。
```objective-c
#import "ViewController.h"

...

- (void)shareContentFailForShareType:(CWShareType)shareType
{
    if (shareType == CWShareTypeSina) {
        NSLog(@"新浪分享内容失败");
    } else if (shareType == CWShareTypeTencent) {
        NSLog(@"腾讯分享内容失败");
    }
}

- (void)shareContentFinishForShareType:(CWShareType)shareType
{
    if (shareType == CWShareTypeSina) {
        NSLog(@"新浪分享内容成功");
    } else if (shareType == CWShareTypeTencent) {
        NSLog(@"腾讯分享内容成功");
    }
}

...
```
更多使用方式可以参考Demo。

### 联系作者
QQ 1749520
如果你有任何问题可以联系作者。
