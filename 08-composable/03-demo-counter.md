# Composable 範例 - 計數器

以下是個在 SFC 中，傳統計數器的寫法。如下:

```vue
// App.vue
<script setup>
import { ref } from 'vue';

const count = ref(0);

function increment() {
  count.value++;
}

function decrement() {
  count.value--;
}

function reset() {
  count.value = 0;
}
</script>

<template>
  <p>{{ count }}</p>
  <input @click="increment" type="button" value="increment" />
  <input @click="decrement" type="button" value="decrement" />
  <input @click="reset" type="button" value="reset" />
</template>
```

如同上個範例的演示，無法達到多元件重複使用相同邏輯。因此，我們將它處理成可重複使用的邏輯函數，也就是 `composable`。

依架構設計與使用方式的規範，新增一個以 `use` 作為開頭，小駝峰方式組合命名的檔案，並將此資源存放在 `composables` 的目錄之中。基本上，核心邏輯完全相同，我們只是把它移到外部的函數中，並回傳要暴露在外的屬性，也就是 `count`、`increment`、`decrement`、`reset`。這樣一來，`useCounter` 就可以在其他元件中重複使用，如下:

```js
// composables/useCounter.js
import { ref } from 'vue';

export default () => {
  const count = ref(0);

  function increment() {
    count.value++;
  }

  function decrement() {
    count.value--;
  }

  function reset() {
    count.value = 0;
  }

  return { count, increment, decrement, reset };
};
```

在 SFC 中引用即可。

```vue
// App.vue
<script setup>
import useCounter from '@/composables/useCounter';

const { count, increment, decrement, reset } = useCounter();
</script>

<template>
  <p>{{ count }}</p>
  <input @click="increment" type="button" value="increment" />
  <input @click="decrement" type="button" value="decrement" />
  <input @click="reset" type="button" value="reset" />
</template>
```
