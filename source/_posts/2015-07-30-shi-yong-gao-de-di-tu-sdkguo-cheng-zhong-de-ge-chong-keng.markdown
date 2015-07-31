---
layout: post
title: "使用高德地图SDK过程中的各种坑"
date: 2015-07-30 15:36:36 +0800
comments: true
categories: 地图
---
## 1. APP内调起高德地图导航 


> **高德文档描写如下:**

```
//配置导航参数
MANaviConfig * config = [[MANaviConfig alloc] init];
config.destination = view.annotation.coordinate;//终点坐标，Annotation的坐标
config.appScheme = [self getApplicationScheme];//返回的Scheme，需手动设置
config.appName = [self getApplicationName];//应用名称，需手动设置
config.style = MADrivingStrategyShortest;
//若未调起高德地图App,引导用户获取最新版本的
if(![MANavigation openAMapNavigation:config])
{
[MANavigation getLatestAMapApp];
}
```

**实际情况**

-  **2.5.0** SDK使用此方法调用导航都无任何反应  `[MANavigation openAMapNavigation:config] 返回为成功`
-  **2.6.0** SDK查无此类


**解决方法**

-  更新SDK为**2.6.0**  `pod 'AMap2DMap', '~> 2.6.0'`

> **代码修改如下:**

```
//配置导航参数
MANaviConfig * config = [[MANaviConfig alloc] init];
config.destination = _parkingCoordinate;         //终点坐标，Annotation的坐标    
config.appScheme = [self getApplicationScheme];  //返回的Scheme，需手动设置
config.appName = [self getApplicationName];      //应用名称，需手动设置
config.strategy = MADrivingStrategyShortest;

//若未调起高德地图App,引导用户获取最新版本的
if(![MAMapURLSearch openAMapNavigation:config])
{
[MAMapURLSearch getLatestAMapApp];
}
```


## 2. - (void)setZoomLevel:(double)newZoomLevel animated:(BOOL)animated; 失效


**使用情况**

-  **2.5.0** SDK使用此方法设置一次,永久有效
-  **2.6.0** SDK使用此方法设置无效,需要延时设置


**解决方法,代码如下**

```
dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(0.5 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
[self.mapView setZoomLevel:14.f animated:YES];
});
```
