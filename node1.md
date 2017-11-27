# 1.渲染视图模板方法：el， template, render

 1> render:
```js
    new Vue({
        el: '.main',
        data: {
            count: 1
        },
        render: function(createElement) {
            return createElement('节点名',{//几点类名等，或数据},[
                // '操作'
                    createElement('div',{},[
                        dcomProps: {
                            innerHTML: this.count
                        }
                    ])
                ],createeElement('div',{},['关于']))
        }
    })
```

# 2.vuex：专为vue应用程序开发的状态管理模式

### 1.下载 cnpm i vue vuex -D

```js
    <script src='./lib/vue.js'></script>
    <script src='./lib/vuex.js'></script>

简单的Store
let store = new Vuex.Store({
    state: {
        count: 1
    },
    mutations:{
        increate (state) {
            state.count += 1
        }
    },
    actions:{
        async_increate () {
            setTimeout (() => {
                store.commit('increate')
            }, 1000) {

            }
        }
    },
    getters: {
        odd (state) {
            return state.count % 2 === 0 ? state.count : '奇数'
        }
    }
})
commit调用mutations里的方法，dispatch调用actions里的方法，actions是定义方法调用的是mutations里的方法
```

### 2.Vue核心概念

#### 1.State

```js
    let {mapStates} = Vuex
    let vm = new Vue({
        el: '.main',
        store: store,
        template: `<div>
            <p>{{this.$store.state.count}}</p>
            <p>{{count}}</p>
        </div>`,
        computed: {
            ...mapStates(['count'])
        }
    })
```
#### 2. getters:对数据进行过滤并计算的时候使用getters方便一些

```js
    let {mapStates, mapGetters} = Vuex
    let vm = new Vue({
        el: '.main',
        store: store,
        template: `<div>
            <p>{{this.$store.getters.odd}}</p>
            <p>{{odd}}</p>
        </div>`,
        computed: {
            ...mapGetters(['odd'])
        }
    })
```
#### 3.mutations:更待Vuex的store中的状态的唯一方法是提交mutations。每一个mutations都有一个字符串的时间类型和一个回调函数
```js
    let {mapMutations} = Vuex
    let vm = new Vue({
        el: '.main',
        store: store,
        template: `<div>
            <p>{{this.$store.state.count}}</p>
            <p>{{count}}</p>
            <button @click='incerate'></button>
        </div>`,
        computed: {
            ...mapStates(['count'])
        },
        methods: {
            mapMutations(['increate'])
        }
    })
    或：methods: {
          increat () {
              this.store.commit('increate')
          }
    }

    含参数的时候：

    mutations: {
        increate (state, step = 1) {
            state.count += step
        }
    }
    methods: {
            mapMutations(['increate'])
        }
    默认值：<button @click='ncreate()'>增加</button>

    参数： <button @click='ncreate(2)'>增加</button>

    或： methods: {
        odd () {
            this.store.commit('increate', 2)
        }  
    }
    <button @click='ncreate'>增加</button>   

    或： methods: {
        odd (step) {
            this.store.commit('increate', step)
        }  
    } 
    <button @click='ncreate(2)'>增加</button> 

```

#### 4.actions: 类似于mutations，单是也有不同之处

    不同点： 1>Acttion提价的是mutation，而不是直接变更的状态
            2>Action可以包含任意异步操作

```js
    let {mapActions} = Vuex
    let vm = new Vue({
        el: '.main',
        store: store,
        template: `<div>
            <p>{{this.$store.state.count}}</p>
            <p>{{count}}</p>
            <button @click='async_decreat'>异步减少</button>
        </div>`,
        methods: {
            async_decreat () {
                    this.$store.dispatch('async_decreat')
            }
        }
    })

    PS:调用mutations的时候使用的是commit，而调用actions的时候用的是dispatch
```
