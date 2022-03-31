## Vue2 的核心模块和历史遗留问题

Vue 2 是一个响应式驱动的、内置虚拟 DOM、组件化、用在浏览器开发，并且有一个运行时把这些模块很好地管理起来的框架。

![img](https://static001.geekbang.org/resource/image/df/a2/df099da509445a941d129eb9696935a2.jpg?wh=1661x957)

Vue2 常见的缺陷：

1、Vue2 是使用 Flow.js 来做类型校验，但现在 Flow.js 已经停止维护了，整个社区都在全面使用 TypeScript 来构建基础库，Vue 团队也不例外。

2、Vue 2 内部运行时，是直接执行浏览器 API 的

3、Vue2 响应式并不是真正意义上的代理，而是基于 `Object.defineProperty()` 实现的。这个 API 并不是代理，而是对某个属性进行拦截，所以有很多缺陷，比如：删除数据就无法监听。

4、Option API 在组织代码较多组件的时候不易维护。

## 七个方面了解 Vue3 新特性

1、RFC 机制

这个新特性和代码无关，而是 Vue 团队开发的工作方式。关于 Vue 的新语法或者新功能的讨论，都会先在 GitHub 上公开征求意见，邀请社区所有的人一起讨论。[点击这里](https://github.com/vuejs/rfcs)

2、响应式系统

Vue2 的响应式机制是基于 `Object.defineProperty()` 这个 API 实现的，此外，Vue 还使用了 `Proxy`，这两者看起来都像是对数据的读写进行拦截，但是 `defineProperty` 是拦截具体某个属性，`Proxy` 才是真正的“代理”。

defineProperty 用法

```js
object.defineProperty(obj, "title", {
  get() {},
  set() {},
});
```

当项目里 “读取 obj.title” 和 “修改 obj.title” 的时候被 `defineProperty` 拦截，但 `defineProperty` 对不存在的属性无法拦截，所以 Vue2 中所有数据必须要在 data 里声明。

Proxy 用法

```js
new Proxy(obj, {
  get() {},
  set() {},
});
```

虽然 `Proxy` 拦截 obj 这个数据，但 obj 具体是什么属性，`Proxy` 则不关心，统一都拦截了。而且 `Proxy` 还可以监听更多的数据格式，比如 Set、Map，这是 Vue2 做不到的。

`Proxy` 存在一些兼容性问题，这也是为什么 Vue3 不兼容 IE11 以下的浏览器的原因。

3、自定义渲染器

Vue 3 通过拆包，使用最近流行的 `monorepo` 管理方式，响应式、编译和运行时全部独立了，变成下图所示的模样。

![img](https://static001.geekbang.org/resource/image/95/0c/9573fb8b18cb694fe9959b82742ecb0c.jpg?wh=1444x824)

在 Vue3 的组织架构中，响应式独立了出来。而 Vue2 的响应式只服务于 Vue，Vue3 的响应式就和 Vue 解耦了

4、全部模块使用 TypeScript 重构

类型系统带来了更方便的提示，并且让我们的代码能够更健壮。

5、Composition API 组合语法

对于 `Options API` 的写法也有几个很严重的问题：

- 由于所有数据都挂载在 this 之上，因而 `Options API` 的写法对 `TypeScript` 的类型推导很不友好，并且这样也不好做 `Tree-shaking` 清理代码。

- 新增功能基本都得修改 `data`、`method` 等配置，并且代码上 300 行之后，会经常上下反复横跳，开发很痛苦。

- 代码不好复用，Vue2 的组件很难抽离通用逻辑，只能使用 `mixin`，还会带来命名冲突的问题。

对于 `Composition API` 带来的好处：

- 所有 API 都是 `import` 引入的，对 `Tree-shaking` 很友好

- 不再上下反复横跳，我们可以把一个功能模块的 `methods`、`data` 都放在一起书写，维护更轻松。

- `Composotion API` 新增的 `return` 等语句，在实际项目中使用

![img](https://static001.geekbang.org/resource/image/a0/5f/a0010538b40e48fc5fc68b0eed2b025f.jpg?wh=3220x2046)

6、新的组件

Vue3 还内置了 `Fragment`、`Teleport` 和 `Suspense` 三个新组件。

7、新一代工程化工具 `Vite`

`Vite` 主要提升的是开发的体验，`Webpack` 等工程化工具的原理，就是根据你的 import 依赖逻辑，形成一个依赖图，然后调用对应的处理工具，把整个项目打包后，放在内存里再启动调试。

现代浏览器已经默认支持了 ES6 的 `import` 语法，`Vit` 就是基于这个原理来实现的。

具体来说，在调试环境下，我们不需要全部预打包，只是把你首页依赖的文件，依次通过网络请求去获取