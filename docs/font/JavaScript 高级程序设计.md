# JavaScript 高级程序设计

红宝书，久仰大名，第一次慢慢的来看看

## 1. JavaScript 简介

JavaScript的三个部分：ESMAScript（语言基础）、DOM（操作 HTML 的接口）、BOM（操作浏览器窗口的浏览器对象模型）

稍微理清了历史和一些专有名词

## 2. 在 HTML 中使用 JavaScript

如何让 JavaScript 与 HTML 页面共存，又不影响页面的呈现效果，最终的解决方案就是为 Web 增加统一的脚本支持

async：异步脚本，可选。表示应该立即下载脚本，但不妨碍页面中的其它操作，比如可以并行下载其它的资源，后续文档的加载和渲染是并行执行的

defer：延迟脚本，可选。表示脚本可以延迟到文档被完全解析和显示之后再执行，js 仅加载不执行

没有async 和 defer：浏览器会立即加载并执行相应的脚本，不等待后续加载的文档元素，读到就开始加载和执行，此举会阻塞后续文档的的加载

![在这里插入图片描述](https://ting-xin.github.io/notebook/image/c78314887538458496efc135123874fb.jpeg)

其它属性有：type、src、charset，，，

## 3. 基本概念

严格模式是为 JavaScript 定义了一种完全不同的解析与执行模型，在严格模式下，一些不确定的行为将会得到处理，一些不安全的操作也会抛出错误

基本数据类型：Undefined、Null、Boolean、Number、String、Object、Symbol（ES6)

typeof 操作符，判断变量类型的，typeof null 是 object，function 也是一种特殊的 object，

undefined 是为了正式区分空对象指针与未经初始化的变量，null 就是一个空对象指针（这也正是 null 返回 object 的原因），null == undefined，因为 undefined 派生自 null

所有类型的值都可以转为 Boolean，0 和 NaN 转化后是 false

在进行算术计算时，所有以八进制和十进制表示的数值最终都会转换成十进制数值，永远不要测试某个特定的浮点数值，0.1 + 0.2 == 0.3，浮点计算具有的通病。NaN（Not a Number）是一个特殊的数值，用来表示一个本来要返回数值的操作数为返回数值的情况，这也就可以不报错了（还不如报错呢）。NaN 与任何值都不相等，包括他自己，isNaN 来判断这个值是不是 NaN，能不能转为数值。isNaN 可以用于对象，它会首先调用对象的 valueOf() 方法，如果不行会接着调用 toString() 方法。null 数值转换的时候转为 0， undefined 转换的时候转为 NaN。Number、ParseInt、ParseFloat 有区别

ECMAScript 中的字符串是不可变的，改变变量的值其实是销毁了原来的字符串，然后重新创建了一个，所以一般效率不高。几乎每个值都有一个 toString 方法，对 null、undefined 应该是用 String() 方法

在ECMAScript 中，Object 类型是所有它的实例的基础，也就是 Object 类型所具有的任何属性和方法也同样存在于更具体的对象中，Object 的每个实例都具有下列属性和方法：

- constructor：保存着用于创建当前对象的函数
- hasOwnProperty(propertyName)：用于检查给定的属性是否在当前的对象实例中
- isPrototypeOf(object)：用于检查传入的对象是否是当前对象的原型
- propertyIsEnumberable(propertyName)：用于检查给定的属性是否能够使用 for-in 语句来枚举
- toLocaleString()：返回字符串表示，与当前的执行环境对应
- toString()、valueOf()

ECMAScript 描述了一组用于操作数据值的操作符，它能适用于很多值：++，--，+，-，~，&，|，^，<<，>>，!，&&，||，关系操作符

当对数值应用位操作符时，后台会发生如下转换过程：64位的数值被转换成32位数值，然后执行位操作，最后再将32位的结果换回64位数值，在对特殊的 NaN 和 Infinity 值应用位操作时，这两个值都会被当作0来处理

非 ~ 操作的本质，操作数的负值减一

