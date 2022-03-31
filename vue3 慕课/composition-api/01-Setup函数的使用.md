# Setup 函数的使用

## 组合式 API 的产生的背景

在之前的组合式 API 写法中，我们会在 data 中定义数据，methods 中定义方法，但是在大型项目开发的时候想找一个模块的逻辑就会变的非常复杂。(总是需要上下跳着查找逻辑)

```js
const app = Vue.createApp({
  template: "<div>{{ name }}</div>",
  data() {
    return {
      name: "",
      age: "",
    };
  },
  computed: {
    // ...
    getNameToLocal() {},
  },
  methods: {
    setName() {},
    // 其他方法
    setAge() {},
    // ...
  },
});
```

组合式 API 可以让我们的相关逻辑更聚合。

## 基础使用

组合式 API 必须使用 setup 函数

```js
const app = Vue.createApp({
  template: "<div @click='handleClick'>{{ name }}</div>",
  // created 实例被完全初始化之前执行
  // 所以不能使用 this
  setup(props, context) {
    // return 必须返回,返回的内容可以在 template 中使用
    return {
      name: "dell",
      handleClick: () => {
        alert("click");
      },
    };
  },
  mounted() {
    // 可以调用 setup
    console.log(this.$options.setup());
  },
});
app.mount("#app");
```