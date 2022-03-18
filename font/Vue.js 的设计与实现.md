# Vue.js 的设计与实现

## 1. 框架设计概览

## 1.1 权衡的艺术

- 声明式代码的性能不优于命令式代码的性能，因为声明式代码会比命令式代码多出找出差异的性能消耗，框架本身就是封装了命令式代码才实现了面向用户的声明式。那为什么 Vue.js 还要选择声明式的设计方案呢？因为声明式代码的可维护性更强，命令式代码需要维护目标的整个过程，包括手动完成 DOM 元素的创建、更新和删除等工作。而声明式代码展示的就是我们要的结果，看上去更直观，并且做的过程让 Vue.js 封装好了。Vue.js 做的就是在保持可维护性的同时让性能损失最小化
- 虚拟 DOM 能够让我们在写声明式代码的情况下，保证我们应用程序的下限。虚拟 DOM 创建页面的过程分为两步：第一步是创建 JavaScript 对象，对真实 DOM 的描述；第二步是递归的遍历虚拟 DOM 并创建真实的 DOM。
- innerHTML 更新页面是重新构建 HTML 字符串，再重新设置 DOM 元素的 innerHTML 属性（销毁所有旧的 DOM 元素，再全量创建一个新的 DOM 元素），性能因素与模板的大小有关。但是虚拟 DOM 在 JavaScript 层面的运算要比创建页面时多出一个 Diff 算法，但是只更新必要的 DOM，性能因素与数据变化量有关。 innerHTML < 虚拟 DOM < 原生 JavaScript
- Render（运行时） -> Compiler、Render（编译 + 运行时） -> Compiler（编译时），如果是纯运行时，我们没有办法分析用户提供的内容，也就不可以做进一步的优化；如果是纯编译时，可能性能会更好，但是会损坏灵活性，即用户提供的内容必须编译后才能使用；Vue.js 仍然保持了运行时 + 编译时的架构，在保持灵活性的基础上尽可能的优化

## 1.2 框架设计的核心要素

1. 提升用户开发体验：提供友好的警告信息
2. 控制框架代码体积：警告信息再生产环境中不提供
3. 良好的 Tree-Shaking：消除那些永远不会被执行的代码
4. 框架应该输出怎样的产物
5. 特性开关
6. 错误处理：统一的错误处理接口，callWithErrorHandling
7. 良好的 TypeScript 类型支持

## 1.3 Vue.js 3 的设计思路

### 1.3.1. 声明式地描述 UI

前端页面设计的内容：

- DOM 元素：例如这个元素是什么标签
- 属性：标签上面的属性
- 事件：click等
- 元素的层级结构

Vue.js 3 的解决方案有：

- 使用于 HTML 标签一致的方式来描述 DOM 元素
- 使用与 HTML标签一致的方式来描述属性
- 使用：或 v-bind 来描述动态绑定的属性
- 使用 @ 或 v-on 来描述事件
- 使用与 HTML 标签一致的方式来描述层级结构

除了这种模板的方式来描述 UI 之外，还可以使用 JavaScript 对象来描述，这样会更加灵活：

```js
const  title = {
  tag: 'h1',
  props: { onClick: handler},
  children: [{tag: 'span'}]
}
```

而使用 JavaScript 对象来描述 UI 的方式，就是所谓的虚拟 DOM。一个组件要渲染的内容是通过渲染函数来描述的，也就是代码中的 render 函数，Vue.js 会根据组件的 render 函数的返回值拿到虚拟 DOM，然后就可以把组件的内容渲染出来

### 1.3.2. 初识渲染器

渲染器 renderer 的作用就是把虚拟 DOM 渲染成真实 DOM，它的工作原理是递归的遍历虚拟 DOM 对象，并调用原生 DOM API 来完成真实的 DOM 的创建，总体来说实现思路分为三步：

1. 创建元素：把 vnode.tag 作为标签名字来创建 DOM 元素
2. 为元素添加属性和事件：遍历 vnode.props 对象，如果 key 以 on 字符开头，按事件处理，否则按属性处理
3. 处理 children：如果 children 是一个数组，就递归地调用 renderer 继续渲染；如果 children 是字符串，则使用 createTextNode 函数创建一个文本节点

