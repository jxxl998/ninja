# ninja

it's about some notes and demos of my reading of Secrets of the Javascript Ninja

### ES6

#### Promise

promise 对象是对我们现在尚未得到但将来会得到值的占位符。它是对我们最终能够得知异步计算结果的一种保证。promise 既可以成功也可以失败，并且一旦设定好了，就不能够有更多改变。

```js
new Promise((resolve, reject) => {
  // 创建新对象
});
```

- resolve 函数 显式兑现一个 promise
- reject 函数 显式拒绝一个 promise
- 但是如果在处理过程中发生异常会隐式拒绝
- then 方法，接收两个回调函数（成功/失败）作为参数并返回一个 promise：

  `myPromise.then(val => console.log("success"), err => console.log("error"));`

- 链式调用 catch 方法可以捕获 promise 的失败异常

  `myPromise.catch(e => alert(e));`

#### Module

模块（Module）是更大的代码组织单元，可以将程序划分为若干个小片段：

```js
export class Ninja {} //导出Ninja类
export default class Ninja {} //使用默认导出
export { ninja }; //导出存在的变量
export { ninja as samurai }; //导出时进行重命名

import Ninja from 'Ninja.js'; //导入默认值
import { ninja } from 'Ninja.js'; //导入单个导出
import * as Ninja from 'Ninja.js'; //导入整个模块的内容
import { ninja as iNinja } from 'Ninja.js'; //导入时重命名单个导出
```

#### destructuring

对象与数组的解构

```js
const { name: ninjaName } = ninja;
const [firstNinja] = ['Yoshi'];
```

#### Set

集合（Set）是一组非重复成员的集合：

- 通过 new Set()创建一个新的集合。

- 使用 add 方法添加成员，delete 方法删除成员，size 属性获取集合中成员的个数。

#### Map

映射（Map）是键与值之间的映射关系：

- 通过 new Map()创建一个新的映射。
- 使用 set 方法添加新映射，get 方法获取映射，has 方法检测映射是否存在，delete 方法删除映射。

#### Proxy

代理（Proxy）可对对象的访问进行控制。当与对象交互时（当获取对象的属性或调用函数时），可以执行自定义操作。

```js
const p = new Proxy(target, {
  get: (target, key) => {
    /*Called when property accessed through proxy*/
  },
  set: (target, key, value) => {
    /*Called when property set through proxy*/
  },
});
```

#### Class

类（Class）是 JavaScript 原型的语法糖：

```js
class Person {
  constructor(name) {
    this.name = name;
  }
  dance() {
    return true;
  }
}

class Ninja extends Person {
  constructor(name, level) {
    super(name);
    this.level = level;
  }

  static compare(ninja1, ninja2) {
    return ninja1.level - ninja2.level;
  }
}
```

#### Generator

生成器（generator）函数能生成一组值的序列，但每个值的生成是基于每次请求，并不同于标准函数那样立即生成。每当生成器函数生成了一个值，它都会暂停执行但不会阻塞后续代码执行。使用 yield 来生成一个新的值：

```js
function* IdGenerator() {
  let id = 0;
  while (true) {
    yield ++id;
  }
}
```

#### Arrow function

箭头函数（arrow function）可以创建语法更为简洁的函数。箭头函数不会创建自己的 this 参数，相反，它将继承使用执行上下文的 this 值：

```js
const values = [0, 1, 2, 3, 4, 5, 6];
values.sort((v1, v2) => v1 - v2);
// OR
values.sort((v1, v2) => {
  return v1 - v2;
});
```

#### Spread operator

扩展语法（spread operator）允许一个表达式在期望多个参数（用于函数调用）或多个元素（用于数组字面量）或多个变量（用于解构赋值）的位置扩展：[...items,3,4,5]。

#### 函数参数

##### Rest parameter

剩余参数（rest parameter）可以将未命中形参的参数创建为一个不定数量的数组。

##### Default parameter

函数默认参数（default parameter）允许在调用时没有值或 undefined 被传入时使用指定的默认参数值。

#### Template literal

模板字面量（template literal）是允许嵌入表达式的字符串字面量：

```js
`${ninja}`;
```

#### 块级作用域变量

- let
- 新的 const 创建块级作用域常量，常量在之后不能被重新赋值

### Chap1

#### 1.1 JavaScript 语言

#### 差异

与纯面向对象语言的差异在于：

- 函数是一等公民（一级对象）
- 函数闭包
- 作用域
- 基于原型的面向对象

对象、原型、函数和闭包的紧密结合组成了 JavaScript。

#### 深入学习

- 生成器，一种可以基于**一次请求生成多次值的函数**，在不同请求之间也能挂起执行。
- Promise，让我们更好地控制异步代码。
- 代理，让我们控制对特定对象的访问。
- 高级数组方法，书写更优雅的数组处理函数。
- Map，用于创建字典集合；Set，处理仅包含不重复项目的集合。
- 正则表达式，简化用代码书写起来很复杂的逻辑。
- 模块，把代码划分为较小的可以自包含的片段，使项目更易于管理。

