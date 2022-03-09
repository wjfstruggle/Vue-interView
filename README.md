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

