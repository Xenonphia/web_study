这是一个非常完美的复盘时机。这道题综合了 PHP 反序列化中几个最经典、最硬核的考点。

我们把整个解题过程像“破案”一样，从头到尾完整的梳理一遍。

---

### 第一阶段：不仅要“通”，还要“懂” —— POP 链条审计

我们的目标是：从 `unserialize` 开始，触发布置在代码深处的“机关”（危险函数）。

1. **入口点 (Entry Point)**：`Sun::__destruct`
    
    - **触发条件**：对象销毁时（脚本结束或被 GC 回收）。
        
    - **代码**：`die("..." . $this->sun);`
        
    - **逻辑**：字符串拼接对象，触发 `$this->sun` 的 `__toString`。
        
    - **连接**：把 `$sun` 设为 `Moon` 对象。
        
2. **跳板 1**：`Moon::__toString`
    
    - **触发条件**：对象被当做字符串使用。
        
    - **代码**：`$starship = $this->nearside; $starship();`
        
    - **逻辑**：把对象当做函数调用，触发 `__invoke`。
        
    - **连接**：把 `$nearside` 设为 `Earth` 对象。
        
    - **隐蔽的大坑**：**`return '';`**。
        
        - 这意味着不管 `$starship()` 执行了什么，返回给上一层 `die` 的永远是空字符串。这就导致了我们后来遇到的“明明执行了却没有回显”的问题。
            
3. **跳板 2**：`Earth::__invoke`
    
    - **触发条件**：对象被当做函数调用。
        
    - **代码**：`$oe->$ie = $ote;`
        
    - **逻辑**：给不可访问的属性赋值，触发 `__set`。
        
    - **连接**：把 `$oe` 设为 `Solar` 对象，`$ie` 随便起个不存在的名字。
        
4. **跳板 3**：`Solar::__set`
    
    - **触发条件**：给不可访问属性赋值。
        
    - **代码**：`$Dyson->$Sphere($this->Mars);`
        
    - **逻辑**：调用一个不存在的方法，触发 `__call`。
        
    - **连接**：
        
        - `$Dyson` (`$Mercury`) 设为另一个 `Solar` 对象（我们叫它 `s1`）。
            
        - `$Sphere` (`$Venus`) 设为不存在的方法名（其实这里我们用来传递原生类名，比如 `SplFileObject`）。
            
5. **终点 (Sink)**：`Solar::__call`
    
    - **触发条件**：调用不存在的方法。
        
    - **代码**：
        
        PHP
        
        ```
        $exploar = new $func($args[0]); // 实例化任意类
        $road = $this->Jupiter;
        $exploar->$road($this->Saturn); // 执行任意方法
        ```
        
    - **利用**：这是我们的最终武器。我们可以利用它实例化 PHP 的**原生类**（Native Classes）来绕过代码中没有直接写 `file_get_contents` 的限制。
        

---

### 第二阶段：过五关斩六将 —— 遇到的坑与绕过方案

理清链条只是第一步，这道题的难点在于它设置了三个障碍。

#### 障碍 1：Regex 正则过滤 (黑名单)

- **现象**：`Solar::__call` 里有 `preg_match("/exec|system|.../i", $args[0])`。
    
- **分析**：它只检查 `$args[0]`（即我们传给构造函数的参数，也就是文件名）。它过滤了 `system`、`flag`（其实代码里没过滤 flag，但过滤了 `highlight_file` 等）。
    
- **对策**：使用 **PHP 原生类**。
    
    - `SplFileObject`：专门处理文件的类，不需要用 `system` 等敏感函数。
        
    - 即使过滤了文件名里的关键字，也可以用 `php://filter` 伪协议绕过（虽然这题没用到）。
        

#### 障碍 2：异常中断 (Exception)

- **现象**：`unserialize` 之后紧接着就是 `throw new Exception`。
    
- **分析**：异常抛出后，程序终止。虽然对象会被销毁触发 `__destruct`，但异常信息的输出可能会覆盖或打断 `die` 的输出，导致我们看不到 Flag。
    
- **对策**：**GC 提前析构 (Fast Destruct)**。
    
    - 利用数组索引覆盖：`a:2:{i:0;O:Sun...;i:0;i:0}`。
        
    - 让对象在反序列化的**过程中**就变成“垃圾”被立刻回收，抢在报错之前执行 Payload。
        

