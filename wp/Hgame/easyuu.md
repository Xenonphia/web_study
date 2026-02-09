题目页面给出了一个上传接口，并且自带一个hello文件提供下载。
通过抓包发现几个接口
分别是
/api/list_dir(列出当前目录文件，通过path=控制)
/api/download_file/xxx(下载文件)
/api/upload_file(上传文件)

在app/update的目录找到easyuu.zip，通过下载接口得到后发现是题目源码
文件夹里面的.git首先查看历史
```rust
xxx
-    let flag = env::var("FLAG").unwrap_or_default();
-    view! { <p>{flag}</p> }
xxx
```
得知flag的位置是在env里面，但是直接用下载接口没有获取到，应该是有权限限制

在src/main.rs的主程序里面
利用ai分析
首先这里有一个上传点能够上传一个程序
```rust
line 88
async fn get_new_version() -> Option<Version> {
    use tokio::process::Command;
// 漏洞点：直接创建一个子进程运行 "./update/easyuu" 
    let output = Command::new("./update/easyuu")
        .output() // 这里触发了执行！
xxx
```
再者这里给了程序的调用点
```rust
line 54
async fn update_watcher() { 
let check_interval = Duration::from_secs(5); 
// 每 5 秒一次循环
loop { 
sleep(check_interval).await; 
// 漏洞点：周期性调用 get_new_version  
if let Some(new_version) = get_new_version().await {
```
最后这里提供了覆盖文件的漏洞
```rust
line 103
async fn update() -> Result<(), Box<dyn std::error::Error>> {
    let new_binary = "./update/easyuu";
    self_replace::self_replace(&new_binary)?;
// 漏洞点：利用 self_replace 库用新文件覆盖当前正在运行的进程
}
```

最后写一个c语言程序在linux环境静态编译获取env中的flag，上传后等执行刷新即可