逻辑非！ 可以应用于任何值，都会返回一个布尔值，如果操作数是一个对象，则返回 false，如果操作数是任意非0的数值，返回 false，如果操作数是 null 或者 NaN，返回 true。使用两个 ！！ 可以将这个数转换为真正对应的布尔值

逻辑与 && 在有一个操作数不布尔值的情况下，不一定返回布尔值。如果第一个数是 null，NaN，undefined，直接返回，如果第一个操作数是对象，返回第二个操作数，如果第二个操作数是对象，只有在第一个操作数为 true 的情况下返回。逻辑与操作属于短路操作，即如果第一个数能够决定结果，那么就不会再对第二个操作数求值，即如果第一个数是 false，就没继续做的必要了

逻辑或 || 操作也是短路操作符，如果第一个数是 true，就没继续做的必要了，如果有一个操作数不是布尔值，返回值也不一定是布尔值。如果两个操作数都是 null，NaN，undefined，则返回这个，如果第一个操作数是对象，则返回第一个操作数，如果第一个操作数的求值是 false，则返回第二个操所数，如果两个数都是对象，则返回第一个操作数

任何操作数与 NaN 进行关系比较，都会返回 false，NaN < 3 和 NaN >= 3 都返回 false

相等和不相等，先比较再转换，全等和不全等，仅比较而不转换。null 和 undefined 相等，NaN 和 NaN 不相等

ECMAScript 对象的属性是没有顺序，因此通过 for-in 输出的属性名是不可预测的，如果要迭代的对象是 null 或 undefined，不执行循环

函数不存在函数签名的概念，所以不能函数重载，没有指定返回值的函数返回的是一个特殊的 undefined 值

ECMAScript 函数不介意传递进来多少个参数，也不在乎传进来的参数是什么数据类型，因为参数在内部是用一个数组表示的，在函数体内可以通过 arguments 对象访问这个参数数组，根据这个特性可以做一个重载？通过检查传入函数中参数的类型和数量并作出不同的反应模仿重载

arguments 的值永远与对应命名参数的值保持同步，但是并不共享内存空间，没有传递值的命名参数自动被赋予 undefined 值，ECMAScript 中的所有参数传递的都是值，不可能通过引用传递参数

## 4. 变量、作用域和内存问题

基本类型：null，undefined，Boolean，Number，String，Symbol（ES6），引用类型：Object。基本类型值保存在变量中，可以直接操作，引用类型是引用保存在变量中，值保存在内存中，JavaScript 不允许直接访问内存中的位置，需要按引用进行访问。基本类型和引用类型在复制变量时候的表现不同，前者直接复制，后者只是复制引用，会同时指向一个内存空间

ECMAScript 中所有函数的参数都是按值传递的，会将函数外的值复制给函数内部的参数，如果复制的是引用类型，函数的操作也会操作到内存空间，从而反映到函数外部

通过 typeof 操作符检查 undefined，Boolean，Number，String，function，对于 null 及数组、对象，typeof 均检测出 object；通过 instanceof 检查 Array，Object，Function，instanceof 不能区别 undefined 和 null，而对于基本类型如果不是使用 new 声明则也检测不出来，对于是使用 new 声明的类型，它还可以检测出多层继承关系；constructor 不能判断 undefined 和 null，并且使用它不安全，constructor 的指向可以改变；在任何值上调用 Object 原生的 toString() 方法，都会返回一个 [object NativeConstructorName] 格式的字符串，但是它不能检测非原生构造函数的构造函数名

js 中有三种执行环境：全局执行环境（Global）、函数执行环境（Function）、Eval

执行环境（execution context）是 JavaScript 中最为重要的一个概念，执行环境定义了变量或函数有权访问的其它数据，决定了它们各自的行为。每个执行环境都有一个与之关联的变量对象（variable object），环境中定义的所有变量和函数都保存在这个对象中。虽然我们编写代码时候不能访问这个对象，但是解析器在后台会使用它

