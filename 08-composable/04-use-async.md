# Composable 範例 - 非同步處理

我們用之前封裝 axios 的案例來示範。

```js
// composables/useFetch.js
import api from '@/utils/axios/api';
import { ref } from 'vue';

export default (endPoint, config = {}) => {
  const data = ref(null);
  const isLoading = ref(false);
  const error = ref(null);

  async function loadData() {
    isLoading.value = true;
    error.value = null;

    try {
      const res = await api.GET(endPoint, config);

      if (!res) return;
      data.value = res;
    } catch (err) {
      error.value = err;
    } finally {
      isLoading.value = false;
    }
  }

  loadData();

  return { data, error, isLoading };
};
```

這裡要動點小手腳，目前已經封裝過的 axios 方法又被 `useFetch()` 封裝一次。我們需要將 axios 捕捉到的錯誤訊息往外傳，不要讓錯誤就停在這裡為止，這樣一來，讓呼叫端可以進行後續的處理。因此，多加了一行程式碼，如下:

```js
// utils/axios/api.js
import instance from './instance';

export default {
  async GET(endPoint, config = {}) {
    try {
      const res = await instance.get(endPoint, config);
      return res.data;
    } catch (err) {
      console.log('Axios 封裝方法失敗:', err);
      throw err; // 重新拋出錯誤給上層處理
      // return Promise.reject(err); // 也可以這樣寫
    }
  },
};
```

這次我們利用 `useFetch()` 傳入一個字串，給 `composable` 作為參數接收，讓此方法更加通用。另外，這次也使用了暴露出來的屬性來進行 UI (畫面) 的控制。

```vue
// App.vue
<script setup>
import useFetch from '@/composables/useFetch';

const {
  data: users,
  isLoading: usersIsLoading,
  error: usersError,
} = useFetch('/users');
</script>

<template>
  <div v-if="!usersIsLoading">
    <h2>Users</h2>
    <p v-if="usersError" v-text="usersError"></p>
    <ul v-else-if="users?.length">
      <li v-for="u in users" :key="u.id">
        <pre v-text="u"></pre>
      </li>
    </ul>
    <p v-else>No users data.</p>
  </div>
  <div v-else>Loading...</div>
</template>
```

::: tip
上述程式碼使用了 ES6 解構復值的語法來重新命名 `useFetch()` 回傳的屬性。主要是為了後續同時處理多個相似的 API 調用時，讓程式碼更為清晰、更容易維護。
:::
