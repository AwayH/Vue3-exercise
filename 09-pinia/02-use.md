# Pinia 基本用法

## 安裝 Pinia

```bash
npm install pinia
# 或簡寫
npm i pinia
```

## 註冊 Pinia

```js
// main.js
import { createApp } from 'vue';
import { createPinia } from 'pinia';
import App from './App.vue';

const app = createApp(App);

app.use(createPinia());
app.mount('#app');
```

## Pinia 的語法風格

### Option Store (選項式)

與 Vue2 架構相似，可以傳入帶有 `state`、`getters`、`actions` 這些屬性的物件。

```js
// stores/counter.js
import { defineStore } from 'pinia';

export default defineStore('counter', {
  state: () => {
    return {
      count: 0,
    };
  },
  getters: {
    doubleCount: (state) => {
      return state.count * 2;
    },
  },
  actions: {
    increment() {
      this.count++;
    },
  },
});
```

::: tip
各位可以把 `state` 想作是 store 儲存資料的地方(data)，`getters` 是 store 的計算屬性(computed)，`actions` 是 store 的方法(functions)。
:::

### Setup Store (組合式)

亦有偏向 Composition API 的寫作方式，傳入一個函數，回傳要暴露在外的屬性。

```js
// stores/counter.js
import { ref, computed } from 'vue';
import { defineStore } from 'pinia';

export default defineStore('counter', () => {
  const count = ref(0);
  const doubleCount = computed(() => count.value * 2);

  function increment() {
    count.value++;
  }

  return {
    count,
    doubleCount,
    increment,
  };
});
```

::: tip

- `ref()`: 儲存資料。
- `computed()`: 計算屬性，相當於 Option Store 中的 `getters`。
- `function()`: 方法，相當於 Option Store 中的 `actions`。
  :::

## 使用 Store

定義好的 Store，必須經過引入與調用，否則，是無法使用的。

```vue
// components/Counter.vue
<script setup>
import useCounterStore from '@/stores/counter';

const counterStore = useCounterStore();
</script>

<template>
  <div>
    <p>計數器: {{ counterStore.count }}</p>
    <p>雙倍計數: {{ counterStore.doubleCount }}</p>
    <input @click="counterStore.increment" type="button" value="increment" />
  </div>
</template>
```

## 從 Store 解構

store 是一個用 `reactive` 包裝過個物件。因此，我們不能直接對它進行解構，若強行解構將會失去原有的響應式特性。

為取出 store 中的屬性並保持其響應式，我們必須使用 `storeToRefs()`，若為 `action` 或非響應式的屬性，直接解構無妨。如下:

```vue
// components/Counter.vue
<script setup>
import { storeToRefs } from 'pinia';
import useCounterStore from '@/stores/counter';

const counterStore = useCounterStore();
const { count, doubleCount } = storeToRefs(counterStore);
const { increment } = counterStore;
</script>

<template>
  <div>
    <p>計數器: {{ count }}</p>
    <p>雙倍計數: {{ doubleCount }}</p>
    <input @click="increment" type="button" value="increment" />
  </div>
</template>
```