```js
function render(vnode, container) {
  const el = document.createElement(vnode.tag)
  for(let key in vnode.props) {
    if(/^on/.test(key)) {
      el.addEventListener(key.substr(2).toLowerCase(), vnode.props[key])
    }
  }
  if(typeof vnode.children === 'string') {
    el.appendChild(document.createTextNode(vnode.children))
  } else if(Array.isArray(vnode.children)) {
    vnode.children.foreach(child => render(child, el))
  }
  constainer.appendChild(el)
}
```

这里只是做了最简单的创建节点，渲染器的精髓阶段在于更新节点的阶段，它需要精确的找到 vnode 对象的变更点并且值更新变更的内容

### 1.3.3. 组件的本质

组件就是一组 DOM 元素的封装，这组 DOM 元素就是组件要渲染的内容，因此我们可以定义一个函数来代表组件：

```js
const MyComponent = function () {
  return {
    tag: 'div',
    props: {
      onClick: () => alert('hello')
    },
    children: 'click me'
  }
}
```

然后定义虚拟 DOM 来描述组件，我们可以让虚拟 DOM 对象中的 tag 属性来存储组件函数：

```js
const vnode = {
  tag: MyComponent
}
```

现在 tag 属性不是标签名称，而是组件函数，为了能够渲染组件，我们需要渲染器的支持：

```js
function renderer(vnode, container) {
  if(typeof vnode.tag === 'string') {
    mountElement(vnode, container)	// 和前面的 renderer 函数的内容一致
  } else if(typeof vnode.tag === 'function') {
    mountComponent(vnode, container)
  }
}
```

mountComponent 函数的实现：

```js
function mountComponent(vnode, container) {
  // 调用组件函数，获取要渲染的虚拟 DOM
  const substree = vnode.tag()
  render(substree, container)
}
```

组件一定是一个函数吗，当然不是，我们完全可以用一个 JavaScript 对象来表达组件，Vue.js 中有状态组件就是使用对象结构来表达

### 1.3.4 模板的工作原理

模板的核心就是编译器，编译器将模板编译为渲染函数

模板如下：

```vue
<div @click='handler'>
  click me
</div>
```

对于编译器来说，模板就是一个普通的字符串，他会分析该字符串并生成一个功能与之相同的渲染函数 ：

```js
render() {
  return h('div', { onClick: handler}, 'click me')
}
```

并添加到 `<script>` 标签块的组件对象上，添加结果：

```js
export default {
  data() {/* ... */ },
  methods: {
    handler() => {/* ... */ }
  },
  render() {
  return h('div', {onClick: handler}, 'click me')
}
}
```

无论是模板还是直接手写渲染函数，对于一个组件来说，他要渲染的内容最终都是通过渲染函数产生的，然后渲染器再把渲染函数返回的虚拟 DOM 渲染为真实的DOM，这就是模板的工作原理，也是 Vue.js 渲染页面的流程

### 1.3.5.  Vue.js 是各个模块组成的有机整体

组件的实现依赖于渲染器，模板的编译依赖于编译器，并且编译后生成的代码是根据渲染器和虚拟 DOM 的设计决定的，因此 Vue.js 的各个模块之间是互相关联的。

假设模板如下：

```html
<div id='foo' :class='cls'></div>
```

编译器会把它编译成一个渲染函数：

```js
function render() {
  return {
    tag: 'div',
    props: {
      id: 'foo',
      class: 'cls'
    },
  }
}
```

但是这个 cls 变量可能随时发生变化，渲染器的作用就是一直寻找并且只更新变化的内容，与其要花性能寻找到 cls 是变量，还不如编译器刚开始就把这些信息提取出来，直接交给渲染器。Vue.js 的模板是有特点的，可以一眼就看出 ： 和 @ 是动态变化的，所以编译器可以识别哪些是静态的，哪些是动态的，在生成代码的时候完全可以附带这些信息：
```js
function render() {
  return {
    tag: 'div',
    props: {
      id: 'foo',
      class: 'cls'
    },
    patchFlags: 1 // 假设 1 代表 cls 是动态的
  }
}
```

