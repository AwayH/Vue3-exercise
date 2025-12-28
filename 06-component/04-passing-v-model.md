# v-model

在元件上實現雙向綁定。

## 使用方式

### 下(子)層。

``` js
const counter = defineModel();
```
``` html
<input type="button" value="-" @click="counter--">
<span v-text="counter"></span>
<input type="button" value="+" @click="counter++">
```

### 上(父)層。

``` html
<Child v-model="counter" />
```

## 使用物件的形式宣告

``` js 
const counter = defineModel({
  required: true,
  type: Number,
  default: 10
});
```