某个执行环境中的所有代码执行完毕后，该环境被销毁，保存在其中的所有变量和函数定义也随之销毁。每个函数都有自己的执行环境，当执行流进入一个函数时，函数的环境就会被推入一个环境栈中，而在函数执行之后，栈将其环境弹出，把控制权返回给之前的执行环境，这也是 ECMAScript 中的执行流的机制

当代码在一个环境中执行，会创建变量对象的一个作用域链，作用域链的用途是保证对执行环境有权访问的所有变量和函数的有序访问。作用域链的前端，始终都是当前执行代码所在环境的变量对象。如果这个环境是函数，则将其活动对象作为变量对象，这时候的活动对象是 arguments ，作用域链就是一个一个变量对象连起来

标识符解析就是沿着作用域链一级一级地搜索标识符的过程，搜索过程始终从作用域前端开始，然后逐级地向后回溯，直到找到标识符为止，没找到报错

延长作用域链：在作用域链的前端临时增加一个变量对象，可以通过 try catch 和 with

es5 之前没有块作用域的存在，声明变量会被自动添加到最接近的环境中，非严格模式下，没有声明变量就赋值，会在全局对象下创建属性，严格模式会报错

变量查询是由代价的，一般局部变量比全局变量快，但是 JavaScript 引擎在优化标识符查询方面的优化做的很好

JavaScript 具有自动垃圾收集机制，执行环境会负责管理代码执行过程中的内存，原理就是找出那些不在继续使用的变量，然后释放其内存，为此垃圾收集器会按照固定的时间间隔执行这操作，常用的策略有：标记清除、引用计数

每次进入一个新执行环境，都会创建一个i用于搜索变量和函数的作用域链，函数的局部环境不仅有权访问函数作用域中的变量，也可访问父级的环境，上级的却不能访问下级的，变量的执行环境有助于确定应何时释放内存

## 5. 应用类型

引用类型的值（对象）是引用类型的一个实例。在 ECMAScript 中，引用类型是一种数据结构，用于将数据和功能组织在一起，可以认为是一种类，但是并不具备传统的面向对象语言所支持的类和接口等基本结构（es6 支持了一些）。引用对象有时候也被称为对象定义，因为它们描述的是一类对象所具有的属性和方法

一般我们采用对象字面量的写法，代码量更少并且有一种封装的感觉；通常除非必须使用变量来访问属性，否则一般采用点表示法

Array 数组的每一项可以保存任何类型的数据，数组的 length 属性不是只读的，通过它可以很方便的在数组的末尾添加新项。数组检测 instanceof 只适合全局，一般采用 Array.isArray() 检测。toString 返回一个以逗号分隔的拼接的字符串，valueOf 返回的还是数组。栈：push，pop；队列：push，shift、unshift，pop；重排序：reverse，sort；操作方法：concat，slice，splice；操作方法：indexof，lastIndexOf；迭代方法：every，some 用来判断，filter，map，foreach（没有返回值），参数：item，index，array；归并方法：reduce，reduceRight

sort 判断比较函数的正负，大于 0 表示交换两个的属性，`return a - b` 代表就是升序

正则表达式有两种创建方法，完全等价，RegExp 的每个实例都具有一下属性：global、ignoreCase、lastIndex、multiline、source，实例方法：exec，search ，构造函数属性：input、lastMatch、lastParen、leftContext、rightContext、multiline

exec：专门为了捕获组而设计的，返回 Array 的实例，但是包含两个额外的属性：index 和 input，数组第一项是整个模式匹配的字符串，即使在模式中设置了全局标志g，也只会返回一个匹配项，但是 lastIndex 的值在每次调用之后都会增加

函数实际上是对象，每个函数实际上都是 Function 类型的实例，而且和其它引用类型一样具有属性和方法，由于函数是对象，函数名实际上也是一个指向函数对象的指针，不会与某个函数绑定，可以通过函数声明和函数表达式定义；函数没有重载，第二次创建的时候函数名的引用会覆盖第一次的引用

