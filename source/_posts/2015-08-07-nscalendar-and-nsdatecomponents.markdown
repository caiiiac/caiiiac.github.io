---
layout: post
title: "NSCalendar &amp; NSDate​Components"
date: 2015-08-07 17:01:59 +0800
comments: true
categories: ios
---


* NSDate只是一个绝对的时间
* NSCalendar对世界上现存的常用的历法进行了封装,既提供了不同历法的时间信息,又支持日历的计算
* NSDateComponents是一个容器,详细包含了年月日时分等信息.将时间表示成适合阅读和使用的方式
* NSDateComponents可以快速而简单地获取某个时间点对应的年, 月,日,时,分,秒,周等信息.例如:三个月，2年，7天，15分钟，60秒等等
* NSDateComponents的返回值day, week, weekday, month, year 都是从1开始


** 当前时间的 年 月 日 时 分**

```
NSDate * date = [NSDate date];//当前时间
NSCalendar * calendar = [NSCalendar currentCalendar];//当前用户的calendar
NSDateComponents * components = [calendar components:NSCalendarUnitMonth | NSCalendarUnitDay | NSCalendarUnitHour | NSCalendarUnitMinute fromDate:date];
NSLog(@"%ld月%ld日%ld时%ld分" ,(long)components.month,(long)components.day,(long)components.hour,(long)components.minute);
```

**今天是今年的第几周**

```
NSCalendar * calendar = [NSCalendar currentCalendar];
NSDate * currentDate = [NSDate date];
NSInteger week = [calendar ordinalityOfUnit:NSCalendarUnitWeekday inUnit:NSCalendarUnitYear forDate:currentDate];
NSLog(@"今天是今年的第%ld周",week);
```

**指定年 月 日 时 分 秒 得到 NSDate**

```
NSDateComponents * components = [[NSDateComponents alloc] init];
components.year = 2015;
components.month = 8;
components.day = 7;
components.hour = 11;
components.minute = 11;
components.second = 11;
NSCalendar * calendar = [NSCalendar currentCalendar];
NSDate * date = [calendar dateFromComponents:components];

NSDateFormatter * formatter = [[NSDateFormatter alloc] init];
formatter.dateFormat = @"yyyy年MM月dd日hh时mm分ss秒";
NSString * time = [formatter stringFromDate:date];
NSLog(@"%@",time);
```

**7天12小时之后**

```
NSDateComponents * components = [[NSDateComponents alloc] init];

components.day = 7;
components.hour = 12;
NSCalendar * calendar = [NSCalendar currentCalendar];
NSDate * currentDate = [NSDate date];
NSDate * nextData = [calendar dateByAddingComponents:components toDate:currentDate options:NSCalendarMatchStrictly];
NSDateFormatter * formatter = [[NSDateFormatter alloc] init];

formatter.dateFormat = @"yyyy年MM月dd日hh时mm分ss秒";
NSString * after = [formatter stringFromDate:nextData];
NSLog(@"7天12小时之后的时间:%@",after);
```

**这个月有几天**

```
NSCalendar * calendar = [NSCalendar currentCalendar];
NSDate * currentDate = [NSDate date];
NSRange range = [calendar rangeOfUnit:NSCalendarUnitDay inUnit:NSCalendarUnitMonth forDate:currentDate];
```