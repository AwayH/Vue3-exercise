# Vue Router 導航守衛

Vue Router 導航守衛就像是網頁路徑的保安或海關。當使用者試圖從 A 頁面跳轉到 B 頁面時，導航守衛會攔住這個請求，執行一些檢查。例如：使用者登入了嗎？有沒有權限？然後再決定要放行？取消跳轉？還是改派到其他地方？

## 檔案架構與內容

我們將新增一個 `Login.vue` 頁面來進行使用者登入。

```
src/
 ├─ router/
 │   └─ index.js
 ├─ views/
 │   ├─ member/
 │   │   ├─ Profile.vue
 │   │   └─ OrderList.vue
 │   ├─ Member.vue
 │   ├─ Login.vue
 │   └─ ...
 ├─ App.vue
 └─ main.js
```

## 檔案內容

### 路由設定

在導航守衛加入之前，我們需要改寫一下程式碼。

```js
// /router/index.js
import { createRouter, createWebHistory } from 'vue-router';

const router = createRouter({
  略...
});

export default router;
```

- 將需要加入導航守衛的路由，新增 `meta: { requiresAuth: true }` 設定。
- 設定每次跳轉前進行攔截檢查有無 token。無 token 即轉導 `/login` 頁面；若有，則放行。

```js
// /router/index.js
略...

const router = createRouter({
  略...,
  routes: [
    略...,
    {
      path: '/member',
      component: () => import('@/views/Member.vue'),
      meta: { requiresAuth: true },
      children: [ 略... ],
    },
    略...
  ],
});

router.beforeEach((to) => {
  if (to.meta.requiresAuth) {
    const token = localStorage.getItem('token');

    if (token) {
      return true;
    }

    return {
      path: '/login',
    };
  }
  return true;
});

略...
```

:::tip
通常會在檢查 token 存在後，進行 token 的有效性驗證。
:::

### 登入頁面

點擊按鈕會寫入 token 並轉導頁面。

```vue
<!-- /views/Login.vue -->
<script setup>
import { useRouter } from 'vue-router';

const router = useRouter();

function setToken() {
  localStorage.setItem('token', 'oooxxx');
  router.push('/member');
}
</script>

<template>
  <h1>Login</h1>
  <input type="button" value="click" @click="setToken" />
</template>
```