解析器在向执行环境加载数据时，会率先读取函数声明，并使其在执行任何代码之前可用（函数声明提升），函数表达式需要执行到它所在的代码行的时候才可以真正的执行。因为函数也是变量，所以可以很轻松的有回调这个写法，一个函数中可以嵌套其它函数

函数内部属性：arguments（类数组对象，保存函数参数，这个对象有个 callee 指向函数，严格模式下不能访问）、this（引用的是函数执行的环境对象），caller（保存着调用当前函数的引用，如果是全局作用域调用，则它的值是 null，但是严格模式也不能访问）

每个函数都包含两个属性：length 和 prototype，length 表示参数的个数，proptotype 保存所有实例方法，只不过是通过各自的对象来访问的，每个函数都包含两个非继承而来的方法：apply 和 call 方法，这两个方法的用途都是再特定的作用域中调用函数，实际上等于设置函数体内的 this 对象的值（这一点上感觉和 with 有一点像），使用 call 和 apply 来扩充作用域的最大好处就是对象不需要与方法有任何的耦合。es5 还定义了一个 bind 方法，这个方法会创建一个函数的实例，其 this 值会被绑定到传给 bind() 函数的值

在严格模式下，未指定环境对象而调用对象，则 this 值不会转型为 window 

为了便于操作基本类型值，ECMAScript 提供了 3 个特殊的引用类型：Boolean，Number，String，这些类型与引用类型相似，但同时也具有各自的基本类型相应的特殊行为。引用类型与基本包装类型的主要区别就是对象的生存期

可以显式地调用 Boolean、Nubmer 和 String 来创建基本包装类型的对象，不过，应该在绝对必要的情况下才这样做，因为这种做法很容易让人分不清自己是在处理基本类型还是引用类型的值

Number 类型的方法：toFiexed，toExponential，toPrecision；String 类型的字符方法：charAt，charCodeAt；字符串操作方法：concat（使用 + 号大多数情况下简单易行），slice，substr，substring；字符串位置方法：indexof，lastIndexOf；trim 方法；大小写转换方法；字符串模式匹配方法：match（本质上与调用 exec 方法相同），search（返回字符串中第一个匹配项的索引，否则返回 -1），replace（可以使用一些特殊的字符序列，将正则表达式操作得到的值插入到结果字符串中，第二个参数也可以使用一个函数），split 方法；localeCompare 方法

单体内置对象：Global，Math。浏览器中 Global 对象就是 window 对象，chrome 中称作 globalThis，Global 对象是个兜底的对象，不属于任何其它对象的属性和方法，最终都是它的属性和方法，事实上并没有全局对象和全局函数，它们都是 Global 对象的属性。Global 对象的 encodeURI 和 encodeURIComponent 都可以对 URI 进行编码，前者针对整个 URI，后者针对 URI 中的某一段，一般使用后者，因为它会对发现的任何非标准对象进行编码

eval 方法就像一个完整的 ECMAScript 解析器，接收一个字符串参数，即要执行的代码的字符串。严格模式下，在外部访问不到 eval 中创建的任何变量。能够解释代码字符串的能力是非常强大但也非常危险的，因此必须谨慎，尤其是用户的代码注入

Math 对象提供的计算执行功能比我们直接编写的计算快得多，舍入方法：ceil，floor，round；random 方法

## 6. 面向对象的程序设计

- 理解对象：属性类型、定义多个属性、读取属性特性
- 创建对象：工厂模式、构造函数模式、原型模式、组合使用构造函数模式和原型模式、动态原型模式、寄生构造函数模式、稳妥构造函数模式
- 继承：原型链、借用构造函数、组合继承、原型式继承、寄生式继承、寄生组合式继承

### 6.1. 基本概念

对于使用过基于类的开发者来说，JavaScript 是动态的，本身不提供一个 class 的实现，即便是在 ES6 中加入了 class 关键字，但那也只是语法糖，JavaScript 仍然是基于原型的。对于继承，JavaScript 只有一种结构：对象（object）。每个实例对象（object）都有一个私有属性（称之为 `__proto__`）指向它的构造函数的原型对象（prototype）。该对象也有一个自己的原型对象（`__proto__`)，层层向上直到一个对象的原型对象为 null，根据定义，null 没有原型，并作为这个原型链中的最后一个环节

