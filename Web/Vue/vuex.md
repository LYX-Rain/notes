# vuex使用经验

## 安装

1. 先安装vue，初始化vue-cli

```
$ vue init <template-name> <project-name>
$ vue init webpack my-project
```

- 测试建议使用webpack-simple学习

2. 安装vuex

```
(c)npm install vuex --save
```

- 要注意的是这里一定要加上 –save，因为你这个包我们在生产环境中是要使用的。

## 使用

- 再次强调，我们通过提交 mutation 的方式，而非直接改变 store.state.count，是因为我们想要更明确地追踪到状态的变化。这个简单的约定能够让你的意图更加明显，这样你在阅读代码的时候能更容易地解读应用内部的状态改变。此外，这样也让我们有机会去实现一些能记录每次状态改变，保存状态快照的调试工具。
- 由于 store 中的状态是响应式的，在组件中调用 store 中的状态简单到仅需要在计算属性中返回即可。触发变化也仅仅是在组件的 methods 中提交 mutation。

### 单页面使用

- store.state 来获取状态对象，以及通过 store.commit 方法触发状态变更。

#### 一个简单的计数器例子：

```html
<div id="app">
  <p>{{ count }}</p>
  <p>
    <button @click="increment">+</button>
    <button @click="decrement">-</button>
  </p>
</div>
```

```javascript
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment: state => state.count++,
    decrement: state => state.count--
  }
})

new Vue({
  el: '#app',
  computed: {
    count () {
      return store.state.count
    }
  },
  methods: {
    increment () {
      store.commit('increment')
    },
    decrement () {
      store.commit('decrement')
    }
  }
})
```

- 每当 store.state.count 变化的时候, 都会重新求取计算属性，并且触发更新相关联的 DOM。

### 组件化使用

1. 在src目录下新建store文件夹用来放vuex配置代码
2. 在该目录下创建store.js作为配置文件

```javascript
//一个最简单的示例
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
    state: {                    //在此处放data
        arr: [
            {age:1,name:"test1"},
            {age:2,name:"test2"},
            {age:3,name:"test3"}
        ]
    }
});
export default store    //export store出去让外部可以调用
```

3. 在组件中直接使用

- 由于 Vuex 的状态存储是响应式的，从 store 实例中读取状态最简单的方法就是在计算属性中返回某个状态：

```javascript
// 创建一个 Counter 组件
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return store.state.count
    }
  }
}
```

- 每当 store.state.count 变化的时候, 都会重新求取计算属性，并且触发更新相关联的 DOM。
- 然而，这种模式导致组件依赖全局状态单例。在模块化的构建系统中，在每个需要使用 state 的组件中需要频繁地导入，并且在测试组件时需要模拟状态。

- 通过在根实例中注册 store 选项，该 store 实例会注入到根组件下的所有子组件中，且子组件能通过 this.$store 访问到。

## 项目结构

- 需遵守的规则：

1. 应用层级的状态应该集中到单个 store 对象中。
2. 提交 mutation 是更改状态的唯一方法，并且这个过程是同步的。
3. 异步逻辑都应该封装到 action 里面。
- 对于大型应用，我们会希望把 Vuex 相关代码分割到模块中。下面是项目结构示例：

```bash
├── index.html
├── main.js
├── api
│   └── ... # 抽取出API请求
├── components
│   ├── App.vue
│   └── ...
└── store
    ├── index.js          # 我们组装模块并导出 store 的地方
    ├── actions.js        # 根级别的 action
    ├── mutations.js      # 根级别的 mutation
    └── modules
        ├── cart.js       # 购物车模块
        └── products.js   # 产品模块
```