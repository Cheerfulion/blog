---
title: 【前端】Vue3.0+TS仿知乎企业级项目
date: 2021-07-12 10:20:50
tags:
  - 前端
---



## 第1章 课程介绍 

**课程链接：**

https://coding.imooc.com/class/449.html



**课程文档：**

http://docs.vikingship.xyz/



**Typescript推荐文档：**

https://itdashu.com/docs/typescriptlesson.html

https://www.wenjiangs.com/docs/typescript-introduction-2



## [第2章 你好 Typescript： 进入类型的世界](http://docs.vikingship.xyz/typescript.html#安装-typescript)

### 开始

除了课程中的内容，结合自己的一些了解对齐做了补充。另外这里推荐一个也还不错的Typescript学习的教程（和慕课的课程做了下比较，这里更为详尽）, 即第一章的Typescript推荐文档。



### 为什么要使用Typescript？

**静态类型**

回答这个问题前，先来看看下面这些 JavaScript 中的常见错误：

- Uncaught TypeError: Cannot read property
- TypeError: ‘undefined’ is not an object
- TypeError: null is not an object
- TypeError: Object doesn’t support property
- TypeError: ‘undefined’ is not a function
- TypeError: Cannot read property ‘length’

仔细看下不难发现，这些错误大都是一些比较初级的类型错误。

JavaScript 只会在 `运行时` 才去做数据类型检查，而 TypeScript 作为静态类型语言，其数据类型是在 `编译期间` 确定的，编写代码的时候要明确变量的数据类型。使用 TypeScript 后，这些低级错误将不再发生。

**越来越多前端框架和第三方库对TS的支持**

我们学习一门新技术会关心它的生命力问题，如果这门技术在较短时间内就要被淘汰，那花费大量的时间学习也是不划算的。TypeScript 能够保持长久生命力的另一个原因，就是统治前端的三大框架对 TypeScript 的支持。

- Angular 是 TypeScript 最早的支持者，Angular 官方推荐使用 TypeScript 来创建应用。
- React 虽然经常与 Flow 一起使用，但是对 TypeScript 的支持也很友好。
- Vue3.0 正式版即将发布，由 TypeScript 编写。

从国内的氛围来看，由前端三大框架引领的 TypeScript 热潮已经涌来，很多招聘要求上也都有了 TypeScript 的身影。

越来越多耳熟能详的大项目都使用了typescript，如Vue3.0, Angular, Ant-design, nextJs等。State of JS权威调查报告显示，2019年58%的开发者正在使用并愿意继续使用Typescript。

**兼容 JavaScript 的灵活性**

TypeScript 虽然严谨，但没有丧失 JavaScript 的灵活性，TypeScript 非常包容：

- TypeScript 通过 `tsconfig.json` 来配置对类型的检查严格程度。
- 可以把 `.js` 文件直接重命名为 `.ts`。
- 可以通过将类型定义为 `any` 来实现兼容任意类型。
- 即使 TypeScript 编译错误，也可以生成 JavaScript 文件。



TS的当然也是缺点的，如增加了一些学习成本，短期内增加了开发成本等。所以在项目中可以根据情况是否使用Typescript。对于中大型项目来说，使用Typescript长期收益是更为明显的，但是对于小型项目或者不需要维护的项目，就没有必要增加开发的时间了。



**编程语言类型**

动态类型语言（Dynamically Typed Language）: 运行期间才会进行类型检查的语言，代表JS，PHP, Python, C#等。这种语言只有在运行的时候才会发现错误。

静态类型语言（Statically Typed Language）：编译期间进行类型检测的语言，代表TS, Java，C, C++等。



### 安装 Typescript

**Typescript 官网地址**: https://www.typescriptlang.org/zh/

使用 nvm 来管理 node 版本: https://github.com/nvm-sh/nvm

安装 Typescript:

```bash
npm install -g typescript
```

使用 tsc 全局命令：

```bash
# 查看 tsc 版本
tsc -v
# 编译 ts 文件 成 js 文件
tsc fileName.ts
```

安装Typescript调试工具

```bash
npm install -g ts-node
# 该ts-node和ts-node-dev类似node 和 nodemon的关系， 使用ts-node-dev可以监听文件变化自动更新
npm install -g ts-node-dev
```

使用 ts-node 全局命令：

```bash
# 查看 ts-node 版本
tsc -v
# 编译 ts 文件并在控制台运行
ts-node fileName.ts
```



### 原始数据类型

