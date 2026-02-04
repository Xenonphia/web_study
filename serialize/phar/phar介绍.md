---
tags:
  - phar
  - PHP
  - serialize
Date: 2026-02-04
---
# 什么是phar?
-JAR是开发Java程序一个应用，包括所有的可执行、可访问的文件，都打包进了一个JAR文件里，使得部署过程十分简单。
==like a Java JAR,but foe PHP==
==PHAR("PhpARchive")==是PHP里类似于JAR的一种打包文件
对于PHP<mark style="background: #FF5582A6;">5.3</mark>或更高版本，Phar后缀文件是默认开启支持的，可以直接使用它。

文件包含：phar伪协议，可读取.phar文件

# phar结构
stub phar文件标识，格式为xxx<?php xxx;__HALT_COMPiLER();?>；（头部信息）
manifest压缩文件的属性等信息，以序列化存储；
contents压缩文件的内容；
signature签名，放在文件末尾；

Phar协议解析文件时，会自动触发对<mark style="background: #FFB8EBA6;">manifest字段</mark>的序列化字符串进行==反序列化==
因此，在有destruct和wakeup等函数的时候就可以利用phar反序列化漏洞