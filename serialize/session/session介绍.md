---
tags:
  - PHP
  - serialize
  - session
Date: 2026-01-31
---
当session_start()被调用或者php.ini中session.auto_start为1时
PHP内部调用会话管理器，访问用户session被序列化以后，存储到指定目录（默认为/tmp）。
存取数据的格式有多种，常用的有三种

### 漏洞产生原因
写入格式和读取格式不一样

![](assets/session介绍/file-20260204152951880.png)


在网页提交字符串后，查看tmp文件夹

![](assets/session介绍/file-20260204153255939.png)

产生了对应的session文件，其格式是键名|键值，也就是第一种php的存储格式

如果使用serialize的格式进行存储也需要handler声明
```php
<?php  
highlight_file(__FILE__);  
error_reporting(0);  
ini_set('session.serialize_handler','php_serialize');  
session_start();  
$_SESSION['benben'] = $_GET['ben'];  
$_SESSION['b'] = $_GET['b'];  
?>
```

![](assets/session介绍/file-20260204153906583.png)

此时就以反序列化`a`数组形式展现

同样的，binary格式也需要声明
```php
<?php  
highlight_file(__FILE__);  
error_reporting(0);  
ini_set('session.serialize_handler','php_binary');  
session_start();  
$_SESSION['benben'] = $_GET['ben'];  
$_SESSION['b'] = $_GET['b'];  
?>
```

![](assets/session介绍/file-20260204154518068.png)

将得到的sess放进winhex查看二进制形式，可以看到键名前面有表示长度(06--benben/01--b)的标识

### 漏洞的产生
1.储存部分
使用serialize的格式存储
```php
<?php  
highlight_file(__FILE__);  
error_reporting(0);  
ini_set('session.serialize_handler','php_serialize');  
session_start();  
$_SESSION['ben'] = $_GET['a'];  
?>
```

2.漏洞页面
使用php的形式读取
```php
<?php   
highlight_file(__FILE__);  
error_reporting(0);  
  
ini_set('session.serialize_handler','php');  
session_start();  
  
class D{  
    var $a;  
    function __destruct(){  
        eval($this->a);  
    }  
}  
?>
```