#### 转换编译器

即：转换器 + 编译器（transformation + compiling）

能够把最前沿的 JavaScript 代码转换为等价的（如果不能实现，则使用相似的）能在当前浏览器中运行的代码。

流行的 transpiler：

- Traceur https://github.com/googLe/traceur-compiler/wiki/Getting-stanted
- Babel http://babeljs.io/docs/setup

#### 1.2 理解浏览器

JS 最初运行环境：浏览器环境 且 提供多种 API 和多种概念

理解浏览器提供的 API

#### 1.3 最佳实践

包括:

- 调试技巧
- 测试
- 性能分析

#### 1.3.2 测试

断言函数：于断定某个假设是真值还是假值

```js
assert(condition, message);

// eg
assert(a === 1, 'Disaster! a is not 1!');
```

#### 1.3.3 性能分析

使用内置对象 console 的 time 以及 timeEnd 方法

```js
console.time('My operation'); // 开始计时器

for (var n = 0; n < maxCount; n++) {
  // 执行多次操作
  // 运行次数可以成百上千，甚至上万，其完全依赖于将被测量的代码性质
}

// 操作结束后则用相同的计时器名字调用console.timeEnd
// 输出从开始到当前的时间差
console.timeEnd('My operation');
```

### Chap2 运行时的页面构建过程

#### Q:

- 浏览器是否总是会根据给定的 HTML 来渲染页面呢？
- Web 应用一次能处理多少个事件？
- 为什么浏览器使用事件队列来处理事件？

#### 生命周期

从用户的角度来说，浏览器构建了发送至服务器的请求，该服务器处理了请求并形成了一个通常由 HTML、CSS 和 JavaScript 代码所组成的响应。当浏览器接收了响应时，我们的客户端应用开始了它的**生命周期**。

包括:

1. 页面构建 - 创建用户界面
2. 事件处理 - 进入循环从而等待事件发生,之后调用事件处理器

#### 页面构建阶段

1. 解析 HTML 代码并构建文档对象模型
   - 在浏览器处理 HTML 节点的过程中执行
2. 执行 JavaScript 代码
   - 在 HTML 解析到脚本节点(包含或引用 JavaScript 代码的节点)时执行

在页面构建阶段, 以上 2 个步骤会交替执行多次

页面构建阶段始于浏览器接收 HTML 代码时，该阶段为浏览器构建页面 UI 的基础。通过解析收到的 HTML 代码，构建一个个 HTML 元素，构建 DOM。在这种对 HTML 结构化表示的形式中，每个 HTML 元素都被当作一个节点。

注意:

尽管 DOM 是根据 HTML 来创建的，两者紧密联系，但需要强调的是，它们**两者并不相同**。你可以把 HTML 代码看作浏览器页面 UI 构建初始 DOM 的蓝图。为了正确构建每个 DOM，浏览器还会修复它在蓝图中发现的问题。

#### 版本规范

HTML 规范和 DOM 规范

当前 HTML 的版本是 HTML5, 可以通过 https://html.spec.whatwg.org/ 查看当前版本中有哪些可用特性。你若需要更易读的文档，我们向你推荐 Mozilla 的 HTML5 指南，可通过https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5 查看。

而另一方面，DOM 的发展则相对缓慢。当前的 DOM 版本是 DOM3，可以通过https://dom.spec.whatwg.org/ 查看该标准。同样，Mozilla 也为 DOM 提供了一份报告，可以通过https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model 进行查看。

#### 执行 JavaScript 代码

所有包含在脚本元素中的 JavaScript 代码由浏览器的 JavaScript 引擎执行

引擎有:

- Firefox 的 Spidermonkey 引擎
- Chrome 和 Opera 的 V8 引擎
- Edge 的（IE 的）Chakra 引擎

由于代码的主要目的是提供动态页面，故而浏览器通过全局对象提供了一个 API 使 JavaScript 引擎可以与之交互并改变页面内容。

主要全局对象为 windows 对象，代表包含一个页面的窗口。属性 document，代表当前页面的 DOM。通过使用 windows 对象，JavaScript 代码能在任何程度上改变 DOM，包括修改/移除现存节点，以及创建和插入新节点。

WebAPI 接口查询：https://developer.mozilla.org/en-US/docs/Web/API

#### 全局代码 函数代码

全局代码&函数代码，区别在于所处位置不同，在执行中也有所不同，Chap5 会有解释

- 函数代码：包含在函数内部。执行需要被调用，全局代码或者其他函数，还可以由浏览器调用
- 全局代码：在所有函数以外的代码。由 JavaScript 引擎以一种直接的方式自动执行，遇到就单行执行（即遇到<script>标签）

