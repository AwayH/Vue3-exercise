# 什麼是 Pinia？

Pinia 為 Vue 的官方推薦的狀態管理工具，用來集中管理 **全域狀態**。它是 Vuex 的後繼方案，由 Vue 核心團隊成員開發，設計更簡潔、API 更直覺，也完整支援 Vue3 的 Composition API。

## 為什麼需要 Pinia、Vuex？

在小型專案中，您可以用 `props` 傳資料、`emit` 傳事件、`provide/inject`、composable。但當專案變大時會出現問題，如: 多層元件傳 `props` 很痛苦、多個頁面需要共用資料、API 資料需要快取、使用者登入狀態需要全域共享、多個模組之間要互相影響。這時候就需要一個 **集中管理狀態** 的工具 **Pinia** 或 **Vuex**。

## 為什麼選擇 Pinia？

管理全域狀態並非只有 Pinia 可以做到，在早期 Vue2 的時代中，官方就是以 Vuex 為主，至目前的 Vue3 亦可使用 Vuex，但官方卻推薦 Pinia。主要原因如下:

| 比較       | Pinia |  Vuex  |
| ---------- | :---: | :----: |
| 語法       | 簡潔  |  複雜  |
| mutation   |   X   |   O    |
| TypeScript |  好   | 較麻煩 |
| 結構彈性   |  高   |  嚴格  |
| 官方推薦   | Vue3  |  Vue2  |

## Store 的檔案結構

`stores` 置放每個不同領域的全域資料。如下：

```
src/
 ├─ stores/
 │   ├─ user.js
 │   ├─ cart.js
 │   └─ product.js
 ├─ App.vue
 └─ main.js
```

## 核心概念

Store 是保存狀態和業務邏輯的實體，類似於元件。主要有三個部分所組成:

| 組成      | 用途                         |
| --------- | ---------------------------- |
| `state`   | 儲存資料                     |
| `getters` | 計算屬性，用於計算衍生狀態。 |
| `actions` | 修改 `state` 或執行非同步    |
