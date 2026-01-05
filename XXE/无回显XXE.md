---
tags:
  - XXE
  - XML
  - WEB
Date: 2026-01-05
---
# 首先查看漏洞是否存在
## DNSlog查看漏洞

首先正常的尝试外部实体调用

![](assets/无回显XXE/file-20260105222313901.png)

可以看到无回显

### 利用参数实体结合http伪协议插入DNS查询
