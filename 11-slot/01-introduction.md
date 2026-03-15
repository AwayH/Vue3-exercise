# Vue Slot - 插槽

Vue 的 **slot(插槽)** 是元件開發中極其重要的觀念。簡單來說，它就是元件中的 **「預留位置」**。

當你開發一個通用元件，如：按鈕、彈窗...等，您可能不知道未來何時會在該元件中增加新內容，這時，就可以挖一個 **「洞」**，也就是 `<slot>`，讓元件在調用時自動填入那個 **「洞」**。

## 基本插槽 - Default Slot

最常見的用法，當元件中間有內容時，這些內容會自動填入元件中的 `<slot>`。

```vue
<!-- /components/Card.vue -->
<template>
  <section class="card">
    <slot>預設文字，如果沒傳內容就會顯示我</slot>
  </section>
</template>
```

```vue
<!-- /views/Slot.vue -->
<script setup>
import Card from '@/components/Card.vue';
</script>

<template>
  <card>我是插槽內容</card>
</template>
```

## 具名插槽 - Named Slots

如果一個元件有很多地方需要讓外部傳入，那就需要給插槽名稱，已指定傳入的位置。

元件中分別給予 `<slot>` 自行定義的名稱，如: `foot`、`extra`。

```vue
<!-- /components/Card.vue -->
<template>
  <div class="card">
    <div class="card-head">
      <img class="card-image" src="https://picsum.photos/320/180" alt="" />
    </div>
    <div class="card-body">
      <h2 class="card-title">卡片標題</h2>
      <p class="card-description">
        卡片描述~卡片描述~卡片描述~卡片描述~片描述~
      </p>
    </div>
    <div class="card-foot">
      <slot name="foot"></slot>
    </div>
    <div class="card-extra">
      <slot name="extra"></slot>
    </div>
  </div>
</template>
```

傳入內容的方式:

- `<template>` 標籤，將內容進行包覆。
- 標籤中加入指定插槽的名稱，指定名稱的方式有二:
  - `v-slot:foot`。
  - `#foot`。

```vue
<!-- /views/Slot.vue -->
<template>
  <card>
    <template #foot>
      <input type="button" value="click" />
    </template>
  </card>
</template>
```

## 作用域插槽 - Scoped Slots

因為作用域不同，外層元件無法直接存取內層元件的資料。但有時候，外層元件需要根據內層元件的資料來決定怎麼渲染內容。這時候，內層元件可以像「傳 Props」一樣，把資料傳回給插槽。

內層元件: 定義插槽並「綁定」數據。

```vue
<!-- /components/UserList.vue -->
<script setup>
import { reactive } from 'vue';

const users = reactive([
  { id: 9, name: 'Away', role: 'admin' },
  { id: 10, name: 'Tony', role: 'user' },
  { id: 11, name: 'Mary', role: 'user' },
]);
</script>

<template>
  <ul>
    <li v-for="u in users" :key="u.id">
      <slot :user="u"></slot>
    </li>
  </ul>
</template>
```

父層元件: 接收數據並「自定義」外觀。

```vue
<!-- /views/Slot.vue -->
<script setup>
import UserList from '@/components/UserList.vue';

function clickHandler(id) {
  console.log(id);
}
</script>

<template>
  <UserList v-slot="slotProps">
    {{ slotProps.user.name }} ({{ slotProps.user.role }})
    <input
      type="button"
      value="edit"
      @click="clickHandler(slotProps.user.id)"
    />
  </UserList>
</template>
```

:::tip
簡單來說，內層元件裡有資料，但不知道怎麼渲染；外層元件知道怎麼渲染，但辦沒有資料。作用域插槽就是那座橋樑，讓內層元件把資料傳給外層元件的插槽內容。
:::

### 總結

| 類型       | 核心概念                                         | 關鍵語法                                                     |
| ---------- | ------------------------------------------------ | ------------------------------------------------------------ |
| 基本插槽   | 簡單的內容替換。                                 | `<slot />`                                                   |
| 具名插槽   | 一個元件多個插槽，精確的內容替換。               | `name="body"` / `v-slot:body` 或 `#body`                     |
| 作用域插槽 | 內層傳外層，讓外層元件拿到內層元件的資料來渲染。 | `:user="u"` / `v-slot="slotProps"` 或 `#default="slotProps"` |
