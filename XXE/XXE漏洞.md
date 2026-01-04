---
tags:
  - WEB
  - XXE
Date: 2026-01-04
---
## 什么是XXE
XXE 全称 XML 外部实体注入
实体来自SYSTEM本地资源public公共计算机

其中DTD声明会读取外部文件，可以使用命名实体呈现到页面
漏洞示例：

```php
<?php

error_reporting(0);

libxml_disable_entity_loader(false);

$xml = file_get_contents('php://input');

if(isset($xml)){

    $dom = new DOMDocument();

    $dom->loadXML($xml, LIBXML_NOENT | LIBXML_DTDLOAD);

    $creds = simplexml_import_dom($dom);

    $benben = $creds->admin;

    echo $benben;

}

highlight_file(__FILE__);

?>
```
最主要的在于`$benben = $creds->admin;`
POST提交参数后展示了admin分支