#### 在页面构建阶段执行 JavaScript 代码

在页面构建阶段，浏览器会遇到特殊类型的 HTML 元素——脚本元素<script>，该元素用于包括 JavaScript 代码。每当解析到脚本元素时，浏览器就会停止从 HTML 构建 DOM，并开始执行 JavaScript 代码。

一般来说，JavaScript 代码能够在任何程度上修改 DOM 结构：它能创建新的节点或移除现有 DOM 节点。但它依然不能做某些事情，例如选择和修改还没被创建的节点。这就是为什么要把 script 元素放在页面底部的原因。如此一来，我们就不必担心是否某个 HTML 元素已经加载为 DOM。

### 事件处理

浏览器执行环境的核心思想基于：**同一时刻只能执行一个代码片段**，即所谓的**单线程执行模型**。一次只处理一个事件。

使用了事件队列实现单线程，将所有已生成的事件（无论是用户生成的，例如鼠标移动或键盘按压，还是服务器生成的，例如 Ajax 事件）都会放在同一个事件队列中，**以它们被浏览器检测到的顺序排列**。

事件处理背后的主要思想是：当事件发生时，浏览器调用相应的事件处理器。

任何后面的事件都只能在当前事件处理器完全结束执行后才能被处理。

**事件处理的过程**可以描述如下：

- 浏览器检查事件队列头
- 如果浏览器没有在队列中检测到事件，则继续检查
- 如果浏览器在队列头中检测到了事件，则取出该事件并执行相应的事件处理器（如果存在）。此时，余下的事件在事件队列中耐心等待，直到轮到它们被处理

#### 事件是异步的

包括：

- 浏览器事件，例如当页面加载完成后或无法加载时；
- 网络事件，例如来自服务器的响应（Ajax 事件和服务器端事件）；
- 用户事件，例如鼠标单击、鼠标移动和键盘事件；
- 计时器事件，当 timeout 时间到期或又触发了一次时间间隔

#### 注册事件处理器

注册方法：

- 通过把函数赋给某个特殊属性（不推荐，原因在于对于某个事件只能注册一个事件处理器，很可能会把上一个事件处理器覆盖/改写掉）
- 通过使用内置 addEventListener 方法（推荐，上述的改进）

```js
// 1
// 将一个函数赋值给window对象上的某个特定属性onload
window.onload = function () {};
// 或者给文档中的body元素单击事件
document.body.onclick = function () {};

// 2
document.body.addEventListener('click', function () {});
```

### 小结

- 浏览器接收的 HTML 代码用作创建 DOM 的蓝图，它是客户端 Web 应用结构的内部展示阶段。
- 我们使用 JavaScript 代码来动态地修改 DOM 以便给 Web 应用带来动态行为。
- 客户端 Web 应用的执行分为两个阶段：
  - **页面构建**代码是用于创建 DOM 的，而全局 JavaScript 代码是遇到 script 节点时执行的。在这个执行过程中，JavaScript 代码能够以任意程度改变当前的 DOM，还能够注册事件处理器——事件处理器是一种函数，当某个特定事件（例如，一次鼠标单击或键盘按压）发生后会被执行。注册事件处理器很容易：使用内置的 addEventListener 方法。
  - **事件处理**——在同一时刻，只能处理多个不同事件中的一个，处理顺序是事件生成的顺序。事件处理阶段大量依赖事件队列，所有的事件都以其出现的顺序存储在事件队列中。事件循环会检查事件队列的队头，如果检测到了一个事件，那么相应的事件处理器就会被调用。

#### Practice

1. 客户端 Web 应用的两个生命周期阶段是什么？

2. 相比将事件处理器赋值给某个特定元素的属性上，使用 addEventListener 方法来注册事件处理器的优势是什么？
3. JavaScript 引擎在同一时刻能处理多少个事件?
4. 事件队列中的事件是以什么顺序处理的？

### Chap3 函数

#### Q

- 回调函数在哪种情况下会同步调用，或者异步调用呢？
- 箭头函数和函数表达式的区别是什么？
- 你为什么需要在函数中使用默认参数？

#### 函数式编程

重要的原因在于，函数是程序执行过程中的主要模块单元。

除了全局 JavaScript 代码是在页面构建的阶段执行的，我们编写的所有的脚本代码都将在一个函数内执行。

函数式编程可以让代码更容易测试、扩展及模块化。

推荐：《JavaScript 函数式编程指南》

对象常用功能：

- 字面量创建{}
- 可以赋值给变量、数组项，或其他对象的属性
- 作为参数传递给函数
- 作为函数返回值
- 具有动态创建和分配的属性

函数是第一类对象，与对象共存，也可以被视为其他任意类型的 JavaScript 对象。并且拥有对象的所有能力。（是不是本质上也是对象？？？

