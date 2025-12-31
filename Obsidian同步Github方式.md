下载Git插件后，如果代理失败
使用ssh链接
生成密钥后上传到github


生成密钥
ssh-keygen -t ed25519 -C "tjs666tjs@gmail.com"
- 私钥：`C:\Users\Skyline\.ssh\id_ed25519`
    
- 公钥：`C:\Users\Skyline\.ssh\id_ed25519.pub`

```Powershell
cd "C:\Users\Skyline\你的Vault目录"
git remote set-url origin git@github.com:Xenonphia/note_vault.git`
```





建议：
- 在 Obsidian Git 插件中，将 **Custom Git binary path** 指向系统 Git
    
- 保持远程 URL 为：
    

`ssh://git@ssh.github.com:443/Xenonphia/note_vault.git`