ECMA-262 把对象定义为：无需属性的集合，其属性可以包含基本值、对象和函数。

ES5 定义了一些特性用来实现 JavaScript 引擎，因此不能在 JavaScript 中之际访问它。ES5 在定义内部才用的特性（attribute）时，描述了属性（property）的各种特征。有两种属性：数据属性，访问器属性

数据属性包含一个数据值的位置，在这个位置可以读取和写入值。数据属性有 4 个描述其行为的特性：[[Configurable]]，[[Enumerable]]，[[Writable]]，[[Value]]。要修改属性的默认特性，必须使用 ES5 的 Object.defineProperty 方法。一旦把属性设置为不可配置，就不能改为可配置了。可以多次调用 Object.defineProperty 方法修改同一个属性，但是他把设置为不可配置之后会有限制

访问器属性不包含数据值，它们包含一对 getter 和 setter 函数（非必需），在读取访问器属性时，会调用 getter 函数，这个函数负责返回有效的值；在写入访问器属性时，会调用 setter 函数并传入新值。访问器属性有 4 个特性：[[configurable]]，[[Enumberable]]，[[Get]]，[[Set]]。访问器属性的常用方式就是设置一个属性的值会导致其他属性发生变化（vue2）

### 6.2. 创建对象

虽然 Object 构造函数或对象字面量都可以用来创建单个对象，但这些方式有个明显的缺点，使用同一个接口创建很多对象，会产生大量重复的代码

#### 6.2.1. 工厂模式

```js
function createPerson(name, age, job) {
  let o = new ObjectI();
  o.name = name;
  o.age = age;
  o.job = job;
  o.sayName = function() {
    alert(this.name)
  }
}
let person1 = createPerson('test', 11, 'testJob')
```

工厂模式可以根据必要的参数多次创建出类似的对象，但是工程模式的缺点是不能解决对象识别的问题，有些功能函数是一样的，但是冗余了

#### 6.2.2. 构造函数模式

ECMAScript 中的构造函数可用来构建特定类型的对象，有例如 Object 和 Array 等原生的构造函数，它们在运行时自动出现在执行环境中，也可以自动创建构造函数，从而自定义对象类型的属性和方法

```js
function Person(name, age, job) {
  this.name = name;
  this.age = age;
  this.job = job;
  this.sayName = function() {
    alert(this.name)
  }
}
let person1 = new Person('test', 11, 'testJob')
```

Person 和 工厂模式的区别：没有显式的创造对象，直接将属性和方法赋值给 this 对象，没有 return 语句，首字母大写（规范）

调用构造函数实际上会经历 4 个步骤：创建一个新对象，将构造函数的作用域赋给新对象（this 指向这个对象），执行构造函数中的代码（添加属性），返回新对象

创造的实例中都有一个 constructor（构造函数）属性，指向 Person，最初 constructor 最初是用来标识对象类型的，但是检测对象类型的时候，还是 instanceof 操作符更可靠

构造函数跟其它函数的唯一区别就是调用它们的方式不同，任何函数，只要通过 new 操作符来调用，也可以看作是构造函数，并没有什么不同，实际上并不存在构造函数，只是对于函数的构造调用

构造函数解决了识别对象类型的问题，但是方法都要在每个实例上重新创建一遍

#### 6.2.3. 原型模式

我们创建的每个函数都有一个 prototype（原型）属性，这个属性是一个指针，指向一个对象（一般是构造函数的原型，可以改动），而这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法，使用原型对象的好处是可以让所有对象实例共享它所包含的属性和方法

```js
function Person() {
}
Person.prototype.name = 'test'
Person.prototype.age = 111
Person.prototype.job = 'testJob'
Person.prototype.sayName = function() { alert(this.name) }
let person1 = new Person()
```

