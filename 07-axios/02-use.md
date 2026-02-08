# Axios 的使用

## 基本型

### 語法一

```js
import axios from "axios";

axios({
  method: "get",
  url: "https://jsonplaceholder.typicode.com/todos",
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
  .get("https://jsonplaceholder.typicode.com/todos")
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

```js
try {
  const res = await axios.get("https://jsonplaceholder.typicode.com/todos");
  console.log(res.data);
} catch (err) {
  console.error(err);
} finally {
  console.log("資料請求結束");
}
```
