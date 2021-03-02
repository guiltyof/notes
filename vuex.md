[toc]
## vuex
#### 什么是vuex
**vuex是一个状态管理模式**

1.采用集中式存储管理应用的所有组件的状态。
2.集成了官方的调试工具，提供了高级调试功能。

用人话讲就是：
**当需要多个组件通信，共享一些变量时，我们知道通常可以使用父子组件的通信。当这些组件关系复杂且层级差距非常大时，我们可以通过定义一个对象用来存储这些需要共享的数据，让其他组件都从这个对象中取数据。但是这种做法并不能实现响应式的数据绑定，即类似v-bind的双向绑定。**

**综上，我们将多个组件共享的变量存储在一个对象中，将这个对象放在顶层的实例中，其他组件都可以使用，这样多个不相干的组件就可以共享这个对象的变量属性了。而vuex的意义就是响应式，他相当于就是封装了上述这么一个对象，但同时可以保证所有属性的响应式实现。**

经典使用场景，共同点是在多个不同的页面多次要使用的属性：
1.用户登录状态、token、头像、id、地理位置等等。
2.商品收藏、购物车。


#### 定义
一般在src目录下新建一个store/index.js，在其中配置store并export。
```
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
  state: {},
  mutations: {},
  actions: {},
  getters: {},
  modules: {}
})

export default store
```
然后在main.js中挂载。当然也可以直接在main.js中定义，但这会使得main.js内容更加杂乱无章。而上述写法更加规范化，更加合理。
```
import Vue from 'vue'
import store from './store'


new Vue({
  el: '#app',
  router,
  store
})
```
最后在不同的组件中就可以通过使用$store来访问不同的属性了。
```
<template>
<h2>{{$store.state.name}}</h2> // 使用store中的定义的name
</template>
```

#### 单一状态树
即single source of truth，可以理解为单一数据源

如一个人的个人档案、户口、医疗、文凭都在不同的单位保存和管理。如果要办理一些需要用多个内容的业务，需要找很多不同的部门签字盖章。这样很低效，而且维护起来也很麻烦。这样就是一个典型的多数据源的例子。

当我们使用vuex时，如果使用多个store对象，管理和维护会特别困难。**因此建议使用一个store，将所有的属性都放在这个store中管理。即单一数据源。**


#### state
存储数据。
```
const store =new Vuex.Store({
    state: {
        counter: 100
       // 这里写你需要的变量 属性
    }
    })
```
#### getters
如果直接使用state中的数据。可以通过$store.state.counter得到数据。但如果需要拿到这个数据的平方，或者需要对这个数据处理后的结果（数据筛选、求平均等等），这个处理过程或处理函数可以写在getters里。当然如果使用次数很少，也可以在组件中的computed里写上处理过程。
```
const store =new Vuex.Store({
    state: {
        counter: 100
       // 这里写你需要的变量 属性
    }，
    getters: {
    powercounter(state) { 
    
 //此外也可以写成['powercounter']（state） {} 中括号中的字符串 //也可以用const定义后用import导入，这样可以多次使用防止写错
   
   return state.counter * state.counter  //计算平方并返回
        }
    }
    })
```
然后通过$store.getters.powercounter可以调用这个平方后的结果。

如果想要在getters中使用带参数的函数，写法如下。

```
const store =new Vuex.Store({
    state: {
        counter: 100
       // 这里写你需要的变量 属性
    }，
    getters: {
   func(state) {
        return function (data) {
               return state.counter+data 
            }  
        } //在getters用含参数的函数
    }
    })
```

#### mutation和使用规范
vuex中有一些官方推荐的使用规范。即

![Image](C:\Users\张恩铭\Desktop\Image.png)

即我们并不应该直接利用vue组件和state进行双向绑定，虽然这样也是可以实现的。当上述store中的state里的属性发生变化时，例子中的组件中打印的<h2>内容也会随之改变。相反在组件中对$store.state.name进行任何改变后，store中的state的name也会改变。

**之所以不推荐这种方法，是因为在实际应用中，有许多组件会共享state中的属性，当程序出现bug时，我们很难知道是谁在修改这个属性。因为大家都是与其双向绑定，任何一方修改了这个值，大家的值也都随之改变了。而如果通过如图所示的actions和mutations修改state，则可以通过devtools这个浏览器插件检测到每一次state的具体修改情况，是谁在修改一目了然，方便查错和测试。**

1.action只用于异步操作，如网络请求。
2.mutation只能用于同步操作


**错误使用方法**
直接在.vue文件中使用methods修改$store的属性是不合适的。如下
```
<button @click="$store.state.counter++"></button>
```
**正确使用方法**
```
const store =new Vuex.Store({
   state: {
        counter:1000
        },
   mutations: { 
   increment(state) {
      state.couter++
      },
   decrement(state)  {
       state.counter--
       }
    }, // 将state的函数写在mutation中，而不是直接在methods中修改。
   
   // 然后在组件中使用methods调用$store.commit('mutation中的函数名')
   
   methods: {
   addition {
       this.$store.commit('increment')
       }
     }  
    // 然后在组件中使用addition函数即可
```


#### actions
  如果在mutation中使用异步操作，devtool没法检测到数据的改变，在测试和调试时会出现问题。因此不建议在mutation中是用异步操作，而使用action进行。

```
actions: {
    fun_actions(context) {
       context.commit
    }
}
```
```
this.$store.dispatch('fun_actions')
```

#### modules

由于vue使用单一状态树，因此当store对象内容十分庞大的时候，管理起来会相当麻烦。因此引入module，允许我们将store分割成不同的模块，每个模块都有自己的state、mutations、actions、getters等属性。



使用方法：
```
const moduleA = {
    state: {eg=1},
    mutations: { 
        fun(state) {
            state.a.eg++
      }
    },
    
   
    //action使用时要用context.commit 只会访问到本module
    actions: {
        fun_action(context) {
            context.commit('fun') 
    },
    
    //getters使用的时候没有区别注意一下rootState
    getters: {
        fun_getter(state) {
            return 1
        }
        
        // 当需要使用根属性时
        fun_getter_root(state,getters,rootState) 
        // 注意这个rootState可以帮助取module的根属性
        { return getters.fun_getter+rootState.eg}
    }
  }
  
  
const store = new Vuex.Store({ 
    storesL{},
    ………………
    modules: {
    a: moduleA
    }
 })

```
在调用时：
```
$store.state.a.eg  //调用state中属性有区别 并不是store.a.state.eg注意

$store.state.gettter.fun_getter //调用getter时没有区别，直接调用


…………


methods: {
   fun2() {
       this.$store.state.a.commit('fun') //commit时没有区别，但同    //时注意modules中的mutations函数不能与外部的mutations重名
       
      this.$store.dispatch(''fun_action")//调用action时没有区别，  //直接调用,注意重名即可
   }
}
```


#### 养成规范目录结构的习惯

把所有的代码写在store目录下的index下是不明智的。
官网推荐的目录结构为
![image-20210112172630310](C:\Users\张恩铭\AppData\Roaming\Typora\typora-user-images\image-20210112172630310.png)
不同的modules放在文件夹下，而state写在index中。而actions、getters、mutations、都谢在外面export default进来。

在index中
```
import mutations from ...
import actions form ...
import getters from ...
import moduleA from ...

const state ={...} // state定义在index中就好

const store = new Vuex.Store ({
  state,
  mutations,
  actions,
  getters,
  
  modules:{
  a: moduleA
  ...
  }
```
这样在写大型项目时会方便管理。