可知，编译器和渲染器是存在交流的，相互配合以提升性能，它们之间交流的媒介就是虚拟 DOM。虚拟 DOM 比模板更加灵活，但是模板比虚拟 DOM 更加直观，其实都是一个信息

## 2.响应系统

## 2.1. 响应系统的作用与实现

### 2.1.1.  响应式数据与副作用函数

这里副作用函数指类似更改 HTML 元素的函数，如果我们数据更改之后，副作用函数自动执行并修改HTML，这样的数据不就是响应式数据了嘛，vue 2在 ES5 时候是通过 Object.defineProperty 函数实现，在现在是通过 Proxy 代理实现的，实现大概思路是：

- 当副作用函数 effect 执行时，触发对象的字段的读取操作
- 当修改字段的时候，触发字段的设置操作，从而触发副作用函数的执行

```js
const bucket = new Set() 	// 存储副作用的桶
const data = {text: 'hello world'}	// 原始数据
const obj = new Proxy(data, {
  get(target, key) {		// 拦截读取操作
  	bucket.add(effect)	// 将副作用函数添加到桶中
    return target[key]
  },
  set(target, key, value) {		// 拦截设置操作
  	targt[key] = value
    bucket.forEach(fn => fn())	// 把副作用函数取出并执行
    return true
  }
})
```

设置拦截函数之后，每次设置操作都会先将原始数据改变，然后取出副作用函数并执行；每次读取操作都会先将副作用函数添加到桶中，然后返回返回值。但是目前的实现还存在很多缺陷：例如只是通过名字（effect）来获取副作用函数，这种硬编码，如果函数名变了或者副作用函数是一个匿名函数就不行了

### 2.1.2.设计一个完善的响应系统

使用一个全局变量存储副作用函数，将上文的副作用函数 effect 改为用于注册副作用函数的函数

```js
let activeEffect;  // 用一个全局变量存储被注册的函数
function effect(fn) { 		// effect 用于注册副作用函数
	activeEffect = fn		// 当调用 effect 注册副作用函数时，将副作用函数注册给 activeEffect
  fn()
}
const bucket = new Set()
const obj = new Proxy(data, {
  get(target, key) {
    if(activeEffect) {
      backet.add(activeEffect)
    }
    return target[key]
  },
  set(target, key, value) {
    target[key] = value
    bucket.foreach(fn => fn())
    return true
  }
})
```

在副作用函数与被操作的目标字段之间建立明确的联系

```js
const bucket = new WeakMap()
const obj = new Proxy(data, {
  get(target, key) {
    if(!activeEffect) return
    let depsMap = bucket.get(target)	// 从桶中取出 depsMap，它也是一个 Map 类型
    if(!depsMap){		// 如果不存在，新建一个 Map 并与之联系
    	bucket.set(target, (depsMap = new Map()))
    }
    let deps = depsMap.get(key)		// 再根据 key 从 depsMap 中取出 deps，他是一个 Set 类型，存储所有与 key 相关的副作用函数
    if(!deps) {
      deps.Map.set(key, (dep = new Set()))
    }
    deps.add(activeEffect)	// 最后将当前激活的副作用函数添加到桶里
  },
  set(target, key, value) {
    target[key] = value
    const depsMap = bucket.get(target)	// 根据 target 从桶中取得 depsMap
    if(!depsMap) return
    const effects = depsMap.get(key)	// 根据 key 取得所有副作用函数 effects
    effects && effects.forEach(fn => fn())
  }
})
```

从这段代码中可以看出构建数据结构的方式：WeakMap、Map 和 Set，WeakMap 由 target --> Map 构成，Map 由 key --> value 构成

为什么要使用 WeakMap：因为 WeakMap 对 key 是弱引用，不影响垃圾回收器的工作。如果 key 被垃圾回收器回收，那么对应的键和值就访问不到了，所以 WeakMap 经常用于存储那些只有当 key 所引用的对象存在时（没有被回收）才有价值的信息。WeakMap 不会导致内存溢出

对以上的代码做一些封装处理。在前面 get 的逻辑的时候，是把所有副作用函数收集到桶里的这部分逻辑，但更好的做法是将这部分逻辑封装到一个 track 函数里

