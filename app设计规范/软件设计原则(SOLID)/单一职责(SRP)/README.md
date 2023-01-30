<!--
 * @Author: hfqf123@126.com
 * @Date: 2023-01-09 08:38:46
 * @LastEditors: user.email
 * @LastEditTime: 2023-01-30 20:18:41
 * @FilePath: /design-pattern/app设计规范/软件设计原则(SOLID)/单一职责(SRP)/README.md
 * @Description: 
 * 
 * Copyright (c) 2023 by hfqf123@126.com, All Rights Reserved. 
-->
# 单一职责原则

### **介绍**
单一职责原则（Single responsibility principle）规定每个类或者模块都应该有且仅有一个单一的功能，并且该功能应该由这个类或者模块完全封装起来。

### **问题来源**
软件开发人员对于这个原则看起来是比较熟悉的，因为这是个常识-一个类只做一个事，但是很多时候由于第一版代码未注意考虑这个原则，导致后续代码逐渐失控，很多不相关的代码全部堆积在一起，让后来人头皮发麻；也可能是开始的时候保持的还比较好，但是某个时间段因为赶时间，被某些开发人员破坏了风格，也可能导致失控。

### **优化示例**

1.修改前

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(jumpLogin) name:@"logOut" object:nil];
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(jumpRoot) name:@"JumpRoot" object:nil];
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(addTags:) name:@"AddTags" object:nil];
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(exchangeStore:) name:@"ExchangeStore" object:nil];
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(launchResource) name:@"HYXLaunchResourceDidChangeNotification" object:nil];
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(saveLoginUserInfo:) name:@"saveLoginUserInfo" object:nil];
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(saveBaseSync:) name:@"saveBaseSync" object:nil];
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(saveSignBaseSync:) name:@"saveSignBaseSync" object:nil];
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(savePhoneSync:) name:@"savePhoneSync" object:nil];
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(saveDpInfo:) name:@"SaveDpInfo" object:nil];
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(receiveIMessage:) name:@"HYXZDHMessageRedPointNotification" object:nil];
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(postRequest) name:HTTPPOSTRequestNotification object:nil];
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(onUploadAppEnter) name:kHYXAppEnterBackgroundNotification object:nil];
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(collectionOCRLog:) name:@"collectionOCRLog" object:nil];
    [self appLunchHandleCollectionLocalLog:launchOptions];
    return YES ;
}
```

2.修改后

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    [self.appNotisManager addObserver];
    return YES ;
}

```

```
- (void)addObserver {
    [self.loginNotisProxy registObserver];
    [self.launchNotisProxy registObserver];
    [self.syncNotisProxy registObserver];
    [self.zdhNotisProxy registObserver];
    [self.httpNotisProxy registObserver];
    [self.customLogNotisProxy registObserver];
    [self.thirdSDKNotisProxy registObserver];
}
```
经过改造可以看到每个业务模块的通知注册和实现被隔离至专门的类中，模块之间不会相互干扰，大大方便了后续扩充和维护。
### **优点**

1.降低类的复杂度，一个类只负责一个职责。

2.提高类的可读性，提高系统的可维护性。

3.降低变更引起的风险。

>特别注意的是,单一职责原则不只是面向对象编程思想所特有的，只要是模块化的程序设计，都适用单一职责原则。

### **参与贡献**

1.  hfqf123@126.com
