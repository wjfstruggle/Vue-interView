## Vue-interView
> `Vue`进阶面试题，源码解读，含`Vue3`源码解读

[源码地址](https://github.com/wjfstruggle/Vue-interView)

欢迎大家来个star，笔芯。

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/dba2720874bd49e6b1183a12ad0b9bfb~tplv-k3u1fbpfcp-watermark.image?)

又到了每年的金三银四，说实话，今年行情有点不太好，我看到很多小伙伴投简历，大部分都没有下文。大厂裁员，公司腾不出职位，考虑给今年应届生职位等等。但是只要有能力，基础打好，放平心态才能找到更好的工作。

话不多说，我们直接进入`Vue`面试专题。

面试主要分了五大专题。分别是`API`专题、原理专题、`Vue3`专题、`Vuex`专题、综合应用专题。

接下来一一解答。

## 目录

### API专题

1. 组件之间通信⽅式有哪些
2. v-if和v-for哪个优先级更⾼？  
3. 简述 `Vue` 的⽣命周期以及每个阶段做的事  
4. 能说⼀说双向绑定使⽤和原理吗？  
5. ⼦组件可以直接改变⽗组件的数据么，说明原因

### 原理专题

1. 说说你对虚拟 DOM 的理解？  
2. 说⼀说你对`vue`响应式理解？  
3. 你了解`diff`算法吗？  
4. 能说说key的作⽤吗？ 
5. 说说从 template 到 render 处理过程  
6. `Vue`实例挂载的过程中发⽣了什么?  
7. 说说`nextTick`的使⽤和原理？  

### Vue3专题

1. 你知道哪些`vue3`新特性  

### Vuex专题

1. 简单说⼀说你对`vuex`理解？  

### 综合应用

1. 从0到1⾃⼰构架⼀个`vue`项⽬，说说有哪些步骤、哪些重要插件、⽬录结构你会怎么组织  
2. 实际⼯作中，你总结的`vue`最佳实践有哪些？  
3. `Vue`要做权限管理该怎么做？控制到按钮级别的权限怎么做？  
4. `Vue`中如何扩展⼀个组件  
5. 怎么定义动态路由？怎么获取传过来的动态参数？  
6. 如果让你从零开始写⼀个`vue`路由，说说你的思路  
7. watch和computed的区别以及选择?  
8. 说⼀下 `Vue` ⼦组件和⽗组件创建和挂载顺序  
9. 怎么缓存当前的组件？缓存后怎么更新？  

#### 1、`Vue`组件之间通信⽅式有哪些

`vue`是组件化开发框架，所以对于`vue`应⽤来说组件间的数据通信⾮常重要。此题主要考查⼤家`vue`基本功，对于`vue`基础`api`运⽤熟练度。另外⼀些边界知识如`provide/inject/$attrs`则提现了⾯试者的知识⼴度。  

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d2926a2d000e40a495c9d135d431be85~tplv-k3u1fbpfcp-watermark.image?)

##### 思路分析：  

1. 总述知道的所有⽅式  
2. 按组件关系阐述使⽤场景  

##### 回答范例：  

1. 组件通信常⽤⽅式有以下8种：  