只要创建了一个新函数，就会根据一组特定的规则为该函数创建一个 prototype 属性，这个属性指向函数的原型对象，在默认情况下，所有原型对象都会自动获得一个 construct 属性，这个属性是一个指向 prototype 属性所在函数的指针

虽然所有实现中都无法访问到 [[prototype]]，但是可以通过 isPrototypeof()  来确定对象之间是否存在这种关系。Object.getPrototypeOf() 可以方便地取得一个兑现的原型。使用 hasOwnProperty() 方法可以检测一个属性是存在于实例中，还是存在于原型中。in 操作符会通过在能访问给定的属性的时候返回 true，无论该属性是在实例中还是原型中。ES5 中的 Object.getOwnPropertyDescriptor() 方法只能检测实例属性。Object.keys 只能拿到可枚举的实例属性，Object.getOwnPropertyNames() 可以拿到所有的实例属性

每当代码开始读取对象的属性时，都会执行一次搜索，目标是具有给定名字的属性。搜索首先从对象实例开始，如果没找到，就继续搜索指针指向的原型对象，直到在原型对象中找到第一个，或者搜索到 Null

虽然可以通过对象实例访问保存在原型中的值，但不能通过对象实例重写原型中的值

如果以对象字面量的形式创建新的对象，最终结果都是相同的，但是有一个例外，constructor 属性不再指向构造函数，因为以前是每创建一个函数，就自动创建一个原型对象，并创建一个 constructor 属性指向函数。如果使用对象字面量的写法，实际上是重新创建了一个对象，这时候 constructor 属性指向了 Object 构造函数

尽管可以随时为原型添加属性和方法，并且修改能够立即在所有对象实例中反映出来，但如果是重写所有原型对象，情况就不一样了。因为调用构造函数的时候会为实例添加一个指向最初的原型的指针（`__proto__`），而把原型修改为另外要给对象就等于切断了构造函数与最初原型之间的联系，实例中的原型指针仅指向调用时候的原型

原型对象的问题：原型中所有属性是被很多实例共享的，这种共享对与函数来说非常合适，对于那些包含基本值的属性倒也说的过去，但是对于引用类型来说问题很大

#### 6.2.4. 组合使用构造函数模式和原型模式

创建自定义类型的最常见的方式就是组合使用构造函数模式与原型模式。构造函数模式用于定义实例模式，而原型模式用于定义方法和共享的属性。结果就是每个实例都会有自己的一份实例属性的副本，并且同时共享着对方法的引用

```js
function Person(name, age, job) {
  this.name = name;
  this.age = age;
  this.job = job;
  this.friends = []
}
Person.prototype = {
  constructor: Person,
  sayName: function() {
    alert(this.name)
  }
}
let person1 = new Person('test', 11, 'testJob')
```

#### 6.2.5. 动态原型模式

有其他 OO 语言开发经验的开发人员可能会很疑惑为什么是独立的构造函数和原型，动态原型模式就是把两者放在了一块

```js
function Person(name, age, job) {
  this.name = name;
  this.age = age;
  this.job = job;
  this.friends = [];
  if(typeof this.sayName != 'function') {		// 初次调用的时候才会执行
    Person.prototype.sayName = function() {
      alert(this.name)
    }
  }
}
```

#### 6.2.6. 寄生构造函数模式

寄生构造函数模式的写法和工厂模式其实一摸一样，但是它会通过 new 调用，而构造函数在不返回值的情况下，默认会返回对象实例，而寄生构造函数模式通过在构造函数末尾添加一个 return 语句，可以重写调用构造函数时返回的值

```js
function SpecialArray() {
  let values = new Array();
  values.push.apply(values, arguments);	// 添加所有值
  values.toPipedString = function() {
    return this.join('|');
  }
  return values;
}
let colors = new SpecialArray('red', 'blue');
```

该模式返回的对象与构造函数或则构造函数之间的原型没有任何关系，因此不能识别对象和共享东西（工厂模式的毛病），所以能使用其它模式的情况下不使用该模式

