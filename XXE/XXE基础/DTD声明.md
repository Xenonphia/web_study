---
tags:
  - PHP
  - WEB
  - XML
date: 2026-01-04
---
### DTD: Document Type Definition
<mark style="background: #FFB8EBA6;">文档类型定义</mark>
DTD可以用以定义标记的含义，约束XML规则的定义和陈述

#### DTD 可以被成行地声明于XML文档内部
#### 也可以作为外部引用，独立的DTD文件

没有DTD声明的时候xml文件可以随意定义添加标签
## 一般使用`<!DOCTYPE 根元素 [元素声明]>`
### 元素定义使用`<!ELEMENT 元素 (子元素)>`

例如如下声明：
```XML
<!DOCTYPE info [

<!ELEMENT info (book)>

<!ELEMENT book (author, title, year, price, description)>

<!ELEMENT author (#PCDATA)>

]>
```
最后的author元素的(#PCDATA)限制了字符串形式文本输入

还有以下常用的声明：
- [0] !ELEMENT 定义元素
- [1] !ATTLIST 定义元素的属性
- [2] !NOTATION 定义不被解释为元素或者属性的符号
- [3] !ENTITY 定义实体，这些实体可以在XML文档中被引用

### 定义实体，可以类比为定义一个变量

# 外部引入DTD
例如
```XML
<!DOCTYPE info SYSTEM "eg.dtd" >
```

对<mark style="background: #FFB8EBA6;">info</mark>这个根元素SYSTEM引用外部DTD规则
其中此时使用的相对路径，eg.dtd在同级目录下

```DTD
<!ELEMENT info (book)>

<!ELEMENT book (author, title, year, price, description)>

<!ELEMENT author (#PCDATA)>
```

因此，可以知道使用DTD声明，容易造成随意的文件读取，就是漏洞来源
<mark style="background: #FFB8EBA6;">SYSTEM</mark>也可读取其他的文件

