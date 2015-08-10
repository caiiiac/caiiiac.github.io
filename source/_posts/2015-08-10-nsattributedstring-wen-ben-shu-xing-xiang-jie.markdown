---
layout: post
title: "NSAttributedString 文本属性详解"
date: 2015-08-10 14:03:23 +0800
comments: true
categories: ios
---


## NSAttributedString包含的所有Key

```
NSFontAttributeName;                  //字体，value是UIFont对象
NSParagraphStyleAttributeName;        //绘图的风格（居中，换行模式，间距等诸多风格），value是NSParagraphStyle对象
NSForegroundColorAttributeName;       // 文字颜色，value是UIFont对象
NSBackgroundColorAttributeName;       // 背景色，value是UIFont
NSLigatureAttributeName;              //  字符连体，value是NSNumber
NSKernAttributeName;                  // 字符间隔
NSStrikethroughStyleAttributeName;    //删除线，value是NSNumber
NSUnderlineStyleAttributeName;        //下划线，value是NSNumber
NSStrokeColorAttributeName;           //描绘边颜色，value是UIColor
NSStrokeWidthAttributeName;           //描边宽度，value是NSNumber
NSShadowAttributeName;                //阴影，value是NSShadow对象
NSTextEffectAttributeName;            //文字效果，value是NSString
NSAttachmentAttributeName;            //附属，value是NSTextAttachment 对象
NSLinkAttributeName;                  //链接，value是NSURL or NSString
NSBaselineOffsetAttributeName;        //基础偏移量，value是NSNumber对象
NSUnderlineColorAttributeName;        //下划线颜色，value是UIColor对象
NSStrikethroughColorAttributeName;    //删除线颜色，value是UIColor
NSObliquenessAttributeName;           //字体倾斜
NSExpansionAttributeName;             //字体扁平化
NSVerticalGlyphFormAttributeName;     //垂直或者水平，value是 NSNumber，0表示水平，1垂直
```


第一步创建UILabel及NSMutableAttributedString的实例对象,之后的效果改变都是作用在它们上面:

```
UILabel *label = [[UILabel alloc] initWithFrame:CGRectMake(0, 0, self.view.bounds.size.width - 40, 50)];
label.textAlignment = NSTextAlignmentCenter;
label.center = self.view.center;
[self.view addSubview:label];

NSMutableAttributedString *attributedString = [[NSMutableAttributedString alloc] initWithString:@"caiiiac.github.io"];


```


**字体 颜色 背景色**

* `NSFontAttributeName`
* `NSForegroundColorAttributeName`
* `NSBackgroundColorAttributeName`

效果

![字体 颜色 背景色](/imagesWithBlog/60734898-ECE5-44D4-898D-E6EF83D7651E.png)

代码

```
NSDictionary * attris = @{NSForegroundColorAttributeName:[UIColor whiteColor],
              NSBackgroundColorAttributeName:[UIColor grayColor],
              NSFontAttributeName:[UIFont boldSystemFontOfSize:30]};

[attributedString setAttributes:attris range:NSMakeRange(0,attributedString.length)];
label.attributedText = attributedString;
```


**下划线**

* `NSUnderlineStyleAttributeName`
* `NSUnderlineColorAttributeName`

效果

![下划线](/imagesWithBlog/6F199774-62FD-4553-AC2C-F0132641DCB7.png)

代码

```
NSDictionary * attris = @{NSFontAttributeName:[UIFont boldSystemFontOfSize:30],
              NSForegroundColorAttributeName:[UIColor orangeColor],                          
              NSUnderlineStyleAttributeName:@(NSUnderlineStyleSingle),                             
              NSUnderlineColorAttributeName:[UIColor blueColor],};

[attributedString setAttributes:attris range:NSMakeRange(0,attributedString.length)];
label.attributedText = attributedString;
```

**描边**

* `NSStrokeColorAttributeName`
* `NSStrokeWidthAttributeName`

效果

![描边](/imagesWithBlog/0C9788C6-1AB5-43E2-9C39-0605E22F2990.png)

代码

```
NSDictionary * attris = @{NSFontAttributeName:[UIFont boldSystemFontOfSize:30],
              NSForegroundColorAttributeName:[UIColor whiteColor],
              NSStrokeColorAttributeName:[UIColor blueColor],
              NSStrokeWidthAttributeName:@(2)};

[attributedString setAttributes:attris range:NSMakeRange(0,attributedString.length)];
label.attributedText = attributedString;
```