#### 6.2.7. 稳妥构造函数模式

稳妥对象：没有公共属性，方法也不引用 this 对象，稳妥对象最适合在一些安全的环境中（这些环境会禁止 this 和 new），或者防止数据被其它应用程序改动时使用。稳妥构造函数模式与寄生构造函数模式类似，但有两点不同：一是新建对象的时候不用 this，二是不适用 new 操作符调用构造函数

```js
function Persion(name, age, job) {
  let o = new Object();
  // 这里定义私有变量和函数
  o.sayName = function() {	// 除了 sayName 之外没有其它方法访问 name 变量
    alert(name)
  }
}
```

### 6.3. 继承

继承是 OO 语言中的一个最为人津津乐道的概念。许多 OO 语言都支持两种继承方法：接口继承和实现继承。接口继承只继承方法签名，而实现继承则继承实际的方法。由于JS中函数没有签名，所以 ECMAScript 中无法实现签名继承，只能支持实现继承，而实现继承主要通过原型链来实现

#### 6.3.1. 原型链

原型链的基本思想是利用原型让一个应用类型继承另一个引用类型的属性和方法（就是让原型对象等于另一个类型的实例，这时候就有一个指向另一个原型对象的的指针）

```js
function SuperType() {
  this.property = true;
}
SuperType.prototype.getSuperType = () => { return this.property }
function SubType() {
  this.subProperty = false;
}
SubType.proptotype = new SuperType();		// 继承了 SuperType,本质是重写了原型对象
SubType.prototype.getSubValue = () => { return this.subProperty }
let instance = new SubType()
```

所有引用类型都默认继承了 Object，这个继承也是通过原型链实现的。所有函数的默认原型都是 Object 的默认原型，这也正是所有自定义类型都会继承 toString、valueOf 等方法的原因

确定原型和实例的关系：instanceof、isPropertyOf 

原型链虽然很强大，能用它来实现继承。其中最主要的问题来自于包含引用类型值的原型。构建对象的时候引用类型已经被放在实例属性中了，但是继承的时候因为原型链中的原型是实例，这时候原型的属性又是共享的应用类型。第二个问题是在创建子类型的实例的时候，不能向超类型的构造函数传递参数

一些概念图，平时可以多看看：

