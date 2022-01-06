# TypeScript

接口是一系列抽象方法的声明，是一些方法特征的集合，这些方法应该都是抽象的，需要由具体的类去实现，然后第三方就可以通过这组抽象方法调用

```typescript
interface IPerson{
  firstName: string,
  lastName: string,
  sayHi: () => string 
}

let customer: IPerson = {
  firstName: 'Tian',
  lastName: 'Wu',
  sayHi: ():string => {return 'hello, world'}
}

console.log('customer: ', customer)

let test: IPerson = {
  firstName: 'testFirst',
  lastName: 'testLast',
  sayHi: ():string => {return 'hey, I\'m testing'}
}

console.log('test: ', test)

interface Person {
  age: number
}

interface Musician extends Person {
  instrument: string 
}

let drummer = <Musician>{}
drummer.age = 27
drummer.instrument = 'Drums'
console.log('drummer: ', drummer)
```

TypeScript 是面向对象的 JavaScript。

类描述了所创建的对象共同的属性和方法。

TypeScript 支持面向对象的所有特性，比如 类、接口等。

定义类的关键字为 class，后面紧跟类名，类可以包含以下几个模块（类的数据成员）：

- **字段** − 字段是类里面声明的变量。字段表示对象的有关数据。
- **构造函数** − 类实例化时调用，可以为类的对象分配内存。
- **方法** − 方法为对象要执行的操作。

TypeScript 支持继承类，即我们可以在创建类的时候继承一个已存在的类，这个已存在的类称为父类，继承它的类称为子类。

类继承使用关键字 **extends**，子类除了不能继承父类的私有成员(方法和属性)和构造函数，其他的都可以继承。

TypeScript 一次只能继承一个类，不支持继承多个类，但 TypeScript 支持多重继承（A 继承 B，B 继承 C）。

子类只能继承一个父类，typescript 不支持继承多个类，但支持多重继承

类继承后，子类可以对父类的方法重新定义，这个过程称之为方法的重写

其中super关键字是对父类的直接引用，该关键字可以直接引用父类的属性和方法

instanceof 运算符用于判断对象是否是指定的类型，如果是返回true， 否则返回false，父类包括了子类

static 关键字用于定义类的数据成员（属性和方法）为静态的，静态成员可以直接通过类名调用。（没有初始化就可以调用）

- 访问控制修饰符

public: 公有，可以在任何地方被访问

protected: 保护，可以被其自身及其子类和父类访问

private： 私有，只能在被其定义所在的类访问

- 类和接口 

类可以实现接口，是有关键字implements(interface)，并将interest作为类的属性使用

- 鸭子类型（duck typing） （还不懂）

鸭子类型（英语：duck typing）是动态类型的一种风格，是多态(polymorphism)的一种形式。

在这种风格中，一个对象有效的语义，不是由继承自特定的类或实现特定的接口，而是由"当前方法和属性的集合"决定。

> 可以这样表述：
>
> "当看到一只鸟走起来像鸭子、游泳起来像鸭子、叫起来也像鸭子，那么这只鸟就可以被称为鸭子。"

在鸭子类型中，关注点在于对象的行为，能作什么；而不是关注对象所属的类型。例如，在不使用鸭子类型的语言中，我们可以编写一个函数，它接受一个类型为"鸭子"的对象，并调用它的"走"和"叫"方法。在使用鸭子类型的语言中，这样的一个函数可以接受一个任意类型的对象，并调用它的"走"和"叫"方法。如果这些需要被调用的方法不存在，那么将引发一个运行时错误

- typescript命名空间

命名空间一个最明确的目的就是解决重名问题

如果一个命名空间在一个单独的typeScript文件忠，则应该使用三斜杠  /// 引用它

```typescript
<reference path = 'someFileName.ts' />
```

我可以认为模块就是一个文件级别的命名空间吗，这种不需要写namespace了（但是实际上不是这样的，编译的结果不一样）

- typescript 声明文件

TypeScript 作为 JavaScript 的超集，在开发过程忠不可避免要引用第三方的JavaScript的库，虽然可以直接通过调用库的类和方法，但是却无法使用 typeScript 诸如类型检查等特性功能。为了解决这个问题，需要将这些库里的函数和方法体去掉后只保留导出类型声明，而产生了一个描述JavaScript库和模块信息的声明文件，通过引用这个声明文件，就可以借用 typescript 的各种特性来使用库文件了

声明文件以 .d.ts 为后缀

Jest 作为 typescript 的单元测试

```typescript
/**
 * 判断一个value是不是 NaN、null、undefined
 * @param value
 */
export function isNull(value): boolean {
    return String(value) ==== 'NaN' || value === null || value === void 0
}
```

