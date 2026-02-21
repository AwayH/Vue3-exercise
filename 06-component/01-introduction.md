# 元件

元件是可重用、封裝的 UI 單元。封裝模板、邏輯、樣式（SFC：.vue），透過 `props` 接收資料、用 `emits` 發出事件、用 `slots` 插入內容。

## 最基本的 Vue3 元件結構

Vue3 常用 Single File Component（.vue）。

```vue
<script setup>
import { ref } from 'vue';

const title = '我是元件';
const count = ref(0);
</script>

<template>
  <div class="card">
    <h2>{{ title }}</h2>
    <button @click="count++">點我 {{ count }}</button>
  </div>
</template>

<style scoped>
.card {
  border: 1px solid #ccc;
}
</style>
```

### 區塊說明

- `<script setup>`: JS 邏輯。
- `<template>`: HTML 結構。
- `<style scoped>`: CSS 樣式。
