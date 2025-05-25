# vue3 入門知識整理

## 一、toRef 和 toRefs

用`reactive`定義的對象，解構賦值，使用解構后的變量，使用它，沒有更新。因爲會丟失響應式。
而用 `toRefs` 包裹一下，再解構，結構后的數據也是響應式的，更新它，值也會發生變化。
`toRef` 是单独拿一个响应式对象中的一个 `key`，将之解构仍然具备响应式的特点。

## 二、計算屬性

- Computed：是一個函數 。具有緩存機制，計算屬性依賴的值沒有變化的時候，計算結果也不會重新計算。區別於函數方法，方法不具有緩存性質。
- 計算結果也具有響應式 computedRefImp 
尽量让模板的里面不要写复杂的代码/逻辑

```vue
const readonlyVal = computed(() => res.value + '111') // 只讀的計算屬性
const compVal = computed({
    get() {
        // 獲取計算的結果
    }
    set(val) {
        // 修改計算的結果值
    }
})
```

## 三、v-bind 和 v-model 
- `v-bind` 是單向數據流，數據流向頁面；頁面 `onChange` 數據不會修改我的變量； 
- `v-model` 是雙向數據綁定； 

## 四、watch 監聽數據的變化
- 是一個函數；watch 監聽數據的變化，回調函數 (oldVal, newVal) => {} 
- 作用：監聽數據的變化；（和 vue2 中的 watch 一樣）
- 特點：vue3 中的 watch 只能監聽以下四種數：
  
  1. ref 定義的數據
    ```js
    const sum = ref(0)
    const stopWatch = watch(sum, 
        (newVal, oldVal) => {
            // 当到了一定情况，可以停止监视这个数据
        }
    )
    const sum = ref({ name:'asdas',age:22 })
    // 监听是的sum对象的地址值，若想监听内部属性的变化，需要开启深度监听模式。 {deep: true}
    // immediate: true 页面加载完成就会执行一次。 oldVal 是 undefined。首次执行的时候
    const stopWatch = watch(sum, 
        (newVal, oldVal) => {
            // 当到了一定情况，可以停止监视这个数据 stopWatch()
        }, 
        { deep: true }
    )
    ```

  2. reactive 定義的數據
    - 在单独修改对象中的某个属性的值的时候，oldVal 和 newVal 都是一样的值，因为引用地址没有被替换。
    - 整个对象的修改，他们才有新旧不一致的情况，因为不是同一个对象了。
    - reactive 定义的类型不可以整理修改数据。只能通过 Object.assign 来修改。
    - watch 监听的reactive 的数据是默认开启深度监听的。
    - 隐式创建深度监听，且不能关闭。
  
  3. 監聽對象中的某個屬性
    -  使用 getter 函数监听对象中的某个复杂数据类型时。可以直接写，也可以写成函数式。
    -  属性为基本数据类型时候，需要写成函数式；才能有效监听其变化。
  4. 函數的一個返回值（ getter 函数）
  
  5. 一个包含上述内容的数组：可以同时监听多个数据。
    ```vue
    watch([a, b], ()=>{})
    ```


## 五、watchEffect
-   会在加载的时候**立即执行一次**。同时会自动追踪依赖，会根据计算的依赖项变化时候，重新执行函数、

## 六、ref 属性
- 操作 DOM 元素；
- 避免 直接操作dom元素，而且可以避免id命名重复的问题等；
- 用于存储 ref 标记的内容 会有隔离。
- 可以获取到组件实例。可以通过它获取组件实例，同时执行他们暴露（defineExpose）的组件的方法和状态。
 
## 七、props
- 子组件接受 通过 defineProps 来接收属性。
```vue
defineProps(['a'])

// a 可以直接使用在模版字符串中

想在JS中使用 需要用变量接收
const props = defineProps(['a','vvv'])
props ==> {a: xx, vvv: ssss}

// 接受+类型限制  限制必要性  指定默认值（通过 withDefaults ）
const props = defineProps<{
	list: Array<T>
}>()
这样来使用。
// 指定默认值
withDefaults(defineProps<{ list: Array<T> }>(), {
	list: () => [{name: '默认名称', age: 1999 }]
})

// defineXXX 在 vue3 中属于宏函数，不用引入
```


## 八、生命周期
生命周期、生命周期函数、生命周期钩子

### vue3的生命周期
- 创建 created 创建组件实例 在setup中执行
- 挂载 mounted onBeforeMount onMounted
- 更新 updated onBeforeUpdate onUpdated 
- 销毁 unmounted onBeforeUnmount onUnmounted


**父子挂载顺序：子先挂载完成、再是父组件 所以APP组件最后挂载完成**

### vue2 的生命周期
vue create vue2_Test 脚手架 创建vue2 课程

- 创建（前beforeCreate 后 created 创建组件实例
- 挂载（前后 mounted
- 更新（前后 updated
- 销毁（前后 destory

