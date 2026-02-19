# provide / inject

跨層級傳資料，不用一層一層 `props` 往下傳。

當我們需要從父元件向子元件傳遞資料時，多層級嵌套的元件會形成一個巨大的元件樹，某個深層的子元件需要一個較外層的祖先傳遞資料，這樣用層層傳遞的 `props`，不僅費時、費工、又不好維護。因此，`provide` 和 `inject` 就是來幫助我們解決此類問題。

## 基本用法

### 父元件: provide

```js
import { provide } from "vue";

provide("theme", "dark");
```

### 子元件: inject

```js
import { inject } from "vue";

const theme = inject("theme");
```

::: tip
如果沒有父元件的 `provide` 時，子元件使用 `inject` 要噴警告! 因此，建議加上預設值較佳。
```js
const theme = inject("theme", "light");
```
:::

## 響應式資料

### `ref` 或 `reactive`

#### 父元件: provide

```js
import { ref, provide } from "vue";

const counter = ref(0);
provide("counter", counter);
```

#### 子元件: inject

```js
import { inject } from "vue";

const counter = inject("counter", 10);

counter.value++;
```

### 方法

#### 父元件: provide

```js
import { provide } from "vue";

function updateTheme(val) {
  theme.value = val;
}

provide("updateTheme", updateTheme);
```

#### 子元件: inject

```js
import { inject } from "vue";

const updateTheme = inject("updateTheme");

updateTheme("light");
```
