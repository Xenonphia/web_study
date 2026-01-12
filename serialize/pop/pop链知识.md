---
tags:
  - pop
  - PHP
  - serialize
Date: 2026-01-12
---
POP链演示
```php
<?php  
highlight_file(__FILE__);  
error_reporting(0);  
class index {  
    private $test;  
    public function __construct(){        $this->test = new normal();  
    }  
    public function __destruct(){        $this->test->action();  
    }  
}  
class normal {  
    public function action(){  
        echo "please attack me";  
    }  
}  
class evil {  
    var $test2;  
    public function action(){  
        eval($this->test2);  
    }  
}  
unserialize($_GET['test']);  
?>
```

1.可利用的漏洞点在evil类的eval()
2.eval调用$test2
3.需要调用action()，上推到index类的destruct()，其中$test又调用了action()
4.php执行的时候$test赋值为normal类的一个对象，最后会执行normal类下的action()

对于最后的接收参数test，并且反序列化，一定会执行destruct()
因此只需要调整test的参数即可

```php
$temp = new index();

var_dump(serialize($temp));
```
先得到原本的对象序列化后的结果
```php
string(52) "O:5:"index":1:{s:11:"\000index\000test";O:6:"normal":0:{}}"

please attack me
```

对test赋值的时候将normal类更改到对应的evil类
