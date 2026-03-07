# Vue Router

Vue Router 是 Vue 官方的路由管理工具，讓客戶端 (Client) 在單頁應用（SPA）中，將瀏覽器的 URL 和使用者看到的內容綁定在一起。當使用者在瀏覽不同頁面時，URL 亦會更新，最大的特徵就是可以在不重新整理頁面的情況下切換畫面。

## Vue Router 的核心功能

- 靜態路由: 也就是 URL → 對應 View 元件，當網址改變時 `<router-view />` 會自動渲染對應的畫面。
- 動態路由: 與上述類似，只是在 URL 中帶有變數、參數的路由。常用在使用者、商品、訂單、文章頁面。
- 巢狀路由: 在一個路由底下，再定義子路由，形成父子頁面結構。簡單講就是，頁面裡面還有子頁面
- 導航守衛: 主要用來控制登入驗證、權限。

## Vue Router 的檔案結構

依專案的大小，結構會有所不同，以下為最基本的布建:

```
src/
 ├─ router/
 │   └─ index.js
 ├─ views/
 │   ├─ Home.vue
 │   └─ About.vue
 ├─ App.vue
 └─ main.js
```

- router/: 只會放路由相關設定，不放 API、商業邏輯。
- views/: 頁面相關的元件，也就是可以對應一個 URL 的元件。

## Vue Router 的安裝

```bash
npm install vue-router
# 或簡寫
npm i vue-router
```
