---
tags:
  - XML
  - WEB
Date: 2026-01-03
---
### 对比HTML
#### XML标签需要闭合

```HTML
<p>This is a test.
```
在HTML标签不闭合是被允许的

```XML
<p>This is a test.</p>
```
而XML标签则必须闭合

### 大小写敏感

```XML
<t1>False</T1>
<t2>True</t2>
```
闭合标签需要一致的形式

### XML必须有根元素
XML必须有一个元素是其他元素的父元素，可以有多个枝节元素

### 实体引用
在XML中，一些字符有特殊含义
例如`<`会被当作新元素的开始
`<message>This is a < message.</message>`
这时就会报错，因为中间的<会导致识别错误