函数同普通数据类型一样：

- 能被变量引用
- 能以字面量形式创建/声明
- 能被作为函数参数进行传递
- 作为函数的返回值（即返回一个新函数
- 具有动态创建和分配的属性

对象能做的任何一件事，函数也能做。函数也是对象。

唯一区别在于函数是可调用的，即函数会被调用以便执行某项动作。

#### 回调函数

将某个函数作为参数传入另一个函数，传入函数会在应用程序执行的未来某个时间点才执行。

也就是在之后调用，即在执行过程中，建立的函数会被其他函数在稍后的某个合适时间点“再回来调用”。

测试使用的断言函数：

```js
function assert(value, text) {
  var li = document.createElement('li');
  li.className = value ? 'pass' : 'fail';
  li.appendChild(document.createTextNode(text));
  var results = document.getElementById('results');
  if (!results) {
    results = document.createElement('ul');
    results.setAttribute('id', 'results');
    document.body.appendChild(results);
  }
  results.appendChild(li);
}
```

JavaScript 的重要特征之一是可以在表达式出现的任意位置创建函数，除此之外这种方式能使代码更紧凑和易于理解（把函数定义放在函数使用处附近）。

Tips：当一个函数不会在代码的多处位置被调用时，该特性可以避免用非必需的名字污染全局命名空间。

#### 存储函数

模拟 ES6 的 Set 新特性

```js
var store = {
  nextId: 1, // ⇽--- 跟踪下一个要被复制的函数
  cache: {}, // ⇽--- 使用一个对象作为缓存，我们可以在其中存储函数
  add: function (fn) {
    if (!fn.id) {
      fn.id = this.nextId++;
      this.cache[fn.id] = fn;
      return true;
    }
  }, // ⇽--- 仅当函数唯一时，将该函数加入缓存
};
function ninja() {}
assert(store.add(ninja), 'Function was safely added.');
assert(!store.add(ninja), 'But it was only added once.'); // ⇽--- 测试上面的代码按预期工作
```

#### 自记忆函数

记忆化（memoization）是一种构建函数的处理过程，能够记住上次计算结果。简而言之，当函数计算得到结果时就将该结果按照参数存储起来。

采用这种方式时，如果另外一个调用也使用相同的参数，我们则可以直接返回上次存储的结果而不是再计算一遍。像这样避免既重复又复杂的计算可以显著地提高性能。

对于动画中的计算、搜索不经常变化的数据或任何耗时的数学计算来说，记忆化这种方式是十分有用的。

有点算法中的 备忘录 的味道

#### 函数定义

4 类：

- 函数声明（function declaration） 和 函数表达式 （function expression）
  - `function myFunc(){ return 1}`
- 箭头函数（或者成为 lambda 函数） ES6 新增的标准
  - `myArg => myArg*2 `
- 函数构造函数 ——_不常用的方式_，能以字符串形式**动态**构造一个函数（可以认为是边缘功能，访问http://mng.bz/ZN8e）
  - `new Function('a', 'b', 'return a + b')`
- 生成器函数 （ES6 新增） ：能创建不同于普通函数的函数，在应用程序执行过程中，能退出再重新进入，保留函数内变量的值。
  - `function* myGen(){yield 1}`

理解以上不同的方式很重要。

因为函数创建的方式很大程度影响了函数可被调用的时间、函数的行为、函数可以再哪个对象上被调用。

#### 函数声明

```js
function myFunctionName(myFirstArg, mySecondArg) {
  myStatement1;
  myStatement2;
}
```

函数声明是独立的 JavaScript 代码块，可以被包含在其他函数中

#### 函数表达式

```js
var myFunc = function () {};
```

区别除了代码位置不同，对于函数声明来说，函数名是强制的，独立语句，必须能够被调用。

对于表达式而言，函数名是可选的。

#### 立即函数

立即调用函数表达式（IIFE）简写为立即函数

```js
(function () {})(3);

// 或许在js库会见到以下形式的表达式
// 一元操作符+ - ！ ~
+(function () {})();
-(function () {})();
!(function () {})();
~(function () {})();
```

#### 箭头函数

ES6 新特性

```js
// 比较箭头函数 和 函数表达式
var greet = (name) => 'Greetings ' + name;

var anotherGreet = function (name) {
  return 'Greetings ' + name;
};
```

函数体是一个表达式，则返回值就是表达式的值

函数体是代码块，返回值与普通函数一样，使用 return。（没有 return，返回值 undefined）

##### this 指向？？

#### 实参 形参

实参 argument：调用函数时所传递给函数的值

形参 parameter：定义函数时所列举的变量，所有类型的函数都能有形参

当函数调用时提供了一系列实参，这些实参就会以形参在函数中定义的顺序被赋值到形参上。

一个实参赋值到第一个形参，第二个实参赋值到第二个形参上，以此类推。

