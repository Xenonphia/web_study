---
tags:
  - XXE
  - WEB
Date: 2026-01-05
---
当直接使用外部实体伪协议file://的时候，如果被过滤

![](assets/参数实体&http协议利用/file-20260105201017199.png)

则需要利用参数实体的方式获取DTD
利用http伪协议远端读取dtd文件给参数实体
然后调用参数实体
构建dtd文件
```XML
<!ENTITY guc SYSTEM "file:///etc/passwd">
```
