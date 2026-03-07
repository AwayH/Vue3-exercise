# Vue Router 巢狀路由

當一個頁面元件內部還包含了自己的 **子路由** 時，我們就會使用 `children` 設定。這能讓我們在同一個佈局(Layout)下，靈活切換局部內容。

## 檔案架構與內容

我們以 **會員中心** 為例，會員中心會有一個外殼，裡面包含個人資料與訂單列表。

- 在 `views` 下新增 `member` 資料夾。
- `Member.vue` 作為父組件（外殼）。
- `Profile.vue` 與 `OrderList.vue` 作為子組件。

```
src/
 ├─ router/
 │   └─ index.js
 ├─ views/
 │   ├─ member/
 │   │   ├─ Profile.vue   (子頁面 1)
 │   │   └─ OrderList.vue (子頁面 2)
 │   ├─ Member.vue        (父頁面/外殼)
 │   └─ ...
 ├─ App.vue
 └─ main.js
```

## 檔案內容

### 父頁面元件 (外殼)

父元件內部必須準備一個 `<router-view />`，用來放置子路由的內容。

```vue
<template>
  <h1>會員中心</h1>
  <router-link to="/member/profile">個人資料</router-link>|
  <router-link to="/member/orders">訂單列表</router-link>
  <RouterView />
</template>
```

### 子頁面元件

這兩個元件會被渲染在父頁面的 `<router-view />` 位置。

```vue
<!-- /views/member/Profile.vue -->
<template>
  <h3>會員個人資料</h3>
</template>
```

```vue
<!-- /views/member/OrderList.vue -->
<template>
  <h3>訂單歷史紀錄</h3>
</template>
```

### 路由設定

在路由表中使用 `children` 屬性。注意：子路由的 `path` 通常不需要加斜線 `/`。如果加了 `/` 會被視為從根目錄開始。

```js
// /router/index.js
import { createRouter, createWebHistory } from 'vue-router';

export default createRouter({
  history: createWebHistory(),
  routes: [
    {
      path: '/',
      name: 'name',
      component: () => import('@/views/Home.vue'),
    },
    {
      path: '/about',
      name: 'about',
      component: () => import('@/views/About.vue'),
    },
    {
      path: '/product/:id',
      name: 'product',
      component: () => import('@/views/Product.vue'),
    },
    {
      path: '/member',
      component: () => import('@/views/Member.vue'),
      children: [
        {
          path: '',
          name: 'member',
          redirect: '/member/profile',
        },
        {
          path: 'profile',
          name: 'member-profile',
          component: () => import('@/views/member/Profile.vue'),
        },
        {
          path: 'orders',
          name: 'member-orders',
          component: () => import('@/views/member/OrderList.vue'),
        },
      ],
    },
    {
      path: '/:pathMatch(.*)*',
      redirect: '/',
    },
  ],
});
```

:::tip
**為什麼要用巢狀路由？**

- 程式碼複用：不需要在每個子頁面都重複寫導覽列或側邊欄。
- 狀態保持：切換子路由時，父組件不會被銷毀，這對於播放器或需要保持狀態的後台非常有用。
  :::

:::tip
通常在開發巢狀路由時，建議把 `name: 'member'` 從父層拿掉或移到子層，因為訪問 `/member` 時，它會立刻跳轉到 `/member/profile`，這時父層的 `name` 其實很少被直接用到。亦可移除因為導向父路由名稱時，系統不確定要停在父層還是直接進入預設子層的警告。
:::
