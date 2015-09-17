---
layout: post
title: "设置Xcode7|iOS9 与服务器交互支支持HTTP"
date: 2015-09-17 13:43:06 +0800
comments: true
categories: ios
---


在iOS9中,苹果将原`http`协议改成了`https`协议,使用`TLS1.2 SSL`加密请求数据

##解决方法

在info.plist中添加

```
<plist>

  <dict>
     ....
     <key>NSAppTransportSecurity</key>
     <dict>
        <key>NSAllowsArbitraryLoads</key>
        <true/>
     </dict>
  </dict>

</plist> 
```