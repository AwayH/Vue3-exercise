# Vue Router 動態路由

當我們的應用程式有無數個頁面（例如：商品頁面、使用者個人資料）且格式一致時，不可能為每個 ID 手動建立靜態路由。這時我們就需要 **動態路徑參數**。

## 檔案架構與內容

我們將新增一個 `Product.vue`，用來模擬根據不同 ID 顯示不同產品內容的場景。

```
src/
 ├─ router/
 │   └─ index.js
 ├─ views/
 │   ├─ Home.vue
 │   ├─ About.vue
 │   └─ Product.vue
 ├─ App.vue
 └─ main.js
```

## 檔案內容

### 動態頁面內容

在元件中，我們可以使用 `$route.params` 來取得網址上的參數。

```vue
<script setup>
import { useRoute } from 'vue-router';

const route = useRoute();

// 在 JS 中取得參數的方法
console.log('產品 ID:', route.params.id);
</script>

<template>
  <h1>產品頁面</h1>
  <p>目前查看的產品 ID 是：{{ $route.params.id }}</p>
</template>
```

### 路由設定

動態路由的特徵是在路徑中使用 **冒號** `:` 開頭。

- `:id`: 這是一個佔位符（Placeholder）。當網址為 `/product/123` 時，`id` 的值就會是 123。

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
      path: '/',
      name: 'about',
      component: () => import('@/views/About.vue'),
    },
    {
      path: '/product/:id', // 動態參數設定
      name: 'product',
      component: () => import('@/views/Product.vue'),
    },
    {
      path: '/:pathMatch(.*)*',
      redirect: '/',
    },
  ],
});
```

::: tip
**同一元件重複使用** : 當您從 `/product/1` 切換到 `/product/2` 時，Vue 為了效能會重複使用同一個元件實體，因此 `onMounted` 不會再次觸發。如果需要監控參數變化，可以使用 `watch` 監聽 `route.params`。
:::

## 建立選單導覽

在導覽中，我們可以透過字串模板或是物件形式來傳遞動態參數。

```vue
<!-- /App.vue -->
<template>
  <router-link to="/">首頁</router-link> |
  <router-link to="/about">關於</router-link> |
  <router-link to="/product/a123">產品 A</router-link> |
  <router-link
    :to="{
      name: 'product',
      params: {
        id: 'b456',
      },
    }"
    >產品 B</router-link
  >

  <RouterView />
</template>

<style>
nav {
  padding: 10px;
}
.router-link-active {
  font-weight: bold;
  color: #42b983;
}
</style>
```

## 動態路由 vs 查詢參數 (Query)

有時候您也會看到網址後方帶有問號，這兩者在 Vue Router 中有不同的處理方式：

| 特性     | 動態路由(Params)            | 查詢參數 (Query)               |
| -------- | --------------------------- | ------------------------------ |
| 網址範例 | `/product/123`              | `/product?id=123`              |
| 定義方式 | 需在路由表定義 `:id`        | 不需要預先定義                 |
| 取得方式 | `$route.params.id`          | `$route.query.id`              |
| 語意     | 代表「資源」的一部分 (必要) | 代表「過濾/排序」條件 (非必要) |

:::tip
如果希望網址看起來像是一個獨立的頁面(產品主頁)，請使用 **Params**；如果是在做搜尋結果或分頁，請使用 **Query**。
:::
