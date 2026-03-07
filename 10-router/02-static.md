# Vue Router 靜態路由

我們需要準備基本的環境設定，接下來的過程會很瑣碎。請各位要耐心跟上:

## 檔案架構與內容

準備以下的資料夾及檔案，至於內容要放些什麼，將會詳細解說。

- 新增 `router`、`views` 資料夾來置放相關檔案。
- `/router/index.js`: 路由相關設定。
- `/views/Home.vue`、`/views/About.vue`: 分別讓 URL 對應到頁面的元件。

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

## 檔案內容

首頁的頁面內容。

```vue
<!-- /views/Home.vue -->
<template>
  <h1>Home</h1>
  <p>這是首頁</p>
</template>
```

關於我們的頁面內容。

```vue
<!-- /views/About.vue -->
<template>
  <h1>About</h1>
  <p>這是關於頁面</p>
</template>
```

路由設定:

- 匯入相關的路由模組。

- 路由模式: 分為 `Hash` 與 `History` 兩種模式。
  - `Hash`: `createWebHashHistory()` 優點是不需要有後端設定，缺點為 URL 中會有 `#`，較不符合主流的使用。
  - `History` (推薦): `createWebHistory()` 優點符合主流使用，缺點是需要後端設定，如: IIS、Nginx。
- 路由列表設定:

  - 設定靜態路由: 特徵是固定的路徑，不會改變。
  - `/:pathMatch(.*)*` 是一個特殊的路由配置，通常用來處理 **404 頁面** 或 **未匹配的路由**。

    - `:pathMath`: 代表動態參數，也就是把匹配到的路徑 **存到一個叫 `pathMatch` 的參數裡**。例如：`/abc/def` 變成 `route.params.pathMatch`。
    - `(.*)`: 它是個正規表達式，意思是 **匹配任何字元**。
    - `*`: 為 Vue Router 的特殊語法，表示可以 **匹配多層路徑**。例如：`/abc`、`/abc/def`、`/abc/def/ghi`。

```js
// /router/index.js
import { createRouter, createWebHistory } from 'vue-router';

export default createRouter({
  history: createWebHistory(),
  routes: [
    {
      path: '/',
      name: 'home',
      component: () => import('@/views/Home.vue'),
    },
    {
      path: '/about',
      name: 'about',
      component: () => import('@/views/About.vue'),
    },
    {
      path: '/:pathMatch(.*)*',
      redirect: '/',
    },
  ],
});
```

::: tip
在 Vue Router 裡，路由是由上往下匹配。所以會先匹配到對象，如果沒有任何匹配才會進入到 `/:pathMatch(.*)*`。很多專案會做 404 頁面而不是 `redirect`，這樣使用者體驗會比較好。
:::

匯入路由物件並安裝到 Vue 的應用程式中。

```js
// /main.js
...
import router from './router';

createApp(App).use(router).mount('#app');
```

建立選單與顯示頁面內容。

- `router-link`: 是 Vue Router 提供的導覽元件，用來切換頁面，但不會重新載入整個頁面。
- `router-view`: 也是 Vue Router 提供的導覽元件，頁面內容顯示區。

```vue
<!-- /App.vue -->
<template>
  <router-link to="/">Home</router-link>｜
  <router-link to="/about">About</router-link>
  <RouterView />
</template>
```
