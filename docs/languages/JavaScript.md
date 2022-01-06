# JavaScript_temp

[toc]



如果在数学表达式中有一个NaN，那么它会一直传播下去

脚本永远不会因为一个致命错误就停止，大不了一个NaN

alert prompt  confirm

显示的转为字符串 String(value)      隐式的转为字符串 alert(value)

对undefined进行数字转换时候，结果为NaN，而不是0

比较的时候注意是否发生了类型转换

- 除了严格相等 `===` 外，其他但凡是有 `undefined/null` 参与的比较，我们都需要格外小心。
- 除非你非常清楚自己在做什么，否则永远不要使用 `>= > < <=` 去比较一个可能为 `null/undefined` 的变量。对于取值可能是 `null/undefined` 的变量，请按需要分别检查它的取值情况。

'2' > '12'  true

js 的 || 能够返回第一个真值或者最后一个假值

两个非运算 `!!` 有时候用来将某个值转化为布尔类型

labelName

**空值的** `return` **或没有** `return` **的函数返回值为** `undefined`

一个函数应该简短且只有一个功能

在其他编程语言中，只要提到函数的名称都会导致函数的调用执行，但 JavaScript 可不是这样。

回调其实就是传递了一个函数变量？

**一个函数是表示一个“行为”的值**

一个函数代表一个行为，我们可以在变量之间传递它

函数声明是在js准备脚本阶段创建的，所以可以在任何位置使用它

箭头函数对于简单行为来说很方便

自动分号插入，有些时候它并不自动插入，代码块之后不需要分号

![image-20201223225900084](http://111.229.14.128:9001/wutian/image-20201223225900084.png)

![image-20201223230334914](http://111.229.14.128:9001/wutian/image-20201223230334914.png)

自描述型代码

$event

计算属性  方括号[]

属性名简写

对象能够访问任何属性，即使该属性不存在也不会报错

一般用in 来判断一个属性是否存在一个对象中，而不是undefined

赋值了对象的变量存储的不是对象本身，而是该对象'在内存中的地址'，换句话说就是对该对象的引用

仅当两个对象为同一对象的时候，它们才叫相等

Object.assign(dest, [src1, src2, src3...])

深层克隆指的是对象里面的对象也拷贝过去

垃圾回收机制，可达性

对象属性的函数称为方法

在js中，this关键字和其它多数编程语言不同，js中的this可以用于任何函数，即使他不是对象的方法，this的值是在代码运行时计算出来的

this的并不取决于方法声明的位置，而是取决于点符号前的是什么对象（this的重要性就是c里面的指针

箭头函数没有自己的this，它取决于外部的正常函数

this通过函数调用才有意义

理解 [this](https://zhuanlan.zhihu.com/p/42145138)：

- this 永远指向一个对象
- this的指向完全取决于函数调用的位置

js 中一切都是对象，运行环境也是一个对象，所以函数实在某个对象下运行，而this就是函数运行时的所在的对象（环境），但是js支持环境的动态切换，简单的说就是 this 的指向是动态的，很难事先确定到底指向哪里

事件绑定：行内绑定，动态绑定，事件监听



构造函数以大写字母开头，只能有'new' 操作符来执行

当一个函数使用new操作符执行时，它按照步骤为：一个新的对象被创建并分配给this；函数体执行，通常它会修改this，为其添加新的属性；返回this的值。隐式创建，隐式返回

可选链  ？.  使用在一些属性可以不存在的地方

symbol 类型    symbol不会被自动转为字符串

对象的属性键只能是字符串类型或则symbol类型，symbol类型可以防止被重写

symbol属性不参与for...in循环

除null 和 undefined 以外的原始类型都提供了许多有用的方法，从形式来说这些方法通过临时对象工作

Math.floor   Math.ceil    Math.round

编写一个随机整数的函数，容易边缘值的概率会低两倍

js中字符串不可更改

str.indexOf()   更现代的方法是  str.includes(substr, pos)  

str.startsWith(str)   str.endsWith(str)    str.slice(start [,end])  str.substr(start, [, length])    

对象能够存储键值集合，但是有些时候我们需要存储有序集合，这时候就到了数组了

数组可以存储任何类型的元素 pop  push shift unshift    从本质上来说，数组仍然是一个对象

将数组视为有序数据的特殊结构

for... in  循环会遍历所有数组所有属性，不仅仅是这些数字属性   类数组对象

不应该用 for ... in 处理数组

我们修改数组的时候，length 属性会自动更新

数组通常都是使用方括号

数组有自己的 toString 实现，返回已逗号隔开的元素列表

不要使用 == 比较数组

仅当两个对象引用的是同一个对象的时候，它们才相等，如果==左右两个参数中有一个参数是对象，另一个参数是原始类型，那么该对象就会转换为原始类型

=== 不会进行类型转换

如果我们使用 == 比较数组，除非它们两个引用了统一数组的变量，否则它们永远不相等，要用迭代的方法逐项的比较它们

遍历数组的元素：

for( let i=0; i<arr.length; ++i)   运行的最快，可兼容就浏览器

for ( let item of arr)     现代语法，只能访问items

for (let i in arr) 			永远不要用这个

传入call 和 apply 的第一个参数都会被看作函数上下文，不同之处在于后续的参数

箭头函数的this与声明所在的上下文相同

调用箭头函数时，不会隐式的传入this参数，而是从定义时的函数继承上下文

所有函数都可以访问bind方法， 可以创建并返回一个新函数，并绑定到传入的对象上，调用bind不会修改原函数，而是创建了一个全新的函数

this 表示函数上下文， 即与函数调用相关联的对象，函数的定义方式和调用方式决定了this的取值

函数的调用方式有四种：

- 作为函数调用 skulk()
- 作为方法调用  ele.skulk()
- 作为构造函数调用 new Skulk()
- 通过apply 和 call方法调用 skulk.apply(ele) skulk.call(ele)

函数调用方式影响this的取值：

- 如果作为函数调用，非严格模式下指向window对象， 严格模式下指向undefined
- 作为方法调用，this通常指向调用的对象
- 作为构造函数调用，this指向新创建的值
- 通过call 或 apply 调用，this 指向 call 或 apply 的第一个参数

箭头函数和bind某些时候都是绑定的感觉

闭包允许函数访问并操作函数外部的变量，只要函数或变量存在于声明函数时的作用域内，闭包即可使函数能够访问这些变量和函数

``` js
function getMaxSubSum(arr) {
    let maxSum = 0;
    let partialSum = 0;
    for (let item of arr) {
        partialSum += item
        maxSum = Math.max(maxSum, partialSum)
        if(partialSum<0) paritalSum = 0
    }
    return maxSum
}
```

arr.splice 方法可以说是处理数组的瑞士军刀  arr.slice 复制一部分值   arr.concat 连接多个数组    

arr.forEach 方法允许为数组的每一个元素都运行一个函数 

[arr.indexOf](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf)、[arr.lastIndexOf](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/lastIndexOf) 和 [arr.includes](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/includes) 方法与字符串操作具有相同的语法，并且作用基本上也与字符串的方法相同

如果是一个对象数组，可以用 arr.find()  方法找到具有特定条件的对象  同理有 arr.findIndex  和  arr.filter

arr.map 方法对数组中的每个元素都调用函数，并返回结果数组，该方法和forEach 很像，但是forEach 并不返回结果数组，这两个应该是最常用的方法了

arr.sort() 方法对数组进行 原位(in-place) 排序，更改元素的顺序，但是默认情况下是按字符串进行排序了，所以一般要使用我们自己的函数进行排序

对于字符串比较算法，最好是使用 str.localeCompare 方法正确的对字母进行排序

arr.split 与 arr.join  恰好相反

arr.reduce  根据数组计算单个值

Array.isArray

thisArg  也可以采用箭头函数

`sort`，`reverse` 和 `splice` 方法修改的是数组本身。

```js
let prices = {
  banana: 1,
  orange: 2,
  meat: 4,
};

let doublePrices = Object.fromEntries(
  // 转换为数组，之后使用 map 方法，然后通过 fromEntries 再转回到对象
  Object.entries(prices).map(([key, value]) => [key, value * 2])
);

```

可以通过这种方式建立强大的转换链, Object.keys, values, entries

```js
function count(obj) {
  return Object.keys(obj).length;
}
```

JS中最常用的两种数据结构是Object 和 Array， 对象能让我们创建键来存储数据项的单个实体， 数组则让我们能够将数据收集到一个有序的集合中，但是，当我们把它们传递给函数时， 函数可能只需要单个块

```js
let [firstName, surname] = "Ilya Kantor".split(' ');
```

解构并不意味着破坏

```js
let user = {
  name: "John",
  age: 30
};

// 循环遍历键—值对
for (let [key, value] of Object.entries(user)) {
  alert(`${key}:${value}`); // name:John, then age:30
}
```

内建对象： 日期（Date），该对象提供了日期/时间的管理方法

JS 提供了如下方法

JSON.stringify  将对象转换为JSON, JSON.parse 将 JSON 转换回对象

一些特定于 JS 的对象属性会被JSON.stringify 跳过，即：函数属性、 Symbol类型的属性、存储undefined的属性

```js
JSON.stringify(user, null, 2)		// 首行缩进 2 个字符

```

```js
let str = '{"title":"Conference","date":"2017-11-30T12:00:00.000Z"}';

let meetup = JSON.parse(str, function(key, value) {
  if (key == 'date') return new Date(value);
  return value;
});

alert( meetup.date.getDate() ); // 现在正常运行了！
```

函数进阶内容

所有函数的处理都是这样的：

1. 当前上下文被记录在堆栈的顶部
2. 为子调用创建一个新的上下文
3. 当子调用结束后，前一个的上下文从堆栈中弹出，并继续执行

递归使用的时候要考虑堆栈的深度

Rest 参数必须放到参数列表的末尾

arguments是一个类数组

箭头函数是没有arguments的

spread语法可以传递多个可迭代对象，可以用来合并数组，和Array.from() 很像，但是后者还可以用来操作类数组

在 JS 中，每个运行的函数，代码块以及整个脚本都有一个成为词法环境(Lexical Environment) 的内部隐藏的关联对象

词法环境由两部分组成：

1. 环境记录(Environment Record)：一个存储所有局部变量作为其属性的对象
2. 对外部词法环境的引用，与外部代码相关联

变量是特殊内部对象的属性，与当前正在执行的块/函数/脚本有关，操作变量实际上是操作该对象的属性

当代码要访问一个变量是，首先回搜索内部词法环境，然后搜索外部环境，然后搜索更外部的环境，直到全局词法环境

在JS中，所有函数都是天生闭包的，闭包就是指内部函数总是可以访问所在的外部函数中声明的变量和参数

词法环境仅在可达时才会保留在内存中

debugger

理论上当函数可达时，它外部变量也都将存在，但在时间中，JS引擎会试图优化它，如果从代码中可以明显看出所有未使用的外部变量，那么就会将它删除，此类变量在调试中将不可用

从程序进入代码块的那一刻开始，变量就开始进入“未初始化”状态，它一直保持未初始化状态，直至程序执行到相应的let语句

var 没有块级作用域，用var声明的变量，不是函数作用域就是全局作用域，var 的变量可以重新声明，可以在声明前被使用，声明会提升，但是赋值不会

(function{...})，立即调用函数声明，但是如今没有理由这么来编写代码

全局对象，浏览器中是 'window' ， 对 Node.js 而言，它的名字是 'global' ，最近 globalThis 被作为全局对象的标准名称加入了JS

全局对象的所有属性都可以直接访问

在浏览器中，使用var 声明的全局函数和变量会变成全局对象的属性

把函数想成一个可被调用的行为对象，我们不仅可以调用它们，还能把它当作对象来处理，函数对象包含一些便于使用的属性

name length   函数也可以添加属性，然后用于闭包

给函数表达式添加名字，可以在局部词法环境中添加一个函数，这样可以以防外面的被更改掉，当我们需要一个可靠的内部名时，就可以吧函数声明写成函数表达式

函数名后面跟着多个括号，为了函数能够正常工作，需要是函数的结果也是一个函数

```js
function makeCounter() {
  let count = 0;

  function counter() {
    return count++;
  }

  counter.set = value => count = value;

  counter.decrease = () => count--;

  return counter;
}
```



```js
function sum(a) {

  let currentSum = a;

  function f(b) {
    currentSum += b;
    return f;
  }

  f.toString = function() {
    return currentSum;
  };

  return f;
}

```

js的函数变量是真的好用啊

"new Function" 很少被使用，但是有些时候只能选择它，它允许我们将任意字符串变为函数，这样我们就可以从服务器接受一个新的函数并执行它，只有这种从服务器动态的创建函数的情况可能才会使用到这个语法

通常，闭包是指使用一个特殊的属性 [[Environment]] 来记录函数自身创建时的环境的函数，它具体指向了函数创建时的词法环境

setTimeout 允许我们将函数推迟到一段时间间隔之后再执行

setInterval  允许我们重复运行一个函数，从一段时间间隔之后开始运行，之后以一个时间间隔重复运行

clearTimeout   clearInterval

```js
let delay = 5000
let timerId = setTimeout(function request(){
    ...
    if(request failen due to server overload){
        delay *= 2
    }
    timerId = setTimeout(request, delay);		// 感觉这就是递归了
}, delay)
```

嵌套的setTimeout 能够精确的设置两次执行之间的延时， 而 setInterval 却不能

```js
function slow(x) {
  // 这里可能会有重负载的 CPU 密集型工作
  alert(`Called with ${x}`);
  return x;
}

function cachingDecorator(func) {
  let cache = new Map();

  return function(x) {
    if (cache.has(x)) {    // 如果缓存中有对应的结果
      return cache.get(x); // 从缓存中读取结果
    }

    let result = func(x);  // 否则就调用 func

    cache.set(x, result);  // 然后将结果缓存（记住）下来
    return result;
  };
}
```

一旦方法被传递到与对象分开的某个地方，this就丢失

setTimeout 方法的函数调用设定了 this =  window

这个需求很典型：我们想将一个对象方法传递到别的地方，然后再该位置调用它，如何确保在正确的上下文中调用它

```js
setTimeout(user.sayHi, 1000); // Hello, undefined!
setTimeout(() => user.sayHi(), 1000); // Hello, John!
```

最简单的解决方案是使用一个包装函数，看起来不错，如果在setTimeout触发前一秒user的值有改变的话就错误了

第二种解决方案是内建方法 bind, 它可以绑定this

```js
let boundFunc = func.bind(context)
```

`func.bind(context)` 的结果是一个特殊的类似于函数的"外来对象"，它可以像函数一样被调用，并且透明地将调用传递给func并将this=context

如果一个函数有多个方法，可以使用bindAll

我们不仅可以绑定this，还可以绑定参数（arguments），但是很少这么做

一个函数不能重绑定

bind之后，函数会丢失原来的函数属性

bind 和 包装都可以解决一样的问题，但是包装可能在一些复杂的场景下失效

```js
function askPassword(ok, fail) {
  let password = prompt("Password?", '');
  if (password == "rockstar") ok();
  else fail();
}

let user = {
  name: 'John',

  login(result) {
    alert( this.name + (result ? ' logged in' : ' failed to log in') );
  }
};

askPassword(()=>user.login(true), ()=>user.login(false))
askPassword(user.login.bind(user, true), user.login.bind(user, false))
```

箭头函数不仅是简洁代码，它还具有一些特性

JS 的精髓在于创建一个函数并将其传递到某个地方，在这样的函数中，我们通常不想离开当前上下文，这就是i箭头函数的主战场

.bind(this)  会创建一个该函数的"绑定版本"，箭头函数没有创建任何绑定，箭头函数只是没有this，this的查找与常规变量的搜索方式完全相同：在外部词法环境中查找

对象可以存储属性，但是目前的属性对我来说只是简单的"键值对"，但是实际上属性是更灵活和强大的东西

对象属性除了value，还有三个特殊的特性：writable、 enumerable、 configurable

getOwnPropertyDescriptor    defindeProperty    defineProperties

除了数据属性，还有访问器属性，它们本质上是用于获取和设置值的函数

```js
let user = {
  name: "John",
  surname: "Smith",

  get fullName() {
    return `${this.name} ${this.surname}`;
  }
};
alert(user.fullName); // John Smith
```

从外表看，访问器属性看起来就是普通属性，这就是访问器属性的设计思想

```js
let user = {
  name: "John",
  surname: "Smith",

  get fullName() {
    return `${this.name} ${this.surname}`;
  },

  set fullName(value) {
    [this.name, this.surname] = value.split(" ");
  }
};

// set fullName 将以给定值执行
user.fullName = "Alice Cooper";

alert(user.name); // Alice
alert(user.surname); // Cooper
```

现在我们有了一个虚拟属性，而且他是可读可写的

访问器属性没有 value 和 writable， 但是有 get 和 set 函数

Getter/Setter 可以用作”真实“属性值的包装其，以便进行更多的控制，如我们可以设置命名的时候判断name是不是太短

```js
function User(name, birthday) {
  this.name = name;
  this.birthday = birthday;

  // 年龄是根据当前日期和生日计算得出的
  Object.defineProperty(this, "age", {
    get() {
      let todayYear = new Date().getFullYear();
      return todayYear - this.birthday.getFullYear();
    }
  });
}

let john = new User("John", new Date(1992, 6, 1));

alert( john.birthday ); // birthday 是可访问的
alert( john.age );      // ……age 也是可访问的
```



在编程中我们经常会像获取并扩展一些东西，原型继承(Prototypal inheritance) 这个语言特性能够帮助我们实现这一个需求

在 JS中，对象有一个特殊的隐藏属性 [[Prototype]] ，它要么为 null，要么就是对另一个对象的引用，该对象被称为原型

当我们从 object中读取一个缺失的属性时，JS会自动从原型中获取该属性，在编程中，这种行为被称为原型继承

`rabbit.__proto__ = animal`   这种感觉和闭包有点像啊，都是继承的关系

原型继承有两个限制

- 引用不能形成闭环
  - `__proto__`的值可以是对象，也可以是 `null` ，而其它类型会被忽略

`__proto__` 是 `[[Prototype]]`不一样

写入不适用原型，原型仅用于读取属性，访问器属性是一个例外，因为分配操作是由setter函数处理的，因此，写入此类属性实际上与调用函数相同

无论在哪里找到方法，在一个对象还是在原型中，在一个方法调用中，this始终是  .  前面的对象

for ... in  循环也会迭代继承的属性

属性查找和执行是两回事，先从原型中找到属性，然后给当前的this 执行

所有描述特定对象状态的属性，都应该被写入改对象中，这样可以避免一些问题

Obejct.create(proto, [descriptors])     Object.getPrototypeOf(obj)         Object.setPrototypeOf(obj, proto)

类的方法之间没有逗号，在JavaScript中，类也是一种函数，准确的说是构造方法

有些人说class是一种语法糖，但是仍有一些区别，首先通过从class创建的函数具有特殊的内部属性标记，类的方法不能枚举，类定义将“prototype”中的所有方法的enumerable标志设置为false，类里面总是使用 use strict

静态方法不是某个特定的方法，而是整个class的方法，可用于搜索/保存/删除，静态属性同理

静态属性和方法是可被继承的

在面向对象中，属性和方法分为两组：

- 内部接口：可以通过该类的其它方法访问，但不能从外部访问的方法和属性
- 外部接口：可以从类的外部访问的属性和方法

受保护的属性通常以下划线_作为前缀

内建的方法也是返回的扩展内建类

类检查 instanceof 操作符用于检查一个对象是否属于某个特定的class

要使 try ... catch  能工作，代码必须是可执行的   try ... catch  同步工作，如setTimeout

如果我们不需要 error 的详细信息， catch 可以忽略它

throw 操作符会生成一个 error 对象

try ... catch ... finally

```js
function loadScript(src, callback){
    let script = document.createElement('script')
    script.src = src
    script.onload = () => callback(script)
    document.head.append(script)
}
loadScript('/my/test.js', function(){newFunction()})
```

这被称为”基于回调“的异步编程风格，异步执行某项功能的函数应该提供一个callback 参数用于在相应事件完成是调用

```js
let promise = new Promise(function(resolve, reject){
    // executer
})
```

promise  我感觉和 setTimeOut  的区别就是区分的了成功和失败，成功了就 resolve， 失败了就 reject

````js
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => reject(new Error("Whoops!")), 1000);
});