实参的数量大于形参的数量时并不会抛出错误。而实额外的实参不会赋值给形参。

反之，如果形参的数量大于实参，那么那些没有对应实参的形参则会被设为 undefined。

#### 剩余参数

rest parameters ES6 新特性

...来作前缀表示

实质是个数组

只有函数的最后一个参数才能是剩余参数。

试图把省略号放在不是最后一个形参的任意形参之前都会报错，错误以 SyntaxError: parameter after rest parameter 的形式展现。

eg：将第一个参数与余下的参数中最大的数相乘

```js
function multiMax(first, ...remainingNumbers) {	// ⇽--- 剩余参数以...作前缀
    var sorted = remainingNumbers.sort(function(a, b) {
        return b – a; 	// ⇽--- 以降序排序余下参数
    });
    return first * sorted[0];
}
```

#### 默认参数

ES6 新特性

许多网页的 UI 组件（尤其是 jQuery 插件）都能被配置。

例如，如果正在开发一个轮播组件，我们可能会给用户提供一个选项，用于指定某个项目多久会被另一个项目替代，以及一段在变化发生时间段内的动画。与此同时，可能某些用户并不关心这些问题，而且无论我们提供什么选项他们都乐于使用。对于这类场景，默认参数是完美选择。

如果指定了实参的值，参数则会被覆盖

可以为默认参数赋任何值：

- 数字或者字符串这样的原始类型
- 可以是对象、数组，甚至函数这样的复杂类型。

每次函数调用时都会从左到右求得参数的值，并且当对后面的默认参数赋值时**可以引用前面的默认参数**。

```js
function performAction(
  ninja,
  action = 'skulking',
  message = ninja + ' ' + action
) {
  return message;
}
```

⚠ 注意：

尽管 JavaScript 提供了这样的功能，我们依然强烈建议您在使用的时候要小心。在我们看来，它不能提高代码的可读性，故而无论何时都需要避免这种写法。

不过适当地使用默认参数——**避免空值**，或作为配置函数的简单标记能够带来简洁优雅的代码。

#### 小结

- 把 JavaScript 看作函数式语言你就能书写复杂代码
- 作为第一类对象，函数和 JavaScript 中其他对象一样。类似于其他对象类型，函数具有以下功能
  - 通过字面量创建
  - 赋值给变量或属性
  - 作为函数参数传递
  - 作为函数的结果返
  - 赋值给属性和方法
- 回调函数是被代码随后“回来调用”的函数，它是一种很常用的函数，特别是在事件处理场景下。
- 函数具有属性，而且这些属性能够被存储任何信息，我们可以利用这个特性来做很多事情；例如：
  - 可以在函数属性中存储另一个函数用于之后的引用和调用（函数库）
  - 可以用函数属性创建一个缓存（记忆），用于减少不必要的计算
- 有很多不同类型的函数：函数声明、函数表达式、箭头函数以及函数生成器等
- 函数声明和函数表达式是两种最主要的函数类型。函数声明必须具有函数名，在代码中它也必须作为一个独立的语句存在。函数表达式可以不必有函数名，但此时它就必须作为其他语句的一部分
- 箭头函数是 JavaScript 的一个新增特性，这个特性让我们可以使用更简洁的方式来定义函数
- 形参是函数定义时列出的变量，而实参是函数调用时传递给函数的值
- 函数的形参列表和实参列表长度可以不同
  - 未赋值的形参求值得到 undefined
  - 传入的额外实参不会被赋给任何一个命名形参
- 剩余参数和默认参数是 JavaScript 的新特性
  - 剩余参数——不与任何形参名相匹配的额外实参可以通过剩余参数来引用
  - 默认参数——函数调用时，若没传入参数，默认参数可以给函数提供缺省的参数值

#### Practice

3．执行了如下的代码片段后，变量 samurai 和 ninja 的值分别是什么？

```js
var samurai = (() => 'Tomoe')(); // undefined
var ninja = (() => {
  'Yoshi';
})(); // undefined
```

4．对于如下两次函数调用，参数 test 函数的函数体内 a、b、c 的值分别是什么？

```js
function test(a, b, ...c) {
  /*a, b, c*/
}

test(1, 2, 3, 4, 5); // a=1; b=2; c=[3,4,5]
test(); // a=undefined; b=undefined; c=[]
```

### Chap4 函数进阶：理解函数调用

#### Q

- 为什么 this 参数表示函数上下文？
- 函数（function）和方法（method）之间有什么区别？
- 如果一个构造函数显式地返回一个对象会发生什么？

#### 使用隐式函数参数

除了显式声明的参数之外，函数调用时默认还会传递两个隐式参数：

- arguments
- this

##### arguments

传递给函数的所有参数集合，可以实现原生 JavaScript 不支持的函数重载特性，剩余参数的存在使得该参数的需求大大减少