- `props`
- `$emit` / ~~$on~~
- ~~$children~~/$`$parent  `
- `$attrs`/~~$listeners~~  
- ref
- `$root  
- `eventbus`  
- `vuex  `

2. 根据组件之间关系讨论组件通信最为清晰有效  

- 父子组件
  - `props/$emit/$parent/ref/$atters`
- 兄弟组件
  - `$parent / $root / eventbus / vuex  `
- 跨层级关系
  - `eventbus / vuex / provide + inject  `

三种不常用的组件通行。

[$children用法](https://v3-migration.vuejs.org/breaking-changes/children.html)

[`$listeners`用法](https://v3-migration.vuejs.org/breaking-changes/listeners-removed.html)

[Events API用法](https://v3-migration.vuejs.org/breaking-changes/events-api.html)

> 注意vue3中废弃的⼏个API  

#### 2、v-if和v-for哪个优先级更高？  

##### 分析：  

此题考查常识，⽂档中曾有详细说明[v2](https://cn.vuejs.org/v2/guide/conditional.html#v-if-%E4%B8%8E-v-for-%E4%B8%80%E8%B5%B7%E4%BD%BF%E7%94%A8)|[v3](https://v3.cn.vuejs.org/guide/list.html#v-for-%E4%B8%8E-v-if-%E4%B8%80%E5%90%8C%E4%BD%BF%E7%94%A8)；也是⼀个很好的实践题⽬，项⽬中经常会遇到，能够看出⾯试者api熟悉程度和应⽤能⼒。  

##### 思路分析：  

1. 先给出结论
2. 为什么是这样的，说出细节
3. 哪些场景可能导致我们这样做，该怎么处理
4. 总结，拔高

##### 回答范例

1. 实践中不应该把`v-for`和`v-if`放⼀起  
2. 在`vue2`中， `v-for`的优先级是⾼于`v-if`，把它们放在⼀起，输出的渲染函数中可以看出会先执⾏循环再判断条件，哪怕我们只渲染列表中⼀⼩部分元素，也得在每次重渲染的时候遍历整个列表，这会⽐较浪费；另外需要注意的是在`vue3`中则完全相反， v-if的优先级⾼于`v-for`，所以`v-if`执⾏时，它调⽤的变量还不存在，就会导致异常报错。
3. 通常有两种情况下导致我们这样做：  

- 为了过滤列表中的项⽬,比如（`<ul v-for="item in userList" v-if="item.isActive" :key="item"></ul>`）,此时定义⼀个计算属性 (⽐如 `activeUsers` )，让其返回过滤后的列表即可（⽐如
  `users.filter(u=>u.isActive`) ）  或者接口拿到数据的时候把不需要展示在列表中的属性过滤掉。
- 为了避免渲染本应该被隐藏的列表 (⽐如`<ul v-for="item in userList" v-if="item.isActive" :key="item"></ul>`)。此时把 `v-if` 移动⾄容器元素上 (⽐如` ul 、 ol `)或者外⾯包⼀层` template` 即可。  

4. ⽂档中明确指出永远不要把 `v-if` 和` v-for `同时⽤在同⼀个元素上，显然这是⼀个重要的注意事项。  
5. 源码⾥⾯关于代码⽣成的部分，能够清晰的看到是先处理v-if还是v-for，顺序上vue2和vue3正好相反，因此产⽣了⼀些症状的不同，但是不管怎样都是不能把它们写在⼀起的。  

测试：

```js
//v-if和v-for哪个优先级更高？V2.html
<ul v-for="item in userList" v-if="item.isActive" :key="item">
    <li>{{item.name}}</li>
</ul>
  <script>
   const app = new Vue({
      el:"#app",
      data:{
        userList:[
          {
            name:'张三',
            isActive:true
          },{
            name:'李四',
            isActive:true
          },{
            name:'王五',
            isActive:false
          },{
            name:'刘六',
            isActive:true
          },{
            name:'傻⑦',
            isActive:false
          }
        ]
      },
    })
    console.log(app.$options.render);
  </script>
```

两者同级时，渲染函数如下

```js
(function anonymous(
) {
with(this){return _c('div',{attrs:{"id":"app"}},_l((userList),function(item){return (item.isActive)?_c('ul',{key:item},[_c('li',[_v(_s(item.name))])]):_e()}),0)}
})
```

Vue3 直接报错

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bd2a4b628fdf43219e6da6b44a846737~tplv-k3u1fbpfcp-watermark.image?)

##### 源码中找答案  

v2： https://github1s.com/vuejs/vue/blob/HEAD/src/compiler/codegen/index.js#L65-L66
v3： https://github1s.com/vuejs/core/blob/HEAD/packages/compiler-core/src/codegen.ts#L586-L587  

#### 3、简述 Vue 的⽣命周期以及每个阶段做的事  

必问题⽬，考查`vue`基础知识。  

##### 思路 ：

1. 给出概念
2. 列举生命周期各个阶段
3. 阐述整体流程
4. 结合实践
5. 扩展：`Vue3`变化

##### 回答范例 :

1. 每个`Vue`组件实例被创建后都会经过⼀系列初始化步骤，⽐如，它需要数据观测，模板编译，挂载实例到`dom`上，以及数据变化时更新`dom`。这个过程中会运⾏叫做⽣命周期钩⼦的函数，以便⽤户在特定阶段有机会添加他们⾃⼰的代码。  
2. Vue⽣命周期总共可以分为8个阶段： 创建前后, 载⼊前后, 更新前后, 销毁前后，以及⼀些特殊场景的⽣命周期。vue3中新增了三个⽤于调试和服务端渲染场景。  

| ⽣命周期v2    | ⽣命周期v3      | 描述                       |
| ------------- | --------------- | -------------------------- |
| beforeCreate  | beforeCreate    | 组件实例被创建之初         |
| created       | created         | 组件实例已经完全创建       |
| beforeMount   | beforeMount     | 组件挂载之前               |
| mounted       | mounted         | 组件挂载到实例上去之后     |
| beforeUpdate  | beforeUpdate    | 组件数据发⽣变化，更新之前 |
| updated       | updated         | 数据数据更新之后           |
| beforeDestroy | beforeUnmounted | 组件实例销毁之前           |
| destroyed     | unmounted       | 组件实例销毁之后           |



| ⽣命周期v2    | ⽣命周期v3      | 描述                                     |
| ------------- | --------------- | ---------------------------------------- |
| activated     | activated       | keep-alive 缓存的组件激活时              |
| deactivated   | deactivated     | keep-alive 缓存的组件停⽤时调⽤          |
| errorCaptured | errorCaptured   | 捕获⼀个来⾃⼦孙组件的错误时被调⽤       |
| -             | renderTracked   | 调试钩⼦，响应式依赖被收集时调⽤         |
| -             | renderTriggered | 调试钩⼦，响应式依赖被触发时调⽤         |
| -             | serverPrefetch  | ssr only，组件实例在服务器上被渲染前调⽤ |

![生命周期图](https://v3.cn.vuejs.org/images/lifecycle.svg)

4. 结合实践

`beforeCreate`：通常⽤于插件开发中执⾏⼀些初始化任务
`created`：组件初始化完毕，可以访问各种数据，获取接⼝数据等
`mounted`： dom已创建，可⽤于获取访问数据和dom元素；访问⼦组件等。
`beforeUpdate`：此时 view 层还未更新，可⽤于获取更新前各种状态
`updated`：完成 view 层的更新，更新后，所有状态已是最新
`beforeunmounted`：实例被销毁前调⽤，可⽤于⼀些定时器或订阅的取消
`unmounted`：销毁⼀个实例。可清理它与其它实例的连接，解绑它的全部指令及事件监听器  

##### 知其所以然

`vue3`中⽣命周期的派发时刻：

https://github1s.com/vuejs/core/blob/HEAD/packages/runtime-core/src/componentOptions.ts#L554-L555

`vue2`中声明周期的派发时刻：

https://github1s.com/vuejs/vue/blob/HEAD/src/core/instance/init.js#L55-L56  

#### 4、能说⼀说双向绑定使⽤和原理吗？  

##### 题⽬分析：  

双向绑定是 vue 的特⾊之⼀，开发中必然会⽤到的知识点，然⽽此题还问了实现原理，升级为深度考查。  

##### 思路分析：  

1. 给出双绑定义
2. 双绑带来的好处
3. 在哪使⽤双绑
4. 使⽤⽅式、使⽤细节、 vue3变化
5. 原理实现描述  

##### 回答范例：

1. `vue`中双向绑定是⼀个指令` v-model `，可以绑定⼀个响应式数据到视图，同时视图中变化能改变该值。
2. ` v-model `是语法糖，默认情况下相当于 `:value 和 @input` 。使⽤` v-model `可以减少⼤量繁琐的事件处理代
   码，提⾼开发效率。
3.  通常在表单项上使⽤ v-model ，还可以在⾃定义组件上使⽤，表示某个值的输⼊和输出控制。
4.  通过 `<input v-model="xxx">` 的⽅式将xxx的值绑定到表单元素value上；对于checkbox，可以使⽤` truevalue` 和`false-value`指定特殊的值，对于radio可以使⽤value指定特殊的值；对于select可以通过options元素的value设置特殊的值；还可以结合`.lazy,.number,.trim对v-mode`的⾏为做进⼀步限定； v-model ⽤在⾃定义组件上时⼜会有很⼤不同， vue3中它类似于 sync 修饰符，最终展开的结果是modelValue属性和update:modelValue事件； vue3中我们甚⾄可以⽤参数形式指定多个不同的绑定，例如v-model:foo和vmodel:bar，⾮常强⼤！
5. `v-model` 是⼀个指令，它的神奇魔法实际上是vue的编译器完成的。我做过测试，包含 v-model 的模板，转换为渲染函数之后，实际上还是是value属性的绑定以及input事件监听，事件回调函数中会做相应变量更新操作。编译器根据表单元素的不同会展开不同的DOM属性和事件对，⽐如text类型的input和textarea会展开为value和input事件； checkbox和radio类型的input会展开为checked和change事件； select⽤value作为属性，⽤change作为事件。  

#### 5、说⼀说你对vue响应式理解？  

##### 分析

这是⼀道必问题⽬，但能回答到位的⽐较少。如果只是看看⼀些⽹⽂，通常没什么底⽓，经不住⾯试官推敲，但像
我们这样即看过源码还造过轮⼦的，回答这个问题就会⽐较有底⽓啦。

##### 答题思路：

1. 啥是响应式？
2. 为什么vue需要响应式？
3. 它能给我们带来什么好处？
4. vue的响应式是怎么实现的？有哪些优缺点？
5. vue3中的响应式的新变化  

##### 回答范例：

1. 所谓数据响应式就是能够使数据变化可以被检测并对这种变化做出响应的机制。
2. MVVM框架中要解决的⼀个核⼼问题是连接数据层和视图层，通过数据驱动应⽤，数据变化，视图更新，要做
   到这点的就需要对数据做响应式处理，这样⼀旦数据发⽣变化就可以⽴即做出更新处理。
3. 以vue为例说明，通过数据响应式加上虚拟DOM和patch算法，开发⼈员只需要操作数据，关⼼业务，完全不
   ⽤接触繁琐的DOM操作，从⽽⼤⼤提升开发效率，降低开发难度。
4. vue2中的数据响应式会根据数据类型来做不同处理，如果是对象则采⽤Object.defineProperty()的⽅式定
   义数据拦截，当数据被访问或发⽣变化时，我们感知并作出响应；如果是数组则通过覆盖数组对象原型的7个
   变更⽅法，使这些⽅法可以额外的做更新通知，从⽽作出响应。这种机制很好的解决了数据响应化的问题，但
   在实际使⽤中也存在⼀些缺点：⽐如初始化时的递归遍历会造成性能损失；新增或删除属性时需要⽤户使⽤
   Vue.set/delete这样特殊的api才能⽣效；对于es6中新产⽣的Map、 Set这些数据结构不⽀持等问题。
5. 为了解决这些问题， vue3重新编写了这⼀部分的实现：利⽤ES6的Proxy代理要响应化的数据，它有很多好
   处，编程体验是⼀致的，不需要使⽤特殊api，初始化性能和内存消耗都得到了⼤幅改善；另外由于响应化的
   实现代码抽取为独⽴的reactivity包，使得我们可以更灵活的使⽤它，第三⽅的扩展开发起来更加灵活了。  

##### 知其所以然

vue2响应式：
https://github1s.com/vuejs/vue/blob/HEAD/src/core/observer/index.js#L135-L136
vue3响应式：
https://github1s.com/vuejs/core/blob/HEAD/packages/reactivity/src/reactive.ts#L89-L90
https://github1s.com/vuejs/core/blob/HEAD/packages/reactivity/src/ref.ts#L67-L68  

#### 6、Vue中如何扩展⼀个组件  

此题属于实践题，考察⼤家对vue常⽤api使⽤熟练度，答题时不仅要列出这些解决⽅案，同时最好说出他们异同。

##### 答题思路：

1. 按照逻辑扩展和内容扩展来列举  
   1. 逻辑扩展有： mixins、 extends、 composition api；  
   2. 内容扩展有slots；  

2. 分别说出他们使⽤⽅法、场景差异和问题。  

3. 作为扩展，还可以说说vue3中新引⼊的composition api带来的变化  

##### 回答范例：

1. 常⻅的组件扩展⽅法有： mixins， slots， extends等
2. 混⼊mixins是分发 Vue 组件中可复⽤功能的⾮常灵活的⽅式。混⼊对象可以包含任意组件选项。当组件使⽤
   混⼊对象时，所有混⼊对象的选项将被混⼊该组件本身的选项。

```js
// 复⽤代码：它是⼀个配置对象，选项和组件⾥⾯⼀样
const mymixin = {
methods: {
dosomething(){}
}
}
// 全局混⼊：将混⼊对象传⼊
Vue.mixin(mymixin)
// 局部混⼊：做数组项设置到mixins选项，仅作⽤于当前组件
const Comp = {
mixins: [mymixin]
}
```

3. 插槽主要⽤于vue组件中的内容分发，也可以⽤于组件扩展。

⼦组件Child  

```html
<div>
    <slot>这个内容会被⽗组件传递的内容替换</slot>
</div>
```

⽗组件Parent  

```html
<div>
	<Child>来⾃⽼爹的内容</Child>
</div>
```

4. 混⼊的数据和⽅法不能明确判断来源且可能和当前组件内变量产⽣命名冲突， vue3中引⼊的composition
   api，可以很好解决这些问题，利⽤独⽴出来的响应式模块可以很⽅便的编写独⽴逻辑并提供响应式的数据，
   然后在setup选项中组合使⽤，增强代码的可读性和维护性。例如：  

```js
// 复⽤逻辑1
function useXX() {}
// 复⽤逻辑2
function useYY() {}
// 逻辑组合
const Comp = {
setup() {
const {xx} = useXX()
const {yy} = useYY()
return {xx, yy}
}
}
```

##### slots原理：

https://github1s.com/vuejs/core/blob/HEAD/packages/runtime-core/src/componentSlots.ts#L129-L130
https://github1s.com/vuejs/core/blob/HEAD/packages/runtime-core/src/renderer.ts#L1373-L1374
https://github1s.com/vuejs/core/blob/HEAD/packages/runtime-core/src/helpers/renderSlot.ts#L23-L24  

#### 7、说说你对虚拟 DOM 的理解？  

##### 分析  :

现有框架⼏乎都引⼊了虚拟 `DOM `来对真实 `DOM` 进⾏抽象，也就是现在⼤家所熟知的 `VNode` 和` VDOM`，那么为
什么需要引⼊虚拟 `DOM` 呢？围绕这个疑问来解答即可！  

##### 思路  :

1. vdom是什么
2. 引⼊vdom的好处
3. vdom如何⽣成，⼜如何成为dom
4. 在后续的diff中的作⽤  

##### 回答范例:

1. 虚拟dom顾名思义就是虚拟的dom对象，它本身就是⼀个 JavaScript 对象，只不过它是通过不同的属性去
   描述⼀个视图结构。

2. 通过引⼊vdom我们可以获得如下好处：
   将真实元素节点抽象成 VNode，有效减少直接操作 dom 次数，从⽽提⾼程序性能

   1. 直接操作 dom 是有限制的，⽐如： diff、 clone 等操作，⼀个真实元素上有许多的内容，如果直接对其
      进⾏ diff 操作，会去额外 diff ⼀些没有必要的内容；同样的，如果需要进⾏ clone 那么需要将其全部内
      容进⾏复制，这也是没必要的。但是，如果将这些操作转移到 JavaScript 对象上，那么就会变得简单
      了。
   2. 操作 dom 是⽐较昂贵的操作，频繁的dom操作容易引起⻚⾯的重绘和回流，但是通过抽象 VNode 进⾏
      中间处理，可以有效减少直接操作dom的次数，从⽽减少⻚⾯重绘和回流。

   ⽅便实现跨平台

   	1. 同⼀ VNode 节点可以渲染成不同平台上的对应的内容，⽐如：渲染在浏览器是 dom 元素节点，渲染在
   Native( iOS、 Android) 变为对应的控件、可以实现 SSR 、渲染到 WebGL 中等等
   	2.Vue3 中允许开发者基于 VNode 实现⾃定义渲染器（renderer），以便于针对不同平台进⾏渲染。

3. vdom如何⽣成？在vue中我们常常会为组件编写模板 - template， 这个模板会被编译器 - compiler编译为渲
   染函数，在接下来的挂载（mount）过程中会调⽤render函数，返回的对象就是虚拟dom。但它们还不是真
   正的dom，所以会在后续的patch过程中进⼀步转化为dom。  

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a990cd21790445e08c0dad93fa9e4d71~tplv-k3u1fbpfcp-watermark.image?)

4. 挂载过程结束后， vue程序进⼊更新流程。如果某些响应式数据发⽣变化，将会引起组件重新render，此时就
   会⽣成新的vdom，和上⼀次的渲染结果diff就能得到变化的地⽅，从⽽转换为最⼩量的dom操作，⾼效更新
   视图。  

##### 知其所以然

vnode定义：
https://github1s.com/vuejs/core/blob/HEAD/packages/runtime-core/src/vnode.ts#L127-L128

创建vnode：
createElementBlock:
https://github1s.com/vuejs/core/blob/HEAD/packages/runtime-core/src/vnode.ts#L291-L292

createVnode:
https://github1s.com/vuejs/core/blob/HEAD/packages/runtime-core/src/vnode.ts#L486-L487

⾸次调⽤时刻：
https://github1s.com/vuejs/core/blob/HEAD/packages/runtime-core/src/apiCreateApp.ts#L283-L284

mount:
https://github1s.com/vuejs/core/blob/HEAD/packages/runtime-core/src/renderer.ts#L1171-L1172  