// reject 运行 .then 中的第二个函数
promise.then(
  result => alert(result), // 不运行
  error => alert(error) // 1 秒后显示 "Error: Whoops!"
);
promise.then(alert); // 只对成功感兴趣
promise.catch(alert);		// 只对失败感兴趣
````

promise  基于回调的模式的一些好处：

- promise 允许我们按照自然顺序进行编码，首先允许函数然后利用 .then 来处理结果，我们可以根据需要多次 .then  ，称之为 Promise链

第二个对resolve的调用会被忽略，只有第一次对 reject/resolve 的调用才会被处理

```js
cnew Promise(function(resolve, reject) {

  setTimeout(() => resolve(1), 1000); // (*)

}).then(function(result) { // (**)

  alert(result); // 1
  return result * 2;

}).then(function(result) { // (***)

  alert(result); // 2
  return result * 2;

}).then(function(result) {

  alert(result); // 4
  return result * 2;

});
```

从技术上我们可以将多个 .then 添加到一个promise上，但这并不是promise链

在前端编程中， promise通常被用于网络请求，也可使用 fetch 方法从远程服务器加载用户信息

Fetch

JS 可以将网络请求发送到服务器，并在需要时加载新信息，对于来自 JS 的网络请求，有一个总称术语 AJAX（Asynchronous JavaScript and XML），但是我们一般不必使用XML，这个是一起的了

` let promise = fetch(url, [options])`

- url 要访问的url
- options 可选参数：method，header

如果没有options，那就是一个简单的 get 请求，下载url 的内容，浏览器立即启动请求，并返回一个该调用代码用来获取结果的promise

获取响应通常需要经历两个阶段：

- 第一阶段，当服务器发送了响应头（response header），fetch 返回的 promise 就使用内建的 Response class 对象来对响应头进行解析

这个阶段，我们通过检查响应头来检查HTTP状态以确定请求是否成功，如果没有响应体（response body），那么promise就会reject，异常的HTTP状态，不会出现error，我们可以从response中看到http状态： status、ok

```js
let res = await fetch(url)
if(res.ok){}
```

- 第二阶段，为了获取response body，我们需要使用一个其它方法调用

response提供了很多基于promise的方法，以不同的格式来访问body：response.text(), response.json(), response.formData(), response.blob(),

我们只能选择一种读取body的方法，因为第二次的是已经被处理过的

response header 位于 response.headers 中的一个类似于 Map 的header 对象

```js
let user = {
    name: 'John',
    surname: 'Smith'
}
let res = await fetch('/article/fetch',{
    method: 'POST',
    headers: {
        'Content-Type': 'application/json;charset=utf-8'
    },
    body: JSON.stringify(user)
})
let result = await res.json()
alert(res.message)
```

同样我们可以使用  Blob 或 BufferSource 对象通过 fetch 提交二进制数据，如图片

```html
<body style="margin:0">
    <canvas id='canvasElem' width='100' height='80' style='border:1px solid'></canvas>
    <input type='button' value='Submit' onclick='submit()'>
    <script>
    	canvasElem.onmousemove = function(e){
            let ctx = canvasElem.getContext('2d')
            ctx.lineTo(e.lientX, e.e.clientY)
            ctx.stroke()
        }
        async function submit(){
            let blob = await new Promise(resolve=>canvasElem.toBlob(resolve, 'image/png'))
            let res = await fetch('/article/fetch/post/image',{
                method:'POST',
                body: blog
            })
           let result = await res.json()
           alert(result.message)
        }
    </script>
```

典型的fetch 请求由两个 await 组成

```js
let res = await fetch(url, [options])
let result = await res.json()

// 或者是
fetch(url, options)
	.then(res => res.json())
	.then(result => /* ... */)
```

下面这段代码有意思，promise和fetch一起联合写的：

```js
// 发送一个对 user.json 的请求
fetch('/article/promise-chaining/user.json')
  // 将其加载为 JSON
  .then(response => response.json())
  // 发送一个到 GitHub 的请求
  .then(user => fetch(`https://api.github.com/users/${user.name}`))
  // 将响应加载为 JSON
  .then(response => response.json())
  // 显示头像图片（githubUser.avatar_url）3 秒（也可以加上动画效果）
  .then(githubUser => {
    let img = document.createElement('img');
    img.src = githubUser.avatar_url;
    img.className = "promise-avatar-example";
    document.body.append(img);

    setTimeout(() => img.remove(), 3000); // (*)
  });
```

这段代码可以工作，但是有个为题， * 行头像显示结束后如果我们还想显示一个编辑的表单是做不到的，为了使得链可以扩展，我们需要显示一个在头像结束时进行 resolve 的 promise

作为一个好的做法，异步行为应该始终返回一个promise

promise 的 executor 周围存在隐式的 try...catch 自动获取了 error， 并将其变为 rejected promise

promise.all  并行执行多个 promise

```js
let names = ['iliakan', 'remy', 'jeresig'];

let requests = names.map(name => fetch(`https://api.github.com/users/${name}`));

Promise.all(requests)
  .then(responses => {
    // 所有响应都被成功 resolved
    for(let response of responses) {
      alert(`${response.url}: ${response.status}`); // 对应每个 url 都显示 200
    }

    return responses;
  })
  // 将响应数组映射（map）到 response.json() 数组中以读取它们的内容
  .then(responses => Promise.all(responses.map(r => r.json())))
  // 所有 JSON 结果都被解析："users" 是它们的数组
  .then(users => users.forEach(user => alert(user.name)));
```

Async/await 是以更舒适的方式使用promise的一种特殊语法，同时它非常易于理解和使用

async 这个单词表达了一个简单的事情，即这个函数总是返回一个promise， 其它值将自动被包装在一个resolved的promise中

await 只在async 中工作，关键字await 让JS引擎等待直到 promise 完成（settle）并返回结果，await 实际上会暂停函数的执行，知道promise的状态变为settled，然后以promise的结果继续执行，这个行为不会耗费任何CPU资源，因为 JS引擎可以同时处理其它脚本

await 不能在顶层代码中运行，但我们可以将其包裹在一个匿名 async 函数中

```js
(async()=>{
    let res = await fetch(/*...*/)
})()
```

async/await 可以和 promise.all 一起使用

常规函数只会返回单一值或者直接不返回任何值，但generator 可以按需一个接一个的返回（“yeild”）多个值，他们可以与iterable完美配合使用，从而可以轻松的创建数据流

要创建一个generator，我们需要一个特殊的语法结构：function*

异步迭代允许我们对按需通过异步请求而得到的数据进行迭代，例如，我们通过网络分段（chunk-by-chunk）下载数据的时候，异步生成器（generator）是这一步骤更加方便

随着我们的应用越来越大，我们想要将其拆分成多个文件，即所谓的模板（module），一个模板可以用户包含用于特定目的的类和函数库

模块可以相互加载，并可以使用特殊的指令export和import来交换功能，从一个模块调用一个模块的函数

import 指令通过相当于当前文件的路径加载模块，并将导入的函数分配给相应的变量

模块始终默认使用 use strict

每个模块都有自己的顶级作用域，一个模块中的顶级作用域变量和函数在其它脚本中是不可见的

如果我们真的需要创建一个 window-level 的全局变量，我们可以明确的赋值给 window

如果同一个模块被导入到多个其它位置，那么它的代码仅会在第一次导入的时候执行，

在一个模块中，this 是 undefined

模块脚本总是延迟的，模块脚本回等到HTML文档完全准备就绪，然后才会运行，保持脚本的相对顺序，在文档中排在前面的脚本先执行

如果内联脚本具有 async 特种，它就不会等待任何东西

```js
<!-- 所有依赖都获取完成（analytics.js）然后脚本开始运行 -->
<!-- 不会等待 HTML 文档或者其他 <script> 标签 -->
<script async type="module">
  import {counter} from './analytics.js';

  counter.count();
