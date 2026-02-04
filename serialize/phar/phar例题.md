---
tags:
  - phar
  - PHP
  - serialize
Date: 2026-02-05
---
```php
<?php  
highlight_file(__FILE__);  
error_reporting(0);  
class TestObject {  
    public function __destruct() {  
        include('flag.php');  
        echo $flag;  
    }  
}  
$filename = $_POST['file'];  
if (isset($filename)){  
    echo md5_file($filename);  
}  
//upload.php  
?>
```
phar文件能上传到服务器端   ---   /upload.php
要有可用反序列化魔术方法作为跳板   ---   destruct()
要有文件操作函数   ---   md5_file
文件操作函数参数可控   ---   \$\_POST['file']

### 解析步骤
1.生成一个phar文件；在mate-data里放置一个包含TestObject()的序列化字符串；
2.上传文件；
3.md5file执行phar伪协议，触发反序列化；反序列化TestObject触发destruck执行echo $flag