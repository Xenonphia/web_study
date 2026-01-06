---
tags:
  - XXE
  - XML
  - WEB
Date: 2026-01-06
---
## 什么是 XInclude？

XInclude 是 XML 标准的一部分，用于在一个 XML 文档中包含<mark style="background: #FFB8EBA6;">另一个 XML 文档</mark>的内容。它类似于编程中的"引入"或"导入"功能，有助于更好地组织和重用 XML 内容。

### 主要用途：

- **内容重用**：将通用内容放在单独文件中，在多个文档中引用
- **模块化管理**：将大型 XML 文档拆分为多个小文件，便于维护
- **简化更新**：修改被包含的文件，所有引用它的文档都会自动更新

### 基本语法：

首先需要声明 XInclude 命名空间，然后使用 <xi:include> 元素引用外部文件：

```XML
<!-- 声明命名空间 --> <root xmlns:xi="http://www.w3.org/2001/XInclude"> 
<!-- 包含外部 XML 文件 --> <xi:include href="header.xml"/> 
<!-- 文档主体内容 --> <content>这是主文档内容</content> 、
<!-- 包含另一个文件 --> <xi:include href="footer.xml"/> </root>
```

	### 功能类似于php include