</script>
```

import 必须给出相对或绝对的 url 路径，没有任何路径的模块被成为裸模块，在import中是不允许这种模块的

在实际开发中，浏览器模块很少被以”原始“形式进行使用，通常我们会使用一些特殊工具，如Webpack将它们打包在一起，然后部署到生产环境的服务器中，使用打包工具的一个好处是它们可以更好的控制模块的解析方式，允许我们使用裸模块和更多功能，例如css/html 模块等

构建工具做以下这些事：

- 从一个打算放在 HTML 中的 `<script type='module'>  ”主“模块开始
- 分析它的依赖，它的导入，它的导入的导入等
- 使用所有模块构建一个文件或者多个文件，并用打包函数（bundler function）替代原生的import调用，以使其正常工作，还支持像 html/css 模块等特殊的模块类型
- 在处理过程中进行一些优化

我们可以通过在声明之前防止export来标记任意声明为导出，在类或者函数前的export不会让它们变成函数表达式，尽管被导出了，但它仍然是一个函数声明，不建议在函数和类声明后使用分号

通通导入一般不可取，如果明确列出我们需要的导入的内容，优化器（optimizer）就会检测到它，从而是构建更小，这就是所谓的”摇树（tree-shaking）“

 import as 可以让导入具有不同的名字

eval  执行代码字符串，代码字符串可能会比较长，eval的结果是最后一条语句的结果



## A re-introduction to JavaScript

[A re-introduction to JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript)

### Numbers

`console.log(3/2) 	// 1.5, not 1​`

an apparent integer is fact implicitly a float

- parseInt

```javascript
parseInt('123', 10); // 123
parseInt('010', 10); // 10
parseInt('11', 2); // 3
parseInt('0x10'); // 16
```

`parseFloat()` always uses base 10.

You can also use the unary + operator to convert values to numbers: 

```javascript
+ '42';   // 42
+ '010';  // 10
+ '0x10'; // 16
```

A special value called NaN(not a number) is returned if the string is non-numeric:  `parseInt('hello')`

NaN is toxic: if you provide it as an operand to any mathematical operation, the result will also be NaN

```javascript
+'10.2abc'		// NaN
parseFloat('10.2abc')		// 10.2
```

### String

Strings are sequences of Unicode characters.

```javascript
'hello'.charAt(0); // "h"
'hello, world'.replace('world', 'mars'); // "hello, mars"
'hello'.toUpperCase(); // "HELLO"
```

- null: only accessible through the null keyword.
- undefined: a value hasn't even been assigned yet.

undefined is actually a constant

You can perform this conversion explicitly using the `Boolean()` function:

```javascript
Boolean('');  // false
Boolean(234); // true
```

1. `false`, `0`, empty strings (`""`), `NaN`, `null`, and `undefined` all become `false`.
2. All other values become `true`.

### Variables

let, const or var

let allows you to declare block-level variables, the declared variable is available from the block it is enclosed in.

```javascript
// myLetVariable is *not* visible out here

for (let myLetVariable = 0; myLetVariable < 5; myLetVariable++) {
  // myLetVariable is only visible in here
}

// myLetVariable is *not* visible out here
```

const allows you to declare variables whose values are never intended to change. The variable is available from the blcok.

var does not have the restrictions, a variable declared with the var keyword is available from the function it is declared in.

```javascript
// myVarVariable *is* visible out here

for (var myVarVariable = 0; myVarVariable < 5; myVarVariable++) {
  // myVarVariable is visible to the whole function
}

// myVarVariable *is* visible out here
```

An important difference between JavaScript and other languages like Java is that in JavaScript, blocks do not have scope, only functions have a scope.

### Operators

The + operator also does string concatenation: ` 'hello' + 'world'`

Adding an empty string to something is a useful way of converting it to a string itself: `' ' + 5`

==: the double-equals operator performs type coercion if you tive it different types.

```javascript
123 == '123'; // true
1 == true; // true
```

to avoid type coercion, user the triple-equals operator:  ===

###  Control structures

The && and || operators use short-circuit logic, which means whether they will execute their second operand is dependent on the first. 

### Objects

JavaScript objects can be thought of as simple collections of name-value pairs.

The 'name' part is a JavaScript string, while the value can be any JavaScript value -- including more objects.

Attribute access can be chained together: `obj.details.color       obj['details']['size']`

The following example creates an object prototype and an instance of that prototype:

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}

// Define an object
var you = new Person('You', 24);
// We are creating a new person named "You" aged 24.
```

```javascript
obj.for = 'Simon'; // Syntax error, because 'for' is a reserved word
obj['for'] = 'Simon'; // works fine
```

### Array

Arrays in JavaScripts are actually a special type of object. The work very much like regular objects but they have one magic property called 'length'.

Note that array.length isn't necessarily the number of items of the array.Consider the following:

```javascript
var a = ['dog']
a[100] = 'fox'
a.length;		// 101
```

Remember -- the length of the array is one more than the highest index

If you query a non-existent array index, you'll get a value of undefined in return: `typeof a[90];   // undefined`

ES2.15 introduced the more concise `for...of` loop for iterable objects such as array.

`for..in` this does not iterate over the array elements, but the array indeces. Futhermore, if someone added new properties to `Array.prototype`, they would also be iterated overby such a loop.

forEach(): `['dog', 'cat', 'hen'].forEach(function(currentValue, index, array)) { }`

### Functions

If no return statement is used(or an empty return with no value), JavaScript returns undefined.

Functions have access to an additional variable inside their body called arguments.

`...variable` will include within that variable the entire list of uncaptured arguments that the function was called with.

JavaScript let you call a function with an arbitrary of arguments, using the `apply()` method of any function object.

### Anonymous functions

Anonymous functions are typically used as arguments to other functions or are made callable by immediately assigning them to a variable that can be used to invoke the function.

```javascript
(function() {
  // …
})();
```

### Recursive functions

### Custom objects

JavaScript uses functions as classes

```javascript
function makePerson(first, last) {
  return {
    first: first,
    last: last
  };
}
function personFullName(person) {
  return person.first + ' ' + person.last;
}
function personFullNameReversed(person) {
  return person.last + ', ' + person.first;
}

var s = makePerson('Simon', 'Willison');
personFullName(s); // "Simon Willison"
personFullNameReversed(s); // "Willison, Simon"
```

This works, but it's pretty ugly. What we really need is a way to attach a function to an object. Since functions are objects, this is easy:

```javascript
function makePerson(first, last) {
  return {
    first: first,
    last: last,
    fullName: function() {
      return this.first + ' ' + this.last;
    },
    fullNameReversed: function() {
      return this.last + ', ' + this.first;
    }
  };
}

var s = makePerson('Simon', 'Willison');
s.fullName(); // "Simon Willison"
s.fullNameReversed(); // "Willison, Simon"
```

We can take advantage of the this keyword to imporove our function: 

```javascript
function Person(first, last) {
  this.first = first;
  this.last = last;
  this.fullName = function() {
    return this.first + ' ' + this.last;
  };
  this.fullNameReversed = function() {
    return this.last + ', ' + this.first;
  };
}
var s = new Person('Simon', 'Willison');
```

`new` is strongly related to this. It creates a brand new empty object, and then calls the function specified, with this set to that new object. Functions that are designed to be called by new are called constructor functions.

```javascript
function personFullName() {
  return this.first + ' ' + this.last;
}
function personFullNameReversed() {
  return this.last + ', ' + this.first;
}
function Person(first, last) {
  this.first = first;
  this.last = last;
  this.fullName = personFullName;
  this.fullNameReversed = personFullNameReversed;
}
```

That's better: we are creating the method functions only once, and assigning references to them inside the constructor. Can we do any better than that? The answer is yes:

```javascript
function Person(first, last) {
  this.first = first;
  this.last = last;
}
Person.prototype.fullName = function() {
  return this.first + ' ' + this.last;
};
Person.prototype.fullNameReversed = function() {
  return this.last + ', ' + this.first;
};
```

`Person.prototype` is an object shared by all instances of `Person`. 

The first argument to `apply()` is the object that should be treated as '`this`'.

`apply()` has a sister function named [`call`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call), which again lets you set `this` but takes an expanded argument list as opposed to an array.

An important detail of nested functions in JavaScript is that they can access variables in their parent function's scope.

### Closures

```javascript
function makeAdder(a) {
  return function(b) {
    return a + b;
  };
}
var add5 = makeAdder(5);
var add20 = makeAdder(20);
add5(6); // ?
add20(7); // ?
```

This allow you to write many similar functions.



## 课外

### JS_轮询

