# NVM 是什麼

* NVM = Node Version Manager，用來在同一台機器上安裝、管理與切換多個 Node.js 版本（含 npm）。
* 解決專案間 Node 版本不一致的問題，讓開發者能依專案需要切換 Node 版本。
* macOS / Linux 通常使用 nvm（shell script）。Windows 有社群版本 nvm-windows（不同專案 repo、細節略有差異）。

## 常用指令與示例

```bash
nvm ls                 # 列出本機安裝的 node 版本以及當前使用中的版本
nvm ls-remote          # 列出遠端可供安裝的 node 版本
nvm ls-remote --lts    # 列出遠端可供安裝的 node lts 版本
nvm install 18         # 安裝 Node.js v18（會安裝對應的 npm）
nvm install 18.17.0    # 安裝特定次版本
nvm use 18             # 切換到 node 18（shell session 當下生效）
nvm alias default 18   # 設定預設版本（新的 shell session 會使用）
nvm uninstall 18       # 移除該版本
nvm current            # 顯示當前版本
```

### 安裝並使用

```bash
nvm install v22.20.0
nvm use v22.20.0
node -v   # v20.20.0
```

### 設定預設

```bash
nvm alias default v22.20.0
```