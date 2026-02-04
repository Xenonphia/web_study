了解了一下go语言的pool
在 Go 语言中，频繁地创建和销毁对象会给垃圾回收（GC）带来压力。为了优化性能，开发者使用了 sync.Pool。

在monitor.go文件中
```go
func UserCmd(c *gin.Context) 
{ （获取对象） 
monitor := MonitorPool.Get().(*MonitorStruct) 
这行代码会在函数结束时执行，把对象放回池子 
defer MonitorPool.Put(monitor)
monitor.Args 就会被写入内存 
if err := c.ShouldBindJSON(monitor); err != nil { // 4. 【致命错误】如果 JSON 解析报错（比如类型对不上） // 程序会直接走这里报错并 return c.JSON(400, gin.H{"error": err.Error()}) return }