[帮你彻底搞懂JS中的prototype、__proto__与constructor（图解）](https://juejin.cn/post/6844903816874164232)

![temp (1)](../image/temp%20(1)-16504410026403.jpg)

<div align='center'>原型基本关系</div>

![JS_constructor 关系](../image/JS_constructor%20%E5%85%B3%E7%B3%BB.png)

<div align='center'>JS 构造关系</div>

![JS_原型链](../image/JS_%E5%8E%9F%E5%9E%8B%E9%93%BE.png)

<div  align='center'>JS 原型链</div>



#### 6.3.2. 借用构造函数

为了能向超类型构造函数传递参数，在子类型的构造函数中的内部调用超类型构造函数。函数只不过是在特定的环境中执行代码的对象

```js
function SuperType() {
  this.colors = [];
}
function SubType() {
  SuperType.call(this);
}
let instance = new SubType();
```

如果只是借用构造函数，就不能继承原型对象的属性和方法，而且无法实现复用，每个子类都有父类实例函数的副本，影响性能

#### 6.3.3. 组合继承

组合继承就是将原型链和借用构造函数的技术组合到一块。其背后思路是利用原型链实现对原型属性和原型方法的继承，借用构造函数实现对实例属性的继承

```js
function SuperType(name) {
  this.name = name;
  this.friends = [];
}
SuperType.prototype.sayName = () => { alert(this.name) }
function SubType(name, age) {
  SuperType.call(this, name);		// 继承实例属性
  this.age = age;
}
SubType.prototype = new SuperType();
SubType.prototype.constructor = SubType;
SubType.prototype.sayAge = () => { alert(this.age) }
let instance = new SubType('test', 11)
```

组合继承避免了原型链和借用构造函数的缺陷，融合了它们的优点，成为了最常用的继承模式。但是组合继承无论再什么情况下都会两次调用超类型函数，子类型最终会包含超类型对象的全部实例属性

#### 6.3.4. 原型式继承

想法是借助原型可以基于已有的对象创建新对象，同时还不必因此创建自定义对象

```js
function object(o) {
  function F() {};
  F.prototype = o;
  return new F();
}
```

ES5 中的 Object.create 方法规范化了原型链继承

如果只是想让一个对象与另一个对象保持类似的情况下，原型式继承是完全可以胜任的。但是其中引用类型的属性失踪都会共享相应的值

#### 6.3.5. 寄生式继承

寄生式继承是与原型式继承是紧密相关联的。寄生式继承的思路与寄生构造函数和工厂模式相同，即创建一个及用于封装继承过程的函数，该函数在内部以某种方式来增强对象，最后再像真的一样返回对象

```js
function createAnother(original) {
  let clone = Object(original);		// 创建一个对象
  clone.sayHi = () => { alert('hi') };		// 增加一些属性或方法
  return clone;
}
```

使用寄生式继承来为对象添加函数，会由于不能做到函数复用而降低效率

#### 6.3.6. 寄生组合式继承

寄生组合式继承通过借用构造函数来继承属性，通过原型链的混成形式来继承方法。基本思路就是：不必为了指定子类型的原型而调用超类型的构造函数，我们所需的无非就是原型的副本，可以通过寄生式继承来继承超类的原型

```js
function inheritPrototype(subType, superType) {
  let prototype = Object(superType.prototype);		// 创建对象
  prototype.constructor = subType;
  subType.prototype = prototype;
}
function SuperType(name) {
  this.name = name;
  this.friends = []
}
SuperType.prototype.sayName = () => { alert(this.name) }
function SubType(name, age) {
  SuperType.call(this, name);
  this.age = age
}
inheritPrototype(SubType, SuperType);
SubType.prototype.sayAge = () => { alert(this.age) };
let instance = new SubType();
```

寄生组合式继承只调用一次 SuperType 构造函数，并且因此避免了在 SubType.prototype 上构建多余的属性，同时原型链和类别检测能正常使用，因此寄生组合式继承是引用类型最理想的继承范式



网上 [OOP 资源](https://tsejx.github.io/javascript-guidebook/object-oriented-programming)

## 7. 函数表达式

函数声明一个重要的特征就是函数变量提升，也就是在执行代码之前会读取函数声明

非严格模式模式下并不支持 arguments.callee，但是可以通过命名表达式来解决这个问题

```js
const fatorial = (function f(n) {
  if(n === 1) return 1;
  return n * f(n - 1);
})
```

不少开发人员总是搞不清匿名函数和闭包这两个概念，因此经常混用。闭包指的是有权访问函数作用域中变量的函数，创建闭包的常见方式就是在一个函数中创建另一个函数

执行上下文，变量对象，作用域链，this，闭包

当某个函数被调用时，会创建一个执行环境集相应的作用域链，然后，使用 arguments 和其它命名参数的值来初始化函数的活动对象。但在作用域链中，外部函数的活动对象始终处于第二位，作用域的终点是全局执行环境

后台的每个执行环境都有一个表示变量的对象：变量对象。全局环境的变量对象始终存在，而像局部环境的对象，则只在函数执行的过程中存在。变量对象创建的过程，形参，函数声明，变量声明，执行代码，AO = VO + 形参 + 函数声明 + 变量声明

闭包中，外层函数在执行完毕后，其活动对象不会被删除，因为内部函数的作用域链仍然在引用这个活动对象，换句话说，外部函数的执行环境的执行上下文，作用域链会被销毁，但它的活动对象会保存在内存中，直到匿名函数被销毁后，外部函数的活动对象才会被销毁

JavaScript 不会告诉你是否多次声明了同一个变量，遇到这种情况，它只会对后续的声明视而不见，不过，它会执行后续声明中的变量初始化

## 8. BOM

