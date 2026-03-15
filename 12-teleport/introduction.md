# Vue Teleport - 傳送

在 Vue 3 中，Teleport 是一個非常強大且實用的內置元件。它的核心功能是: 將元件內部的 HTML 結構傳送到元件 DOM 階層以外的指定位置，同時保留該組件的邏輯狀態。

## 使用 Vue Teleport 的理由

如果在一個深度嵌套的元件裡寫了一個全螢幕彈窗(Modal)，會遇到以下麻煩：

- CSS 限制: 如果外層組件設定了 `overflow: hidden` 或 `z-index`，彈窗可能會被遮擋或切掉。
- 層級混亂: 為了讓彈窗蓋住全螢幕，可能被迫把彈窗組件寫在 `App.vue` 最外層，這會導致邏輯分散，按鈕在內層元件，彈窗卻在外層組件。

:::tip
`Teleport` 的出現，可以在內層元件內編寫彈窗程式碼，但渲染出來的 DOM 卻出現在 `<body>` 或任何指定的地方。
:::

## 基本用法

```vue
<!-- /views/Slot.vue -->
<script setup>
import Modal from '@/components/Modal.vue';
</script>

<template>
  <Modal />
</template>
```

`<teleport>` 接收一個 `to` 的屬性來指定傳送的目標。`to` 的值可以是一個 CSS 選取器，也可以是一個 DOM 元素。以下程式碼的作用就是告訴 Vue 把這個元件傳送到 body 標籤內。

```vue
<!-- /components/Modal.vue -->
<script setup>
import { ref } from 'vue';

const open = ref(false);
</script>

<template>
  <button @click="open = true">Open Modal</button>

  <teleport to="body">
    <div v-if="open" class="modal">
      <p>Hello from the modal!</p>
      <button @click="open = false">Close</button>
    </div>
  </teleport>
</template>

<style scoped>
.modal {
  position: fixed;
  z-index: 999;
  top: 20%;
  left: 50%;
  width: 300px;
  margin-left: -150px;
}
</style>
```
