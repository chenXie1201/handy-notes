# ref 模版引用

文档：https://v3.cn.vuejs.org/guide/composition-api-template-refs.html

在 composition api 中如何使用 ref？

```js
// 1、起一个名字 ref="dom"
const app = Vue.createApp({
  template: `
      <div ref="dom">Hello Word</div>
      `,
  setup() {
    const { ref, onMounted } = Vue;

    // 2、必须声明一个和「1」命名一样的变量名，且必须用 ref(null)
    const dom = ref(null);

    // 3、必须在生命周期函数中使用
    onMounted(() => {
      console.log(dom.value);
    });

    return {
      dom,
    };
  },
});
app.mount("#app");
```
