```php
<?php  
highlight_file(__FILE__);  
/*hint.php*/  
session_start();  
class Flag{  
    public $name;  
    public $her;  
    function __wakeup(){        $this->her=md5(rand(1, 10000));  
        if ($this->name===$this->her){  
            include('flag.php');  
            echo $flag;  
        }  
    }  
}  
?>
```
### 分析
Flag类里面的属性her会在反序列化的时候触发wakeup赋值1-10000随机md5值
判断条件是her\=\=name

### 解决
通过引用的方式令name为her的值

首先打开hint.php
```php
<?php  
highlight_file(__FILE__);  
error_reporting(0);  
ini_set('session.serialize_handler', 'php_serialize');  
session_start();  
$_SESSION['a'] = $_GET['a'];  
?>
```
提供了一个session提交的接口
此时只需要构造新的Flag类即可