**length 属性**，表示实参的确切个数，通过数组索引访问单个参数的值

```js
// 使用arguments参数对所有函数参数执行求和操作
function sum() {
  var sum = 0;
  for (var i = 0; i < arguments.length; i++) {
    sum += arguments[i];
  }
  return sum;
}
```

实际上，大多数情况下可以使用剩余参数来代替 arguments 参数

⚠arguments 实质上并非 JavaScript 数组！!因此没有数组的方法使用，会报错

arguments 对象仅是一个类数组结构，在使用中要尤其注意

剩余参数是真正的 Array 实例，可以使用所有的数组方法

一个特性： **可以作为函数参数的别名**。

例如，如果为 arguments[0]赋一个新值，那么，同时也会改变第一个参数的值。

反之亦然。如果我们更改了某个参数值，会同时影响参数和 arguments 对象

可以说明他是使用同一个空间？

建议避免使用别名：

将 arguments 对象作为函数参数的别名使用时会影响代码的可读性，因此在 JavaScript 提供的严格模式（strict mode）中将无法再使用它。即不会改变传进来的实参

##### this

表示函数调用相关联的对象，通常称为是函数上下文

函数上下文是来自面向对象语言（如 Java）的一个概念。在这些语言中，this 通常指向定义当前方法的类的实例。

在 JavaScript 中，将一个函数作为方法（method）调用仅仅是函数调用的一种方式。事实上，this 参数的指向不仅是由定义函数的方式和位置决定的，同时还严重受到函数调用方式的影响。

#### 函数调用

事实上，函数的调用方式对函数内代码的执行有很大的影响，主要体现在 this 参数以及函数上下文是如何建立的。

在之前的函数创建的方式也有影响

4 种方式调用一个函数：

- 作为一个函数(function)，直接被调用
- 作为一个方法（method），关联在一个对象上，实现面向对象编程
- 作为一个构造函数（constructor），实例化一个新的对象
- 通过函数的 apply 或者 call 方法

##### 直接调用

this 有两种可能：

- 非严格模式，全局上下文（window 或者 node
- 严格模式，undefined

##### 作为方法被调用

当函数作为**某个对象的方法**被调用时，该对象会成为函数的上下文，并且在函数内部可以通过参数访问到。

```js
// 测试上下文函数
function whatsMyContext() {
  return this;
}

whatsMyContext(); // 作为函数被调用，返回windows
var getMyThis = whatsMyContext; // 创建函数的引用，不会重复创建函数的实例

var ninja1 = {
  getMyThis: whatsMyContext, // 在ninja1对象创建一个名为getMyThis方法
};
// 值得注意的是，whatsMyContext是一个独立的函数，可以有多种调用方式
ninja1.whatsMyContext(); // 通过方法引用调用函数，上下文是方法所在的对象
```

⚠ 将函数作为方法调用对于实现 JavaScript 面向对象编程至关重要。这样你就可以通过 this 在任何方法中引用该方法的“宿主”对象——这也是面向对象编程的一个基本概念。

上下文最主要取决于函数的调用方式

##### 作为构造函数调用

若要通过构造函数的方式调用，需要在函数调用之前使用关键字 new。

```js
function whatsMyContext() {
  return this;
}

// 通过构造函数方式重新调用上面的函数
new whatsMyContext();

// 不是特别有用
// 原因在于有悖于构造函数的2、3两点
// 构造函数的目的是创建一个新对象，并进行初始化设置，然后将其作为构造函数的返回值。
```

！！ 注意 函数的构造器 （Chap3 有讲） 与 构造函数 的区别，二者差别虽小，但却至关重要。

- 函数的构造器我们可以将动态创建的字符串创建为函数。

- 构造函数用来创建和**初始化实例**的函数*（那创建是怎么理解？？java 里面创建时用 new，构造函数只用来初始化实例）*怀疑翻译不通

```js
// 构造函数
function Ninja() {
  this.skulk = function () {
    return this;
  };
}

// new关键字调用时会创建一个空的对象实例，并将其作为函数上下文（this参数）传递给函数
var ninja1 = new Ninja();
```

一般来讲，当**调用构造函数**时会发生一系列特殊的操作：（如下图）

1. 创建一个新的空对象
2. 该对象作为 this 参数传递给构造参数，从而成为构造函数的函数上下文
3. 新构造的对象作为 new 运算符的返回值

![image-20210107001408146](C:\Users\ljs\AppData\Roaming\Typora\typora-user-images\image-20210107001408146.png)

测试：

构造函数返回一个数值：

- 直接调用函数则会返回数值
- new 方式调用 则会返回对象

构造函数返回另一个对象，而不是构造函数本身（也就是指新实例化的对象）：

