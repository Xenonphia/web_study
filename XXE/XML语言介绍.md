---
tags:
  - XML
  - WEB
Date: 2026-01-03
---
# XML是一种标记语言
XML指可扩展标记语言
把数据<mark style="background: #FFB8EBA6;">结构化</mark>后<mark style="background: #FFB8EBA6;">传输</mark>给对方让对方在结构化的数据中读取
例如<mark style="background: #FFB8EBA6;">表格</mark>

结构类似于 <mark style="background: #FFB8EBA6;">根，枝，叶</mark>

## XML示例
```XML
<?xml version="1.0" encoding="utf-8"?> //XML声明
<root>
	<student>
			<name>Guc</name>
			<age>17</age>
	</student>
</root>
```
它的标签没有被预定义，需要自我定义，因此可以任意标签
而HTML的`<h1></h1>`就是预定义的一级标题
主要功能作为数据传输

## 对比HTML

| XML     | HTML  |
| ------- | ----- |
| 数据读取    | 开发网页  |
| 标签无意义   | 标签预定义 |
| 传输和存储数据 | 显示数据  |
因此，XML没有任何行为功能