# axios 的使用

## 基本型

### 語法一

```js
import axios from 'axios';

axios({
  method: 'get',
  url: 'https://jsonplaceholder.typicode.com/todos',
})
  .then((res) => {
    console.log(res.data);
  })
  .catch((err) => {
    console.error(err);
  });
```

### 語法二

```js
axios
  .get('https://jsonplaceholder.typicode.com/todos')
  .then((res) => {
    console.log(res.data);
  })
  .catch((err) => {
    console.error(err);
  });
```

## 常用設定

- `url`: 請求的 **網址**，為必填欄位。
- `method`: 請求的 **方法**，有 `GET`、`POST`、`PUT`、`PATCH`、`DELETE`，預設為 `GET`。

* `data`: 請求時所送出的資料，一般稱為 Resquest body。通常在 `POST`、`PUT`、`PATCH`、`DELETE` 才會用到。
* `baseURL`: 為請求網址可拆成兩段，此設定通常為 `url` 最前段。
* `headers`: 請求的附加資訊。常用來放置以下內容:
  - `Authorization`: Token / JWT。
  - `Content-Type`: 資料格式。
* `timeout`: 超過設定的請求時間便會停止請求。預設為 `0` 毫秒，永不逾時，只要連線沒斷就一直等。

## 實務型

在業界，常會使用 ES7 的 `async`、`await` 這種 `Promise` 的語法糖來處理非同步操作，讓非同步的程式碼看起來更加簡潔。如下：

```js
async function setData() {
  const res = await axios.get('https://jsonplaceholder.typicode.com/todos');
  console.log(res.data);
}

setData();
```

但這樣的語法糖卻又缺少了錯誤的處理。因此，使用 `try...catch` 來捕捉 `await` 產生的錯誤，會比傳統的 `then()` `catch()` 更為直觀。如下：

```js
async function setData() {
  try {
    const res = await axios.get('https://jsonplaceholder.typicode.com/todos');
    console.log(res.data);
  } catch (err) {
    console.log(err);
  } finally {
    console.log('資料請求結束');
  }
}

setData();
```
