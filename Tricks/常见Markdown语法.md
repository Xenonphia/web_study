---
tags:
  - study
Date: 2026-01-01
---

# 链接笔记
# 内部链接[[]]
## 1.使用两个方括号即可
直接括起来
[ReadME](../ReadME.md)

## 2.可以在语句中链接
例如如下：
==这里有我的一份[[../ReadME|笔记]]==

### 使用括号内添加管道符|即可

## 3.链接到具体的某个章节标题内容
### 添加#号即可链接到笔记的内容
例如：
这是笔记的[[../ReadME#Markdown|内容]]

## 4.链接到具体的某字符
### 使用^符号链接
例如；
我想知道readme文件的内容是[什么](../ReadME.md#^1778d3)？

## 5.直接嵌入到当前位置
### 使用叹号!嵌入
![ReadME](../ReadME.md)
同样的，只需要嵌入某一个部分根据上述操作即可

![ReadME](../ReadME.md#^1778d3)

# 外部链接[]
## 使用一对中括号包裹文字然后()链接地址
[Google](https://google.com)

# 标题设置
## 标题根据#号数量确定级数

# 高亮
## 使用等于符号==
==我正在使用高亮==

# 粗体
**粗体设置****
可以使用星号直接包裹
或者使用快捷按键

# 斜体
## 一个星号
*==斜体文字==*

# 删除线
## 两根波浪线~
~~删除示例~~

# 无序列表
## 使用一个-加上point
- 这是一个实例

# 有序列表
## 直接数字.的形式
1.
	1.1
	1.2
2.
	2.1


# 代办事项
## -加上中括号
- [x] This is your none. ✅ 2026-01-02
### 可以用鼠标直接点击打勾
### 也可以在括号内输入对应字符或者数字
### 选择特殊标识或者气泡颜色
- [?] This is your none.
- [!] This is your none.
- [0] This is your none.
- [1] This is your none.
- [2] This is your none.
- [4] This is your none.

- [a] Yes 

#### 特殊功能对应主题的style编辑


# 引用 callout
强调功能
>==这是一次引用加上强调的内容==
>--引用实例
## 也可以嵌套

>第一次？
>>第二次
>>>第三次~!


# 标注
>[!caution] Title
>This is a noted callout!
>

## 使用提示词来改变前面的图标
例如bug note danger等
也可以鼠标右键点击修改
如果加-/+会默认收起/展开

>[!caution]- Title
>This is a noted callout!

>[!caution]+ Title
>This is a noted callout!

# 注释
在文字后加上[^1]的形式[^2]
并且可以直接放文段后也可以（需要空行一行）
也可以直接在末尾添加注释的形式^[这是第三个注释，RECO]
甚至可以链接到其他的笔记^[[ReadME](../ReadME.md)]

[^2]:可以通过阅读模式观看注释形式






[^1]:This is a footnote
