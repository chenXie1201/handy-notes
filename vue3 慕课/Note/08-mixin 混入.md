# mixin

文档：https://v3.cn.vuejs.org/guide/mixins.html

两个对象混合成一个对象

1、组件的中的 `data` 优先级高于 `mixin` 里的 `data`

2、混入的生命周期函数会比组件的生命周期函数先执行

3、组件里的 `methods` 里面的方法会覆盖 `mixin` 里面的 `methods` 方法

4、自定义的属性，组件中的属性优先级高于 `mixin` 属性的优先级

## 局部使用

子组件不能直接使用 `num` ，需要和父组件一样 `mixins[myMixin]`

```js
const myMixin = {
  data() {
    return {
      num: 2,
    };
  },
};
const app = Vue.createApp({
  template: `<div>{{num}}</div>`,
  mixins: [myMixin],
});
const vm = app.mount("#app");
```

## 全局使用

```js
app.mixin({
  data() {
    return {
      num: 1,
    };
  },
});
```

## $options

所有自定义的属性都会保存在 $options 中

```js
const myMixin = {
  number: 2,
};
const app = Vue.createApp({
  template: `<div>{{ $options.number }}</div>`,  // 1
  number: 1,
  mixins: [myMixin],
});
const vm = app.mount("#app");
```

## 更改 mixin 的优先级

优先使用 mixinVal 

```js
app.config.optionsMergeStrategies.number = (mixinVal, appValue) => {
  return mixinVal || appValue;
};
```