```js
// 全局对象
var puppet = {
  rules: false,
};

// 构造函数
function Emperor() {
  this.rules = true;
  return puppet; // 返回的是上面的全局对象
}

var emperor = new Emperor(); // 尽管初始化传入了this对象，但返回的是另一个全局对象

// test
emperor.rules === false; //表明，变量emperor的值为由构造函数返回的对象，而不是new表达式所返回的对象
```

从以上 case 可以得出的结论：

- 如果构造函数返回的是一个对象（另外定义的 对象），则该对象将作为整个表达式的值返回，而传入构造函数的 this 将被丢弃
- 但是，如果构造函数返回的是非对象类型，则忽略返回值，返回新创建的对象

###### 编写构造函数的注意事项

构造函数的目的是 根据初始条件对函数调用创建的新对象进行**初始化**。

虽然这些函数也可以被“正常”调用，或者被赋值为对象属性从而作为方法调用，但这样并没有太大的意义，如下：

```js
function Ninja() {
  this.skulk = function () {
    return this;
  };
}

var whatever = Ninja(); // 非严格模式下返回 window对象 严格模式下则this未定义
```

以命名约定来区分构造函数和普通的函数及方法：

- 函数和方法的命名通常以描述其行为（skulk、creep、sneak、doSomethingWonderful 等）的动词开头，且第一个字母小写
- 构造函数则通常以描述所构造对象的名词命名，并以大写字母开头：Ninja、Samurai、Emperor、Ronin 等

很显然，通过构造函数我们可以更优雅地创建多个遵循相同模式的对象，而无需一次次重复相同的代码。通用代码只需要作为构造函数的主体写一次即可。

#### apply 与 call 方法调用

两者都可以改变函数上下文，并且以显式指定的方式

对要特殊指定一个函数的上下文对象时特别有用，在执行回调函数时可能会经常用到

apply 方法与 call 方法的使用方式类似，不同点在于是直接以参数列表的形式，而不再是作为数组传递

参数：

- call 参数列表
- apply 数组

#### 用箭头函数绕过函数上下文

调用箭头函数时，不会隐式传入 this 参数，而是从定义时的函数继承上下文。

即箭头函数在创建时确定了 this 的指向。

————还没有完全搞明白 该复习了

#### bind 方法

函数还可访问 bind 方法创建新函数。无论使用哪种方法调用，bind 方法创建的新函数与原始函数的函数体相同，新函数被绑定到指定的对象上。

`elem.addEventListener("click", button.click.bind(button));`

调用 bind 不会修改原始函数，而是创建一个全新函数

#### 小结

- 当调用函数时，除了传入在函数定义中显式声明的参数之外，同时还传入两个隐式参数：arguments 与 this。
  - arguments 参数是传入函数的所有参数的集合。具有 length 属性，表示传入参数的个数，通过 arguments 参数还可获取那些与函数形参不匹配的参数。在非严格模式下，arguments 对象是函数参数的别名，修改 arguments 对象会修改函数实参，可以通过严格模式避免修改函数实参。
  - this 表示函数上下文，即与函数调用相关联的对象。函数的定义方式和调用方式决定了 this 的取值。
- 函数的调用方式有 4 种。
  - 作为函数调用：skulk()。
  - 作为方法调用：ninja.skulk()。
  - 作为构造函数调用：new Ninja()。
  - 通过 apply 与 call 方法调用：skulk.apply(ninja)或 skulk.call(ninja)。
- 函数的调用方式影响 this 的取值。
  - 如果作为函数调用，在非严格模式下，this 指向全局 window 对象；在严格模式下，this 指向 undefined。
  - 作为方法调用，this 通常指向调用的对象。
  - 作为构造函数调用，this 指向新创建的对象。
  - 通过 call 或 apply 调用，this 指向 call 或 apply 的第一个参数。
- 箭头函数没有单独的 this 值，this 在箭头函数创建时确定。
- 所有函数均可使用 bind 方法，创建新函数，并绑定到 bind 方法传入的参数上。被绑定的函数与原始函数具有一致的行为。

#### Practice

1. 通过使用前一章介绍的剩余参数，不使用 arguments 对象，重写 sum 函数。

   ```js
   function sum(...rest) {
     let sum = 0;
     for (let i = 0; i < rest.length; i++) {
       sum += rest[i];
     }
     return sum;
   }
   ```

2. ```js
   function getSamurai(samurai) {
     'use strict';
     arguments[0] = 'Ishida';
     return samurai;
   }
   function getNinja(ninja) {
     arguments[0] = 'Fuma';
     return ninja;
   }
   var samurai = getSamurai('Toyotomi'); // Toyotomi 严格模式
   var ninja = getNinja('Yoshi'); // Fuma 非严格模式
   ```

3. ```js
   function whoAmI1() {
     'use strict';
     return this;
   }

   function whoAmI2() {
     return this;
   }

   assert(whoAmI1() === window, 'Window?'); //fail
   assert(whoAmI2() === window, 'Window?'); //pass
   ```

