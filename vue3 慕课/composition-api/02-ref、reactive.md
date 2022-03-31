## ref、reactive 响应式引用

下面是一个案例:

```js
const app = Vue.createApp({
  template: `<div></div>`,
  setup(props, context) {
    let name = "dell";

    setTimeout(() => {
      name = "lee";
    }, 2000);

    return {
      name,
    };
  },
});
app.mount("#app");
```

以上代码定义一个了变量 name ，两秒后我们改变 name 的值，这个时候看页面却一直显示的是 dell 而不是 lee

### ref

Vue 为我们提供了 ref 的 api 用来处理基础数据类型响应式

```js
const app = Vue.createApp({
  // Vue 发现 name 是被 ref 处理的相应式数据就会对 name 进行处理，不需要我们写 name.value
  template: `<div>{{ name }}</div>`,
  setup(props, context) {
    const { ref } = Vue;

    // ref 利用 proxy 对 name 封装成 proxy({value:'dell'}),这样的响应式引用
    // 通过 proxy 对数据进行封装，当数据发生变化的时，触发模版等内容更新
    // ref 处理基础类型的数据（String、Number、Boolen、Symbol、Undefind、Null）
    let name = ref("dell");

    setTimeout(() => {
      name.value = "lee";
    }, 2000);

    return {
      name,
    };
  },
});
app.mount("#app");
```

### reactive

Vue 为我们提供了 reactive 的 api 用来处理引用类型的响应式

```js
// reactive 处理引用类型数据
const app = Vue.createApp({
  template: `<div>{{ nameObj }}</div>`,
  setup(props, context) {
    const { reactive } = Vue;
    const nameObj = reactive({
      name: "Dell",
    });

    setTimeout(() => {
      nameObj.name = "change Dell";
    }, 2000);

    return {
      nameObj,
    };
  },
});
app.mount("#app");
```

### readonly

Vue 为我们提供了 readonly 的 api 用来限制数据只读

```js
const app = Vue.createApp({
  template: `<div>{{ arr }}</div>`,
  setup(props, context) {
    const { reactive, readonly } = Vue;
    let arr = readonly([123]);
    arr[0] = [456]; // 发出警告

    return {
      arr,
    };
  },
});
app.mount("#app");
```

### toRefs

利用 toRefs 可以将对象解构的属性返回，且为响应式数据

```js
const app = Vue.createApp({
  template: `
  <div>{{ name }}</div>
  <div>{{ personObj }}</div>
  `,
  setup(props, context) {
    const { reactive, toRefs } = Vue;

    let personObj = reactive({
      name: "Dell",
    });

    setTimeout(() => {
      personObj.name = "change Dells";
    }, 2000);

    const { name } = toRefs(personObj);

    // toRefs 的底层工作，直接解构是不能响应式的，需要将整个 personObj 封装处理
    // reactive 把 personObj 封装成 Proxy({name:"Dell"})
    // toRefs 把 personObj 封装成 { name:Proxy({value:'Dell'}) }

    return {
      name,
      personObj,
    };
  },
});
app.mount("#app");
```

### toRef

利用 toRef 可以将对象中不存在的属性设置为响应数据

```js
const app = Vue.createApp({
  template: `<div>{{ name }}---{{ age }}</div>`,
  setup(props, context) {
    const { reactive, toRefs, toRef } = Vue;

    const data = reactive({ name: "dell" });
    const { name } = toRefs(data);
    const age = toRef(data, "age");

    setTimeout(() => {
      age.value = 18;
    }, 2000);

    return {
      name,
      age,
    };
  },
});
app.mount("#app");
```

### context

context 包含三个参数，分别是 attrs、slots、emit

```js
const app = Vue.createApp({
  template: `<child @change="..."  />`,
});
app.component("child", {
  template: `<div>child</div>`,
  setup(props, context) {
    const { attrs, slots, emit } = context;
    // console.log(attrs)
    // console.log(slots.default())
    function handleClick() {
      emit("change");
    }
    return {
      handleClick,
    };
  },
});
app.mount("#app");
```