## 九、自定义hooks
```
符合封装 hooks 的思路
让一个数据和修改该数据的方法 封装起来， 模块化开发 mixin
```

## 十、路由
- 在 vue 項目中通常路由使用 vue-router 包來實現。
- 路由通常分爲導航區和展示區；
- 路由器管理路徑和頁面的關係；
- 指定一個路由規則，形成一個個的 .vue 文件；
    ```js
    // 創建路由器，管理路由配置對象
    createRouter({
        // 路由器的路由模式
        history: createWebHistory(),
        routes: [
            {
                path: '',
                component
            }
        ]
    })

    // 路由组件占位
    <router-view></router-view> 

    // 匹配的组件在哪里展示。
    <router-link></router-link> 
    // 切换路由 再点击的时候，避免手动修改页面路由切换
    to='/path' 跳转路径 :to={path: '/xxx'} （两种写法）
    active-class 激活的样式
    ```



#### 路由组件和一般组件
- 路由组件：靠配置的路由规则渲染出来的组件 { component: Persion, path: '/persion' }
- 一般组件：由标签写出来的 <Demo /> 来渲染生成的。通常封装再components 文件中

**当路由切换的时候，之前的路由组件会被卸载了。需要的时候再重新挂载；**

### 9.1、路由器的工作模式

1、**history 模式**：没 # 号。更接近传统网站。更美观。但是后期项目上线，需要服务器配合处理路径问题，否则刷新会404；

```js
nignx中配置 location 中的 try_files 配置

vue2 中：mode: ‘history’
vue3 中：createWebHistory()
react 中：BrowserRouter()
```

**2、hash 模式**：兼容性更好 有# 号 不用后端处理 SEO比较差

```js
vue3 中：createWebHashHistory()
命名路由
{ name: '路由名' , path:'' ,component: ''}
根据路由名称跳转： :to = {name: '路由名'}
嵌套路由
children: [
    { path: 'child1', component: Child1 } // path不需要 / 开头
]
// 对应位置：RouterView 插入组件
```

#### 路由传参
两种传参：
**1、query**
```js
?xxx=xxx&yyy=yyy
const route = useRoute()
route.query  获取到{xxx:xxx,yyy:yyy}

:to={ path: '', query: { id：id, uuu: uuu } }
route.query  获取到{xxx:xxx,yyy:yyy}
使用的时候不要直接解构route, 会丢失响应式
除非解构的时候，将解构的对象包裹一层toRef()
const { query } = toRefs(route)
```

**2、params 参数**
```js
// path/aaa/bbb/ccc
路由配置的时候：参数需要占位
{ name: 'zzz', path: 'path/:aaa/:bbb/:ccc?' } 
ccc 表示参数可选。可传可以不传递。

// to="path/aaa/bbb/ccc"
:to={ name: 'zzz', params: { aaa: xxx,bbb: bbb，ccc: ccc } } 
注意：params 中的参数不能为对象和数组类型；

在写params 传递的时候，对象参数中要以name来定位路由配置，而不是path
route.params   获取到{aaa: xxx,bbb: bbb，ccc: ccc}

// 路由的props配置 
{	
	...
    props: true // 将路由收到的pramas参数作为props传递给组件

    // 第二种写法
    props: (route) => {
        return {
            // 参数 传递指定参数。
            route.query || route.params
        }
    }
    props: {
        a: 'aaa' // 写死了传递指定的参数值。
    }
}

组件中接收：const { } = defineProps(['', ''])
```


### 操作路由的历史记录
- replace  替换
- push 推进栈

默认是push, 再 RouterLink 上 加上 replace  改成此模式 

### 编程式导航
```json
脱离 RouterLink 实现路由导航
useRouter(string | object)  跳转... 路由
类似RouterLink中的to的属性使用
重定向
{
    path: '/',
    redirect: '/home' // 重定向到其他路径
}

```
## 十、pinia
- 符合直觉的vuejs状态管理工具     （集中式状态管理）
- 组件的共享数据进行集中式状态管理，而不是组件自身的数据。
- 工厂函数模式  设计思想

```
v-model.number 尽可能使之input输入的值变成number
:value 也可以转

再reactive中定义了ref类型的数据，在使用的时候不要再.value 获取那个ref的数据，因为会自动解包。

```

### storeToRefs

- state	 修改数据封装成函数
- actions
- getters  基于 state 二次加工数据
- 再里面使用this 调用state的时候，不可以使用箭头函数...
- XXX.$subscribe 监听数据的变化
- 可利用做数据持久化...
- 组合式、选项式两钟方式实现
- 集中式状态管理

## 十一、组件通信
1、**组件之间的关系**
- 父子 / 子父 `props` 非函数 / 回调函数
```
const car = ref()   :car=car   --> defineProps()
getProps(val){}       send(val)
自定义事件 -- $event 事件对象
什么时候触发    实现子传父
// 绑定事件
<Child @abc="saveAbc">
function saveAbc(val: string) {
	console.log(val) // 收到子组件传递的数据
}

```

