---
layout: post
title: "常用宏汇总"
date: 2015-08-07 15:54:02 +0800
comments: true
categories: ios
---


NSString 

以@方式生成的字符串，会作为字符串常量，在程序过程中，会一直存在，占用着内存。

```
#define STR(str) [NSString stringWithCString:(str) encoding:NSUTF8StringEncoding]

#define STR(str)[[NSString alloc] initWithUTF8String:str];
```

IOS7判断

```
#define IOS7 [[[UIDevice currentDevice]systemVersion] floatValue]>=7.0
```

屏幕宽高

```
#define kScreenWidth [[UIScreen mainScreen] bounds].size.width
#define kScreenHeight [[UIScreen mainScreen] bounds].size.height
```

颜色 

```
//RGB
#define RGBA(R, G, B, A) [UIColor colorWithRed:R/255.0f green:G/255.0f blue:B/255.0f alpha:A]

//（16进制->10进制）  
#define UIColorFromRGB(rgbValue) [UIColor colorWithRed:((float)((rgbValue & 0xFF0000) >> 16))/255.0 green:((float)((rgbValue & 0xFF00) >> 8))/255.0 blue:((float)(rgbValue & 0xFF))/255.0 alpha:1.0]  

//透明色
#define CLEARCOLOR [UIColor clearColor] 
```


图片 

```
//读取本地图片 
#define LOADIMAGE(file,type) [UIImage imageWithContentsOfFile:[[NSBundle mainBundle]pathForResource:file ofType:type]]  

//定义UIImage对象 
#define IMAGE(A) [UIImage imageWithContentsOfFile:[[NSBundle mainBundle] pathForResource:A ofType:nil]]  
```

weakSelf

```
#define WS(weakSelf)  __weak __typeof(&*self)weakSelf = self;
```

重写NSLog,Debug模式下打印日志和当前行数  

```
#if DEBUG  
#define NSLog(FORMAT, ...) fprintf(stderr,"\nfunction:%s line:%d content:%s\n", __FUNCTION__, __LINE__, [[NSString stringWithFormat:FORMAT, ##__VA_ARGS__] UTF8String]);  
#else  
#define NSLog(FORMAT, ...) nil  
#endif 
```

判断是真机还是模拟器 

```
#if TARGET_OS_IPHONE  
//iPhone Device  
#endif  

#if TARGET_IPHONE_SIMULATOR  
//iPhone Simulator  
#endif  
```