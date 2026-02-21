# emit

用事件通知上(父)層。

## 快速觸發事件

在元件的樣版中直接使用 `$emit` 方法向上(父)層通知。

### 下(子)層。

```html
<input type="button" @click="$emit('someEvent')" value="通知上(父)層" />
```

### 上(父)層。

```html
<Counter @some-event="counter++" />
<Counter @some-event="() => counter++" />
<Counter @some-event="updateCounter" />
```

```js
function updateCounter() {
  counter.value++;
}
```

::: tip
若有需要向父層傳遞特殊資料，可用參數的方式傳遞。如: `$emit('someEvent', 100)`;
:::

## 宣告觸發事件

元件內用 `defineEmits()` 來宣告要觸發的事件，如下:

```js
const emit = defineEmits(['someEvent']);

function clickHandler() {
  emit('someEvent');
}
```
