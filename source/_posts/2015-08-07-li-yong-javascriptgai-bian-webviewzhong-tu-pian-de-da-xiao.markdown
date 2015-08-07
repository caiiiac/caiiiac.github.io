---
layout: post
title: "利用JavaScript改变webView中图片的大小"
date: 2015-08-07 14:53:29 +0800
comments: true
categories: JavaScript
---

## webView中图片过大


最简单的图文混排就是用webView去实现,如果已经在html源码中添加了不同设备的适配,其中的文字就会自动换行,但是图片如果大过屏幕的宽度,html的约束就会失去效果,这样webView显示出来后图片显示不全,出现了可以左右滑动的情况.解决方式如下:
>  这种情况一般 `webView.scalesPageToFit = YES` 也无效

```
- (void)webViewDidFinishLoad:(UIWebView *)webView
{
    //拦截网页图片  并修改图片大小
    [webView stringByEvaluatingJavaScriptFromString:
    @"var script = document.createElement('script');"
    "script.type = 'text/javascript';"
    "script.text = \"function ResizeImages() { "
    "var myimg,oldwidth,oldheight;"
    "var maxwidth=300;" //缩放系数
    "for(var i=0;i <document.images.length;i++){"
    "myimg = document.images[i];"
    "oldwidth = myimg.width;"
    "oldheight = myimg.height;"
    "if(myimg.width > maxwidth){"
    "myimg.style.width = maxwidth+'px';"
    "myimg.style.height = (oldheight * (maxwidth/oldwidth))+'px';"
    "}"
    "}"
    "}\";"
    "document.getElementsByTagName('head')[0].appendChild(script);"];

    [webView stringByEvaluatingJavaScriptFromString:@"ResizeImages();"];

}

```

## 禁用webView中点击or长按响应

在webView中长按就会弹出拷贝等功能的弹窗,如果图片或文字添加了超链接还能进行跳转.通常我们看别人的app里的图文控件是不是webView,用这种方式就可以看出来.
如果你不想让别人看出来你这是webView,添加以下代码

```
- (void)webViewDidFinishLoad:(UIWebView *)webView
{
    ...
    //禁用webView长按弹窗
    [webView stringByEvaluatingJavaScriptFromString:@"document.documentElement.style.webkitUserSelect='none';"];

    [webView stringByEvaluatingJavaScriptFromString:@"document.documentElement.style.webkitTouchCallout='none';"];

}
//该方法的返回值用以控制是否允许加载目标链接页面的内容，返回YES将直接加载内容，NO则反之。
- (BOOL) webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType
{
    //用户触击了一个链接
    if (navigationType == UIWebViewNavigationTypeLinkClicked) {
      return NO;
    }

    return YES;
}

```

UIWebViewNavigationType枚举:

* UIWebViewNavigationTypeLinkClicked，      用户触击了一个链接。
* UIWebViewNavigationTypeFormSubmitted，    用户提交了一个表单。
* UIWebViewNavigationTypeBackForward，      用户触击前进或返回按钮。
* UIWebViewNavigationTypeReload，           用户触击重新加载的按钮。
* UIWebViewNavigationTypeFormResubmitted，  用户重复提交表单
* UIWebViewNavigationTypeOther，            发生其它行为。


## webView链接失效的时候出现 404 页面

如果不想在网页加载失败的时候出现404扫兴,就添加代码吧!

```
- (BOOL) webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType
{
    static BOOL isRequestWeb = YES;

    if (isRequestWeb) {
        NSHTTPURLResponse *response = nil;

        NSData *data = [NSURLConnection sendSynchronousRequest:request   returningResponse:&response error:nil];

        if (response.statusCode == 404) {
            // code for 404
            return NO;
        } else if (response.statusCode == 403) {
            // code for 403
            return NO;
        }

        [webView loadData:data MIMEType:@"text/html" textEncodingName:nil baseURL:[request URL]];

        isRequestWeb = NO;
        return NO;
    }

    return YES;
}

```
