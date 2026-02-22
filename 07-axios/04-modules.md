# Axios 模組化

在實務開發中，Axios 模組化的目的是：

- 統一 `baseURL`、`headers`、token、錯誤處理。
- API 分類管理。
- 方便維護與擴充。

咦！？這不就是前一個章節做的事情嗎？沒錯！就是前面介紹的 **Axios 封裝**，剩下來就是將檔案分開維護。這樣一來，其他元件若要使用非同步載入就可以匯入封裝好的模組，再呼叫指定的方法即可。

## Axios 模組化的檔案結構

`utils` 所置放的內容為工具、無框架依賴、可重用的通用功能。如下：

- API/Axios 相關。
- 格式化日期、貨幣、數字工具。
- Storage 封裝。
- 驗證工具。
- 效率相關，如：debouncd。
- 權限判斷。
- 常數管理。

```
src/
 ├─ utils/
 │   └─ axios/
 │       ├─ instance.js
 │       ├─ api.js
 │       └─ interceptors.js
 ├─ App.vue
 └─ main.js
```

### Axios 實體 - instance.js

```js
import axios from 'axios';

const instance = axios.create({
  baseURL: 'https://jsonplaceholder.typicode.com',
  timeout: 5000,
  headers: {
    'Content-Type': 'application/json',
  },
});

export default instance;
```

### Axios 方法封裝 - api.js

```js
import instance from './instance';

export default {
  async GET(endPoint, config = {}) {
    try {
      const res = await instance.get(endPoint, config);
      return res.data;
    } catch (err) {
      console.log(err);
    }
  },
};
```

### Axios 攔截器 - interceptors.js

```js
import instance from './instance';

export default () => {
  instance.interceptors.request.use(
    (req) => {
      const token = localStorage.getItem('token');

      if (token) req.headers.Authorization = `Bearer ${token}`;
      return req;
    },
    (err) => Promise.reject(err),
  );

  instance.interceptors.response.use(
    (res) => res,
    (err) => {
      switch (err?.response?.status) {
        case 401:
          console.log('權限過期');
          break;
        case 403:
          console.log('沒有權限');
          break;
        case 500:
          console.log('伺服器錯誤');
          break;
        default:
          console.log('網路有問題');
          break;
      }

      return Promise.reject(err);
    },
  );
};
```

### Axios 註冊 - main.js

```js
import { createApp } from 'vue';
import App from './App.vue';
import setupInterceptors from '@/utils/axios/interceptors';

setupInterceptors();
createApp(App).mount('#app');
```
