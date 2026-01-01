---
Trick: 软件同步
Date: 2026-01-01
---

下载Git插件后，如果代理失败
使用ssh链接
生成密钥后上传到<mark style="background: #FFB8EBA6;">Github</mark>接收

```Powershell
ssh-keygen -t ed25519 -C "tjs666tjs@gmail.com"
ssh私钥存储地址
- 私钥：`C:\Users\Skyline\.ssh\id_ed25519`
    
- 公钥：`C:\Users\Skyline\.ssh\id_ed25519.pub`
```

进入仓库面板打开终端
```Powershell
cd "C:\Users\Skyline\你的Vault目录"
git remote set-url origin git@github.com:Xenonphia/note_vault.git`
```

git remote set-url origin ssh://git@ssh.github.com:443/Xenonphia/note_vault.git
*注意使用443端口不被拦截，默认的22端口不能联通*