<!--
 * @Author: hfqf123@126.com
 * @Date: 2023-01-09 08:38:46
 * @LastEditors: user.email
 * @LastEditTime: 2023-01-30 19:38:45
 * @FilePath: /软设流程图/app设计规范/软件设计原则(SOLID)/接口隔离原则(ISP)/README.md
 * @Description: 
 * 
 * Copyright (c) 2023 by hfqf123@126.com, All Rights Reserved. 
-->
# 接口隔离原则

### **介绍**
依赖倒置原则（Dependence Inversion Principle，DIP）是指设计代码结构时，高层模块不应该依赖低层模块，二者都应该依赖其抽象。

抽象不应该依赖细节，细节应该依赖抽象。通过依赖倒置，可以减少类与类之间的耦合性，提高系统的稳定性，提高代码的可读性和可维护性，并且能够降低修改程序所造成的风险。

### **问题来源**
说到单一职责原则，很多人都会不屑一顾，因为它太简单了。稍有经验的程序员即使从来没有读过设计模式，从来没有听说过单一职责原则，在设计软件的时候也会自觉遵守这个重要原则，因为这是一个常识。在软件编程中，谁也不希望因为修改了一个功能导致其他的功能发生故障。而避免出现这一问题的方法便是遵循单一职责原则。虽然单一职责原则如此简单，并且被认为是常识，但是即使是经验丰富的程序员写出的程序也会有违背这一设计原则的代码存在。这是职责扩散导致的。

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
    [launchOptions objectForKey:UIApplicationLaunchOptionsRemoteNotificationKey]];
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

### **优点**

1.降低类的复杂度，一个类只负责一个职责。这样写出来的代码逻辑肯定要比负责多项职责简单得多。

2.提高类的可读性，提高系统的可维护性。

3.降低变更引起的风险。变更是必然的，如果单一职责原则遵守得好，当修改一个功能的时候可以显著降低对其他功能的影响。

>需要说明的一点是，单一职责原则不只是面向对象编程思想所特有的，只要是模块化的程序设计，都适用单一职责原则。比如说单一职责原则不仅仅适用于类，还适用于方法。

### **参与贡献**

1.  hfqf123@126.com