[思否](https://segmentfault.com/a/1190000014460305)

轮询其实就是一个循环了

```html
<!DOCTYPE HTML>
<html>
<head>
  <title>轮询的坑</title>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
</head>
<body>
</body>
<script type="text/javascript">
  function getData() {
      return new Promise((resolve,reject) => {
          setTimeout(() => {
              resolve({data:666})
          },500)
      })
  }
  // 轮询
  async function start () {
    const { data } = await getData() // 模拟请求
    console.log(data)
    timerId = setTimeout(start, 1000)
  }
  start ()
</script>
</html>
```

想暂停，就是清除 timeId 了，继续就是重新轮询

```html
<!DOCTYPE HTML>
<html>
<head>
  <title>轮询的坑</title>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
</head>
<body>
    <button id="button">暂停</button>
</body>
<script type="text/javascript">
  let timerId = null
  function getData() {
      return new Promise((resolve,reject) => {
          setTimeout(() => {
              resolve({data:666})
          },500)
      })
  }
  // 轮询
  async function start () {
    const { data } = await getData() // 模拟请求
    console.log(data)
    timerId = setTimeout(start, 1000)
  }
  // 暂停
  function stop () {
    clearTimeout(timerId)
  }

  start ()

  const botton = document.querySelector("#button")
  let isPlay = true
  botton.addEventListener("click", function(){
    isPlay = !isPlay
    botton.innerHTML = isPlay ? '暂停' : '播放'
    isPlay ? start() : stop()
  }, false)
</script>
</html>

```

有bug，因为js是单线程，如果快速点击或则异步的运行很久的话，就会有几个轮询的情况出现，所以应该有个标记，但是一个变量的 true false 不满足快速点击的情况

```html
<!DOCTYPE HTML>
<html>
<head>
  <title>轮询的坑</title>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
</head>
<body>
    <button id="button">暂停</button>
</body>
<script type="text/javascript">
  let timerId = 1 // 模拟计时器id，唯一性
  let timerObj = {} // 计时器存储器
  function getData() {
      return new Promise((resolve,reject) => {
          setTimeout(() => {
              resolve({data:666})
          },500)
      })
  }
  // 轮询
  function start () {
    const id = timerId++
    timerObj[id] = true
    async function timerFn () {
      if (!timerObj[id]) return
      const { data } = await getData() // 模拟请求
      console.log(data)
      setTimeout(timerFn, 1000)
    }
    timerFn()
  }
  // 暂停
  function stop () {
    timerObj = {}
  }

  start ()

  const botton = document.querySelector("#button")
  let isPlay = true
  botton.addEventListener("click", function(){
    isPlay = !isPlay
    botton.innerHTML = isPlay ? '暂停' : '播放'
    isPlay ? start() : stop()
  }, false)
</script>
</html>
```



### Promise

[现代JavaScript教程](https://zh.javascript.info/)

Promise 这部分知识挺有用的，仔细看一遍，当时是 Promise 链没有看熟悉，回头重新看看

#### 简介：回调

##### 什么是回调

JavaScript 主机（host）环境提供了许多函数，这些函数允许我们计划异步行为（action），现在看是执行的行为，但是在稍后才完成。eg：setTimeout 函数

例如加载脚本的一个函数：

```js
function loadScript(src) {
    let script = document.createElement('script')
    script.src = src
    document.head.append(script)
}
loadScript('/my/test1.js')
console.log('test1')
```

在执行loadScript 的函数的时候是异步，函数会在异步执行这个函数的时候接着往下走，如果我们想用这个脚本中的函数，我们其实并不知道什么时候这个异步行为能结束，虽然我们不知道这个异步行为什么时候结束，但是我们可以传递一个函数作为参数，只要它一结束就可以执行这个传递的函数，这就是回调

```js
function loadScript(src, callback) {
    let script = document.createElement('script')
    script.src = src
    document.head.append(script)
    script.onload = () => callback()		// 脚本加载完之后执行
}
loadScript('my/test1.js', () => {console.log('append test1.js')})
```

##### 回调中回调

如何在第一个回调后结束第二个回调，自然的想法就是第一个执行的回调中有第二个回调

```js
loadScript('/my/test1.js', () => {
    console.log('append test1.js')
    loadScrit('my/test2.js', () => {console.log('append test2.js')})
})
```

每一个新的行为（action）都在回调的内部，如果只是几个行为还好，如果行为多了，就会出现回调地狱

##### 处理Error

在上述的例子当中，我们都没有考虑出现error的情况，如果脚本加载失败怎么办？回调需要能对此做出反应

```js
function loadScript(src, callback) {
    script = document.createElement('script')
    script.src = src
    script.onload = () => callback(src)
    script.onerror = () => callback(new Error(`append error for ${src}`))
    document.head.append(script)
}
loadScript('my/test1.js', (err, src) => {
    if(err) {
        console.error('error: ', err)
        return
    }
    console.log(`${src} append success.`)
})
```

Error 优先回调（error-first callback）风格，约定是：

- callback的第一个参数是为 error 而保留的，一点出现 error，callback(err) 就会被调用
- 第二个参数及以后的参数用于成功的结果，此时 callback(null, result1, result2, ...) 就会被调用

##### 厄运金字塔

看起来这是一种可行的异步编程方式，的确对于一个或两个嵌套的调用看起来还不错，但是对于一个接一个的多个异步行为，代码会变成这样：

```js
loadScript('1.js', function(error, script) {

  if (error) {
    handleError(error);
  } else {
    // ...
    loadScript('2.js', function(error, script) {
      if (error) {
        handleError(error);
      } else {
        // ...
        loadScript('3.js', function(error, script) {
          if (error) {
            handleError(error);
          } else {
            // ...加载完所有脚本后继续 (*)
          }
        });

      }
    });
  }
});
```

嵌套调用的“金字塔”随着每个异步行为会向右增长，很快它就失控了，解决方法是独立函数，但是这些函数都是一次性使用的，代码看起来就像是撕裂的表格，可读性很差，在阅读的时候需要在各个代码块之间跳转。解决方法最好的办法之一就是 “promise”

#### Promise

e.g: 想象一下，你是一个顶尖的歌手，粉丝没日没夜的问你什么时候发新歌，终于你忍受不了了，然后想了一个解决方案：你发了一个订阅列表，愿意的粉丝在上面填上自己的邮箱，你承诺一首歌出来（或者出了事故不能发歌）的时候立马向这个列表的邮箱发送消息。

Promise 与这个例子的类比：

1. “生产者代码（producing code）”：做事情，但是需要一段时间
2. “消费者代码（consuming code）”：想要在“生产者代码（producing code）”出现的第一时间获得结果
3. Promise 是将“生产者代码”和“消费者代码”连接在一起的的一个特殊的 JavaScript 对象，就好比前面说的订阅列表

Promise 对象的构造器（constructor）语法如下：

```js
let promise = new Promise((resolve, reject) => {
    // executor 生产者代码
})
```

传递给 `new Promise` 的代码被称为 executor，当 new Promise 被创建，executor 会自动运行，它包含最终应产出结果的生产者代码

当 executor 获得了结果，无论或早或晚都可以，它应该调用以下回调之一：

- resolve(value)：如果任务成功完成并带有结果 value
- reject(err)：如果出现了error

Promise 对象具有以下内部属性：

- state：最初是 pending，然后再 resolve 被调用的时候变成 fulfilled，或者在 reject 被调用时变成 rejected
- result：最初是 undefined，然后在 resolve 被调用的时候变为 value，或者在 reject 被调用的时候变成 error

![image-20211213150924510](http://111.229.14.128:9001/wutian//image-20211213150924510.png)

<div align='center'>promise 状态变化</div>

小提示：

- 与最初的 "pending" promise 相反，一个 resolved 或 rejected 都会被称为 "settled"，任何状态的更改都是最终的，也就是只能调用一个 resolve 或者 一个 reject
- 如果什么出了问题，exector 应该调用 reject，这可以使用任何类型的参数来完成，但是建议使用 Error 对象
- state 和 result 是内部的，Promise 对象的 state 和 result 属性都是内部的，我们无法直接访问它们，但我们可以对它们使用 .then / .catch / .finally 方法

##### 消费者：then，catch，finally

promise 对象充当的是 executor 和消费者代码之间的连接，后者将接收结果或者 error，通过使用 .then / .catch / .finally 来为消费者函数注册

踩了一个坑，如果executor中有异常，则promise 必须要有 catch 的错误回调函数，否则会报错，所以说写promise随手写 catch 是一个好习惯

最重要最基础的就是 then， then 的第二个参数可以是执行失败的回调函数：

```js
let promise = new Promise((resolve, reject) => {
    setTimeout(resolve('done'), 1000)
})
promise.then(
	value => console.log('value: ', value),		// 执行
    err => console.error('error: ', err)	// 不执行
)
let promise2 = new Promise((resolve, reject) => {
    setTimeout(() => reject(new Error('not')))
})
promise2.then(
	value => console.log('value: ', value),		// 不执行
    err => console.error('error: ', err)	// 执行
)
```

如果我们只对成功的完成感兴趣，那么可以只为 .then 提供一个函数参数：

```js
let promise = new Promise((resolve, reject) => {
    setTimeout(resolve('done'), 1000)
})
promise.then(console.log)
```

如果我们只对 error 感兴趣，那么我们可以使用 null 作为 .then 的第一个参数或者使用 .catch，这两者是一样的：

```js
let promise = new Promise((resolve, reject) => {
  	  setTimeout(() => reject(new Error('not done')), 1000)
})
promise.catch(console.log)
```

.finally(f) 调用和 .then(f, f)，类似，在某种意义上，f 总是在 promise 被 settled 时执行，finally 是执行清理（cleanup）的很好的程序（handler），它们之间有一些细微的区别：

- finally 处理程序时没有参数，也不知道 promise 是否成功
- finally 处理程序将结果和 error 传递给力下一个处理程序

JavaScript 不具有 sleep() 函数，但是可以尝试使用 setTImeout 和 promise 来共同实现它，也可以使用 Date.now  直接阻塞主线程（但是一般不考虑这个），一个同步一个异步

```js
// 用 promise 模拟 sleep 函数
let delay = (time = 0) => {return new Promise((resolve, reject) => setTimeout(resolve, time))}
console.log('delay time: ', new Date().toLocaleTimeString())
delay(3000).then(() => console.log('runs after 3 seconds first at ', new Date().toLocaleTimeString()))
// 使用 Date.now() 阻塞主线程
let delay2 = (time = 0) => {
	let record = Date.now()
	while(Date.now() - record < time) ;
}
delay2(3000)
console.log('delay2 time: ', new Date().toLocaleTimeString())
console.log('runs after 3 seconds second at ', new Date().toLocaleTimeString())
```

#### Promise 链

回调我们提过，有一系列的异步任务要一个接一个地执行，加载脚本，利用 Promise 链我们可以写出更好的代码，它看起来就像这样：

```js
new Promise((resolve, reject) => {
    setTimeout(() => resolve(1), 1000)
}).then(value =>{
    console.log('value: ', value)
    return 2
}).then(value => {
    console.log('value: ', value)
    return 3
}).then(value => {
    console.log('value: ', value)
    console.log('finished.')
})
```

它的理念是将 result 通过 .then 处理程序（handler）链进行传递

![image-20211215145805750](http://111.229.14.128:9001/wutian/1639551486_image-20211215145805750.png)

<div align='center'>Promise 链</div>

新手常犯的一个经典错误：将多个 .then 添加到一个 promise 上，但这并不是 promise 链（chaining）：

```js
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => resolve(1), 1000);
});

promise.then(function(result) {
  alert(result); // 1
  return result * 2;
});

promise.then(function(result) {
  alert(result); // 1
  return result * 2;
});

promise.then(function(result) {
  alert(result); // 1
  return result * 2;
});
```

![image-20211215150003300](http://111.229.14.128:9001/wutian/1639551603_image-20211215150003300.png)

<div align='center'>Promise 多处理程序情况</div>

同一个 promise 上的所有 .then 获得的结果都相同，实际上我们极少遇到一个 promise 需要多处理程序（handler）的情况

##### 返回Promise

.then（handler）中所使用的处理程序（handler）可以创建并返回一个 Promise，在这种情况下，其它的处理程序（handler）将等待它 settled 后再获得其结果（result）

```js
new Promise((resolve, reject) => {
    setTimeout(() => resolve(1), 1000)
}).then(value => {
    console.log('value: ', value)
    return new Promise((resolve, reject) => {
        setTimeout(() => resolve(99), 1000)
    })
}).then(value => {
    console.log('value: ', value)
    return new Promise((resolve, reject) => {
        setTimeout(() => resolve(8), 1000)
    })
}).then(value => {
    console.log('value: ', value, '\nfinished.')
})
```

返回 promise 让我们能够构建异步行为链

##### 更复杂的示例：fetch

一个好的做法，异步行为应该始终返回一个 promise，这样就可以使得我们计划后续的行为成为可能，即使我们现在不打算对链进行扩展，但我们之后可能会需要

```js
function loadJson(url) {
    return fetch(url)
    .then(response => response.json())
}
function loadGithubUser(name) {
    return fetch(`https://api.github.com/users/${name}`)
    .then(response => response.json())
}
function showAvatar(githubUser) {
    return new Promise((resolve, reject) => {
        let img = document.createElement('img')
        img.src = githubUser.avatar_url
        img.className = 'promise-avatar-examle'
        document.body.append(img)
        
        setTimeout(() => {
            img.remove()
            resolve(githubUser)
        }, 3000)
    })
}
loadJson('/user.json')
.then(user => loadGithubUser(user.name))
.then(showAvatar)
.then(githubUser => alert(`Finished showing ${githubUer.name}`))
```

如果 .then （或 catch/finally 都可以）处理程序（handler）返回一个promise，那么链的其余部分将会等待，知道它的状态变为 settled。当它被 settled 后，其 result（或 error）将被进一步传递下去

![image-20211215160634310](http://111.229.14.128:9001/wutian/1639555594_image-20211215160634310.png)

<div align='center'> Promise chain flow chart</div>

#### 使用 promise 进行错误处理

Promise 链在错误（error）处理中十分强大。当一个 promise 被 reject 时，控制权将移交给最近的 rejection 处理程序。

```js
fetch('/article/promise-chaining/user.json')
  .then(response => response.json())
  .then(user => fetch(`https://api.github.com/users/${user.name}`))
  .then(response => response.json())
  .then(githubUser => new Promise((resolve, reject) => {
    let img = document.createElement('img');
    img.src = githubUser.avatar_url;
    img.className = "promise-avatar-example";
    document.body.append(img);

    setTimeout(() => {
      img.remove();
      resolve(githubUser);
    }, 3000);
  }))
  .catch(error => alert(error.message));
```

.catch 不必是立即的，它可能在一个或多个 .then 之后出现，捕获所有 error 最简单的方法是，将 .catch 附加到链的末尾

Promise 的执行者（executor） 和 promise 的处理程序（handler）周围有一个“隐式”的 try...catch 。如果发生异常，它就会被捕获，并被视为 rejection 进行处理，一下这两段代码是等价的

```js
new Promise((resolve, reject) => {
    throw new Error('error')
}).catch(console.log)
new Promise((resolve, reject) => {
    reject(new Error('error'))
}).catch(console.log)
```

##### 再次抛出（rethrowing）

正如我们所注意到的，链尾端的 .catch 的表现有点像 try...catch，我们可能由许多个 .then 处理程序（handler），然后在尾端使用一个 .catch 处理上面所有的 error

在常规的 try...catch 中，我们可以分析错误（error），如果我们无法处理它，可以将其再次抛出，对于promise来说，这样也是可以的

如果我们在 .catch 中 throw，那么控制权就会被移交到下一个最近的 error 处理程序，如果我们处理该 error 并正常完成，那么它将继续到最近的成功的 .then 处理程序

```js
new Promise((resolve, reject) => {
    reject(new Error('err'))
})
.catch(console.log)
.then(() => console.log('finish'))
```

catch 立马继续 throw，忽略 then 处理程序，传给下一个 catch 处理程序

```js
new Promise((resolve, reject) => {
	reject(new Error('err'))
})
.catch(err => {
	if(err instanceof URIError) {
		// handle it
	} else {
		throw err 
	}
})
.then(() => console.log('no handle'))
.catch(err => console.log('unknown err: ', err))
```

##### 未处理的 rejection

当一个 error 没有被处理，未被 try...catch 捕获，那么脚本就死了，并在控制台留下来一个信息，同理在 promise 中未被处理的rejection 也会发生类似的事

JavaScript 引擎会跟踪此类 rejection，在这种情况下会生成一个全局的 error，在浏览器中我们可以使用 unhandledrejection 事件来捕获这类 error：

```js
window.addEventListener('unhandledrejection', (event) => {
    console.log(event.promise)
    console.log(event.reason)
})
```

通常此类 error 是无法恢复的，所以我们最好的解决方案是将问题告知用户，并且可以将事件报告给服务器，在 Node.js 等非浏览器环境中，有其他用于跟踪未处理的 error 的方法

##### Fetch 错误处理示例

当请求无法发出时，fetch reject 会返回 promise，但是如果远程服务器返回错误响应 404 或者是 500，这些都被认为是合法的响应，但是这应该当作错误来处理，并且无论是 404 或者 500 的响应，它的response都会被当成 json 处理，都会报 SyntaxError ，但是实际上并不是这个错误，错误只是落在了链上，并没有相关细节信息

```js
fetch('test.json')
.then(response => response.json())
.catch(console.log)	// SyntaxError: Unexpected token < in JSON at position 0
```

所以我们可以为 HTTP 错误 创建一个自定义类用来区分 HTTP 错误和其它类型错误，这个新类有一个 constructor，它接受response 对象，并将其保存到 error 中

然后我们将请求和错误处理代码包装进一个函数，它能 fetch url 并将所有状态码不是 200 的视为错误，我们通常需要这样的逻辑

```js
// fetch 请求 github 用户
function printOneGithubUser() {
	let name = prompt('Enter your name', 'name')

	fetch(`https://api.github.com/users/${name}`)
	.then(response => {
		if(response.status == 200) {
			return response.json()
		} else {
			throw new HttpError(response)
		}
	})
	.then(user => console.log('user: ', user))
	.catch(err =>{
		if(err instanceof HttpError && err.response.status != 200) {
			console.log('no such user')
			return printOneGithubUser()
		} else {
			console.log('other error: ', err)
		}
	})
}
printOneGithubUser()
```

如果我们有加载指示（load-indication），finally 就是一个很好的处理程序，在 fetch 完成时停止。try...catch 结构只能抓捕同步的错误。

#### Promise API

在 Promise 类中，有 5 中静态方法

##### Promise.all

假设我们需要并行执行多个 promise，并等待所有 promise 都准备就绪，比如并行下载交给 URL，并等到所有内容都下载完毕后再对它们进行处理。

```js
// promise.all 简单使用
Promise.all([
    new Promise(resolve => setTimeout(() => resolve(1), 1000)),
    new Promise(resolve => setTimeout(() => resolve(2), 2000)),
    new Promise(resolve => setTimeout(() => resolve(3), 3000))])
.then(console.log)
let urls = ['https://api.github.com/users/Ting-xin', 'https://api.github.com/users/Ting-xin']
Promise.all(urls.map(url => fetch(url)))
.then(responses => responses.forEach(response => {
	console.log('response: ', response)
}))
```

一个更加实际的例子是 response.json()，应该有一个 promise.all 专门来处理这个，踩了一个坑，第二个Promise.all 使用 {} 会有问题，暂时我也不知道为啥：

```js
let urls = ['https://api.github.com/users/Ting-xin', 'https://api.github.com/users/Ting-xin']
Promise.all(urls.map(url => fetch(url)))
.then(responses => Promise.all(responses.map(response => response.json())))
.then(resultArr => resultArr.forEach(result => console.log('name: ', result.name)))
```

如果任意一个 promise 被 reject，由 Promise.all 返回的 promise 就会立即 reject，并且带有这个error，其它的 promise 将被忽略，其中一个失败，但是其余的 fetch 操作仍然会继续执行，但是结果还是会被忽略，promise 中没有取消的概念，只能通过 AbortController 来取消

`Promise.all(iterable)` 允许在 `iterable` 中使用 non-promise 的“常规”值

##### Promise.allSettled

在 Promise.all 中，如果任意的 promise reject，整个都会 reject，当我们需要所有的结果，无论结果是啥的时候，就可以用 Promise.allSettled

例如，我们想要获取多个用户的信息，即使其中一个请求失败，我们仍然对其它的感兴趣，就可以使用 Promise.allSettled

```js
let urls = [
  'https://api.github.com/users/iliakan',
  'https://api.github.com/users/remy',
  'https://no-such-url'
];

Promise.allSettled(urls.map(url => fetch(url)))
  .then(results => { // (*)
    results.forEach((result, num) => {
      if (result.status == "fulfilled") {
        alert(`${urls[num]}: ${result.value.status}`);
      }
      if (result.status == "rejected") {
        alert(`${urls[num]}: ${result.reason}`);
      }
    });
  });
```

##### Promise.race 

与 Promise.all 类似，但只等待一个 settled 的 promise 并获取其结果

```js
Promise.race([
  new Promise((resolve, reject) => setTimeout(() => resolve(1), 1000)),
  new Promise((resolve, reject) => setTimeout(() => reject(new Error("Whoops!")), 2000)),
  new Promise((resolve, reject) => setTimeout(() => resolve(3), 3000))
]).then(alert); // 1
```

##### Promise.resolve、Promise.reject

已经过时了，因为 async/await 语法

#### Promisification

Promisification 是指将一个接收回调的函数转换为一个返回promise的函数。由于许多函数和库都是基于回调的，因此在实际开发中经常会需要进行这种转换，因为使用 promise 更加方便，所以将基于回调的函数和库 promisfy 是有意义的

```js
function loadScript(src, callback) {
    let script = document.createElement('script')
}
```







### await 处理 promise 返回 reject 报错的问题

[blog](https://blog.csdn.net/fesfsefgs/article/details/107438395)

promise 用 async，awati 优化，不仅是为了更优雅的书写和阅读，通过优化把异步代码写成同步，也是为了更加符合人正常的思维方式

#### ES5的异常捕获

```js
try{
    throw new Error(3)
} catch(e) {
    console.log(e)
}
```

![image-20211118105217837](http://111.229.14.128:9001/wutian/image-20211118105217837.png)

这是ok的，但是如果try中出现了异步的代码呢

```js
try{
    setTimeout(() => {
        throw new Error(3)
    }, 1000)
} catch(e) {
    console.log(e)
}
```

![image-20211118105226403](http://111.229.14.128:9001/wutian/image-20211118105226403.png)

try-catch 是同步的，setTimeout 是在异步任务队列中的，同步任务执行完再从任务队列中的回调函数拿来执行就出错了

改正

```js
setTimeout(() => {
    try{
        throw new Error(3)
    } catch(e) {
        console.log(e)
    }
}, 0)
```

#### ES6 promise 方式的错误捕获

异步任务完成调用 resolve 的做法：

```js
function fn() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve('ok')
        }, 1000)
    })
}
fn().then(res => {
    console.log(res)
})
```

异步任务失败的，调用reject的捕获异常方法：

```js
function fn() {
    return new Promise((resolve,  reject) => {
        setTimeout(() => {
            reject(3)
        }, 1000)
    })
}
fn().catch(err => console.log(err))
```

#### ES6 通过 async， await优化promise时的异常捕获

问题，通过 async，await的方式去优化promise，只能接收到成功 resolve() 的结果，对与 reject() 的记过是回报错的

```js
// resolve情况，正常
function fn1() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve('ok')
        }, 0)
    })
}
async function test1() {
    let res = await fn1()
    console.log(res)
}
test1()
// reject情况，异常
function fn2() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            reject('error')
        }, 0)
    })
}
async function test2() {
    let res = await fn2()
    console.log(res)
}
test2()
```

![image-20211118111932371](http://111.229.14.128:9001/wutian/image-20211118111932371.png)

#### 解决async/await 的promise 返回错误 reject 的问题及错误捕获

方式一：async/await 已经把异步代码优化成了同步代码，同步代码可以通过 try, catch捕获异常

```js
function fn() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            reject('error')
        }, 0)
    })
}
async function test() {
    try{
        let test = await fn()
    } catch (e) { console.log(e) }
}
test()
```

方式2：通过返回一个pending 状态的结果，中断promise链的方式处理错误

```js
function fn() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            // reject('error')
            return new Promise(() => {})	// 中断 promise 链
        })
    })
}
async function test() {
    let res = await fn()
    console.log(res)
}
test()
```

方法2不会出现多余且不好看的代码，但是根本不能拿到错误了呀



### 正则表达式

#### 为什么要学习字符串 处理

[为什么学习字符串处理](http://yphuang.github.io/blog/2016/03/15/regular-expression-and-strings-processing-in-R/)

传统的统计学教育几乎没有告诉过我们如何进行文本的统计建模分析。然而我们日常生活中接触到的大部分数据都是以文本的形式存在，文本分析与挖掘在业界有着非常广泛的应用。

由于文本数据大多数属于非结构化数据，要想对文本数据进行传统的统计模型分析，必须要经过层层的数据清洗和整理，正则表达式就用来干这个的。

和建立酷炫的模型比起来，数据的清洗与整理似乎是一种低档次的工作。如果吧建模比作高级厨师的工作，那么数据清洗就类似于切菜洗碗打扫卫生的活儿。然而想要成为资深的“数据玩家”，这种看似低档次的工作是必不可少的，并且这种工作极有可能占据整个建模流程的 80% 的时间。如果我们能掌握高效的数据清洗工作，那么我们将拥有更多的时间来选择模型和参数选择。

#### 教程

以前一直都是草草了事，这次一定要把这个好好看懂

[教程](https://zh.javascript.info/regexp-introduction)

#### 模式（Paterns）和修饰符（flags）

正则表达式是搜索和替换字符串的一种强大方式，在JavaScript中，正则表达式通过内置的“RegExp”类的对象实现，并与字符串集成

创建正则表达式有两种语法

```js
// 较长的语法
regexp = new RegExp("pattern", "flags")
// 较短的语法
regexp = /pattern/;		// 没有修饰符
regexp = /pattern/gmi;		// 伴随修饰符 g m i 
```

如果要在字符串种进行搜索，可以使用search 方法

```js
let str = 'I love JavaScript'
let regexp = /love/
alert(str.search(regexp))		// 2
// 上面这段代码等价于
let str = 'I love JavaScript'
let substr = 'love'
alert(str.search(substr))		// 2
```

`str.search` 方法会查找模式 `/love/` ，然后返回匹配项在字符串中的位置

通常我们使用的都是简短语法 `/../` 。但是它不接受任何变量的插入，`new RegExp` 允许从字符串中动态的构造模式

```js
let search = prompt('What you want to search?', 'love')
let regexp = new RegExp(search)
alert('I love JavaScript'.search(regexp))
```

正则表达式的修饰符可能会影响搜索结果，在JavaScript中有5个修饰符

- i	不区分大小写
- g   查找所有的匹配项，而不只是第一个
- m   多行模式
- u    开启完整的 unicode 支持，能修正对于代理对的处理
- y    粘滞模式

```js
alert('love'.search(/LOVE/))	// -1，没找到
alert('love'.search(/LOVE/i))	// 0
```

如果不使用我们后面将会学到的修饰符和特殊标志，正则表达式的搜索就等同于字符串查找了

#### 字符类

字符类（character classes）是一个特殊的符号，匹配特定集合中的任何符号

`str.match(regexp)` 方法在字符串中 `str` 中找到匹配 `regexp` 的字符

```js
// 考虑一个实际的例子，只保留字符中的所有数字
console.log('+1,3,5,,,,'.match(/\d/g).join(''));		// 135
```

常用的字符类有：

- \d    d 来自 digit，从 0 到 9 的字符
- \s    s 来自 space，空格符号，包括空格，制表符\t，换行符\n，及其他稀有字符，如 \v，\f 和 \r
- \w   w 来自 word，拉丁字符或数字或下划线

正则表达式可能同时包含常规符号和字符类，匹配项（每个正则表达式字符类都有对应的结果字符）

![image-20211119222056851](http://111.229.14.128:9001/wutian/image-20211119222056851.png)

对于每个字符类，都有一个反向类，用相同的字符表示，但是要大写，反向代表它与所有其它字符匹配

- \D    非数字，匹配除 \d 以外的所有字符
- \S    非空格字符，匹配除 \s 以外的所有字符
- \W    非单字字符，匹配除 \w 以外的所有字符

```js
// 考虑一个实际的例子，只保留字符中的所有数字
console.log('+1,3,5,,,,'.match(/\d/g).join(''));		// 135
// 另一种快捷的方式是将所有的非数字字符找到，并直接删除
console.log('+1,3,5,,,'.replace(/\D/g, ''));		// 135
```

点（.）是一种特殊的字符类，匹配任何字符，点表示任何字符，而不是缺少字符，必须有一个与之匹配的字符

默认情况下，点与换行符 \n 不匹配，但在很多情况下，当我们希望用点来表示任何字符（包括换行符\n时），就要用到标识符 s

```js
console.log('a\nb'.match(/a.b/))		// null
console.log('a\nb'.match(/a.b/s))		// a\nb 的 object
```

但是 \s 的方式不是所有的浏览器都适用，有一种替代方法在任何地方都适用，可以使用 `[\s\S]` 之类的正则表达式来匹配任何字符

```js
alert("a\nb".match(/a[\s\S]b)/)		// a\nb
```

在正则表达式中，所有字符都很重要，空格也很重要

#### Unicode：修饰符 'u' 和 class  \n{...}

很久以前，当JavaScript被发明出来的时候，Unicode的编码要更加简单：当时并没有 4 个字节长的字符，所以一部分语言特性在现在仍旧无法队Unicode进行正确的处理

与字符串有所不同的是，正则表达式有一个修饰符 u 被用以解决这类问题，但一个正则表达式使用了这个修饰符后，4个字节长的字符将被正确的处理

我们可以查找具有某种属性的字符，写作 `\p{...}` ，为了顺利使用 `\p{...}`，一个正则表达式必须使用修饰符u

`\p{Letter}` 表示任何语言中的一个字母，可以使用 `\p{L}` 代替

```js
let str = "A ბ ㄱ"		// 英语，格鲁吉亚语，韩语字母
console.log(str.match(/\p{L}/gu))		// ['A', 'ბ', 'ㄱ']
console.log(str.match(/\p{L}/g))		// null
```

一个 16 进制数字可以表示为 `\p{Hex_Digit}`：

```javascript
let regexp = /x\p{Hex_Digit}\p{Hex_Digit}/u;

