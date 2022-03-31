# Provide / Inject

文档：https://v3.cn.vuejs.org/guide/composition-api-provide-inject.html

在 setup 中如何使用 provide/inject 呢？

```js
const app = Vue.createApp({
  template: `
      <div>
        <child />  
      </div>
      `,
  setup() {
    const { ref, readonly, provide } = Vue;

    // 利用 ref 添加响应式
    const name = ref("张三");

    // 使用 provide(属性，数据)
    provide("name", readonly(name));
    provide("changeName", (val) => {
      name.value = val;
    });
  },
});

app.component("child", {
  template: `
      <div @click="handleClick">{{name}}</div>
      `,
  setup() {
    const { ref, inject } = Vue;

    // 使用 inject(属性，默认值可选)
    const name = inject("name");
    const changeName = inject("changeName");
    const handleClick = () => {
      changeName("里氏");
    };

    return {
      name,
      handleClick,
    };
  },
});

app.mount("#app");
```
