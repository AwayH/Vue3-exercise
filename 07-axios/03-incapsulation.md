# axios 的封裝

經過前面的說明應該都已經體驗過 `axios` 的使用與好處了。不過，隨著專案規模不斷地擴大，如果每次發送一次請求，就要把 `timeout`、`header`、`url`、錯誤處理等操作，全部都寫過一次，這樣不僅浪費時間又不好管理。為了提高程式碼的維護性、擴增性，我們需要將功能分類一下。

就如同上述所說，每次發送一個請求就要撰寫這些繁瑣的配置，以下為傳統寫法：

```js
axios({
  method: "get",
  url: "https://jsonplaceholder.typicode.com/users",
  timeout: 5000,
  headers: {
    'Content-Type': 'application/json',
    Authorization: 'xxx',
  },
})
  .then((res) => {
    console.log(res.data);
  })
  .catch((err) => {
    const status =  err?.response?.status;
    
    if(status === 401) console.log('權限過期');
    if(status === 403) console.log('無此權限');

    console.log(err);
  });
```

## 如何為 axios 進行封裝

我們必須統一設定，如：請求標頭、狀態碼、超過請求的時間、不同環境設置不同接口，並將請求的方法再次封裝與設置請求與回應的攔截器。

### axios 實體化

* `baseURL`：建議利用環境變數來判斷 `local`、`development`、`staging`、`production` 等環境。
* `timeout`：設置超時的請求時間。
* `headers`：設置預設的請求標頭。如果有特殊標頭以參數的方式傳入，將會覆蓋預設的請求標頭。

```js
const instance = axios.create({
  baseURL: 'https://jsonplaceholder.typicode.com',
  timeout: 5000,
  headers: {
    'Content-Type': 'application/json',
  },
});
```

### axios 方法封裝

主要是封裝一些常用的方法，如：`get`、`post`、`delete`、`put`、`patch`。自訂方法為大寫主要是要跟 `axios` 做區分。

```js
const api = {
  async GET(endPoint, params = {}) {
    try {
      const res = await instance.get(endPoint, { params });
      return res.data;
    } catch (err) {
      console.log(err);
    }
  },
};
```

::: tip
回傳 `res.data` 主要是讓方法的回傳不用多一層解構。
:::

### axios 攔截器

顧名思義，就是攔截一些資訊進行處理。主要是在 **送出請求之前** 和 **收到回應之後**，在還沒進入 `then()` 或 `catch()`
之前，將動作攔截下來，可進行一些加工的動作。基本語法如下：

``` js
function interceptors() {
  instance.interceptors.request.use(
    (req) => req,
    (err) => {},
  );
  instance.interceptors.response.use(
    (res) => res,
    (err) => {},
  );
}

interceptors();
```

* `request`：有時後端的 API 回應需要透過 token 的身份驗證，才會回傳資料。因此，我們就可以透過此攔截器在發送請求之前，將 token 加入到 `header` 中，這樣一來就能通過驗證。
* `response`: 攔截 API 回應的資料，並進行一些狀態的處理。

再做一點補強，如下：

``` js
function interceptors() {
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
}

interceptors();
```

> 參考來源：[axios](https://axios-http.com)

## axios 的封裝總結

|分層|責任|
|-|-|
|instance.js|建立 axios 實體|
|api.js|自訂的方法封裝|
|interceptors.js|建立攔截器來統一處理請求與回應|