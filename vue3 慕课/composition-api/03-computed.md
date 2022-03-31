# computed

文档：https://v3.cn.vuejs.org/api/computed-watch-api.html#computed

如何在 setup 中使用 computed ?

```js
const app = Vue.createApp({
  template: `
        <div @click="handleClick">{{ count }}</div>
        <div>{{ countAddFive }}</div>
        `,
  setup() {
    // 先从 Vue 中导入 computed
    const { ref, computed } = Vue;

    const count = ref(0);
    const handleClick = function () {
      count.value++;
    };

    // 方法一
    // const countAddFive = computed(() => {
    //   return count.value + 5;
    // });
    // 方法二
    const countAddFive = computed({
      get: () => {
        return count.value + 5;
      },
      set: (param) => {
        console.log(param);
        count.value = 5;
      },
    });

    return {
      count,
      handleClick,
      countAddFive,
    };
  },
});
app.mount("#app");
```
