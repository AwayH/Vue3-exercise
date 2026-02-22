# Composable 可重複使用的邏輯函數

簡單地說，就是把可以重複使用的 **狀態** 和 **行為** 封裝成 `function`。

## 常見的 Composable 使用情境

| 名稱            | 用途             |
| --------------- | ---------------- |
| useFetch        | API 請求         |
| useAuth         | 登入狀態         |
| useMouse        | 滑鼠座標         |
| useLocalStorage | 同步 localStorge |

## Composable 的架構設計與使用方式

`composables` 裡面放的內容通常用 Vue 特性把邏輯抽離出來、可重用的邏輯函數。

- 將目錄名稱設定為 `composables`。
- 新增的檔案以 `use` 作為開頭，小駝峰方式組合命名。
- 不要在 `composable` 裡直接操作 DOM，應該透過 `ref` 或生命週期。
- 只負責邏輯，不應該負責 UI。
- 以單一職責為前提，把同一個領域的邏輯聚合在一起。例如: 取得多個使用者與單一使用者的資料就可以寫在一起。

```
src/
 ├─ composables/
 │   ├─ useFetch.js
 │   ├─ useMouse.js
 │   └─ useAuth.js
 ├─ App.vue
 └─ main.js
```
