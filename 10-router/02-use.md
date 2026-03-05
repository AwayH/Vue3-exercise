# Vue Router 的使用

## 安裝 Vue Router

```bash
npm install vue-router
# 或簡寫
npm i vue-router
```

## 基本設定

我們需要準備基本的環境設定，接下來的過程會很瑣碎。請各位要耐心跟上:

### 檔案架構與內容

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

#### 相關的檔案內容

首頁的頁面內容

```vue
<!-- /views/Home.vue -->
<template>
  <h1>Home</h1>
  <p>這是首頁</p>
</template>
```

關於我們的頁面內容

```vue
<!-- /views/About.vue -->
<template>
  <h1>About</h1>
  <p>這是關於頁面</p>
</template>
```

路由設定:

- 匯入相關的路由模組。
- 匯出路由的設定，內容如下:
  - 路由模式: 分為 `Hash` 與 `History` 兩種模式。
    - `Hash`: `createWebHashHistory()` 優點是不需要有後端設定，缺點為 URL 中會有 `#`，較不符合主流的使用。
    - `History` (推薦): `createWebHistory()` 優點符合主流使用，缺點是需要後端設定，如: IIS、Nginx。
  -

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
  ],
});
```

```js
// main.js
...
import router from './router';

const app = createApp(App);

app.use(router);
app.mount('#app');
```

```vue
<template>
  <router-link to="/">Home</router-link>
  <router-link to="/about">About</router-link>
  <RouterView />
</template>
```
