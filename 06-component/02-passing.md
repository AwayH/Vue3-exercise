# 傳遞

## props

需要宣告接受傳入的資料，這樣 Vue 元件才能知道外部傳入的資料哪些是 `props`? 哪些是穿透屬性? `props` 可以使用 `defineProps()` 來宣告：

```js
<script setup>
defineProps(['name', 'age']);
</script>
```

```js
<script setup>
const props = defineProps(['name', 'age']);

console.log(props.name);
console.log(props.age);
</script>
```

```js
<script setup>
defineProps({
  name: String,
  age: Number
});
</script>
```

```js
<script setup>
defineProps({
  name: {
    type: String,
    default: ''
  },
  age: {
    type: Nubmer,
    default: 10
  }
});
</script>
```

```tip
* 靜態：值是固定字串，不會隨資料變動。
* 動態：綁定變數、表達式、運算結果。
```

## emit


## v-model

## 深層

## 狀態管理






## 靜態與動態

* https://cdsassets.apple.com/live/7WUAS350/images/iphone/iphone-17-pro-max-colors.png
* iPhone 17 Pro Max

### 動態

``` javascript
['html', 'rwd', 'javascript', 'vue', 'sass']
```

``` css
.bevel__item {
  position: relative;
  padding: 1rem;
  color: #666;
  line-height: 1;
  list-style-type: none;
  text-transform: uppercase;
  border: solid 1px #ccc;
}

.bevel__item::before,
.bevel__item::after {
  position: absolute;
  width: 16px;
  height: 16px;
  background-color: #fff;
  content: '';
  transform: rotate(45deg);
}

.bevel__item::before {
  top: -9px;
  left: -9px;
  border-right: solid 1px #ccc;
}

.bevel__item::after {
  bottom: -9px;
  right: -9px;
  border-left: solid 1px #ccc;
}

.bevel__item+.bevel__item {
  margin-top: 1rem;
}
```

### 型別與驗證

``` javascript
{
  name: 'Away',
  fullName: () => `${user.name} Hung`,
  age: 41,
  isMale: true,
  skills: ['RWD', 'CSS', 'Javascript', 'HTML'],
  books: {
    name1: 'ACA 國際認證教戰手冊：Dreamweaver CS5 完全攻略',
    name2: 'Illustrator CS5（補習班內部用書）',
    name3: 'Illustrator CS4（補習班內部用書）',
  },
}
```

### 渲染

``` javascript
[
  {
    id: 1,
    isRelease: false,
    name: '梵蒂岡驅魔士',
  },
  {
    id: 2,
    isRelease: true,
    name: '超級瑪利歐兄弟電影版',
  },
  {
    id: 3,
    isRelease: true,
    name: '捍衛任務4',
  },
  {
    id: 4,
    isRelease: false,
    name: '星際異攻隊3',
  },
  {
    id: 5,
    isRelease: true,
    name: '鈴芽之旅',
  },
]
```

``` css
.box {
  padding: .7rem 1rem;
  margin-bottom: 1rem;
}

.box--release {
  background-color: lightblue;
}

.box--no-release {
  color: #ccc;
  background-color: #eee;
}
```