#### 障碍 3：无回显 (The Silent Trap)

- **现象**：Payload 打过去，只显示 `Maybe you should fly to the`，后面是空的。
    
- **分析**：这是最折磨的一步。
    
    - 我们一开始用了 `current()`，它确实读取了文件第一行并**返回**了字符串。
        
    - 但是！`Moon::__toString` 里的代码是 `return '';`。它把 `current()` 返回的 Flag 丢进了垃圾桶，然后返回了空字符串给 `die`。
        
- **对策**：**直接输出 (Direct Output)**。
    
    - 不能依赖“返回值”，必须找一个能直接把内容打印到屏幕上的方法。
        
    - **`fpassthru()`**：这个方法的作用就是“读取剩余数据并**直接输出**到输出缓冲区”。它不经过 `return`，直接显示。
        

---

### 第三阶段：终极 Payload 的构成

最后成功的 Payload 是精密组装的机器：

PHP

```
// 1. 数组包装 (GC 绕过)
Array (
    0 => Sun Object (
        // 2. 入口
        sun => Moon Object (
            // 3. 触发 Invoke
            nearside => Earth Object (
                // 4. 触发 Set
                onearth => Solar Object (s2) (
                    // 5. 触发 Call (传递类名)
                    Mercury => Solar Object (s1)
                    Venus   => "SplFileObject" // 也就是 $func
                )
                // 6. 传递参数 (传递文件名)
                outofearth => "/flag" // 也就是 $args[0]
            )
        )
    )
    // 7. 触发 GC
    0 => 0 (覆盖上面的对象)
)

// Solar Object (s1) 的配置：
// 8. 最终执行的方法
Jupiter => "fpassthru" // 也就是 $road
Saturn  => ""          // 方法参数
```

### 总结与经验

这道题教会了我们：

1. **原生类是好东西**：当代码里没有文件读取函数，或者被 `disable_functions` 禁用时，`SplFileObject`、`GlobIterator`、`ReflectionClass` 是反序列化题目的三板斧。
    
2. **不仅要看入口，还要看中间件**：`Moon` 类里的 `return ''` 是最大的误导。审计代码时，要注意数据的流向，是 `return` 传递还是 `echo` 输出。
    
3. **生命周期控制**：利用数组索引冲突来触发 GC，是在严苛环境（如立马抛出异常、脚本超时前）下执行析构函数的必备技巧。
    

这次分析非常完整，祝贺你完全吃透了这道题！

源代码
```php
<?php  
  
highlight_file(__FILE__);  
error_reporting(0);  
  
class Sun{  
    public $sun;  
    public function __destruct(){  
        die("Maybe you should fly to the ".$this->sun);  
    }  
}  
  
class Solar{  
    private $Sun;  
    public $Mercury;  
    public $Venus;  
    public $Earth;  
    public $Mars;  
    public $Jupiter;  
    public $Saturn;  
    public $Uranus;  
    public $Neptune;  
    public function __set($name,$key){        $this->Mars = $key;        $Dyson = $this->Mercury;        $Sphere = $this->Venus;        $Dyson->$Sphere($this->Mars);  
    }  
    public function __call($func,$args){  
        if(!preg_match("/exec|popen|popens|system|shell_exec|assert|eval|print|printf|array_keys|sleep|pack|array_pop|array_filter|highlight_file|show_source|file_put_contents|call_user_func|passthru|curl_exec/i", $args[0])){            $exploar = new $func($args[0]);            $road = $this->Jupiter;            $exploar->$road($this->Saturn);  
        }  
        else{  
            die("Black hole");  
        }  
    }  
}  
  
class Moon{  
    public $nearside;  
    public $farside;  
    public function __tostring(){        $starship = $this->nearside;        $starship();  
        return '';  
    }  
}  
  
class Earth{  
    public $onearth;  
    public $inearth;  
    public $outofearth;  
    public function __invoke(){        $oe = $this->onearth;        $ie = $this->inearth;        $ote = $this->outofearth;        $oe->$ie = $ote;  
    }  
}  
  
  
  
if(isset($_POST['travel'])){    $a = unserialize($_POST['travel']);  
    throw new Exception("How to Travel?");  
}
```