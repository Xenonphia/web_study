---
tags:
  - XXE
  - SSRF
  - WEB
Date: 2026-01-05
---
通过当前主机A作为跳板利用SSRF获取内网主机的信息

`*`得到内网ip`10.1.2.3`后
使用http伪协议

![](assets/XXE进行SSRF/file-20260105203516407.png)

回显了执行命令的提示
这就是获取了内网主机的信息
```http
<result><code>0</code><msg>请输入要执行的命令，例如：http://ip/?cmd=ls</msg></result>
```