```js
const obj = new Proxy(data, {
  get(target, key) {
    track(target, key)
    return target[key]
  },
  set(target, key, value) {
    target[key] = value
    trigger(target, key)
  }
})
function track(target, key) {
  if(!activeEffect) return
  let depsMap = bucket.get(target)
  if(!depsMap) {
    bucket.set(target, (depsMap = new Map()))
  }
  let deps = depsMap.get(key)
  if(!deps) {
    depsMap.set(key, (deps = new Set()))
  }
  deps.add(activeEffect)
}
function trigger(target, key) {
  const depsMap = bucket.get(target)
  if(!depsMap) return
  const effects = depsMap.get(key)
  effects && effects.foreach(fn => fn())
}
```

### 2.1.3. 分支切换与 cleanup

`obj.ok ? obj.text : 'not'`分支切换可能会产生遗留的副作用函数，text 只是有些时候会执行，但是也会存储副作用函数，导致不必要的更新。解决思路：每次副作用函数执行时，我们可以先把它从所有与之关联的集合中删除，当副作用函数执行完毕，会重新建立联系，但在新的联系中不会包含遗留的副作用函数

重新定义副作用函数，在 effect 内部定义了 effectFn 函数，并添加了 effectFn.deps 属性，该属性是一个数组，用来存储所有包含当前副作用函数的依赖集合：

```js
let activeEffect
function effect(fn) {
  const effectFn = () => {
    activeEffect = effectFn		// 当 effectFn 执行时，将其设置为当前激活的副作用函数
    fn()
  }
  effectFn.deps = []		// 用来存储所有与之相关的依赖集合
  effectFn() 	// 执行副作用函数 
}
function track(target, key) {	// 收集依赖集合改进
  if(!activeEffect) return
  let depsMap = bucket.get(target)
  if(!depsMap) {
    bucket.set(target, (depsMap = new Map()))
  }
  let deps = depsMap.get(key)
  if(!deps) {
    depsMap.set(key, (deps = new Set()))
  }
  deps.add(activeEffect)
  activeEffect.deps.push(deps) 		// 将依赖集合添加到 activeEffect.deps 数组中
}
let activeEffec
function effect(fn) {		// 注册函数改进，根据 effectFn.deps 获取所有相关联的依赖集合，进而将副作用函数从依赖集合中移除
	const effectFn = () => {
    cleanUp(effectFn)		// 调用cleanUp 完成清楚工作
    activeEffect = effectFn
    fn()
  }
  effectFn.deps = []
  effectFn()
}
function cleanUp(effectFn) {
  for(let i of effectFn.deps) {
    i.delete(effectFn)	 // 将 effectFn 从依赖集合中删除
  }
  effectFn.deps.length = 0	// 重置 deps 数组
}
```

现在已经解决依赖副作用函数的问题了，但是在 trigger 函数的 Set 中，我们会调用执行函数，会把副作用函数从 Set 剔除，但是执行的时候可能又把它加入 Set 中了，会导致这个函数无限循环。解决办法很简单，构造另外一个 Set 集合遍历它

```js
function trigger(target, key, value) {
  const depsMap = bucket.get(target)
  if(!depsMap) return
  const effects = depsMap.get(key)
  const effectsToRun = new Set(effects)	// 新建一个集合来运行，避免无限循环
  effects.forEach(effectFn => effectFn())
}
```

### 2.1.4. 嵌套的 effect 与 effect 栈

实际上副作用函数没有这么简单，很可能副作用函数里面也嵌套了一个副作用函数（渲染父组件，然后再渲染子组件），所以当我们执行父副作用函数的时候会过多的牵扯多个字段的改变。因为当我们用变量 activeEffect 来存储通过 effect 函数注册的副作用函数时，它只能存储一个，内层的副作用函数会覆盖父副作用函数，并且永远不会恢复原来的值，为了解决这个问题，应该在全局变量 activeEffect 的基础上加一个副作用函数栈 effectStack：

