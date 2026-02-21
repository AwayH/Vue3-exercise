# 環境變數的使用

在 `vite.config.js` 配置中，`mode` 參數表示當前的建構模式，它決定了應用程式的運行環境和行為。

## 預設模式

此模式僅能分辨出以下兩種模式:

- `development` - 開發模式（執行 `vite` 或 `vite dev` 時）
- `production` - 生產模式（執行 `vite build` 時）

```js
export default defineConfig(({ mode }) => {
  console.log(mode);

  return {
    略,
  };
});
```

### 建立環境檔

控制環境變數載入。在專案根目錄建立以下檔案:

- `.env` - 所有模式都會載入
- `.env.development` - 只在開發模式載入
- `.env.production` - 只在生產模式載入
- `.env.[mode]` - 載入特定模式的環境變數

| 檔案               | 說明                     |
| ------------------ | ------------------------ |
| `.env`             | 所有環境共用             |
| `.env.development` | `npm run dev` 時使用     |
| `.env.production`  | `npm run build` 時使用   |
| `.env.[mode]`      | 載入特定的環境變數時使用 |

### env 環境變數的命名規則

要讓程式碼中讀取的環境變數好辨識，變數名稱建議以 `VITE_` 開頭。

```ini
VITE_API_URL = https://api.example.com
VITE_APP_NAME = MyApp
```

## 進階模式

手動載入環境變數。

```js
import { defineConfing, loadEnv } from 'vite';

export default defineConfig(({ mode }) => {
  const env = loadEnv(mode, process.cwd(), '');

  console.log(env);

  return {
    略,
  };
});
```

### loadEnv 作用是什麼？

Vite 提供的 API，用來載入 .env 系列檔案，例如：

- `.env`。
- `.env.development`。
- `.env.production`。

```js
loadEnv(mode, process.cwd(), '');
```

#### mode

是 Vite 執行時的模式，例如 development 或 production，Vite 會依據 mode 去找正確的 `.env` 檔案。

- dev 時讀：.env + .env.development
- build 時讀：.env + .env.production

#### process.cwd()

代表目前專案的根目錄路徑。讓 Vite 知道到哪裡找 `.env` 檔案。

#### ''（前綴字）

環境變數的前綴過濾條件。沒寫 `''` 時，Vite 只會讀取以 `VITE_` 開頭的環境變數。

::: tip
若是遇到 `EsLint` 警告，以下任一方式都可以解決:

- 該行程式碼上方加上 `// eslint-disable-next-line no-undef`。
- 該檔案的第一行加上 `/* eslint-disable no-undef */`。
- 該檔案的第一行加上 `/* global process */`。
- 在 `ESLint` 配置中設定環境：

```js
{
  env: {
    node: true;
  }
}
```

:::
