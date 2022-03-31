# watchEffect

文档：https://v3.cn.vuejs.org/guide/reactivity-computed-watchers.html#watcheffect

watchEffect 监听器偏向于 effect

```js
setup() {
    const { reactive, toRefs, watchEffect } = Vue;
    const person = reactive({
        name: "",
    });
    const { name } = toRefs(person);

    // watchEffect 函数和 watch 方法的区别：
    // 立即执行，immediate,页面加载立即执行
    // 不需要传递你要监听的内容，方法会自己感知，只需要传递一个回调函数
    // 不能获取改变之前的数据
    const stop = watchEffect(() => {
        console.log(person.name);
    });

    // 停止监听
    setTimeout(() => {
        stop();
    }, 5000);

    return {
    name,
    };
}
```