```js
let activeEffect		// 用一个变量存储当前激活的 effect 函数
const effectStack = []
function effect(fn) {
  const effectFn = () => {
    cleanUp(effectFn)
    activeEffect = effectFn
    effectStack.push(effectFn)		// 在副作用函数调用其将他压入栈中
    fn()
    effectStack.pop()	// 当前副作用函数执行完毕后，将当前副作用函数弹出栈，并把 activeEffect 函数还原
    activeEffect = effectStack[effectStack.length - 1]
  }
  effectFn.deps = []
  effectFn()
}
```

### 2.1.5. 调度执行

`obj.foo = obj.foo + 1` 这种语句会先读取 obj.foo 的值，然后触发 track 操作，然后设置 obj.foo 的值，触发 trigger 操作。可能导致该副作用函数还在执行中，然后就要开始下一次执行，这样就会导致无限递归调用自己，产生栈溢出。解决方法：在 trigger 将要发生时候添加条件，如果 trigger 触发的副作用函数正在执行中，则不触发执行：

```js
function trigger(target, key) {
  const depsMap = bucket.get(target)
  if(!depsMap) return
  const effects = depsMap.get(key)
  const effectsToRun = new Set()
  effects && effects.forEach(fn => {
    if(fn != activeEffect) {	// 如果 trigger 触发的副作用函数与当前正在执行的函数相同，则不触发执行
      effectsToRun.add(fn)
    }
  })
  effectsToRun.forEach(fn => fn())
}
```

可调度性是响应系统非常重要的特性，所谓可调度性就是指当 trigger 动作触发副作用函数执行时，有能力决定副作用函数执行的时机，次数以及方式。所以可以为 effect 函数设计一个选项参数 options，允许用户指定调度器：

```js
function effect(fn, options = {}) {
  const effectFn = () => {
    cleanUp(effectFn)
    activeEffect = effectFn
    effectStack.push(effectFn)
    fn()
    effectStack.pop()
    activeEffect = effectStack[effectStack.length - 1]
  }
  effectFn.options = options	// 将 options 挂载到 effectFn 上
  effectFn.deps = []
  effectFn()
}
```

有了调度函数，我们在 trigger 函数中触发副作用函数重新执行时，就可以直接调用用户传递的调度器函数，从而把控制权交给用户：

```js
function trigger(target, key) {
  const depsMap = bucket.get(target)
  if(!depsMap) return 
  const effects = depsMap.get(key)
  const effectsToRun = new Set()
  effects && effects.forEach(effectFn => {
    if(effectFn != activeEffect) {
      effectsToRun.add(effectFn)
    }
  })
  effectsToRun.forEach(effectFn => {
    if(effectFn.options.scheduler) {		// 如果存在一个调度器，则调用该调度器，并将副作用函数作为参数传递
    	effectFn.options.scheduler(effectFn)
    } else {
      effectFn()		// 否则直接执行副作用函数，默认行为
    }
  })
}
```

如上所示，在 trigger 函数执行时，会判断这个副作用函数是否存在调度器，如果存在调度器，则把这个函数作为参数传递给调度器函数执行，一个简单的调度器可以通过一个任务队列实现

```js
const jobQueue = new Set()	// 定义一个任务队列
const p = Promise.resolve()		// 使用 Promise.resolve() 创建一个 Promise 队列，用它将一个任务添加到微任务队列
let isFlushing = false	// 一个标志表示是否在刷新队列
function fulshJob() {
  if(isFlushing) return	// 任务队列正在刷新，什么都不做
  isFlushing = true	// 代表任务队列正在刷新
  p.then(() => {		// 在微任务队列中刷新 jobQueue 队列
  	jobQueue.forEach(job => job())
  }).finally(() => {	// 结束后重置 isFlushing
  	isFlushing = false
  })
}
effect(() => {
  console.log(obj.foo)
}, {
  scheduler(fn) {
    jobQueue.add(fn)	// 每次调度时，将副作用函数添加到 jobQueue 中
    flushJob()
  }
})
```

这个任务队列使用了 Set，具备了副作用函数的自动去重的能力。调度器每次在执行的时候将副作用函数添加到 jobQueue 队列中，再调用 flushJob 函数刷新队列，因为有 isFlushing 标志的存在，类似与 Vue.js 中连续多次修改响应式数据但只会触发更新一次