alert("number: xAF".match(regexp)); // xAF
```

有一个 unicode 属性 `Script` （一个书写系统），这个属性可以有一个值：`Cyrillic`，`Greek`，`Arabic`，`Han` （中文）等等

```js
let regexp = /\p{sc=Han}/gu;		// 返回中文
console.log(`Hello Привет 你好 123_456`.match(regexp).join(''))		// 你好
```

接着是一个数字”的价格文本：

```javascript
let regexp = /\p{Sc}\d/gu;

let  str = `Prices: $2, €1, ¥9`;

alert( str.match(regexp) ); // $2,€1,¥9
```

修饰符 `u` 在正则表达式中提供对 Unicode 的支持。

这意味着两件事：

1. 4 个字节长的字符被以正确的方式处理：被看成单个的字符，而不是 2 个 2 字节长的字符。
2. Unicode 属性可以被用于查找中 `\p{…}`。

有了 unicode 属性我们可以查找给定语言中的词，特殊字符（引用，货币）等等。

#### 锚点（anchors）： 字符串开始^和末尾$

插入符号 ^匹配文本开头，而美元符号 $ 匹配文本末尾

测试完全匹配，这两个锚点放在一起常常被用于测试一个字符串是否完全匹配一个模式

```js
// 测试字符是否属于 12:34 的格式
let goodInput = '12:34'
let errInput = '12:345'
let regexp = /^\d\d:\d\d$/
console.log(regexp.test(goodInput))		// true 
console.log(regexp.test(errInput))		// false
```

锚点 `^` 和 `$` 属于测试。它们的宽度为零。

换句话来说，它们并不匹配一个具体的字符，而是让正则引擎测试所表示的条件（文本开头/文本末尾）

#### 多行模式Flag "m"

通过flag /.../m 可以开启多行模式，这仅仅影响 ^ 和 $ 锚符的行为，在多行模式下，它们不仅仅匹配文本的开始和结束，还匹配每一行的开始和结束

```js
let str = `1st place: Winnie
2nd place: Piglet
33rd place: Eeyore`;
console.log(str.match(/^\d+/gm))		// ['1', '2', '33']
```

#### 词边界 \b

词边界 \b 是一种检查，就像 ^ 和 $ 一样，match 返回的应该是一个类数组

```js
console.log('hello, java'.match(/\bjava\b/))		// ['java']
console.log('hello, javascript'.match(/\bjava\b/))		// null
```

\b 既可以用于单词，也可以用于数字

例如，模式 `\b\d\d\b` 查找独立的两位数。换句话说，它查找的是两位数，其周围是与 `\w` 不同的字符，例如空格或标点符号（或文本开头/结尾）。

#### 转义，特殊字符

所有特殊字符的列表：[ \ ^ $ . | ? * + ( )

如果要把特殊字符作为常规字符来使用，只需要在它的前面加一个反斜杠

斜杠符号 / 并不是一个特殊符号，他被用于在 JavaScript 中开启和关闭正则匹配

```js
console.log('/'.match(/\//))		// '/'
console.log('/'.match(new RegExp('/')))		// '/'
```

在字符串中的反斜杠表示转义或者类似 `\n` 这种只能在字符串中使用的特殊字符。这个引用会“消费”并且解释这些字符，所以调用 new RegExp 会获得一个灭有反斜杠的字符串，如果要修复这个，需要双斜杠

- 要在字面（意义）上搜索特殊字符 `[ \ ^ $ . | ? * + ( )`，我们需要在它们前面加上反斜杠 `\`（“转义它们”）。
- 如果我们在 `/.../` 内部（但不在 `new RegExp` 内部），还需要转义 `/`。
- 传递一个字符串（参数）给 `new RegExp` 时，我们需要双倍反斜杠 `\\`，因为字符串引号会消费其中的一个。

#### 集合和范围 [...]

在方括号 [...] 中的几个字符或者字符类意味着”搜索给定的字符中的任意一个"

 ```js
console.log('Mop top'.match(/[tm]op/gi))	// ['Mop', 'top']
 ```

请注意尽管在集合中有多个字符，但它们只会对应其中一个

方括号可以包含字符范围，比如说 [a-z]， [0-5]

[0-8A-F] 表示两个范围：它搜索一个字符，满足其中一个条件，如果我们还想查找小写字母，则可以添加范围 `a-f`：`[0-9A-Fa-f]`。或添加标志 `i`。

由于字符类 `\w` 是简写的 `[a-zA-Z0-9_]`，因此无法找到中文象形文字

除了普通的范围匹配，还有类似 `[^...]` 的排除范围匹配

通过在匹配查询的开头添加插入符号 ^ 来表示，它会匹配所有除了给定的字符之外的任意字符

`[^aeyo]` 匹配任何除了 a, e, y, o 之外的字符

在 [...] 中不转义， `[-().^+]` 会查找 `-().^+` 的其中任意一个字符：

范围和标志 'u' 

```js
alert( '𝒳'.match(/[𝒳𝒴]/u) ); // 𝒳
```

#### 量词 +, *, ?, {n}

数量 {n}  \d{5} 表示 5 位的数字

某个范围的位数： {3, 5}

大多数量词都可以有缩写

- `+` 代表一个或者多个，相当于 {1,}
- `?` 代表一个或者0个，相当于{0, 1}
- `*` 代表零个或者多个，相当于 {0,}

正则表达式越精确，它就越长且越复杂

#### 贪婪量词和惰性量词

量词看上去很简单，但实际上它可能会很棘手，如果我们打算寻找比 /\d+/ 更加复杂的工作，就需要理解搜索工作是如何进行的

eg：有一个文本，我们需要用书名号： 《...》 来代理所有的 引号 “...”，第一想法是不是采用 `/".+"/g` 的正则去寻找，但是实际情况并不是这样的

```js
str = '"str" is "test" str'
console.log(str.match(/".+"/g))		// ['"str" is "test"'], 返回了一整个，而不是两个
```

- 贪婪搜索

为了查找到一个匹配项，正则表达式采用了以下算法：对于字符串中的每一个字符，用这个模式来匹配此字符，若无匹配，则移至下一个字符（回溯），在贪婪模式下（默认情况），量词都会尽可能的重复多次

- 懒惰模式

懒惰模式中的量词与贪婪模式中的是相反的，我们通过在量词后添加一个问好 ？ 来启用它，？它本身就是一个量词（0 或 1）

上例子中，正则表达式 `/".+?"/g` 就会如预期工作了

```js
str = '"str" is "test" str'
console.log(str.match(/".+"/g))		// ['"str"', '"test"'], 返回了两个
```

懒惰模式只能通过带 ？ 的量词启用

```js
alert( "123 456".match(/\d+ \d+?/g) ); // 123 4
```

总结：量词有两种工作模式，贪婪模式和懒惰模式

#### 捕获组

模式的一部分可以用括号括起来，这称为捕获组，它有两个影响：

- 它允许将匹配的一部分作为结果数组中的单独项
- 如果我们将量词放在括号后，则它将括号视为一个整体

eg： gogogo

```js
'Gogogo now'.match(/(go)+/i)		// 'Gogogo'
```

eg：域名

```js
let regexp = /(\w+\.)+\w+/g
"site.com my.site.com".match(regexp)		//site.com my.site.com
```

eg: email  名称可以是任何单词，可以使用连字符和点，在正则表达式是 `[-.\w]+`

```js
let regexp = /[-.\w]+@([\w-]+\.)+[\w-]+/g
"my@mail.com @ his@site.com.uk".match(regexp)		// ['my@mail.com', 'his@site.com']
```

eg： 匹配括号中的内容

括号可以嵌套，在这种情况下，编号从左往右

```js
let str = '<span class="my">';
let regexp = /<(([a-z]+)\s*([^>]*))>/;
let result = str.match(regexp);
alert(result[0]); // <span class="my">
alert(result[1]); // span class="my"
alert(result[2]); // span
alert(result[3]); // class="my"
```

可选组，即使组是可选的并且在匹配项中不存在，也存在相应的 result 数组项，并且等于 undefined

```js
let match = 'a'.match(/a(z)?(c)?/);

alert( match.length ); // 3
alert( match[0] ); // a（完全匹配）
alert( match[1] ); // undefined
alert( match[2] ); // undefined
```

搜索所有具有组的匹配项：matchAll，由 `matchAll` 所返回的每个匹配，其格式与不带标志 `g` 的 `match` 所返回的格式相同：它是一个具有额外的 `index`（字符串中的匹配索引）属性和 `input`（源字符串）的数组

命名组：用数字记录组很困难。对于简单模式，它是可行的，但对于更复杂的模式，计算括号很不方便。我们有一个更好的选择：给括号起个名字。

```js
let dateRegexp = /(?<year>[0-9]{4})-(?<month>[0-9]{2})-(?<day>[0-9]{2})/;
let str = "2019-04-30";

let groups = str.match(dateRegexp).groups;

alert(groups.year); // 2019
alert(groups.month); // 04
alert(groups.day); // 30
```

替换捕获组：方法 `str.replace(regexp, replacement)` 用 `replacement` 替换 `str` 中匹配 `regexp` 的所有捕获组。这使用 `$n` 来完成，其中 `n` 是组号

#### 模式中的反向引用： \N 和  \k <name>

我们可以将两种引号放在方括号中：`['"](.*?)['"]`，但它会找到带有混合引号的字符串，例如 `"...'` 和 `'..."`。当一种引号出现在另一种引号内，比如在字符串 `"She's the one!"` 中时，便会导致不正确的匹配：

```js
let str = `He said: "She's the one!".`;

let regexp = /['"](.*?)['"]/g;

// 不是我们想要的结果
alert( str.match(regexp) ); // "She'
```

如我们所见，该模式找到了一个开头的引号 `"`，然后文本被匹配，直到另一个引号 `'`，该匹配结束。

为了确保模式查找的结束引号与开始的引号完全相同，我们可以将其包装到捕获组中并对其进行反向引用：`(['"])(.*?)\1`。

这是正确的代码：

```js
let str = `He said: "She's the one!".`;

let regexp = /(['"])(.*?)\1/g;

alert( str.match(regexp) ); // "She's the one!"
```

#### 选择 |

- `gr(a|e)y` 严格等同 `gr[ae]y`。
- `gra|ey` 匹配 “gra” or “ey”。

时间正则表达式

```js
// 错误版本
let reg = /[01]\d|2[0-3]:[0-5]\d/g;
alert("12".match(reg)); // 12 (matched [01]\d)
// 正确版本
let reg = /([01]\d|2[0-3]):[0-5]\d/g;
alert("00:00 10:10 23:59 25:99 1:2".match(reg)); // 00:00,10:10,23:59
```

正则表达式引擎查找选择模式的时是挨个查找的。意思是：它先匹配是否存在 `Java`，否则 —— 接着匹配 `JavaScript` 及其后的字符串。

1. 变更匹配顺序，长的字符串优先匹配：`JavaScript|Java|C\+\+|C|PHP`。
2. 合并相同前缀：`Java(Script)?|C(\+\+)?|PHP`。

运行代码如下：

```javascript
let reg = /Java(Script)?|C(\+\+)?|PHP/g;
let str = "Java, JavaScript, PHP, C, C++";
alert( str.match(reg) ); // Java,JavaScript,PHP,C,C++
```

#### 前瞻断言与后瞻断言

有时候我们需要匹配后面跟着特定模式的一段模式。比如，我们要从 `1 turkey costs 30€` 这段字符中匹配价格数值。

我们需要获取 `€` 符号前面的数值（假设价格是整数）。

那就是前瞻断言要做的事情。

- 前瞻断言

语法为：`x(?=y)`，它表示 “匹配 `x`, 仅在后面是 `y` 的情况"”

语法为：`x(?!y)`，意思是 “查找 `x`, 但是仅在不被 `y` 跟随的情况下匹配成功”。

- 后瞻断言

前瞻断言允许添加一个“后面要跟着什么”的条件判断。

后瞻断言也是类似的，只不过它是在相反的方向上进行条件判断。也就是说，它只允许匹配前面有特定字符串的模式。

语法为:

- 后瞻肯定断言：`(?<=y)x`, 匹配 `x`, 仅在前面是 `y` 的情况。
- 后瞻否定断言：`(?<!y)x`, 匹配 `x`, 仅在前面不是 `y` 的情况。

#### 灾难性回溯

有些正则表达式看上去很简单，但是执行起来耗时非常非常非常长，甚至会导致 JavaScript 引擎「挂起」。

开发者们很容易一不小心就写出这类正则表达式，所以我们迟早会面对这种意外问题。

典型的症状就是 —— 一个正则表达式有时能正常工作，但对于某些特定的字符串就会消耗 100% 的 CPU 算力，出现“挂起”现象。

在这种情况下，Web 浏览器会建议杀死脚本并重新载入页面。这显然不是我们愿意看到的。

在服务器端 JavaScript 中，在使用这种正则表达式处理用户数据时可能会引发程序漏洞。





### 搭建一个图床

[利用Minio搭建私有图床](https://www.naeco.top/2020/08/11/private-oss-for-image/)

#### 简介

图片存储服务，也叫图床，是博客需要具备的基础服务。过往我们采用的是公共免费的图床，但是这些服务有一些缺点：

- 存储容量受限
- 稳定性，可能突然对方不提供服务了
- 安全性，没在自己这始终不放心

因此，大家开始自建私有图床，参考的博客的采用的存储服务是[Minio](https://min.io/)

#### AWS S3

[一文读懂 AWS S3](http://www.thinkingincrowd.me/2020/03/10/aws-s3/)

S3 的全名是 simple storage service，简单的存储服务。它的作用有：

- 提供了同一的接口 REST/SOAP 来统一访问任何数据
- 对 S3 来说，存在里面的就是对象名和数据
- 不限量，单个文件最高可达 5 TB
- 高速，每个 bucket 下每秒可达 3500 put/copy/post/delete 或 5500 get/head 请求
- 具备版本、权限控制能力
- 具备数据生命周期管理能力

每个对象最多指定 10 个标签（其实数据标签真的很有用）







#### 防盗链

[传说中防盗链的爱恨情仇](https://juejin.cn/post/6844903825124360200)

盗链是指自己的页面上展示一些并不在自己服务器上的内容。通常的做法是通过技术手段获得它人服务器上的资源地址，绕过别人的资源展示页面，直接在自己的页面上向最终用户提供此内容，比较常见的是一些小站盗用大站的资源

防盗链的原理：在HTTP协议中，如果从一个页面调到另一个页面，header字段里面会带个Referer，图片服务器通过检测Referer是否来自规定的域名来进行防盗链





### 前端常用功能集合

[掘金](https://juejin.cn/post/6844904066418491406#heading-1)

这位大佬自己动手做了前端的一些无依赖，精简高效的东西，然后按需应用到实际项目中，小白也想要一个一个的试试，就当练手了

#### http请求

前端必备技能，也是使用最多的技能。

##### 基础知识

[XMLHttpRequest](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest)  (XHR) 对象用于与服务器交互，通过 XMLHttpRequest 可以在不刷新页面的情况下请求特定 URL，获取数据。这允许页面在不影响用户操作的情况下更行页面的局部内容，XMLHttpRequest 在 AJAX 编程中被大量使用。 [AJAX](https://developer.mozilla.org/zh-CN/docs/Glossary/AJAX)(Asynchronous JavaScript And XML) 是一种使用 XMLHttpRequest 技术构建更复杂，动态的网页的编程实践。AJAX 允许只更新一个 HTML 页面的部分 DOM，而无须重新加载整个页面。AJAX 还允许异步工作，这意味着当网页的一部分正试图重新加载时，代码可以继续运行

##### AJAX

1. 发送请求

为了使用 JavaScript 向服务器发送一个 http 请求，所以需要一个包含必要函数功能的对象实例，这就是为什么会有 XMLHttpRequest 的原因

创建一个 XMLHttpRequest 实例：

```js
// Old compatibility code, no longer needed.
if (window.XMLHttpRequest) { // Mozilla, Safari, IE7+...
	httpRequest = new XMLHttpRequest()
} else {
    httpRequest = new ActiveXObject("Microsoft.XMLHTTP")
}
```

发送一个请求后会收到响应，在这个阶段，需要告诉 XMLHttp 请求对象应该由哪一个 JavaScript 函数处理响应

```js
httpRequest.onreadystatechange = nameofTheFunction;		// 这个只是应用赋值，为了真正的调用，应该用个匿名函数
httpRequest.onreadystatechange = () => {
    // process the server response here.
}
```

接下来，通过调用 HTTP 请求对象的 open() 和 send() 方法：

```js
httpRequest.open('GET', 'http://www.example.org/some.file', true)
httpRequest.send()
```

- open 的第一个参数是 HTTP 请求，由 GET, POST, HEAD 以及服务器支持的其它方法，这些方法都是大写

- 第二个参数是你要发送的URL。由于安全原因，默认不能调用第三方 URL 域名。确保使用正确的域名，否则调用时会有"permission denied" 错误提示
- 第三个参数是设置请求是否异步，如果是 true 即代表是异步

send() 方法的参数可以是任何你想发送给服务器的内容，如果是 POST 请求的话，发送表单数据应该用服务器可以解析的格式

2. 处理服务器响应

在发送请求时，我们提供的JavaScript函数负责处理响应，这个函数首先检查请求的状态，如果状态值是 XMLHttpRequest.DONE(对应的值是4)，意味着服务器响应收到了并且是没有问题的，然后可以继续执行

```js
httpRequest.onreadystatechange = () => {
    if(httpRequest.readyState = XMLHttpRequest.DONE) {
        // everythin is good, the response is received.
    } else {
        // not ready yet.
    }
}
```

接下来通过检查 200 OK 判断AJAX有没有成功

```js
if (httpRequest.status === 200) {
    // perfect!
} else {
    
}
```

3. 一个简单的例子

```js
<button id='test' type='button'>make a request</button>
<script type="text/javascript">
(function() {
	var httpRequest;
	document.getElementById('test').addEventListener('click', makeRequest)

	function makeRequest() {
		httpRequest = new XMLHttpRequest()
		if(!httpRequest) {
			alert('Browser don\'t support XMLHttpRequest.')
			return
		}
		httpRequest.onreadystatechange = handleRequest
		httpRequest.open('GET', 'www.test.com/test.html')
		httpRequest.send()
	}

	function handleRequest() {
		if(httpRequest.readystate = XMLHttpRequest.DONE) {
			if(httpRequest.status = 200) {
				alert(httpRequest.responseText)
			} else {
				alert('Something wrong.')
			}
		}
	}
})
</script>
```

在这个例子中：

- 用户点击“make a request” 按钮
- 事件处理调用 makeRequest() 函数
- 请求通过 onreadystatechange 传给 handleRequest 函数
- handleRequest 函数首先进行判断，然后处理

提示：

- 如果你向一个代码片段发送请求，将返回 XML，而不是静态 XML 文件，在 IE 浏览器上则必须要设置响应头才能正常工作。如果不设置响应头 Content-Type: application/xml，IE 浏览器会在访问你的 XML 元素的时候抛出 “Object Expected” 错误
- 如果不设置响应头 Cache-Control: no-cache，那么浏览器会把响应缓存下来而且再也无法重新提交请求。还有一种做法是添加一个总是不同的 GET 参数，比如时间戳或者随机数
- 如果变量 httpRequest 在全局范围内使用，那么它会在 makeRequest() 函数中被相互覆盖，从而导致资源竞争，为了避免这个情况，请求包含 AJAX 函数的闭包中声明 httpRequest 变量

在通信错误的事件中（例如服务器宕机），在访问响应状态 onreadystatechange 方法中会掏出一个例外 exception，为了缓和这种情况，则可以使用 `try...catch...`

```js
function handleRequest() {
    try{
        if(httpRequest.onreadystate === XMLHttpRequest.DONE) {
            if(httpRequest.status === 200) {
                // do something with httpRequest.responseText
            } else {
                alert('something wrong')
            }
        }
    } catch(e) {
        alert('Caught Exception: ' + e.description)
    }
}
```

XMLHttpRequest.readyState:

- 0	未初始化 或 请求还未初始化
- 1    正在加载 或 已建立服务器链接
- 2    加载成功 或 请求已接收
- 3    交互 或 正在处理请求
- 4    完成 或 请求已完成并且响应已准备好

XMLHttpRequest.status:

- 100-199    信息响应
- 200-299    成功响应
- 300-399    重定向消息
- 400-499    客户端错误响应
- 500-599    服务端错误响应

4. 处理 XML 响应

在上一个例子中，在收到 HTTP 请求的响应后我们会使用对象的 responseText 属性，包含 test.html 文件的内容。现在我们试试 responseXML 属性

有效的 xml 文档：

```xml
<?xml version='1.0'>
<root>test</root>
```

将 `test.html` 改为 `test.xml` ，然后再把 `httpRequest.responseText` 改为：

```js
let xmlDoc = httpRequest.responseXML;
let rootNode = xmlDoc.getElementByTagName('root').item(0)
alert(rootNode.firstChild.data)
```

##### Fetch API

Fetch API 提供了一个获取资源的接口（包括跨域请求）。任何使用过 XMLHttpRequest 的人都能轻松上手，新的 API 提供了更强大和灵活的功能集。Fetch 提供了对 [`Request`](https://developer.mozilla.org/zh-CN/docs/Web/API/Request) 和 [`Response`](https://developer.mozilla.org/zh-CN/docs/Web/API/Response) （以及其他与网络请求有关的）对象的通用定义。使之今后可以被使用到更多的应用场景中：无论是service worker、cache api、又或者是其它处理请求和响应的方式、甚至是任何一种需要再自己程序中生成响应的方式

fetch() 必须接受一个参数：资源的路径。无论请求成功与否，它都返回一个 Promise 对象，resolve 对应请求的 Response

简单使用：

```js
fetch('http://example.com/movies.json').then(response => response.json()).then(data => console.log(data))
```

fetch() 接收第二个可选参数，一个可以控制不同配置的 init 对象：

```js
// example of post method implementation by fetch api
async function postData(url = '', data = {}) {
	const response = await fetch(url, {
        // default option are marked with *
		method: 'POST',	// *GET, POST, PUT, DELETE, etc.
		mode: 'cors',	// no-cors, *cors, same-origin
		cache： 'no-cache',    // *default, no-cache, reload, force-cache, only-if-cached
		credentials: 'same-origin',		// include, *same-cache, omit
		headers: {
			'Content-Type': 'application/json'
        	//'Content-Type': 'application/x-www-form-urlencoded'
		},
		redirect: 'follow', //manual, *follow, error
		referrerPolicy: 'no-referrer',	// no-referrer, *no-refer-when-downgrade, etc.
		body: JSON.stringfy(data)	// body data type must match "Content-Type" header
	})
	return response.json()	// parses JSON response into native JavaScript objects
}
```

使用 fetch() 发送 json 数据：

```js
// Post json data by fetch api
fetch(url, {
	method: 'Post',
	headers: {
		'Content-Type': 'application/json'
	},
	body: JSON.stringfy(data)
})
.then(res => res.json())
.then(data => console.log('success: ',data))
.catch((e) => {
	console.error('error: ', e)
})
```

通过 HTML `<input type='file' />` 元素，[`FormData()`](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData/FormData) 和 [`fetch()`](https://developer.mozilla.org/zh-CN/docs/Web/API/fetch) 上传文件：

```js
// Put file by fetch api
const formData = new FormData()
const fileField = document.querySelector('input[type="file"]')
formData.append('name', 'test')
formData.append('file', fileField.files[0])
fetch(url, {
	method: 'PUT',
	body: formData
})
.then(res => res.json())
.then(data => console.log('success: ', data))
.catch((e) => {
	console.error('error: ', e)
})

```

通过 HTML `<input type='file' multiple />` 元素，[`FormData()`](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData/FormData) 和 [`fetch()`](https://developer.mozilla.org/zh-CN/docs/Web/API/fetch) 上传文件：

```js
// Post files by fetch api
const filesField = document.querySelector('input[type="file"][multiple]')
const formData2 = new FormData()
formData2.append('name', 'multiple files')
for(let i = 0; i < filesField.files.length; ++i) {
	formData2.append(`file${i}`, filesField.files[i])
}
fetch(url, {
	method: 'POST',
	body: formData2
})
.then(res => response.json())
.then(data => console.log('success: ', data))
.catch(e => console.error('error: ', e))
```

从响应中读取的分块并不是按行分割的，并且是 Uint8Array 数组类型（不是字符串类型）。如果通过 fetch() 获取一个文本文件并逐行处理它，那需要自行处理这些复杂的情况：

```js
// Processing text files line by line
async function* makeTextFileLineIterator(fileURL) {
	const utf8Decoder = new TextDecoder('utf-8')
	const response = new fetch(fileURL)
	const reader = response.body.getReader()
	let { value: chunk, done: readerDone} = await reader.read()
	chunk = chunk ? utf8Decoder.decode(chunk): ''

	const re = /\n|\r|\r\n/gm;
	let startIndex = 0
	let result
	for(;;) {
		let result = re.exec(chunk)
		if(!result) {
			if(readerDone) {
				break
			}
			let remainder = chunk.substr(startIndex)
			({ vlaue: chunk, done: readerDone} = await reader.read());
			chunk = remainder + (chunk ? utf8Decoder.decode(chunk): '')
			startIndex = re.lastIndex = 0
			continue
		}
		yield chunk.substring(startIndex, result.index)
		startIndex = re.lastIndex
	}
	if(startIndex < chunk.length) {
		yield chunk.substr(startIndex)
	}
}
async function run() {
	for await(let line of makeTextFileLineIterator(urlOfFile)) {
		processingLine(line)
	}
}
run()
```

如果遇到网络故障或者服务端的 CORS 配置错误时， fetch() promise 将会 reject，带上一个 TypeError 对象

```js
// Detecting the success of a request
fetch('test.jpg')
.then(response => {
	if(!response.ok) {
		throw new Error("Network response was not ok")
	}
	return response.blob()
})
.then(myBlob => {
	myImage.src = URL.createObjectURL(myBlob)
})
.catch(err => console.error('error: ', error))
```

除了传给 fetch() 一个资源的地址，还可以通过 Request() 构造函数来创建一个 request 对象，然后在作为参数传给 fetch()：

```js
// Customizing the request object
const myHeader = new Headers()
const request = new Request('test.jpg', {
	method: 'GET',
	headers: myHeader,
	mode: 'cors',
	cache: 'default'
})
fetch(request)
.then(response => response.blob())
.then(myBlob => myImage.src = URL.createObjectURL(myBlob))
```

 

**提示：**

当请求使用 `credentials: 'include'` 时，响应的 `Access-Control-Allow-Origin` 不能使用通配符 `*`，在这种情况下， `Access-Control-Allow-Origin` 必须是当前请求的源

function* 是定义一个生成器函数（generator function），它返回一个 generator 对象，generator 函数在执行时能暂停，后面又能从暂停出继续执行。调用一个 generator 函数并不会马上执行它里面的语句，而是返回一个这个生成器的迭代器（iterator）对象。当这个迭代器的 next() 方法被首次（后续）调用时，其内的语句会执行到一个出现 yield 的位置为止，yield 后紧跟迭代器要返回的值，如果使用的是 yield* 则表示执行权移交给另一个生成器函数（当前生成器暂停执行）。当在生成器函数中显示 return 时，会导致生成器立即变成完成状态

Request()  构造器创建一个新的 Request 对象，语法： `let myRequest = new Request(input[, init])` 

不管是请求还是响应都能够包含 body 对象，body 也可以是以下任意类型的实例：

- ArrayBuffer
- ArrayBufferView(Uint8Array 等)
- Blob/File
- string
- URLSearchParams
- FormData

Body 类定义了以下方法以获取 body 内容，这些方法都会返回一个别解析后的 Promise 对象和数据，相比于 XHR，这些方法让非文本化数据的使用更加简单：

- Request.arrayBuffer()
- Request.blob()
- Request.formData()
- Request.json()
- Request.text()





### JSDoc

[使用JSDoc提高代码的可读性](https://juejin.cn/post/6844903828123320334)

讲的很好，脑容量要用到改用的地方，写出别人看不懂的代码不是什么能力，写出所有人能看懂的代码才是真的厉害



### RESTful

#### 理解 RESTful 架构

[理解RESTful架构](https://www.ruanyifeng.com/blog/2011/09/restful.html)

越来越多的人意识到，网络即软件，而且是一种新型的软件，这种“互联网软件”采用客户端/服务端模式，建立在分布式体系上，通过互联网通信，具有高延时（hige latency），高并发等特点。网站开发，完全可以采用软件开发的模式，但是传统上，软件和网络是两个不同的领域，软件开发主要针对单机环境，网络则主要研究系统之间的通信。互联网的兴起，这两个领域开始融合，现在我们必须考虑，如何开发在互联网环境中使用的软件。

RESTful 架构是目前流行的一种互联网软件架构，它结构清晰、符合标准、易于理解、扩展方便，被多个网站所采用

#### 起源

Fielding 在 2000 年的博士论文中提出，作者目的是在符合架构原理的前提下，理解和评估以网络为基础的应用软件的架构设计，得到一个功能强，性能好，适宜通信的架构。Fielding 将他对互联网软件的架构原则，定名为 REST，即 Representational State Transfer 的缩写，即 表现层状态变化。要理解 RESTful 架构，最好的方法就是去理解 Representational State Transfer 这个词组的意思。

#### 资源（Resources)

REST 的名称“表现层状态转化”中，省略了主语，表现层其实指的是资源（Resorces）的表现层

所谓资源，就是网络上的一个实体，或者说是网络上的一个具体的信息，可以用一个URI（统一资源定位符）指向它，每种资源对应一个特定的 URI，要获取这个资源，访问相应的 URI 即可。上网其实就是与互联网上的一系列资源的互动，调用它的URI

#### 表现层（Representation）

资源是一种信息实体，它可以有很多种外在表现形式。我们把资源具体呈现出来的形式，叫做它的表现层（Representation）。比如，文本可以用txt格式表现，也可以用 HTML 格式、 XML 格式、 JSON 格式呈现；图片可以用 JPG 格式表现，PNG 格式表现。URI 只代表资源的实体，不代表它的具体形式，它的具体表现形式应该在 HTTP 请求的头信息中用 Accept 和 Content-Type 字段指定。

#### 状态转化（State Transfer）

访问一个网站，就代表了客户端和服务端的一个互动过程，在这个过程中，势必涉及到数据和状态的变化。互联网通信协议 HTTP 协议是一个无状态协议，这意味着所有的状态都保存在服务器端，如果客户端想要操作服务器，必须通过某种手段让服务器端发生“状态转化”（State Transfer），而这种状态变化是建立在表现层之上的，所以就是“表现层状态转化”

客户端用到的手段只能是 HTTP 协议，具体来说就是 HTTP 协议中四个表示操作方式的动词：GET、POST、POST、PUT、 DELETE。它们分别对应四种基本操作：GET 用来获取资源，POST 用来新建资源（也可以用来更新资源），PUT 用来更新资源，DELETE 用来删除资源。

#### 综述

综上，我们总结一下什么是RESTful 架构：

- 资源（Resource）：每一种 URI 表示一种资源
- 表现层（Representation）：客户端与服务器端传递这种资源的某种表现层
- 状态转化（State Transfer）：客户端通过 HTTP 四个动词对服务端资源进行操作，实现“表现层状态转化”

RESTful 架构中不应该有动词





### 深入理解浏览器

原文：[Inside look at modern web browse](https://link.juejin.cn/?target=https%3A%2F%2Fdevelopers.google.com%2Fweb%2Fupdates%2F2018%2F09%2Finside-browser-part1)

翻译：[深入了解现代浏览器](https://juejin.cn/post/7035791817803038728)

#### 精读

##### 分层结构

模块之间需要合理分工，一般称之为 renderer process 而不是 renderer thread，因为 process（进程）相比 thread（线程），之间的数据是被操作系统隔离的，为了网页间无法相互读取数据，浏览器必须为每个 tab 创建一个独立的进程，甚至每个 iframe 都必须是独立进程（但是我查询的 vue 中 tab 和 iframe 存在数据交互）

UI thread 处理浏览器 UI 的展现与用户交互，比如当前加载的状态变化，历史前进后退，浏览器地址的输入、校验与监听按下 Enter 等事件，但不会涉及诸如发送请求、解析网页内容、渲染等内容

network thread 仅处理网络相关的事情，它主要关心通信协议、安全协议，目标就是快速准确的找到网站服务器，并读取其内容。network thread 会读取内容头做一些前置判断，读取内容和 renderer process 做的事情是有一定重合，但是 network thread 读取内容头仅为了判断内容类型，以便交给渲染引擎或者下载管理器（content-type 等设置），所以为了不让渲染引擎知道下载管理器的存在，读取内容头必须交给 network thread 来做

browser process 做与 renderer process 的通信， 也就是 UI thread、network thread 一旦要创建或与 renderer process 通信，都会交由它们所在的 browser process 处理

renderer process 仅处理渲染逻辑，它不关心从哪来的，比如是网络请求来的，还是 service worker 拦截后修改的，也不关心当前浏览器的状态是什么，它只管按照约定的接口规范，在指定的节点抛出回调，而修改应用状态由其它关心的模块复杂，比如 onload 回调触发后，browser process 处理浏览器的状态就是一个例子

假如 renderer process 里点击了一个新的跳转链接，这个事情是发生在 renderer process，但会交给 browser process 处理，因为每个模块解耦的非常彻底，所以任何复杂工作都能找到一个能响应它的模块，而这个模块也只要处理这个复杂工作的一部分，其余部分交给其它模块就好了，这就是大型应用维护的秘诀（类比 GFS 中的一个 master 节点，这些不就是设计的一些思想吗，说起来设计模式也只是看了一个开头）

提到加速优化，Chrome 惯用技巧就是用资源换时间（其实硬盘现在都不值钱，都是资源换时间，分布式也是想要通过空间换取其它东西）。即宁可浪费潜在资源，也要让事物尽可能的并发，这些从提前创建 renderer process、提前发起 network proces 都能看出来

##### 从渲染分层看性能优化

渲染有 5 个重要环节：解析、样式、布局、绘图、合成，这是前端开发者日常工作中对浏览器体感最深的部分，也是优化最常发生的部分

从性能优化角度来看，解析环节可以被替代为 JS 环节，因为现代 JS 框架往往没有什么 HTML 模板内容要解析，几乎全是 JS 操作 DOM，所以可以看作 5 个新的环节：JS、样式、布局、绘图、合成

几乎每层的计算都依赖上层的结果，但并不是每层都一定会重复计算，尤其是以下几点：

- 修改元素的几何属性（位置、宽高等）会触发所有层的重新计算，因为这是一个非常重量级的修改
- 修改某个元素绘图属性（颜色、背景色），并不影响位置，则会跳过布局层
- 修改比如 transform 属性会跳过布局与绘图层，transform 的内容会提升到合成层并交由 GPU 渲染

##### 隐式合成层、层爆炸、层自动合并

除了 transform、will-change 属性外，还有很多种情况元素会提升到合成层，比如 video、canvas、iframe、fixed 元素，这些都有明确的规则，所以属于显式合成

而隐式合成是指元素没有被特别标记，但也被提升到合成层的情况，这种情况常见发生在 z-index 元素产生重叠时，下方的元素显式申明提升到合成层，则浏览器为了保证 z-index 覆盖关系，就要隐式把上方的元素提升到合成层

层爆炸是指隐式合成的原因，当 css 出现一些复杂行为时（比如轨迹动画），浏览器无法实时捕捉到哪些元素位于当前元素上方，所以只好把所有元素都提升到合成层，当合成层数量过多，主线程与 GPU 的通信可能会成为瓶颈，反而影响性能

浏览器也会支持层自动合并，比如隐式提升到合成层时，多个元素会自动合并到一个合成层里。但这种方式不总是靠谱的，自动处理毕竟猜不到开发者的意图，所以最好的优化方式就是开发自己主动干预

浏览器规范由于是逐步迭代的，因此看似都在描述位置的 css 属性其实背后实现原理都是不同的，虽然这个规则体现在 W3C 规范上，但如果仅从属性名是很难看出端倪的，因此想要做极致性能优化就必须了解浏览器实现原理

#### 深入理解浏览器一

##### CPU、GPU、操作系统、应用的关系

cpu：可以处理几乎所有计算，现在的cpu是多核的

gpu：开始专门为图像处理设计，现在拥有大量并行处理简单事物的能力，非常适合矩阵运算，矩阵运算又是图形学的基础，所以大量用在可视化领域

这些硬件各自都提供了一些接口供汇编语言使用，而操作系统基于它们之上用 c 语言（如Linux）将硬件管理了起来，包括进程调度、内存分配、用户内核态切换等

运行在操作系统之上的就是应用程序了，应用程序不直接和硬件打交道，而是通过操作系统间接操作硬件

为什么应用程序不能直接操作硬件呢？这样做有巨大的安全隐患，因为硬件是没有任何抽象与安全措施的，这意味着理论上一个网页可以通过 js 程序，在你打开网页时直接访问你的任意内存地址，读取你的聊天记录，甚至读取历史输入的银行卡密码进行转账操作。

##### 进程与线程

为了让程序运行的更安全，操作系统创造了进程与线程的概念（Linux对进程与线程的实现是同一套），进程可以分配独立的内存空间，进程内可以创建多个线程进行工作，这些线程共享内存空间

因为线程间共享内存空间，因此不需通信就能交流，但内存地址相互隔离的进程间也有通信需求，需通过IPC（inter process communication）进行通信

进程之间相互独立，即一个进程挂了不会影响到其它进程，而在一个进程中可以创建一个新进程，并与之通信，所以浏览器就采用了这种策略，将UI、网络、渲染、插件、存储等模块进程独立，并且任意挂掉之后都可以被重新唤起

##### 浏览器架构

浏览器可以拆分为许多独立的模块，比如

- 浏览器模块（Browser）：负责整个浏览器内行为协调，调用各个模块
- 网络模块（Network）：负责网络 I/O
- 存储模块（Storage）：负责本地 I/O
- 用户界面模块（UI）：负责浏览器提供给用户的界面模块
- GPU模块：负责绘图
- 渲染模块（Rencderer）：负责渲染网页
- 设备模块（Device）：负责与各种本地设备交互
- 插件模块（Plugin）：负责处理各类浏览器插件

基于这些模块，浏览器有两种可用的架构设计：一种是少进程，一种是多进程

##### Chrome 多进程架构的优势

Chrome 尽量为每个 tab 单独创建一个进程，所以我们才能在某个tab未响应时，从容的关闭它，而其它tab不会受影响。不仅是tab间，一个tab内的iframe也会创建一个单独的进程

Chrome 并不满足于采用一种架构，而是在不同环境下切换不同的架构。Chrome 将各功能模块化后，就可以自由决定当前将哪些模块放在一个进程中，将哪些模块启动独立的进程，即可以在运行时决定采用哪套进程架构

###### 浏览器的主从架构

类似应用模式的主从模式，浏览器的 Browser 模块可以看作是主模块，它本身用于协调其它模块的运行，并维持其它各模块的正常工作，在其它模块失去响应时等待或重新唤起，在模块销毁时进行内存回收

各从模块也分工明确，比如在浏览器敲击URL地址时，会先通过 UI 模块响应用户的输入，并判断输入是否是 URL 地址，若校验通过之后，会通知 Network 网络模块发送请求，UI 模块就不再关心请求是如何处理了。Network模块也是相对独立的，仅处理请求的发送与接收，如果接收到的是 HTML 网页，则交给 Render 模块进行渲染

有了这些相对独立且分工明确的模块划分后，将这些模块作为线程或进程管理就都不会影响他们的业务逻辑了，唯一影响的就是内存是否共享，以及某个模块 crash 后是否会影响其它模块了。

#### 深入理解浏览器二

##### 概述

重点介绍了浏览器路由跳转后发生了什么，上一篇介绍了，browser process 包含了 UI thread，network thread 和 storage thread，当我们在浏览器菜单栏输入网址并敲击回车时，这套动作均由 browser process 的 UI thread 响应

##### 普通的跳转

1. UI thread 响应输入，并判断是否是合法的网址，如果是搜索协议，导致分发到另外的服务处理
2. 如果第一步输入的是合法地址，则 UI thread 会通知 network thread 获取网页内容，network thread 会寻找合适的协议处理网络请求，一般会通过 DNS协议 寻址，通过 TLS 协议 建立安全链接。如果服务器返回了比如 301 重定向信息，network thread 会通知 UI thread 这个信息，再启动一遍第二步
3. 读取响应内容，在这一步 network thread 会首先读取首部一些字节，即我们常说的响应头，其中包含 Content-Type 告知返回内容是什么，如果返回内容是 HTML，则 network thread 会将数据传送给 renderer process。这一步还会校验安全性，比如 CORB 或 cross-site 问题
4. 寻找 renderer process，一旦所有检查都完成，network thread 会通知 UI thread 已经准备好跳转了（注意此时并没有加载完所有数据，第三步只是检查了首字节），UI thread 会通知 renderer process 进行渲染。为了提升性能， UI thread 在通知 network thread 的同时会实例化一个 renderer process 等着，一旦 network thread 完毕后就可以立即进入渲染阶段
5. 确认导航。第四步后，browser process 通过 IPC 向 renderer process 传送 stream 数据，此时导航会被确认，浏览器的各个状态将会被修改，同时为了方便 tab 关闭后快速恢复，会话记录会被存储在硬盘

##### 跳转到别的网站

如果是跳转到别的网站，在执行普通跳转流程前，还会响应 beforeunload 事件，这个事件注册在 renderer process，所以 browser process 需要检查 renderer process 是否注册了这个响应，如无必要，请勿注册。

如果跳转是 js 发出的，那么执行跳转就由 renderer process 触发，browser process 来执行，后续流程就是普通的跳转流程。需要注意的是，执行跳转时，会触发原网站 upload 等事件，这个由旧的 renderer process 响应，而新网站会创建一个新的 render process 处理，当旧网页全部关闭时，才会销毁旧的 renderer process

###### Service Worker

Service Worker 可以在页面加载前执行一些逻辑，甚至改变网页的内容，但是浏览器仍然把 Service Worker 实现在了 renderer process 中

#### 深入理解现代浏览器三

##### 概述

宏观介绍 renderer process 做了哪些事情。浏览器 tab 内 html、css、JavaScript 内容基本上都由 renderer process 的主线程处理，除了一些 js 代码会放在 web worker 或 service worker 内，所以浏览器主线程的核心工作就是解析 web 三件套并生成可交互的用户界面

##### 解析阶段

首先 renderer process 主线程会解析 HTML 文本为 DOM（Document Object Model，文档对象模型），要把文本结构化才能继续处理。

对于 HTML 的 link、script、img 需要加载远程资源的，浏览器会调用 network thread 优先并行处理，但遇到 script 标签就必须停下来优先执行，因为 js 代码可能会改变任意 DOM 对象，这可能导致浏览器重新解析。如果 js 代码中没有修改 DOM 的副作用，可以添加 async， defer 标签，或则弄成 js 模块的方式，这样浏览器不必等待 js 的执行

##### 样式计算

只有 DOM 是不够的，style 标签申明的样式需要作用在 DOM 上，所以基于 DOM，浏览器要生成 CSSOM，这个 CSSOM 主要基于 css 选择器（selector）确定作用节点的

##### 布局

有了 DOM、CSSOM 仍然不足以绘制网页，因为仅知道结构和样式，但不知道元素的位置，这就需要生成 LayoutTree 以描述布局的结构。LayoutTree 和 DOM 结构很像，但比如 `display:none` 的元素不会出现在 LayoutTree 上，所以 LayoutTree 仅考虑渲染结构，而 DOM 是一个综合性描述结构，它不适合用来渲染

原文特别提到，Layout Tree 有个很大的技术难点：排版，Chrome 专门有一整个团队在攻克这个技术难题。为什么排版难：盒模型之间的碰撞、字体撑开内容导致换行，引发更大区域的的重新排版、一个盒模型撑开挤压另外一个盒模型... 布局最难的地方在于，需要对所有奇奇怪怪的布局做一个尽量合理的处理，而很多时候布局定式间的规则是相互冲突的。而且这还不考虑布局引擎的修改在数亿网页上引发 bug 的风险

##### 绘图

最后一环 PaintRecord，绘图记录。它会记录元素的层级关系，以决定元素绘制的顺序。因为 LayoutTree 仅决定了物理结构，但不决定元素的上下空间结构

有了 DOM、CSSOM、LayoutTree、PaintRecord 之后，终于可以绘图了。然后当 HTML 变化时，重绘的代价是巨大的，因为上面的任何一步的计算结果都依赖前面一步，HTML 改变时，需要对 DOM、CSSOM、LayoutTree、PaintRecord 重新计算。

大部分时候浏览器都可以在 16 ms 内完成，使 FPS 保持在 60 左右，但当页面结构过于复杂，这些计算本身超过了 16 ms，或其中遇到了 js 代码的阻塞，都会导致用户感到卡顿。对于 js 卡顿可以通过 `requestAnimationFrame` 把逻辑运算分散在各帧空闲时进行，也可以独立到 web worker 里。

##### 合成

绘图的步骤称为 rasterizing（光栅化）。在 Chrome 最早发布时，采用了一种较为简单的光栅化方案，即仅渲染可视区域内的像素点，当滚动后，再补充渲染当前滚动位置的像素点。这样做会导致渲染永远滞后于滚动

现在一般采用较为成熟的合成技术（compositing），即将渲染内容分层绘制与渲染，这可以大大提升性能，并可通过 CSS 属性 `will-change` 手动申明一个新层（不要滥用）

浏览器会根据 LayoutTree 分析后得到 LayerTree（层树），并根据它逐层渲染，合成层会将绘图内容切分为多个栅格并交由 GPU 渲染，因此性能会非常好