Typescript 文档地址：[Basic Types](https://www.typescriptlang.org/docs/handbook/basic-types.html#boolean)

Javascript 类型分类：

原始数据类型 (primitive values)：

- Boolean
- Null
- Undefined
- Number
- BigInt
- String
- Symbol

```javascript
let isDone: boolean = false

// 接下来来到 number，注意 es6 还支持2进制和8进制，让我们来感受下

let age: number = 10
// 二进制0b开头
let binaryNumber: number = 0b1111
// 八进制0开头
let octonaryNumber: number = 070
// 十六进制0x开头
let hexNumber: number = 0x36

// 之后是字符串，注意es6新增的模版字符串也是没有问题的
let firstName: string = 'viking'
let message: string = `Hello, ${firstName}, age is ${age}`

// 还有就是两个奇葩兄弟两，undefined 和 null
let u: undefined = undefined
let n: null = null

// 注意 undefined 和 null 是所有类型的子类型。也就是说 undefined 类型的变量，可以赋值给 number 类型的变量：
let num: number = undefined
```

[any 类型](https://www.typescriptlang.org/docs/handbook/basic-types.html#any)

```javascript
let notSure: any = 4
notSure = 'maybe it is a string'
notSure = 'boolean'
// 在任意值上访问任何属性都是允许的：
notSure.myName
// 也允许调用任何方法：
notSure.getName()
```

### Array 和 Tuple

Typescript 文档地址：[Array 和 Tuple](https://www.typescriptlang.org/docs/handbook/basic-types.html#array)

```javascript
//最简单的方法是使用「类型 + 方括号」来表示数组：
let arrOfNumbers: number[] = [1, 2, 3, 4]
//数组的项中不允许出现其他的类型：
//数组的一些方法的参数也会根据数组在定义时约定的类型进行限制：
arrOfNumbers.push(3)
arrOfNumbers.push('abc')

// 元组的表示和数组非常类似，只不过它将类型写在了里面 这就对每一项起到了限定的作用
// 元组在编译后的JavaScript中还是一个数组，可以用于数组不同元素类型限定
let user: [string, number] = ['viking', 20]
// 但是当我们写少一项 就会报错 同样写多一项也会有问题
user = ['molly', 20, true]
// 元组可以使用数组的方法，但是这里只能添加字符串和数值类型（联合类型）
user.push('123')
```

### interface 接口

Typescript 文档地址：[Interface](https://www.typescriptlang.org/docs/handbook/interfaces.html)

Duck Typing（interface 接口称号） 概念：

> 如果某个东西长得像鸭子，像鸭子一样游泳，像鸭子一样嘎嘎叫，那它就可以被看成是一只鸭子。

```javascript
// 我们定义了一个接口 Person
interface Person {
  name: string;
  age: number;
}
// 接着定义了一个变量 viking，它的类型是 Person。这样，我们就约束了 viking 的形状必须和接口 Person 一致。
let viking: Person ={
  name: 'viking',
  age: 20
}

// 有时我们希望不要完全匹配一个形状，那么可以用可选属性（?）：
interface Person {
    name: string;
    age?: number;
}
let viking: Person = {
    name: 'Viking'
}

// 接下来还有只读属性，有时候我们希望对象中的一些字段只能在创建的时候被赋值，那么可以用 readonly 定义只读属性
interface Person {
  readonly id: number;
  name: string;
  age?: number;
}
// Cannot assign to 'id' becasuse it is read-only property.
viking.id = 9527
```

### 函数

Typescript 文档地址：[Functions](https://www.typescriptlang.org/docs/handbook/functions.html)

```javascript
// 来到我们的第一个例子，约定输入，约定输出
function add(x: number, y: number): number {
  return x + y
}
// 可选参数
function add(x: number, y: number, z?: number): number {
  if (typeof z === 'number') {
    return x + y + z
  } else {
    return x + y
  }
}

// 函数本身的类型
const add2: (x: number, y: number, z?:number) => number = add

// interface 描述函数类型
const sum = (x: number, y: number) => {
  return x + y
}
interface ISum {
  (x: number, y: number): number
}
const sum2: ISum = sum
```

### 类型推论，联合类型 和 类型断言

Typescript 文档地址：[类型推论 - type inference](https://www.typescriptlang.org/docs/handbook/type-inference.html)

[联合类型 - union types](https://www.typescriptlang.org/docs/handbook/unions-and-intersections.html#union-types)

```javascript
// 我们只需要用中竖线来分割两个
let numberOrString: number | string 
// 当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们只能访问此联合类型的所有类型里共有的属性或方法：
numberOrString.length
numberOrString.toString()
```

[类型断言 - type assertions](https://www.typescriptlang.org/docs/handbook/basic-types.html#type-assertions)

```javascript
// 这里我们可以用 as 关键字，告诉typescript 编译器，你没法判断我的代码，但是我本人很清楚，这里我就把它看作是一个 string，你可以给他用 string 的方法。
function getLength(input: string | number): number {
  const str = input as string
  if (str.length) {
    return str.length
  } else {
    const number = input as number
    return number.toString().length
  }
}
```

[类型守卫 - type guard](https://www.typescriptlang.org/docs/handbook/advanced-types.html#type-guards-and-differentiating-types)

```javascript
// typescript 在不同的条件分支里面，智能的缩小了范围，这样我们代码出错的几率就大大的降低了。
function getLength2(input: string | number): number {
  if (typeof input === 'string') {
    return input.length
  } else {
    return input.toString().length
  }
}
```

### Class 类

面向对象编程（OOP）的三大特点

- **封装（Encapsulation）**：将对数据的操作细节隐藏起来，只暴露对外的接口。外界调用端不需要（也不可能）知道细节，就能通过对外提供的接口来访问该对象，
- **继承（Inheritance）**：子类继承父类，子类除了拥有父类的所有特性外，还有一些更具体的特性。
- **多态（Polymorphism）**：由继承而产生了相关的不同的类，对同一个方法可以有不同的响应。

[类 - Class](https://www.typescriptlang.org/docs/handbook/classes.html)

```javascript
class Animal {
  name: string;
  constructor(name: string) {
    this.name = name
  }
  run() {
    return `${this.name} is running`
  }
}
const snake = new Animal('lily')

// 继承的特性
class Dog extends Animal {
  bark() {
    return `${this.name} is barking`
  }
}

const xiaobao = new Dog('xiaobao')
console.log(xiaobao.run())
console.log(xiaobao.bark())

// 这里我们重写构造函数，注意在子类的构造函数中，必须使用 super 调用父类的方法，要不就会报错。
class Cat extends Animal {
  constructor(name) {
    super(name)
    console.log(this.name)
  }
  run() {
    return 'Meow, ' + super.run()
  }
}
const maomao = new Cat('maomao')
console.log(maomao.run())
```

[类成员的访问修饰符](https://www.typescriptlang.org/docs/handbook/classes.html#public-private-and-protected-modifiers)

- **public** 修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是 public 的
- **private** 修饰的属性或方法是私有的，不能在声明它的类的外部访问
- **protected** 修饰的属性或方法是受保护的，它和 private 类似，区别是它在子类中也是允许被访问的

### 类与接口

[类实现一个接口](https://www.typescriptlang.org/docs/handbook/interfaces.html#class-types)

```javascript
interface Radio {
  switchRadio(trigger: boolean): void;
}
class Car implements Radio {
  switchRadio(trigger: boolean) {
    return 123
  }
}
class Cellphone implements Radio {
  switchRadio(trigger: boolean) {
  }
}

interface Battery {
  checkBatteryStatus(): void;
}

// 要实现多个接口，我们只需要中间用 逗号 隔开即可。
class Cellphone implements Radio, Battery {
  switchRadio(trigger: boolean) {
  }
  checkBatteryStatus() {

  }
}

// 接口继承
interface RadioWithBattery extends Radio {
  checkBatteryStatus(): void;
}
class Cellphone implements RadioWithBattery {
  switchRadio(trigger: boolean) {
  }
  checkBatteryStatus() {

  }
}
```

### 枚举 Enums

[枚举 Enums](https://www.typescriptlang.org/docs/handbook/enums.html)

```javascript
// 数字枚举，一个数字枚举可以用 enum 这个关键词来定义，我们定义一系列的方向，然后这里面的值，枚举成员会被赋值为从 0 开始递增的数字,
enum Direction {
  Up,
  Down,
  Left,
  Right,
}
console.log(Direction.Up)

// 还有一个神奇的点是这个枚举还做了反向映射
console.log(Direction[0])
// 如果指定 Up = 10, 那么枚举的起始下标就是10了

// 字符串枚举
enum Direction {
  Up = 'UP',
  Down = 'DOWN',
  Left = 'LEFT',
  Right = 'RIGHT',
}
const value = 'UP'
if (value === Direction.Up) {
  console.log('go up!')
}
    
// 常量值可以使用常量枚举，可以优化性能（编译出来的代码更简洁）
const enum Direction { ……
```

### 泛型 Generics（重难点）

[泛型 Generics](https://www.typescriptlang.org/docs/handbook/generics.html)

泛型（Generics）是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性。

```javascript
function echo(arg) {
  return arg
}
const result = echo(123)
// 这时候我们发现了一个问题，我们传入了数字，但是返回了 any

function echo<T>(arg: T): T {
  return arg
}
const result = echo(123)

// 泛型也可以传入多个值
function swap<T, U>(tuple: [T, U]): [U, T] {
  return [tuple[1], tuple[0]]
}

const result = swap(['string', 123])
```

### 泛型第二部分 - 泛型约束

在函数内部使用泛型变量的时候，由于事先不知道它是哪种类型，所以不能随意的操作它的属性或方法

```javascript
function echoWithArr<T>(arg: T): T {
  console.log(arg.length)
  return arg
}

// 上例中，泛型 T 不一定包含属性 length，我们可以给他传入任意类型，当然有些不包括 length 属性，那样就会报错

interface IWithLength {
  length: number;
}
function echoWithLength<T extends  >(arg: T): T {
  console.log(arg.length)
  return arg
}

echoWithLength('str')
const result3 = echoWithLength({length: 10})
const result4 = echoWithLength([1, 2, 3])
```

### 泛型第三部分 - 泛型与类和接口

```javascript
class Queue {
  private data = [];
  push(item) {
    return this.data.push(item)
  }
  pop() {
    return this.data.shift()
  }
}

const queue = new Queue()
queue.push(1)
queue.push('str')
console.log(queue.pop().toFixed())
console.log(queue.pop().toFixed())

//在上述代码中存在一个问题，它允许你向队列中添加任何类型的数据，当然，当数据被弹出队列时，也可以是任意类型。在上面的示例中，看起来人们可以向队列中添加string 类型的数据，但是那么在使用的过程中，就会出现我们无法捕捉到的错误，

class Queue<T> {
  private data = [];
  push(item: T) {
    return this.data.push(item)
  }
  pop(): T {
    return this.data.shift()
  }
}
const queue = new Queue<number>()

//泛型和 interface
interface KeyPair<T, U> {
  key: T;
  value: U;
}

let kp1: KeyPair<number, string> = { key: 1, value: "str"}
let kp2: KeyPair<string, number> = { key: "str", value: 123}
```

### 类型别名 和 交叉类型

[类型别名 Type Aliases](https://www.typescriptlang.org/docs/handbook/advanced-types.html#type-aliases)

类型别名，就是给类型起一个别名，让它可以更方便的被重用。

```javascript
let sum: (x: number, y: number) => number
const result = sum(1,2)
type PlusType = (x: number, y: number) => number
let sum2: PlusType

// 支持联合类型
type StrOrNumber = string | number
let result2: StrOrNumber = '123'
result2 = 123

// 字符串字面量
type Directions = 'Up' | 'Down' | 'Left' | 'Right'
let toWhere: Directions = 'Up'
const str: 'name' = 'name'  
// 该方式只能传入'name'字符串， 相当于：
type Name = 'name'
const str: Name = 'name'

```

[交叉类型 Intersection Types](https://www.typescriptlang.org/docs/handbook/unions-and-intersections.html#intersection-types)

```javascript
interface IName  {
  name: string
}
type IPerson = IName & { age: number }
let person: IPerson = { name: 'hello', age: 12}
```

### 声明文件

[声明文件](https://www.typescriptlang.org/docs/handbook/declaration-files/introduction.html)

[@types 官方声明文件库](https://github.com/DefinitelyTyped/DefinitelyTyped/) [@types 搜索声明库](https://microsoft.github.io/TypeSearch/)

```typescript
// 通常写在定义文件里面（如：jQuery.d.ts），仅仅用于编译检查，并没有实现，要通过script标签引入jQuery才行。
// 这里只是举例说明用法，实际上会有很多声明变量才对，如jquery就可以用 npm install -S @types/jquery 来安装jquery的类型声明文件
// 更多源代码不是用ts写的库可以在这里搜索它的声明文件：https://microsoft.github.io/TypeSearch/
declare var jQuery: (selector: string) => any;
// 通过声明文件，这里编译就不会报错了。
jQuery('#foo')
```



### 内置类型

[内置类型](https://github.com/Microsoft/TypeScript/tree/master/src/lib)

```javascript
// global objects
const a: Array<number> = [1,2,3]
// 大家可以看到这个类型，不同的文件中有多处定义，但是它们都是 内部定义的一部分，然后根据不同的版本或者功能合并在了一起，一个interface 或者 类多次定义会合并在一起。这些文件一般都是以 lib 开头，以 d.ts 结尾，告诉大家，我是一个内置对象类型
const date: Date = new Date()
const reg = /abc/

// 我们还可以使用一些 build in object，内置对象，比如 Math 与其他全局对象不同的是，Math 不是一个构造器。Math 的所有属性与方法都是静态的。
Math.pow(2,2)

// DOM 和 BOM 标准对象
// document 对象，返回的是一个 HTMLElement
let body: HTMLElement = document.body
// document 上面的query 方法，返回的是一个 nodeList 类型
let allLis: NodeList = document.body.querySelectorAll('li')

//当然添加事件也是很重要的一部分，document 上面有 addEventListener 方法，注意这个回调函数，因为类型推断，这里面的 e 事件对象也自动获得了类型，这里是个 mouseEvent 类型，因为点击是一个鼠标事件，现在我们可以方便的使用 e 上面的方法和属性。
document.addEventListener('click', (e) => {
  e.preventDefault()
})
```

[Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html)

Typescript 还提供了一些功能性，帮助性的类型，这些类型，大家在 js 的世界是看不到的，这些类型叫做 utility types，提供一些简洁明快而且非常方便的功能。

```javascript
// partial，它可以把传入的类型都变成可选
interface IPerson {
  name: string
  age: number
}
let viking: IPerson = { name: 'viking', age: 20 }
type IPartial = Partial<IPerson>
let viking2: IPartial = { }

// Omit，它返回的类型可以忽略传入类型的某个属性
type IOmit = Omit<IPerson, 'name'>
let viking3: IOmit = { age: 20 }
```



## [第3章 初识 Vue3.0： 新特性详解](http://docs.vikingship.xyz/vue3-basic.html)

### 开始

Vue3 的文档地址: https://v3.cn.vuejs.org/



1. Vue3：两年开发，99位贡献者，2600次提交，628次Pull Request

2. Vue3 支持 2 的大多数特性

3. 性能提升
   - 打包大小减少 41%
   - 初次渲染快 55%，更新快 133%
   - 内存使用减少 54%

4. Composition API （组合API）
   - ref 和 reactive
   - computed 和 watch
   - 新的生命周期函数
   - 自定义函数 - Hook 函数
5. 其他新增特性
   - Teleport - 瞬移组件的位置
   - Suspense - 异步加载组件的新福音
   - 全局API的修改和优化
   - 更多的试验性特性
6. 更好的 Typescript 支持
7. 为什么要有 Vue3.0?
   - Vue2 中，随着功能的增长，复杂组件的代码维护变得难以维护。（由于Vue2组件变量方法都在一个对象里面，逻辑多的时候不方便查找相关的方法或属性，在Vue2用通常用mixin来解决，但是mixin虽然把相同的逻辑放到一起了，但是又命名冲突和不方便复用问题）
   - Vue2 对于 Typescript 的支持非常的有限

### 配置 vue3 开发环境

[Vue cli](https://cli.vuejs.org/zh/)

```javascript
// 安装或者升级
npm install -g @vue/cli
# OR
yarn global add @vue/cli

// 保证 vue cli 版本在 4.5.0 以上
vue --version

// 创建项目
vue create my-project
```

然后的步骤

- Please pick a preset - 选择 **Manually select features**
- Check the features needed for your project - 多选择上 **TypeScript**，特别注意点空格是选择，点回车是下一步
- Choose a version of Vue.js that you want to start the project with - 选择 **3.x (Preview)**
- Use class-style component syntax - 输入 **n**，回车
- Use Babel alongside TypeScript - 输入**n**，回车
- Pick a linter / formatter config - 直接回车
- Pick additional lint features - 直接回车
- Where do you prefer placing config for Babel, ESLint, etc.? - 直接回车
- Save this as a preset for future projects? - 输入**n**，回车

启动图形化界面创建

```text
vue ui
```



**遇到问题：**

这里我启动会报错： `ERROR  Error: Cannot find module 'vue-loader-v16/package.json'`， 根据网上的教程安装 `npm i -D vue-loader-v16`并且升级npm后还是报错，变成了 `ERROR  Error: Cannot find module 'fork-ts-checker-webpack-plugin-v5'`

解决方法：重新创建项目（导致原因不清楚，这里我重新创建项目就好了，现在想想可能是npm更新后应该要重新开启一个cmd来让npm最新版本生效。另外，npm经常会遇到这种依赖的问题，如果不是开发自己的插件的话，我觉得用yarn来管理包会好点）



### 项目结构和插件

推荐给大家安装的插件

**[Eslint 插件](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)**

如果 eslint 不生效，可以在根目录创建 .vscode 文件夹，然后在文件夹中创建 settings.json 然后输入

```json
{
  "eslint.validate": [
    "typescript"
  ]
}
```

**[Vetur 插件](https://marketplace.visualstudio.com/items?itemName=octref.vetur)**

### Ref 语法

[setup 方法](https://v3.cn.vuejs.org/guide/composition-api-introduction.html#setup-component-option)

[ref 函数](https://v3.cn.vuejs.org/guide/reactivity-fundamentals.html#creating-standalone-reactive-values-as-refs)

```vue
<template>
  <h1>{{count}}</h1>
  <h1>{{double}}</h1>
  <button @click="increase">+1</button>
</template>

<script lang="ts">
import { ref, computed } from "vue"

export default {
  name: 'App',
  setup() {
    // ref 是一个函数，它接受一个参数，返回的就是一个神奇的 响应式对象 。我们初始化的这个 0 作为参数包裹到这个对象中去，在未来可以检测到改变并作出对应的相应。
    const count = ref(0)
    const double = computed(() => {
      return count.value * 2
    })
    const increase = () => {
      // 注意在这里count的值存储在count.value
      count.value++
    }
    return {
      count,
      increase,
      double
    }
  }
}

</script>
```

### Reactive 函数

[Reactive 函数](https://v3.cn.vuejs.org/guide/reactivity-fundamentals.html#declaring-reactive-state)

```javascript
import { ref, computed, reactive, toRefs } from 'vue'

interface DataProps {
  count: number;
  double: number;
  increase: () => void;
}

setup() {
  const data: DataProps  = reactive({
    count: 0,
    increase: () => { data.count++},
    double: computed(() => data.count * 2)
  })
  const refData = toRefs(data)
  return {
    ...refData
  }
}
```

使用 ref 还是 reactive 可以选择这样的准则

- 第一，就像刚才的原生 javascript 的代码一样，像你平常写普通的 js 代码选择原始类型和对象类型一样来选择是使用 ref 还是 reactive。
- 第二，所有场景都使用 reactive，但是要记得使用 toRefs 保证 reactive 对象属性保持响应性。

### Vue3 生命周期

[生命周期](https://v3.cn.vuejs.org/guide/composition-api-lifecycle-hooks.html)

在 setup 中使用的 hook 名称和原来生命周期的对应关系

- beforeCreate -> 不需要
- created -> 不需要
- beforeMount -> onBeforeMount
- mounted -> onMounted
- beforeUpdate -> onBeforeUpdate
- updated -> onUpdated
- beforeUnmount -> onBeforeUnmount
- unmounted -> onUnmounted
- errorCaptured -> onErrorCaptured
- renderTracked -> onRenderTracked
- renderTriggered -> onRenderTriggered

```javascript
setup() {
  onMounted(() => {
    console.log('mounted')
  })
  onUpdated(() => {
    console.log('updated')
  })
  onRenderTriggered((event) => {
    console.log(event)
  })
}
```

### 侦测变化 - watch

[Watch 文档地址](https://v3.cn.vuejs.org/guide/reactivity-computed-watchers.html#watch)

```javascript
// watch 简单应用
watch(data, () => {
  document.title = 'updated ' + data.count
})
// watch 的两个参数，代表新的值和旧的值
watch(refData.count, (newValue, oldValue) => {
  console.log('old', oldValue)
  console.log('new', newValue)
  document.title = 'updated ' + data.count
})

// watch 多个值，返回的也是多个值的数组
watch([greetings, data], (newValue, oldValue) => {
  console.log('old', oldValue)
  console.log('new', newValue)
  document.title = 'updated' + greetings.value + data.count
})

// 使用 getter 的写法 watch reactive 对象中的一项
watch([greetings, () => data.count], (newValue, oldValue) => {
  console.log('old', oldValue)
  console.log('new', newValue)
  document.title = 'updated' + greetings.value + data.count
})
```

### 模块化开发 第一部分 鼠标追踪器

```javascript
// 在单组件内添加对应逻辑
const x = ref(0)
const y = ref(0)
const updateMouse = (e: MouseEvent) => {
  x.value = e.pageX
  y.value = e.pageY
}
onMounted(() => {
  document.addEventListener('click', updateMouse)
})
onUnmounted(() => {
  document.removeEventListener('click', updateMouse)
})

// 将组件内逻辑抽象成可复用的函数
function useMouseTracker() {
  // const positions = reactive<MousePostion>({
  //   x: 0,
  //   y: 0
  // })
  const x = ref(0)
  const y = ref(0)
  const updatePosition = (event: MouseEvent) => {
    x.value = event.clientX
    y.value = event.clientY 
  }
  onMounted(() => {
    document.addEventListener('click', updatePosition)
  })
  onUnmounted(() => {
    document.removeEventListener('click', updatePosition)
  })
  return { x, y }
}

export default useMouseTracker
```

**vue3 这种实现方式的优点**

- 第一：它可以清楚的知道 xy 这两个值的来源，这两个参数是干什么的，他们来自 useMouseTracker 的返回，那么它们就是用来追踪鼠标位置的值。
- 第二：我们可以xy 可以设置任何别名，这样就避免了命名冲突的风险。
- 第三：这段逻辑可以脱离组件存在，因为它本来就和组件的实现没有任何关系，我们不需要添加任何组件实现相应的功能。只有逻辑代码在里面，不需要模版。

### 模块化难度上升 - useURLLoader

[axios 文档地址](https://github.com/axios/axios)

```bash
// 安装 axios 注意它是自带 type 文件的，所以我们不需要给它另外安装 typescript 的定义文件
npm install axios --save
import { ref } from 'vue'
import axios from 'axios'
// 添加一个参数作为要使用的 地址
const useURLLoader = (url: string) => {
// 声明几个ref，代表不同的状态和结果
  const result = ref(null)
  const loading = ref(true)
  const loaded = ref(false)
  const error = ref(null)

// 发送异步请求，获得data
// 由于 axios 都有定义，所以rawData 可以轻松知道其类型
  axios.get(url).then((rawData) => {
    loading.value = false
    loaded.value = true
    result.value = rawData.data
  }).catch((e) => {
    error.value = e
  })
  // 将这些ref 一一返回
  return {
    result,
    loading,
    error,
    loaded
  }
}


export default useURLLoader
```

[免费获取狗狗图片的 API 地址](https://dog.ceo/api/breeds/image/random)

```javascript
// 使用 urlLoader 展示狗狗图片
const { result, loading, loaded } = useURLLoader('https://dog.ceo/api/breeds/image/random')

...
<h1 v-if="loading">Loading!...</h1>
<img v-if="loaded" :src="result.message" >
```

### 模块化结合typescript - 泛型改造

```javascript
// 为函数添加泛型
function useURLLoader<T>(url: string) {
  const result = ref<T | null>(null)
// 在应用中的使用，可以定义不同的数据类型
interface DogResult {
  message: string;
  status: string;
}
interface CatResult {
  id: string;
  url: string;
  width: number;
  height: number;
}

// 免费猫图片的 API  https://api.thecatapi.com/v1/images/search?limit=1
const { result, loading, loaded } = useURLLoader<CatResult[]>('https://api.thecatapi.com/v1/images/search?limit=1')
```

### 使用 defineComponent 包裹组件

[defineComponent 文档地址](https://v3.cn.vuejs.org/api/global-api.html#definecomponent)

### Teleport - 瞬间移动 第一部分

[Teleport 文档地址](https://v3.cn.vuejs.org/guide/teleport.html)

```javascript
<template>
// vue3 新添加了一个默认的组件就叫 Teleport，我们可以拿过来直接使用，它上面有一个 to 的属性，它接受一个css query selector 作为参数，这就是代表要把这个组件渲染到哪个 dom 元素中
  <teleport to="#modal">
    <div id="center">
      <h1>this is a modal</h1>
    </div>
  </teleport>
</template>
<style>
  #center {
    width: 200px;
    height: 200px;
    border: 2px solid black;
    background: white;
    position: fixed;
    left: 50%;
    top: 50%;
    margin-left: -100px;
    margin-top: -100px;
  }
</style>
```

### Teleport - 瞬间移动 第二部分

Modal 组件

```vue
<template>
<teleport to="#modal">
  <div id="center" v-if="isOpen">
    <h2><slot>this is a modal</slot></h2>
    <button @click="buttonClick">Close</button>
  </div>
</teleport>
</template>
<script lang="ts">
import { defineComponent } from 'vue'
export default defineComponent({
  props: {
    isOpen: Boolean,
  },
  emits: {
    'close-modal': null
  },
  setup(props, context) {
    const buttonClick = () => {
      context.emit('close-modal')
    }
    return {
      buttonClick
    }
  }
})
</script>
<style>
  #center {
    width: 200px;
    height: 200px;
    border: 2px solid black;
    background: white;
    position: fixed;
    left: 50%;
    top: 50%;
    margin-left: -100px;
    margin-top: -100px;
  }
</style>
```

在 App 组件中使用

```javascript
const modalIsOpen = ref(false)
const openModal = () => {
  modalIsOpen.value = true
}
const onModalClose = () => {
  modalIsOpen.value = false
}

<button @click="openModal">Open Modal</button><br/>
<modal :isOpen="modalIsOpen" @close-modal="onModalClose"> My Modal !!!!</modal>
```

### Suspense - 异步请求好帮手第一部分

定义一个异步组件，在 setup 返回一个 Promise，AsyncShow.vue

```vue
<template>
  <h1>{{result}}</h1>
</template>
<script lang="ts">
import { defineComponent } from 'vue'
export default defineComponent({
  setup() {
    return new Promise((resolve) => {
      setTimeout(() => {
        return resolve({
          result: 42
        })
      }, 3000)
    })
  }
})
</script>
```

在 App 中使用

```html
<Suspense>
  <template #default>
    <async-show />
  </template>
  <template #fallback>
    <h1>Loading !...</h1>
  </template>
</Suspense>
```

### Suspense - 异步请求好帮手第二部分

使用 async await 改造一下异步请求, 新建一个 DogShow.vue 组件

```vue
<template>
  <img :src="result && result.message">
</template>

<script lang="ts">
import axios from 'axios'
import { defineComponent } from 'vue'
export default defineComponent({
  async setup() {
    const rawData = await axios.get('https://dog.ceo/api/breeds/image')
    return {
      result: rawData.data
    }
  }
})
</script>
```

Suspense 中可以添加多个异步组件

```html
<Suspense>
  <template #default>
    <async-show />
    <dog-show />
  </template>
  <template #fallback>
    <h1>Loading !...</h1>
  </template>
</Suspense>
```

### 全局 API 修改

[Global API Change](https://v3.cn.vuejs.org/guide/migration/global-api.html#global-api)

Vue2 的全局配置

```javascript
import Vue from 'vue'
import App from './App.vue'

Vue.config.ignoredElements = [/^app-/]
Vue.use(/* ... */)
Vue.mixin(/* ... */)
Vue.component(/* ... */)
Vue.directive(/* ... */)

Vue.prototype.customProperty = () => {}

new Vue({
  render: h => h(App)
}).$mount('#app')
```

Vue2 这样写在一定程度上修改了 Vue 对象的全局状态。

- 第一，在单元测试中，全局配置非常容易污染全局环境，用户需要在每次 case 之间，保存和恢复配置。有一些 api （vue use vue mixin）甚至没有方法恢复配置，这就让一些插件的测试非常的困难。
- 第二，在不同的 APP 中，如果想共享一份有不同配置的 vue 对象，也变得非常困难。

**Vue3 的修改**

```typescript
import { createApp } from 'vue'
import App from './App.vue'

const app = createApp(App)
// 这个时候 app 就是一个 App 的实例，现在再设置任何的配置是在不同的 app 实例上面的，不会像vue2 一样发生任何的冲突。

app.config.isCustomElement = tag => tag.startsWith('app-')
app.use(/* ... */)
app.mixin(/* ... */)
app.component(/* ... */)
app.directive(/* ... */)

app.config.globalProperties.customProperty = () => {}

// 当配置结束以后，我们再把 App 使用 mount 方法挂载到固定的 DOM 的节点上。
app.mount(App, '#app')
```



## [第4章 项目起航 - 准备工作和第一个页面](http://docs.vikingship.xyz/first-page.html)





## [第5章 表单的世界 - 完成自定义 Form 组件](http://docs.vikingship.xyz/validate-form.html)





## [第6章 请你吃全家桶 - 初步使用 vue-router 和 vuex](http://docs.vikingship.xyz/vuerouter-vuex.html)





## 第7章 前后端结合 - 项目整合后端接口





## 第8章 通行凭证 - 权限管理







## 第9章 道高一尺 - 上传组件





## 第10章 最终的功能 - 编辑和删除文章





## 第11章 持续优化







## 第12章 项目构建和部署







## 第13章 课程总结

