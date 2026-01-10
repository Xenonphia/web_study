---
tags:
  - XML
  - WEB
date: 2026-01-04
---
实体是对数据的引用：根据实体种类的不同，XML解析器将使用实体的替代文字或者外部文档的内容代替实体引用
使用实体可以去掉符号的[[XML语法基础#实体引用|内容]]

### 字符实体就是<,>等几个符号

利用外部实体DTD读取文件后，再使用命名实体呈现出来
如果没有回显的时候，结合参数实体

## 实体的规范格式
都是以&开始;结束

```xml
<?xml version = "1.0" encoding = "utf-8"?>
<!DOCTYPE info[
    <!ELEMENT info (book)>
    <!ELEMENT book (author, title, year, price, description)>
    <!ELEMENT author (#PCDATA)>
    <!ENTITY writer "Guc">
    <!ENTITY popular "5.0">
    ]>

<info>
    <book>
        <author>&writer;</author>
        <title>XXE in PHP</title>
        <year>2024</year>
        <price>39.99</price>
        <description>This XML file is used to demonstrate XXE vulnerabilities in PHP applications.</description>
        <high>&popular;</high>
    </book>
    <book>
        <author>Jane Doe</author>
        <title>Secure Coding Practices</title>
        <year>2023</year>
        <price>49.99</price>
        <description>A comprehensive guide to secure coding techniques and best practices.</description>
    </book>
</info>
```
定义了实体writer和popular并且在下面调用

并且可以迭代调用

```XML
    <!ENTITY writer "Guc">

    <!ENTITY ch "&writer; skyline">
```

![](assets/XML实体/file-20260110220950234.png)

## 参数实体
替代可以重用的文本部分
```XML
<?xml version="1.0" encoding="utf-8"?>

<!DOCTYPE info[

    <!ENTITY % contents "author, title, year, price, description">

    <!ELEMENT info (book,video)>

    <!ELEMENT book (%contents;)>

    <!ELEMENT video (%contents;)>

    <!ELEMENT author (#PCDATA)>

]>
```
当定义元素的时候可能会重复使用，就可以设定一个参数替代重复文本然后调用
其中参数实体使用`％`调用
