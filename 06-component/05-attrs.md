# 傳透屬性

父元件將資料傳遞給子元件，但子元件沒有宣告在 `defineProps` 中，都會被歸屬傳透屬性中 (`attrs`)。

元件的屬性設定:

```html
<MyButton class="primary" id="submit-btn" disabled text="送出" />
```

元件內的接收設定:

```js
defineProps({
  text: {
    type: String,
    required: true,
    default: 'button',
  },
});
```

以上的的狀況:

- `text`: `props`。
- `class`、`id`、`disabled`: 皆為傳透屬性 (`attrs`)。

## 傳透控制

預設為自動傳透，若要讓元件不要自動傳透，可以在元件中設定為 `false`。如下:

```js
defineOptions({
  inheritAttrs: false,
});
```

::: tip

- 傳透屬性就不會自動加到元件的根元素上。
- 必須用 `$attrs` 來進行控制。
  :::

## 使用 attrs 的方式

直接在元件中使用 `$attrs`。

```html
<p>{{ $attrs }}</p>
```

或是取得 `$attrs`。

```js
import { useAttrs } from 'vue';

const attrs = useAttrs();
```

```html
<ThroughIn v-bind="attrs" />
```

## $attrs 向外傳遞事件

必須使用 `onClick`。

```html
<input type="button" value="Out 按鈕" @click="$attrs.onClick" />
```
