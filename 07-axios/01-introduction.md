# Axios 的介紹

Axios 是一個用來發送 HTTP 請求的 JavaScript 套件，可以用在瀏覽器與 Node.js。在網頁前端開發中，常見的呼叫後端 API、取得/送出 `JSON` 資料、登入/登出、表單發送、資料列表，全部都逃不開 Axios。

## 為什麼要用 Axios?

比起 Wed API 原生的 `XMLHttpRequest`、`fetch`，Axios 很貼心地做了很多事情，如下:

- 自動將回傳資料轉成 `JSON`。
- 支援 Request / Response 攔截器。
- 內建 timeout、錯誤處理。
- 支援舊瀏覽器。

## Axios 的安裝。

```bash
npm install axios
# 或簡寫
npm i axios
```

或

```html
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
```