4. ```js
   var ninja1 = {
     whoAmI: function () {
       return this;
     },
   };

   var ninja2 = {
     whoAmI: ninja1.whoAmI,
   };

   var identify = ninja2.whoAmI;

   //pass: whoAmI called as a method of ninja1
   assert(ninja1.whoAmI() === ninja1, 'ninja1?');

   //fail: whoAmI called as a method of ninja2
   assert(ninja2.whoAmI() === ninja1, ' ninja1 again?');

   //fail: identify calls the function as a function
   //because we are in non-strict mode, this refers to the window
   assert(identify() === ninja1, 'ninja1 again?');

   //pass: Using call to supply the function context
   //this refers to ninja2
   assert(ninja1.whoAmI.call(ninja2) === ninja2, 'ninja2 here?');
   ```

5.

### Chap5 精通函数：闭包和作用域

#### Q

- 一个变量或方法有几种不同的作用域？作用域分别是什么？
  - 2 种作用域：全局执行作用域、函数执行作用域
- 如何定位标识符及其值？
  - 通过词法环境
- 什么是可变变量？如何在 JavaScript 中定义可变变量？
  - 使用 const/let 定义的非静态类型变量，定义空对象或者空数组

#### 使用闭包

##### 封装私有变量

使用构造函数内的变量和构造方法来模拟对象的私有变量

```js
function Ninja() {
  var feints = 0;

  this.getFeints = function () {
    return feints;
  };

  this.feint = function () {
    feints++;
  };
}

var ninja1 = new Ninja();
ninja1.feint();
```

#### 在回调中使用

处理回调函数，简化代码

#### 词法环境

lexical environment

- 是 JavaScript 引擎内部用来跟踪标识符和特定变量之间的映射关系
- JavaScript 作用域内部的实现机制，也就是所说的作用域
- 相关属性存储在内部对象`[[Environment]]`，无法直接访问或操作

词法环境执行过程
1、一旦创建，就会执行第一个阶段，该阶段没有执行代码，JavaScript 引擎会访问并注册当前词法环境中的所声明的变量和函数
2、第二阶段，具体执行取决于变量的类型（`let`、`var`、`const` 和函数声明）以及环境类型（全局环境、函数环境或块级作用域）

#### var

- 在距离最近的函数内或者全局词法环境中注册，不关注块级作用域
- 存在变量提升

#### let & const

- 定义最近的块级别、函数级别、全局级别的变量
- 块级别在 ES6 之前无法实现
- `const` 允许定义只赋值一次的变量
- `let` 可以再次赋值

闭包允许访问函数创建时所在的作用域内的变量  
闭包是消耗内存成本的，因为闭包始终保持创建时所在的作用域内的变量

### Chap6 未来的函数：生成器和 Promise

#### Q

- 生成器函数的主要用途是什么？
- 在异步代码中，为什么使用 promise 比使用简单的回调函数更好？
- 使用 promise.race 来执行很多长期执行的任务时，promise 最终会在什么时候变成 resolved 状态？它什么时候会无法变成 resolved 状态？

#### 🌰

使用生成器和 promise 编写优雅的代码

```js
async (function*() {
  try {
    const ninjas = yield getJSON('ninjas.json')
    const missions = yield getJSON(ninjas[0].missionsUrl)
    const missionDescription = yield getJSON(missions[0].detailsUrl)
    // Study the mission details
  }
  cache (e) {
    // oh no, we weren't able to get the mission details
  }
})
```

#### 生成器函数

features：

- 函数类型
- 生成一组值的序列
- 每个值的生成基于每次请求
- 必须显式向生成器请求
- 没有返回值
- 调用并不会执行生成器函数，而是创建迭代器（iterator）对象

notice：区别于

定义生成器：

- 关键字 `function` 后添加 `*` 定义生成器函数
- 在生成器函数内部使用 `yield` 关键字生成独立的值

```js
function* test() {
  yield '1';
  yield '2';
  yield '3';
}
```

#### 迭代生成的值序列

使用 while

```js
function* test() {
  yield '1';
  yield '2';
  yield '3';
}

const testIterator = test();
let item;

while (!(item = test.next()).done) {
  // item !== null
}
```

#### 使用场景

##### 生成唯一 ID 序列

简单不易被修改

```js
function* IdGenerator() {
  let id = 0;
  while (true) {
    yield id++;
  }
}

const idIterator = IdGenerator();

const ninja1 = {
  id: idIterator.next().value,
};

const ninja2 = {
  id: idIterator.next().value,
};
```

##### 递归遍历 DOM

DOM 结构=》树形结构，每个节点只有一个跟节点以及 0 到多个子节点  
通常，遍历 DOM 采用递归方式

`yield*`

#### 与生成器交互