在child 组件中借助`defineEmit` 宏函数来定义**自定义事件**
```js
// 申明事件
const emit = defineEmits(['abc'])

// 触发事件
emit('abc', val)

```

**2、mitt 可以实现任意组件的通信。**
- 消息订阅发布
- .pubsub
- .$bus
- .mitt   工具需要安装   on  off  
- 接收数据的：提前绑定好事件（提前订阅好消息）
- 提供数据的：在合适的时候触发事件（发布事件）

```vue

import mitt from 'mitt'

const mitter = mitt()
export const mitter

在组件解绑的时候，尽量解绑事件。【对内存不友好】

v-model （: value + @input 事件）
UI 组件库 底层组件 大量使用 v-model 进行通信 
用于html标签上
(<HMTLInputElement>$event.target).value  处理事件错误类型
用在组件标签上
在二次封装的组件中使用。等价于需要做：
v-model 等价于：

@modelValue  @update:modelValue=“”

```

**在封装的子组件中接收定义：**
```js
defineProps(['modelValue'])
const emit  =  defineEmits(['update:modelValue'])

<input :value="modelValue" @input="emit('update:modelValue', $event.target.value)"  />
$event 到底时啥？什么时候能够.target？
对于原生事件，$event 就是事件对象。能点
对于自定义事件，$event 就是触发事件的时候，所传递的值。不能dian .
```

**value 是可以修改的，如果 value 更换了，就可以在组件标签上面多次使用 v-model** 

```vue
<MyInput v-model:password="password" v-model:username="username" />
```

#### 常见的组件通信方式
- $attr
- $refs --- $paren
- project inject
- pinia
- slot

##### $attrs：用于实现当前组件的父组件，向当前组件的子组件通信
祖 --> 孙

子组件没有接受的属性props，会全存在 $attrs 中。  可以统一将它传递给自己的后代组件
```js
v-bind="$attrs"
$parrent 和 $refs
ref + defineExpose({ 暴露的属性 })
获取所有的子组件实例
$refs
获取父级组件
$parent  + defineExpose({ ... })

```
##### provide 和 inject: 祖孙之间直接通信
##### ref 响应式数据和函数...
##### slot
- 默认插槽  
  ```
  <slot></slot>  占位符   一个位置一个坑  name="default"
  ```
- 具名插槽  
  ```
  <slot name=""></slot>
  ```
- 作用域插槽
  ```
    v-slot ==> # 
        <template v-slot="params">{{ params }}</template>
    <slot chuanzhi="abxde"></slot>
  ```


**值的作用域在哪儿。数据在子组件那边，根据数据生成的结构，但是由父亲决定。**

## 十二、其他API

1. shallowRef 处理第一层的响应式
2. shallowReactive 浅层次的响应式. 内部属性的第一层...
   主要是为了绕开深度响应。浅层次的API创建状态只在其顶层是响应式的，对所有深层的对象不会做任何处理。避免对每一个内部属性做响应式所带来的性能成本。这使得属性的访问变得更快，可提升性能。
3. readonly(响应式数据)
   不能修改 只能读取相关的数据， 相当于拷贝一份使用。（深只读副本）
4. shollowReadonly 浅层层次的只读
5. toRaw     用于获取一个响应式对象的原始数据。返回一个原始的数据
6. markRaw    标记一个原始对象，使其永远不能变为一个响应式数据，
   应用于 mockjs 时候，为了防止把第三方库引入的对象变为 响应式对象，可以用它标记。
7. **customRef**     创建要给自定义的 ref 并且对其依赖项跟踪和更新触发进行逻辑控制。

   ```js
    trick(跟踪)  trigger(触发)
    let initialVal = ''
    let msg = customRef((trick, trigger)=>{
    return {
            set(newVal){
                initialVal = newVal
                trigger() // 通知 vue msg 变化了
            },
            get(){
                trick() // 跟踪数据的变化
                return initiaVal
            }
        }
    })
   
   ```
    实际应用的时候，会将它封装成一个 hooks 使用



## 十三、teleport

- 做弹窗组件实现
- 色彩饱和度 css 实现

    ```js
    filter: saturate(0%)     会让子组件的fixed定义参考对象为父组件。。。
    <teleport to="定位到哪个元素，， body"> </teleport>
    ```



## 十四、suspense

- **异步加载**：在子组件中存在异步任务获取数据，同时需要使用这个数据的时候。可以借助它做一些优化提示。
- `#fallback` 和 `#default` 插槽实现
- 全局API 转移到应用对象上
  - app.component
  - app.config
  - app.directive
  - app.mount
  - app.unmount
  - app.use


## 十五、非兼容性改变
## 十六、vue3 与 vue2 有什么区别。

在官网可看。