### 2.1.6. 计算属性 computed 与 lazy

前文已经了解到利用 effect 函数注册副作用函数，同时它允许执行一些选项参数 options，例如指定 schedule 调度器来控制副作用函数的执行时机和方式；利用 track 函数用来追踪和收集依赖；利用 trigger 函数来触发副作用函数和执行。实际上综合这些，我们可以实现 Vue.js 中一个非常重要并且极具特色的能力：计算属性

比如 lazy 的实现：

```js
function effect(fn, options = {}) {
  const effectFn = () => {
    cleanup(effectFn)
    activeEffect = effectFn
    effectStack.push(effectFn)
    fn()
    effectStack.pop()
    activeEffect = effectStack[effectStack.length - 1]
  }
  effectFn.options = options
  effectFn.deps = []
  if(!options.lazy) {		//只有非 lazy 的时候，才执行
  	effectFn()		// 执行副作用函数
  }
  return effectFn	// 将副作用函数作为返回值返回
}
const effectFn = effect(() => {
  console.log(obj.foo)
},{
  lazy: true		// 指定 lazy ，这个函数就不会立即执行
})
effectFn()	// 手动执行副作用函数
```

但是仅仅能够手动执行副作用函数的意义并不大，如果可以把传递给 effect 的函数换做一个 getter，那么这个 getter 函数可以返回任何值：

```js
function effect(fn, options = {}) {
  const effectFn = () => {
    cleanUp(effectFn)
    activeEffect = effectFn
    effectStack.push(effectFn)
    const res = fn()	// 将 fn 的执行结果存储到 res 中
    effectStack.pop()
    activeEffect = effectStack[effectStack.length - 1]
    return res
  }
  effectFn.options = options
  effectFn.deps = []
  if(!options.lazy) {
    effectFn()
  }
  return effectFn
}
```

通过新增代码可以看到，传递给 effect 函数的参数 fn 是副作用函数，但是我们把它封装了一层，如果是存在 lazy 则返回 effectFn，并且 effectFn 函数中可以拿到fn 的计算结果

现在我们可以实现懒执行的副作用函数了，并且可以拿到副作用函数的执行结果了，这样的话我们就可以实现计算属性了

```js
function computed(getter) {
  const effectFn = effect(getter, {		// 把 getter 作为副作用函数，创建一个 lazy 的 effect
    lazy: true
  })
  const obj = {
    get value() {		// 当读取 value 时才执行 effectFn
      return effectFn()
    }
  }
  return obj
}
```

现在实现的计算属性只是还是懒计算，还没有做到对结果值的缓存，多次访问会导致 effectFn 的多次计算，所以添加对值的缓存功能：

```js
function computed(getter) {
  let value		// 用来缓存上一次计算的值
  let dirty = true 		// 用来标识是否重新计算值
  const effectFn = effect(getter, {
    lazy: true
  })
  const obj = {
    get value() {
      if(dirty) {
        value = effectFn()
        dirty = false
      }
      return value
    }
  }
  return obj
}
```

这样就做到了缓存，但是当值的确发生变化的时候，需要重新计算一次，也就是需要把 dirty 标志改为 false，解决方法是调度器 schedule

```js
function computed(getter) {
  let value
  let dirty = true
  const effectFn = effect(geter, {
    lazy: true,
    schedule() {	// 添加调度器，在调度器中将 dirty 重置为 true
      dirty = true
    }
  })
  const obj = {
    get value() {
      if(dirty) {
        vlaue = effectFn
        dirty = false
      }
      return value
    }
  }
  return obj
}
```

scheduler 调度器函数会在 getter 函数中所依赖的响应式数据变化时重新执行，然后将 dirty 重置为 true，当下一次访问值的时候就会重新计算属性了。

但是这里还有一个缺陷，因为现在的计算属性是个懒执行，只能通过副作用函数重新执行拿到缓存，不能在相关联的数据更改之后，缓存就跟着改，所以需要改进。改进方法是当读取计算属性的值的时，手动调用 track 函数进行追踪；当计算属性依赖的响应式数据发生变化时，手动调用 trigger 函数触发响应

```js
```



