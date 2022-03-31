# directive

文档：https://v3.cn.vuejs.org/guide/custom-directive.html#%E7%AE%80%E4%BB%8B

## 全局使用

```js
// 注册一个全局自定义指令 `v-focus`
app.directive("focus", {
  // 当被绑定的元素挂载到 DOM 中时……
  mounted(el) {
    el.focus();
  },
});
```

## 局部使用

```js
const directives = {
  focus: {
    mounted(el) {
      el.focus();
    },
  },
};

const app = Vue.createApp({
  template: `
        <input v-focus />
        `,
  directives,
});
```
