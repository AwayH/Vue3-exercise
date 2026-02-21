# Composable 範例 - 取得滑鼠的移動座標

在 SFC 中，實現移動滑鼠取得即時的座標。如下:

```vue
// App.vue
<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const x = ref(0);
const y = ref(0);

function update(e) {
  x.value = e.pageX;
  y.value = e.pageY;
}

onMounted(() => {
  window.addEventListener('mousemove', update);
});

onUnmounted(() => {
  window.removeEventListener('mousemove', update);
});
</script>

<template>
  <p>Mouse Position: {{ x }}, {{ y }}</p>
</template>
```

但上述的撰寫方式無法達到多元件重複使用相同邏輯。因此，我們將它處理成可重複使用的邏輯函數，也就是 `composable`。

依架構設計與使用方式的規範，新增一個以 `use` 作為開頭，小駝峰方式組合命名的檔案，並將此資源存放在 `composables` 的目錄之中。基本上，核心邏輯完全相同，我們只是把它移到外部的函數中，並回傳要暴露在外的屬性，也就是 `x`、`y`。這樣一來，`useMouse` 就可以在其他元件中重複使用，如下:

```js
// composables/useMouse.js
import { ref, onMounted, onUnmounted } from 'vue';

export default () => {
  const x = ref(0);
  const y = ref(0);

  function update(e) {
    x.value = e.pageX;
    y.value = e.pageY;
  }

  onMounted(() => {
    window.addEventListener('mousemove', update);
  });

  onUnmounted(() => {
    window.removeEventListener('mousemove', update);
  });

  return { x, y };
};
```

在 SFC 中引用即可。

```vue
// App.vue
<script setup>
import useMouse from '@/composables/useMouse';

const { x, y } = useMouse();
</script>

<template>
  <p>Mouse Position: {{ x }}, {{ y }}</p>
</template>
```

::: tip
每一個引用 `useMouse` 的元件，都有獨立 `x`、`y` 狀態，不會相互影響。
:::