**阴影**

* `NSShadowAttributeName`

效果

![阴影](/imagesWithBlog/FC216299-3DFD-4897-9340-1BDCD9EE1C7A.png)

代码

```
NSShadow * shadow = [[NSShadow alloc] init];
shadow.shadowColor = [UIColor blueColor];
shadow.shadowBlurRadius = 4.0;
shadow.shadowOffset = CGSizeMake(2.0, 2.0);
NSDictionary * attris = @{NSFontAttributeName:[UIFont systemFontOfSize:30],
              NSShadowAttributeName:shadow};

[attributedString setAttributes:attris range:NSMakeRange(0,attributedString.length)];
label.attributedText = attributedString;
```


**字符间隔**

* `NSKernAttributeName`

默认间隔

![默认间隔](/imagesWithBlog/1B2B4760-01BD-44C8-90A1-41B1B101B622.png)

间隔为5

![间隔为5](/imagesWithBlog/D7997F03-CE7E-4689-8E2E-23C6318CE56D.png)

代码

```
NSDictionary * attris = @{NSKernAttributeName:@(5),
              NSFontAttributeName:[UIFont systemFontOfSize:30]};

[attributedString setAttributes:attris range:NSMakeRange(0,attributedString.length)];
label.attributedText = attributedString;
```


**字体倾斜**

* `NSObliquenessAttributeName `

效果

![字体倾斜](/imagesWithBlog/A3BCEAB0-8946-4A3F-8545-A0CD624047D3.png)

代码

```
NSDictionary * attris = @{NSObliquenessAttributeName:@(0.8),
              NSFontAttributeName:[UIFont systemFontOfSize:30]};

[attributedString setAttributes:attris range:NSMakeRange(0,attributedString.length)];
label.attributedText = attributedString;
```

**字体扁平化**

* ` NSExpansionAttributeName`

效果

![字体扁平化](/imagesWithBlog/E00D97D1-F0AC-4ED3-82C5-96FA8BA19551.png)

代码

```
NSDictionary * attris = @{NSExpansionAttributeName:@(1.0),
              NSFontAttributeName:[UIFont systemFontOfSize:30]};

[attributedString setAttributes:attris range:NSMakeRange(0,attributedString.length)];
label.attributedText = attributedString;

```


添加图片

* `NSAttachmentAttributeName`

效果

![添加图片](/imagesWithBlog/3A996070-E495-48B8-A3D1-39B217D28866.png)

代码

```
NSTextAttachment * attach = [[NSTextAttachment alloc] init];
attach.image = [UIImage imageNamed:@"收藏后"];
attach.bounds = CGRectMake(2, -4, 20, 20);
NSAttributedString * imageStr = [NSAttributedString attributedStringWithAttachment:attach];

[attributedString appendAttributedString:imageStr];

label.attributedText = attributedString;

```


绘图风格

* `NSMutableParagraphStyle`

效果

![绘图风格](/imagesWithBlog/021BF7E2-1BE4-4509-B70C-95268B594D8C.png)

代码
自定义TextView类

```
@implementation TextView

- (void)drawRect:(CGRect)rect {
// Drawing code

    NSMutableAttributedString * attributeStr = [[NSMutableAttributedString alloc] initWithString:@"绘图风格（居中，换行模式，间距等诸多风格），value是NSParagraphStyle对象"];
    NSMutableParagraphStyle * paragraphStyle = [[NSMutableParagraphStyle alloc] init];
    paragraphStyle.alignment = NSTextAlignmentRight;
    paragraphStyle.headIndent = 4.0;
    paragraphStyle.lineBreakMode = NSLineBreakByCharWrapping;
    paragraphStyle.lineSpacing = 2.0;
    NSDictionary * attributes = @{NSParagraphStyleAttributeName:paragraphStyle};
    [attributeStr setAttributes:attributes range:NSMakeRange(0, attributeStr.length)];
    [attributeStr drawInRect:self.bounds];

}

@end
```

ViewController中添加代码

```
TextView *textView = [[TextView alloc] initWithFrame:CGRectMake(0, 0, 100, 80)];
textView.backgroundColor = [UIColor whiteColor];
textView.center = CGPointMake(self.view.center.x, self.view.center.y + 30);
[self.view addSubview:textView];
```

**Demo地址:**[点此查看](https://github.com/caiiiac/AttributeStringDemo)
