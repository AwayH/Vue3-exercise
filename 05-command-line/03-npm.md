# 什麼是 NPM

NPM 全名是 Node Package Manager（Node 套件管理器），是用來 安裝、管理、分享 JavaScript 套件（packages） 的工具。

## NPM 基本指令

### 安裝套件 （dependencies）

專案執行時會用到的套件（上線時也需要）。

```bash
npm install lodash --save
# 或簡寫
npm i lodash --save
```

#### NPM 5 之後，`--save` 成為預設行為。

```bash
npm install lodash
# 或簡寫
npm i lodash
```

### 安裝開發套件（devDependencies）

只在開發階段使用的套件（上線時不需要）。

```bash
npm install vite --save-dev
# 或簡寫
npm i vite -D
```

### 指定版本安裝

```bash
npm install express@4.18.3
```

### 全域安裝

```bash
npm install -g nodemon
# 或簡寫
npm i -g nodemon
```

### 更新套件

```bash
npm update
```

### 移除套件

```bash
npm uninstall lodash
```

## package.json 與 package-lock.json

| 檔案 | 功能 |
|-|-|
| `package.json` | 專案描述檔，記錄專案資訊、依賴套件、腳本（scripts） |
| `package-lock.json` | 鎖定套件版本，確保每次安裝結果一致 |

### package.json

```json
{
  "name": "my-app",
  "version": "1.0.0",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "start": "node index.js"
  },
  "dependencies": {
    "vue": "^3.5.0"
  },
  "devDependencies": {
    "vite": "^5.0.0"
  }
}
```

> `scripts` 可執行 npm script
> ```bash
> npm run dev
> ```

## 開發版本管理

網站或軟體的 版號規劃，通常是為了清楚標示軟體的更新內容、穩定性與相容性。

## 版本

### 格式

主版號.次版號.修訂號[-預發行版本][.建構元資訊]

```matlab
1.2.3
1.2.3-alpha.1
2.0.0-beta
```

### 組成說明

| 部分 | 說明 |
| - | - |
| **主版號 (Major)** | 大更新、破壞相容性，舊版可能無法直接升級 |
| **次版號 (Minor)** | 新功能更新，仍向下相容 |
| **修訂號 (Patch)** | 修正錯誤、微小改進，完全向下相容 |
| **預發行版本 (Pre-release)** | alpha / beta / rc (release candidate) 等，表示尚未正式穩定 |
| **建構元資訊 (Build metadata)** | 通常用來記錄 build number 或 commit id，不影響版本排序 |


## 其他套件管理工具

| 工具 | 特性 |
|-|-|
| npm | Node 官方預設，穩定、普遍 |
| yarn | 速度快、鎖檔一致性強、離線快取 |
| pnpm | 以硬連結節省磁碟空間、安裝極快、近年最受歡迎 |

## 套件版本管理

| 符號 | 說明 |
|-|-|
| `^1.2.3` | 可升級到 `1.x.x` 但不含 `2.0.0` |
| `~1.2.3` | 可升級到 `1.2.x` 但不含 `1.3.0` |
| `1.2.3` | 鎖定該版本 |