[正则表达式](https://github.com/cdoco/learn-regex-zh) （元字符感觉还行，但是简写字符集、断言自己有i点会懵）

元字符

| .     | 匹配除换行符以外的任意字符。                                 |
| ----- | ------------------------------------------------------------ |
| [ ]   | 字符类，匹配方括号中包含的任意字符。                         |
| [^ ]  | 否定字符类。匹配方括号中不包含的任意字符                     |
| *     | 匹配前面的子表达式零次或多次                                 |
| +     | 匹配前面的子表达式一次或多次                                 |
| ?     | 匹配前面的子表达式零次或一次，或指明一个非贪婪限定符。       |
| {n,m} | 花括号，匹配前面字符至少 n 次，但是不超过 m 次。             |
| (xyz) | 字符组，按照确切的顺序匹配字符 xyz。                         |
| \|    | 分支结构，匹配符号之前的字符或后面的字符。                   |
| \     | 转义符，它可以还原元字符原来的含义，允许你匹配保留字符 `[ ] ( ) { } . * + ? ^ $ \ |` |
| ^     | 匹配行的开始                                                 |
| $     | 匹配行的结束                                                 |

简写字符集

| .    | 匹配除换行符以外的任意字符               |
| ---- | ---------------------------------------- |
| \w   | 匹配所有字母和数字的字符：`[a-zA-Z0-9_]` |
| \W   | 匹配非字母和数字的字符：`[^\w]`          |
| \d   | 匹配数字：`[0-9]`                        |
| \D   | 匹配非数字：`[^\d]`                      |
| \s   | 匹配空格符：`[\t\n\f\r\p{Z}]`            |
| \S   | 匹配非空格符：`[^\s]`                    |

断言

| ?=   | 正向先行断言 |
| ---- | ------------ |
| ?!   | 负向先行断言 |
| ?<=  | 正向后行断言 |
| ?<!  | 负向后行断言 |

标记

| i    | 不区分大小写：将匹配设置为不区分大小写。   |
| ---- | ------------------------------------------ |
| g    | 全局搜索：搜索整个输入字符串中的所有匹配。 |
| m    | 多行匹配：会匹配输入字符串每一行。         |



# TypeScript 教程

[入门教程](https://ts.xcatliu.com/introduction/what-is-typescript.html)

[toc]

## 简介

大部分JavaScript代码都只需要少量的修改就变成typescript，这得益于typescript强大的[类型推论] []，即使不去手动声明变量的类型，typescript也可以自动推论出它的类型

```typescript
console.log(1 + '1');	// 打印字符串
print(1 + '1');		// python代码，报错
```

typescript是完全兼容JavaScript的，它不会修改JavaScript运行时的特性，所以它们完全是弱类型的，作为对比，python是强类型的

## 基础

### 原始数据类型

JavaScript的类型分为两种：原始数据类型和对象类型

原始数据类型：布尔值、数值、字符串、null、undefined、ES6中的新类型symbol，ES10中的bigint

#### boolean

使用构造函数Boolean 创造的对象不是布尔值，布尔值boolean和布尔对象Boolean不是一个东西

```typescript
let isDone:boolean = false
let createByNewBoolean:Boolean = new Boolean(1)
```

#### 数值

`0b1010` 和 `0o744` 是 [ES6 中的二进制和八进制表示法](http://es6.ruanyifeng.com/#docs/number#二进制和八进制表示法)，它们会被编译为十进制数字。

#### 字符串

其中`用来定义定义ES6中的模板字符串，${expr}用来在模板字符串中嵌入表达式

```typescript
let myName:string = 'Tom'
let myAge:number = 25
//模板字符串
let sentence:string = `Hello, ${myName} will be ${myAge + 1} nest year.`
```

#### 空值

JavaScript中没有空值（void）的概念，在typescript中，可以用void表示没有任何返回值的函数：

```typescript
function alertName(myName:string):void {
    alert(`my name is ${myName}.`)
}
```

声明一个void类型的变量没有什么用，因为你只能将它复制为undefined和null

```typescript
let unusable:void = undefined
```

#### Null 和 Undefined

在typescript中，可以用null和undefined来定义这两个原始数据类型：

```typescript
let u:undefined = undefined
let n:null = null
```

于void 的区别是，undefined和null是所有类型的子类型，也就是说undefined类型的变量可以赋值给number类型的变量：

```typescript
let num:number = undefined		// 不会报错
let u:undefined
let num:number = u		// 这样也不会报错
```

void类型不能赋值给number类型的变量

### 任意值

任意值（Any）用来表示允许赋值为任意类型

#### 什么是任意类型

如果是一个普通类型，在赋值过程中改变类型是不允许的，但是如果是any类型的，则允许被赋值为任意类型

```typescript
let myFavoriteNumber:string = 'seven'
myFavoriteNumber = 7	// error
let myFavoriteNumber:any = 'seven'
myFavoriteNumber = 7	//正常
```

#### 任意值的属性和方法

在任意值上访问任何属性都是允许的，也允许调用任何方法，可以认为声明一个变量是任意值之后，对它的任何操作，返回的类型都是任意值

```typescript
let anyThing:any = 'hello'
console.log(anyThing.myName)
console.log(anThing.myName.firstName)
anyThing.setName('Jerry')
anyThing.setName('Jerry').sayHello()
```

#### 未声明类型的变量

变量如果在声明的时候，为指定其类型，那么它会被识别为任意值类型：

```typescript
let something
somthing = 'seven'
somthing = 7
something.setName('Tom')
```

等价于

```typescript
let something:Any
somthing = 'seven'
somthing = 7
something.setName('Tom')
```

### 类型推论

如果没有明确的指定类型，那么typescript会按照类型推论（Type Inference）的规则推断出一个类型

#### 什么是类型推论

一下代码虽然没有指定类型，但是会在编译的时候报错

```typescript
let myFavoriteNumber = 'seven'
myFavoriteNumber = 7		// error
```

事实上，它等价于

```typescript
let myFavoriteNumber:string = 'seven'
myFavoriteNumber = 7		// error
```

typescript会在没有明确的指定类型的时候推测出一个类型，这就是类型推论

```typescript
let myFavoriteNumber
myFavoriteNumber = 'seven'
myFavoriteNumber = 7
```

如果定义的时候没有赋值，不管之后有没有赋值，都会被推断成any类型而完全不被类型检查

### 联合类型

联合类型（union types）表示取值可以为多种类型的一种

#### 简单例子

```typescript
let myFavoriteNumber:string|number
myFavoriteNumber = 'seven'
myFavoriteNumber = 7
myFavoriteNumber = true		// error
```

联合类型使用 | 分隔每一个类型

这里 `let myFavoriteNumber:string|number` 的含义是 myFavorite 的类型可以是 string 或者 number， 但是不可以是其它类型

#### 访问联合类型的属性或方法

当typescript不确定一个联合类型的变量到底是哪个类型的时候，我们只能访问此两盒类型的所有类型里共有的属性或方法

```typescript
function getLength(something:string|number):number{
    return something.length
}
// error, something 不确定类型，length只是string的属性
function getString(something:string|number):string{
    return something.toString()		// 正确，这是共有属性
}
```

联合类型的变量在赋值的时候，会根据类型推论的规则推断出一个类型：

```typescript
let myFavoriteNumber:string|number
myFavoriteNumber = 'seven'
console.log(myFavoriteNumber.length)	// 正确
myFavoriteNumber = 7
console.log(myFavoriteNumber.length)		// 编译时报错
```

### 对象的类型——接口

在typescript中，我们使用接口（interfaces）来定义对象的类型

#### 什么是接口

在面向对象语言中，接口是一个很重要的概念，它是对行为的抽象，而具体如何行动需要类去实现

typescript中的接口是要给非常灵活的概念，处理可用于类的一部分行为进行抽象意外，也常对对象的形状（形状）进行描述

```typescript
interface Person{
    name: string;
    age: number;
}
let tom: Person = {
    name: 'Tom',
    age: 25
}
```

在这个例子中，我们定义了一个接口 Person，接着定义了一个变量 tom，它的类型是Person，这样我们就约束了tom的形状必须和接口Person一致

接口一般首字母大写，有些编程语言会建议接口的名称前加一个 I

定义的变量比接口少了一些属性是不允许的

```typescript
interface Person {
    name: string;
    age: number;
}
let tom: Person = {
    name: 'tom'
}
// Propertyp 'age' is missing in type '{name: string}'
```

多一些属性也是不允许的：

```typescript
let tom: Person = {
    name: 'tom',
    age: 25,
    gender: 'male'
}
// 'gender' does not exist in type 'Person'
```

可见，赋值的时候，变量的形状必须和接口的形状保持一致

#### 可选属性

有时我们希望不要完全匹配一个形状，那么可以用可选属性：

```typescript
interface Person {
    name: string;
    age?: number
}
let tom: Person = {
    name: 'Tom'
}
```

但是仍然不允许添加未定义的属性

```typescript
let tom: Person = {
    name: 'tom',
    age: 25,
    gender: 'male'
}
// 'gender' does not exist in type 'Person'
```

#### 任意属性

有时候我们希望一个接口允许有任意的属性，可以使用如下方式：

```typescript
interface Person {
    name: string;
    age?: number;
    [propName: string]: any;
}
let tom: Person = {
    name: 'tom',
    gender: 'male'
}
```

使用 [propName: string] 定义了任意属性取string类型的值

需要注意的是，一旦定义了任意属性，那么确定属性和可选属性的类型必须都是它的类型的子集

```typescript
interface Person {
    name: string;
    age?: number;
    [propName: string]: strnig;
}
let tom: Person = {
    name: 'tom',
    age: 25,
    gender: 'male'
}
// error, type 'number' is not assignable to type 'string'
```

上例中，任意属性的值允许是 string，但是可选属性age的值确实number，number不是string的子属性，所以报错了

一个接口中只能定义一个任意属性，如果接口中有多个类型的属性，则可以在任意属性中使用联合类型

#### 只读属性

有时候我们希望对象中的一些字段只能在创建的时候赋值，那么可以使用readonly定义只读属性：

```typescript
interface Person {
    readonly id: number;
    name: string;
    age?: number;
    [propName: string]: any;
}
let tom: Person = {
    id: 1,
    name: 'tom',
    gender: 'male'
}
tom.id = 2		// error, id is read-only
```

注意，只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候

```typescript
let tom: Person = {
    name: 'tom'
}
tom.id = 1
// error 两个错误，第一次初始化没有给id赋值，重新给id赋值的时候已经晚了
```

### 数组的类型

在typescript中，数组类型有多种定义方式，比较灵活

#### [类型+方括号]表示法

最简单的方法是使用[类型+方括号]来表示数组：

```typescript
let fibonacci: number[] = [1, 1, 2, 3]
```

数组的项中不允许出现其它的类型，数组的一些方法的参数也会根据数组在定义时按约定的类型进行限制

```typescript
let fabonacci: number[] = [1, '1', 2, 3]	// error, 类型不对
let fibonacci.push('5')		// error, 方法也被类型限制了
```

上例中，push 方法只允许传入number类型的参数

#### 数组泛型

我们也可以使用数组泛型（array generic）Array<elemType> 来表示数组：

```typescript
let fibonacci: Array<number> = [1, 1, 2, 3, 5]
```

#### 用接口表示数组

```typescript
interface NumberArray {
    [index: number]: number;
}
```

NumberArray 表示： 只要索引的类型是数字时，那么值的类型必须是数字

虽然接口也可以用来描述数组，但是我们一般不会这么做，因为这种方式比前两种复杂多了

有一种情况例外： 类数组

#### 类数组

类数组（Array-like Object）不是数组类型，比如arguments：

```typescript
function sum() {
    let args: number [] = arguments
}
// error, 类数组不能用普通的数组的方式描述，而应该用接口
```

```typescript
function sum() {
    let args: {
        [index: number]: number;
        length: number;
        callee: Function;
    } = arguments
}
```

事实上常用的类数组都有自己的接口定义，如 IArguments， NodeList， HTMLCollectoin等：

```typescript
function sum() {
    let args: IArguments = arguments
}
```

#### any 在数组中的应用

一个比较常见的做法是，用 `any` 表示数组中允许出现任意类型：

```ts
let list: any[] = ['xcatliu', 25, { website: 'http://xcatliu.com' }];
```

### 函数类型

函数是JavaScript中的一等公民

#### 函数声明

在JavaScript中，有两种常见的定义函数的方式：函数声明和函数表达式

```typescript
// 函数声明（Function Declaration）
function sum(x, y) {
    return x + y
}
// 函数表达式（Function Expression)
let mySum = function(x, y) {
    return x + y
}
```

一个函数有输入输出，要在typescript中对其进行约束，需要把输入和输出都考虑到，其中函数声明的类型定义较为简单：

```typescript
function sum(x: number, y: number): number {
    return x + y
}
```

输入多余的（或者少于要求的）参数，都是不允许的

函数表达式在typescript中的定义：

```typescript
let mySum = function(x: number, y: number): number {
    return x + y
}
```

事实上，我们只对右侧的匿名函数进行了类型定义，而等号左边的可以通过赋值操作进行类型推论而推断出来，如果我们需要手动给mySum添加对象，则应该是这样：

```typescript
let mySum:(x: number, y: number) => number = function(x: number, y: number): number {
    return x + y
}
```

注意不要混淆了typescript中的 => 和ES6 中的 =>

在typescript中，=> 用来表示函数的定义，左边是输入类型，需要用括号，右边是输出类型，es6 中这个是箭头函数

#### 用接口定义函数的形状

我们也可以使用接口的方式来定义一个函数需要符合的形状：

```typescript
interface SearchFunc {
    {source: string, subString: string}: boolean
}
let mySearch: SearchFunc
mySearch = function(source: string, subString: string) {
    return source.search(sbString) != -1
}
```

采用函数表达式|接口定义函数的方式时，对等号左侧进行类型限制，可以在以后对函数名赋值时保证参数个数、参数类型、函数值返回不变

#### 可选参数

前面提到，输入多余的（或者少于要求的）参数，是不被允许的，那么如何定义可选参数呢，与接口类似，我们用 ？ 表示可选的参数

```typescript
function buildName(firstName: string, lastName?: string) {
    if(lastName) {
        return firstName + ' ' + lastName
    } else {
        return firstName
    }
}
```

可选参数后面不允许再出现必需参数了

#### 参数默认值

在 ES6 中，我们允许给函数的参数添加默认值，typescript会将添加量默认值的参数识别为可选参数，这时候就可以不受顺序影响

#### 剩余参数

ES6中，可以使用 ...rest 的方式获取函数中的剩余参数

```javascript
function push(array, ..items) {
    items.forEach(function(item) {
        array.push(item)
    })
}
```

事实上items是一个数组，我们可以用数组的类型来定义它

```typescript
function push(array: any[], ...items: any[]) {
    items.forEach(function(item) {
        array.push(item)
    })
}
```

rest参数只能是最后一个参数

#### 重载

重载允许一个函数接受不同数量或类型的参数时，做出不同的处理

```typescript
function reverse(x: number | string): number | string | void {
    if(typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''))
    } else if(typeof x === 'string') {
        return x.split('').reverse().join('')
    }
}
```

然而这样有一个缺点，就是不够精确的表达，输入数字的时候，输出也应该是数字，输入是字符串的时候，输出也应该是字符串

```typescript
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string | void {
    if(typeof x === 'number') {
        return Number(x.tostring().split('').reverse().join(''))
    } else if(typeof x === 'string') {
        return x.split('').reverse().join('')
    }
}
```

typescript会优先从最前面的函数定义开始匹配，所以多个函数定义如果有包含关系，需要优先把精确的定义写在前面

### 类型断言

类型断言（type assertion）可以用来手动指定一个值的类型

#### 语法

值 as 类型   或者     <类型>值

tsx语法必须使用前者，建议在使用类型断言时，统一使用 值 as 类型 这样的语法

#### 将一个联合类型断言为其中一个类型

```typescript
interface Cat {
    name: string;
    run(): void;
}
interface Fish {
    name: string;
    run(): void;
}
function isFish(animal: Cat | Fish) {
    if(typeof animal.swim === 'function') {
        return true
    }
    return false
}
// Error, Property 'swim' does not exit on type 'Cat'
function isFish(animal: Cat | Fish) {
    if (typeof (animal as Fish).swim === 'function') return true
    return false
}
```

这样就可以解决访问 animal.swim 报错的问题，但是类型断言只能够欺骗typescript编译器，无法避免运行时的错误，滥用类型断言可能会导致运行时错误

#### 将一个父类断言为具体的子类

当类之间有继承关系时，类型断言也很常见

```typescript
class ApiError extends Error {
    code: number = 0
}
class HttpError extends Error {
    statusCode: number = 200
}
function isApiError(error: Error) {
    if(typeof (error as ApiError).code === 'number') {		// instanceof 也是一个办法，如果判断的是类的话
        return true
    }
    return false
}
```

#### 将任何一个类型断言成any

在any类型的变量上，访问任何属性都是允许的，但是将一个变量断言成any可以说是解决typescript中类型问题的最后一个手段

它极有可能掩盖了真正的类型错误，所以如果不是非常确定，不要使用 as any

```typescript
(window as any).foo = 1
```

上面例子中最好的解决办法是扩展window 的类型，但是使用  as any 更加方便

我们需要在类型的严格性和开发的便利性之间掌握平衡

#### 将any断言成一个具体的类型

遇到 `any` 类型的变量时，我们可以选择无视它，任由它滋生更多的 `any`。

我们也可以选择改进它，通过类型断言及时的把 `any` 断言为精确的类型，亡羊补牢，使我们的代码向着高可维护性的目标发展。

#### 类型断言的限制

- 联合类型可以被断言为其中一个类型
- 父类可以被断言为子类
- 任何类型都可以被断言为 any
- any 可以被断言为任何类型
- 要使得 `A` 能够被断言为 `B`，只需要 `A` 兼容 `B` 或 `B` 兼容 `A` 即可

前四种情况都是最后一个的特例

#### 双重断言

既然任何类型都可以被断言成any，any可以被断言成任何类型，双重断言基本就可以断言成想要的类型，但是这样做的大部分都是错误的

#### 类型断言 vs 类型转换 vs 类型声明

类型断言只会影响 typescript编译时的类型，类型断言语句会在编译结果中被删除

```typescript
function toBoolean(something: any): boolean {
    return something as boolean
}
toBoolean(1)		// 返回值 1
// 编译结果
function toBoolean(something: any): boolean {
    return something
}
toBoolean(1)
```

类型转换就是一句允许的语句了

- `animal` 断言为 `Cat`，只需要满足 `Animal` 兼容 `Cat` 或 `Cat` 兼容 `Animal` 即可
- `animal` 赋值给 `tom`，需要满足 `Cat` 兼容 `Animal` 才行

我们还有第三种方式可以解决这个问题，那就是泛型

### 声明文件

当使用第三方库时，我们需要引用它的声明文件，才能获得对应的代码补全、接口提示等功能。

- [`declare var`](https://ts.xcatliu.com/basics/declaration-files.html#declare-var) 声明全局变量
- [`declare function`](https://ts.xcatliu.com/basics/declaration-files.html#declare-function) 声明全局方法
- [`declare class`](https://ts.xcatliu.com/basics/declaration-files.html#declare-class) 声明全局类
- [`declare enum`](https://ts.xcatliu.com/basics/declaration-files.html#declare-enum) 声明全局枚举类型
- [`declare namespace`](https://ts.xcatliu.com/basics/declaration-files.html#declare-namespace) 声明（含有子属性的）全局对象
- [`interface` 和 `type`](https://ts.xcatliu.com/basics/declaration-files.html#interface-和-type) 声明全局类型
- [`export`](https://ts.xcatliu.com/basics/declaration-files.html#export) 导出变量
- [`export namespace`](https://ts.xcatliu.com/basics/declaration-files.html#export-namespace) 导出（含有子属性的）对象
- [`export default`](https://ts.xcatliu.com/basics/declaration-files.html#export-default) ES6 默认导出
- [`export =`](https://ts.xcatliu.com/basics/declaration-files.html#export-1) commonjs 导出模块
- [`export as namespace`](https://ts.xcatliu.com/basics/declaration-files.html#export-as-namespace) UMD 库声明全局变量
- [`declare global`](https://ts.xcatliu.com/basics/declaration-files.html#declare-global) 扩展全局变量
- [`declare module`](https://ts.xcatliu.com/basics/declaration-files.html#declare-module) 扩展模块
- [`/// `](https://ts.xcatliu.com/basics/declaration-files.html#san-xie-xian-zhi-ling) 三斜线指令

暴露在最外层的interface 或 type 会作为全局类型作用于整个项目中，我们应该尽可能的减少全局变量或全局类型的变量，故最好将它们放到namespace下

注意只有function、class和interface可以直接默认导出，其它的变量需要先定义出来，在默认导出

## 进阶

### 类型别名

类型别用来给一个类型起一个新的名字

```typescript
type Name = string
type NameResolver = () => string
type NameOrResolver = Name | NameResolver
function getName(n: NameOrResolver) : Name {
    if(typeof n === 'string') return n
    return n()
}
```

### 字符串字面量类型

字符串字面量类型用来约束取值只能是某几个字符串中的一个(枚举？)

```typescript
type EventNames = 'click' | 'scroll' | 'mousemove'
function handleEvent(ele: Element, event: EventNames) {
    
}
handleEvent(document.getElementById('hello'), 'scroll')		// 没问题
handleEvent(document.getElementById('hello'), 'dbclick')	// error 没有这个类型
```

我们使用type定义了一个字符串字面量类型，它只能取这几个字符串中的一个

类型别名与字符串字面量类型都使用 type 进行定义

### 元组

数组合并了相同类型的对象，而元组（tuple）合并了不同类型的对象

#### 简单例子

```typescript
let tom: [string, number] = ['Tom', 25]
tom[0] = 'Tom'		// 可以只赋值其中一项
tom = ['Tom', 25]	// 直接对元组类型的变量进行初始化，需要提供所有元组类型中指定的项
```

### 枚举

枚举（Enum）类型用于取值被限定在一定范围内的场景，比如一周内只有七天

#### 简单的例子

枚举成员会被赋值为从0开始递增的数字，同时也会对枚举值到枚举名进行反向映射

```typescript
enum Days {Sun, Mon, Tue, Wed,Thu, Fri, Sat}
console.log(Days["Sun"] === 0); // true
console.log(Days["Mon"] === 1); // true
console.log(Days["Tue"] === 2); // true
console.log(Days["Sat"] === 6); // true

console.log(Days[0] === "Sun"); // true
console.log(Days[1] === "Mon"); // true
console.log(Days[2] === "Tue"); // true
console.log(Days[6] === "Sat"); // true
// 事实上，上面会被编译成
var Days;
(function (Days) {
    Days[Days["Sun"] = 0] = "Sun";
    Days[Days["Mon"] = 1] = "Mon";
    Days[Days["Tue"] = 2] = "Tue";
    Days[Days["Wed"] = 3] = "Wed";
    Days[Days["Thu"] = 4] = "Thu";
    Days[Days["Fri"] = 5] = "Fri";
    Days[Days["Sat"] = 6] = "Sat";
})(Days || (Days = {}));
```

#### 手动赋值

```typescript
enum Days {Sun = 7, Mon = 1, Tue, Wed, Thu, Fri, Sat};

console.log(Days["Sun"] === 7); // true
console.log(Days["Mon"] === 1); // true
console.log(Days["Tue"] === 2); // true
console.log(Days["Sat"] === 6); // true
```

未手动赋值的枚举项会接着上一个枚举项递增。

### 类

在传统方法中，JavaScript通过构造函数实现类的概念，通过原型链实现继承，而在es6中，我们终于迎来的class

#### 类的概念

- 类（Class）：定义了一件事物的抽象特点，包含它的属性和方法
- 对象（Object）：类的实例，通过 `new` 生成
- 面向对象（OOP）的三大特性：封装、继承、多态
- 封装（Encapsulation）：将对数据的操作细节隐藏起来，只暴露对外的接口。外界调用端不需要（也不可能）知道细节，就能通过对外提供的接口来访问该对象，同时也保证了外界无法任意更改对象内部的数据
- 继承（Inheritance）：子类继承父类，子类除了拥有父类的所有特性外，还有一些更具体的特性
- 多态（Polymorphism）：由继承而产生了相关的不同的类，对同一个方法可以有不同的响应。比如 `Cat` 和 `Dog` 都继承自 `Animal`，但是分别实现了自己的 `eat` 方法。此时针对某一个实例，我们无需了解它是 `Cat` 还是 `Dog`，就可以直接调用 `eat` 方法，程序会自动判断出来应该如何执行 `eat`
- 存取器（getter & setter）：用以改变属性的读取和赋值行为
- 修饰符（Modifiers）：修饰符是一些关键字，用于限定成员或类型的性质。比如 `public` 表示公有属性或方法
- 抽象类（Abstract Class）：抽象类是供其他类继承的基类，抽象类不允许被实例化。抽象类中的抽象方法必须在子类中被实现
- 接口（Interfaces）：不同类之间公有的属性或方法，可以抽象成一个接口。接口可以被类实现（implements）。一个类只能继承自另一个类，但是可以实现多个接口

### es6 中类的用法

#### 属性和方法

使用class定义类，使用constructor定义构造函数

通过new生成新实例的时候，会自动调用构造函数

```typescript
class Animal {
    public name;
    constructor(name) {
        this.name = name
    }
    sayHi() {
        return `my name is ${this.name}`
    }
}
let tian = new Animal('Tian')
console.log(tian.sayHi())		// my name Tian
```

#### 类的继承

使用extends关键字实现继承，子类中使用super关键字来调用分类的构造函数和方法

```typescript
class Cat extends Animal {
    constructor(name) {
        super(name);		// 调用父类的构造方法
        console.log(this.name)
    }
    sayHi() {
        return 'Meow,' + super.sayHi()
    }
}
let mango = new Cat('Mongo')
console.log(mongo.sayHi())		// Meow, my name is Mongo
```

#### 存取器

使用getter和setter可以改变属性的赋值和读取行为

```typescript
class Animal {
    constructor(name) {
        this.name = name
    }
    get name() {
        return 'Jack'
    }
    set name(value) {
        console.log('setter: ' + value)
    }
}
let test = new Animal('Kitty')
test.name = 'Tom'		// setter： Tom
console.log(test.name)		// Jack
```

#### 静态方法

使用 static 修饰符修饰的方法称为静态方法，他们不需要实例化，而是直接通过类来调用

```typescript
class Animal {
    static isAnimal(a) {
        return a instanceof Animal
    }
}
lat a = new Animal('Jack')
Animal.isAnimal(a)		// true
a.isAnimal(a)		// error, a.isAnimal is not a function
```

#### es7 中类的用法

ES6 中实例的属性只能通过构造函数中的 `this.xxx` 来定义，ES7 提案中可以直接在类里面定义

ES7 提案中，可以使用 `static` 定义一个静态属性

### typescript 中类的用法

#### public private 和 protected

TypeScript 可以使用三种访问修饰符（Access Modifiers），分别是 `public`、`private` 和 `protected`。

- `public` 修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是 `public` 的
- `private` 修饰的属性或方法是私有的，不能在声明它的类的外部访问
- `protected` 修饰的属性或方法是受保护的，它和 `private` 类似，区别是它在子类中也是允许被访问的

当构造函数修饰为private，该类不允许被继承或者实例化，当构造函数修饰为protected时，该类只允许被继承，不能用super来调用

#### 参数属性

修饰符和`readonly`还可以使用在构造函数参数中，等同于类中定义该属性同时给该属性赋值，使代码更简洁。

只读属性关键字 `readonly`，只允许出现在属性声明或索引签名或构造函数中。

#### 抽象类

`abstract` 用于定义抽象类和其中的抽象方法

抽象类是不允许被实例化的

```typescript
abstract class Animal {
    public name;
    public constructor(name) {
        this.name = name
    }
    public abstract sayHi()
}
let a = new Animal('Jack')		// error， cannot create an instance of the abstract class
```

其次，抽象类中的抽象方法必须被子类实现：

```typescript
abstract class Animal {
    public name;
    public constructor(name) {
        this.name = name
    }
    public abstract sayHi()
}
// 子类实现
class Cat extends Animal {
  public sayHi() {
    console.log(`Meow, My name is ${this.name}`);
  }
}

```

### 类的类型

给类加上typescript中的类型很简单，与接口类似：

```typescript
class Animal {
    name: string;
    constructor(name: string) {
        this.name = name
    }
    sayHi(): string {
        return `my name is ${this.name}`
    }
}
let a:Animal = new Animal('Jack')
console.log(a.sayHi())
```

### 类与接口

接口可以用于对对象的形状进行描述，也可以对类的一部分行为进行抽象

#### 类实现接口

实现（implements）是面向对象中的一个重要概念，一般来说，一个类只能继承自另一个类，有时候不同了类之间可以有一些共有的特性，这时候就可以把特性提取成接口（interfaces），用 implements 关键字来实现，这个特性大大提高了面向对象的灵活性

举例来说，门是一个类，防盗门是门的子类。如果防盗门有一个报警器的功能，我们可以简单的给防盗门添加一个报警方法。这时候如果有另一个类，车，也有报警器的功能，就可以考虑把报警器提取出来，作为一个接口，防盗门和车都去实现它

```typescript
interface Alarm {
	alert(): void
}
class Door {}
class SecurityDoor extends Door implements Alarm {
    alert() {
        console.log('SecurityDoor alert')
    }
}
class Car implements Alarm {
    alert() {
        console.log('Car alert')
    }
}
```

一个类可以实现多个接口

```typescript
interface Alarm {
    alert(): void
}
interface Light {
    lightOn(): void;
    lightOff(): void;
}
class Car implements Alarm, Light {
    alert() {
        console.log('Car alert')
    }
    lightOn() {
        console.log('Car light on')
    }
    lightOff() {
        console.log('Car light off')
    }
}
```

#### 接口继承接口

接口与接口之间可以是继承关系：

```ts
interface Alarm {
    alert(): void;
}

interface LightableAlarm extends Alarm {
    lightOn(): void;
    lightOff(): void;
}
```

#### 接口继承类

常见的面向对象语言中，接口是不能继承类的，但是在 TypeScript 中却是可以的：

```ts
class Point {
    x: number;
    y: number;
    constructor(x: number, y: number) {
        this.x = x;
        this.y = y;
    }
}

interface Point3d extends Point {
    z: number;
}

let point3d: Point3d = {x: 1, y: 2, z: 3};
```

当我们在声明 class Point 时，除了会创建一个名为 Point 的类之外，同时也创建了一个名为 Point 的类型

「接口继承类」和「接口继承接口」没有什么本质的区别

### 泛型

泛型（Generics）是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性

#### 简单的例子

首先，我们来实现一个函数 `createArray`，它可以创建一个指定长度的数组，同时将每一项都填充一个默认值：

```ts
function createArray(length: number, value: any): Array<any> {
    let result = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']
```

这个函数使用了数组泛型来定义返回值的类型，这段代码有一个缺陷，它并没有准确的定义返回值的类型：

`Array<any>`允许数组的每一项都是任意类型，但是实际上我们希望每一项都是 value 的类型

```ts
function createArray<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for(let i = 0; i < length; ++i) {
        result[i] = value
    }
    return result
}
createArray<string>(3, 'x')		// ['x', 'x', 'x']
```

 调用的时候可以不指定 T 的具体类型，而让类型推论自动推论出来

#### 多个类型参数

定义泛型的时候，可以一次定义多个类型参数：

```ts
function swap<T, U>(tuple: [T, U]): [U, T] {
    return [tuple[1], tuple[0]]
}
swap([7, 'seven'])		// ['seven', 7]
```

#### 泛型约束

再函数内部使用泛型变量的时候，由于事先不指定它是那种类型，所以不能随意的操作它的属性

```ts
function loggingIdentity<T>(arg: T): T{
    console.log(arg.length)
    return arg
}
// error, Property 'length' does not exist on type 'T'
```

泛型 T 不一定包含属性 length，所以编译的时候报错了，这时候我们可以对泛型进行约束，只允许这个函数传入那些包含length属性的变量，这就是泛型约束：

```ts
interface Lengthwise {
    length: number
}
function loggingIdentity<T extends Lengthwise>(arg: T): T{
    console.log(arg.length)
    return arg
}
```

我们使用了 extends 约束了泛型 T 必须符合接口 Lengthwise 的形状，也就是必须包含 length 属性

此时如果调用 `loggingIdentity` 的时候，传入的 `arg` 不包含 `length`，那么在编译阶段就会报错了

多个类型参数之间也可以互相约束：

```ts
function copyFields<T extends U, U>(target: T, source: U): T {
    for (let id in source) {
        target[id] = (<T>source)[id];
    }
    return target;
}

let x = { a: 1, b: 2, c: 3, d: 4 };

copyFields(x, { b: 10, d: 20 });
```

上例中，我们使用了两个类型参数，其中要求 `T` 继承 `U`，这样就保证了 `U` 上不会出现 `T` 中不存在的字段。

#### 泛型接口

可以使用接口的方式来定义一个函数需要符合的形状：

```ts
interface SearchFunc {
  (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
    return source.search(subString) !== -1;
}
```

当然也可以使用含有泛型的接口来定义函数的形状：

```ts
interface CreateArrayFunc {
    <T>(length: number, value: T):Array<T>
}
```

#### 泛型类

与泛型接口类似，泛型也可以用于类的类型定义汇总：

```ts
class GenericNumber <T> {
    zeorValue: T;
    add: (x: T, y:T) => T
}
let myGenericNumber = new GentericNumber<number>();
myGenericNumber.zeroValue = 0
myGenericNumber.add = function(x, y) {return x + y};
```

#### 泛型参数的默认类型

在 TypeScript 2.3 以后，我们可以为泛型中的类型参数指定默认类型。当使用泛型时没有在代码中直接指定类型参数，从实际值参数中也无法推测出时，这个默认类型就会起作用。

```ts
function createArray<T = string>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}
```

### 声明合并

如果定义了两个相同名字的函数、接口或类，它们会合并成一个类型

合并的属性的类型必须是唯一的

## 工程

掌握了typescript的语法就像是学会了砌墙的工艺

### 代码检查

代码检查主要是用来发现代码错误，同意代码风格，eslint 用的比较多

### 编译选项

TypeScript 提供了非常多的编译选项，但是官方文档对每一项的解释很抽象，这一章会详细介绍每一个选项的作用，并给出对应的示例。