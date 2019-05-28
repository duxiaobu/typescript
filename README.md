# TypeScript风格指南

TypeSctipt作为JS的超集，在编码规范上也做了很多规定，我们平时在编写TS时，可以按照下面的规范去完善，下面是我翻译github上的一个关于TS编码规范的[项目](https://github.com/excelmicro/typescript)。

1. 类型
2. 引用
3. 对象
4. 数组
5. 解构
6. 字符串
7. 函数
8. 箭头函数
9. 构造函数
10. 模块
11. 迭代器和生成器
12. 属性
13. 变量
14. 变量提升
15. 比较运算符与相等
16. 块
17. 注释
18. 空白
19. 逗号
20. 分号
21. 类型铸造与强制
22. 命名约定
23. 访问器
24. 事件
25. JQuery
26. 类型声明
27. 接口、

## 1.类型

1.1 基本类型：当使用基础类型时，可以直接使用它的值

- string
- number
- boolean
- null
- undefined

```js
const foo = 1;
let bar = foo;
bar = 9;
console.log(foo, bar);  // => 1, 9
```

1.2 复杂类型：当使用复杂类型时，你使用的是该值的引用

- object
- array
- function

```js
const foo = [1, 2];
const bar = foo;
bar[0] = 9;
console.log(foo[0], bar[0]); // => 9, 9
```

## 2.引用

2.1 使用const声明你的变量，避免使用var

> 为什么？这将确保你不能重新赋值你声明的变量(变异)，那将容易出现bug并让代码难以理解

```js
// bad
var a = 1;
var b = 2;

// good
const a = 1;
const b = 2;
```

2.2 如果必须改变变量，使用let而不是var

> let是块作用域而不是像var那样的函数作用域

```js
// bad
var count = 1;
if (true) {

  count += 1;

}

// good, use the let.
let count = 1;
if (true) {

  count += 1;

}
```

2.3 注意let和const都是块作用域

```js
// const and let only exist in the blocks they are defined in.
{
  let a = 1;
  const b = 1;
}
console.log(a); // ReferenceError
console.log(b); // ReferenceError
```

## 3.对象

3.1 直接使用符号定义对象，而不是实例化对象

```js
// bad
const item = new Object();

// good
const item = {};
```

3.2 不要使用关键字作为对象的key，它在IE8不起作用

```js
// bad
const superman = {
  default: { clark: 'kent' },
  private: true,
};

// good
const superman = {
  defaults: { clark: 'kent' },
  hidden: true,
};
```

3.3 使用可读的同义词来取代关键字作为key

```js
// bad
const superman = {
  class: 'alien',
};

// bad
const superman = {
  klass: 'alien',
};

// good
const superman = {
  type: 'alien',
};
```

3.4 当使用动态属性名创建对象时，使用计算属性名

> 它允许你在一个地方定义所有的属性

```js
const getKey = function(k) {

  return `a key named ${k}`;

}

// bad
const obj = {
  id: 5,
  name: 'San Francisco',
};
obj[getKey('enabled')] = true;

// good
const obj = {
  id: 5,
  name: 'San Francisco',
  [getKey('enabled')]: true,
};
```

3.5 定义对象方法使用箭头函数而不是匿名函数或快捷属性

```js
// bad
const atom = {
  value: 1,
  addValue: function (value) {
    return atom.value + value;
  },
};

// bad
const atom = {
  value: 1,
  addValue(value) {
    return atom.value + value;
  },
};

// good
const atom = {
  value: 1,
  addValue: (value) => atom.value + value
};
```

3.6 使用简略属性值

```js
const lukeSkywalker = 'Luke Skywalker';

// bad
const obj = {
  lukeSkywalker: lukeSkywalker,
};

// good
const obj = {
  lukeSkywalker,
};
```

3.7 在声明对象时，将简略属性值放在前面

> 更容易显示哪些属性是快捷属性值

```js
const anakinSkywalker = 'Anakin Skywalker';
const lukeSkywalker = 'Luke Skywalker';

// bad
const obj = {
  episodeOne: 1,
  twoJedisWalkIntoACantina: 2,
  lukeSkywalker,
  episodeThree: 3,
  mayTheFourth: 4,
  anakinSkywalker,
};

// good
const obj = {
  lukeSkywalker,
  anakinSkywalker,
  episodeOne: 1,
  twoJedisWalkIntoACantina: 2,
  episodeThree: 3,
  mayTheFourth: 4,
};
```

## 4.数组

4.1 直接使用符号声明数组，而不是实例化对象

```js
// bad
const items = new Array();

// good
const items = [];
```

4.2 使用Array的push方法添加元素而不是直接赋值

```js
const someStack = [];


// bad
someStack[someStack.length] = 'abracadabra';

// good
someStack.push('abracadabra');
```

4.3 使用**...**复制数组

```js
// bad
const len = items.length;
const itemsCopy = [];
let i;

for (i = 0; i < len; i++) {
  itemsCopy[i] = items[i];
}

// good
const itemsCopy = [...items];
```

4.4 使用Array的from方法来转换数组化对象为数组

```js
const foo = document.querySelectorAll('.foo');
const nodes = Array.from(foo);
```

## 5.解构

5.1 访问和使用对象的多个属性时使用解构

> 解构使你不用为这些属性创建l临时引用

```js
// bad
const getFullName = function(user) {

  const firstName = user.firstName;
  const lastName = user.lastName;

  return `${firstName} ${lastName}`;

}

// good
const getFullName = function(obj) {

  const { firstName, lastName } = obj;
  return `${firstName} ${lastName}`;

}

// best
const getFullName = function({ firstName, lastName }) {

  return `${firstName} ${lastName}`;

}
```

5.2 数组解构

```js
const arr = [1, 2, 3, 4];

// bad
const first = arr[0];
const second = arr[1];

// good
const [first, second] = arr;
```

5.3 对多个返回值使用对象解构，而不是数组解构

> 随着时间的推移，你可以添加新的属性或者更改事物的顺序，而不会破坏调用的地方

```js
// bad
const processInput = function(input) {
  // then a miracle occurs
  return [left, right, top, bottom];

}

// the caller needs to think about the order of return data
const [left, __, top] = processInput(input);

// good
const processInput = function(input) {
  // then a miracle occurs
  return { left, right, top, bottom };

}

// the caller selects only the data they need
const { left, right } = processInput(input);
```

## 6.字符串

6.1 对字符串使用单引号

```js
// bad
const name = "Capt. Janeway";

// good
const name = 'Capt. Janeway';
```

6.2 当字符串长度超过80字符时，应当使用字符串连接跨多行编写

6.3 注意：如果过度使用，使用连接的长字符串可能会影响性能

```js
// bad
const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

// bad
const errorMessage = 'This is a super long error that was thrown because \
of Batman. When you stop to think about how Batman had anything to do \
with this, you would get nowhere \
fast.';

// good
const errorMessage = 'This is a super long error that was thrown because ' +
  'of Batman. When you stop to think about how Batman had anything to do ' +
  'with this, you would get nowhere fast.';
```

6.4 在编程中构建字符串时，请使用模板字符串而不是连接字符串

> 模板字符串提供简洁，易读的语法，具有正确的换行符和字符串插值功能

```js
// bad
const sayHi = function(name) {

  return 'How are you, ' + name + '?';

}

// bad
const sayHi = function(name) {

  return ['How are you, ', name, '?'].join();

}

// good
const sayHi = function(name) {

  return `How are you, ${name}?`;

}
```

## 7.函数

7.1 使用函数表达式而不是函数声明

```js
// bad
function foo() {
}

// good
const foo = function() {
};

// good
const foo = () => {
};
```

7.2 函数表达式

```js
// immediately-invoked function expression (IIFE)  立即调用函数表达式
(() => {
  console.log('Welcome to the Internet. Please follow me.');
})();
```

7.3 不要在非函数块中(比如：if，while)声明一个函数，而是将函数赋值给一个变量

7.4 ECMA-262定义一个块作为许多声明。就是将块作用域中的变量名在外面声明。

```js
// bad
if (currentUser) {

  const test = function() {

    console.log('Nope.');

  }

}

// good
let test;
if (currentUser) {

  test = () => {

    console.log('Yup.');

  };

}
```

7.5 不要使用``arguments``命名形参，会覆盖原arguments的作用

```js
// bad
const nope = function(name, options, arguments) {
  // ...stuff...
}

// good
const yup = function(name, options, args) {
  // ...stuff...
}
```

7.6 不要使用``arguments``，而是使用不定长参数``...``语法

> ...rest作为形参时，rest是真正的数组，而不是像arguments那样的类数组对象

```js
// bad
const concatenateAll = function() {

  const args = Array.prototype.slice.call(arguments);
  return args.join('');

}

// good
const concatenateAll = function(...args) {

  return args.join('');

}
```

7.7 使用默认参数而不是改变函数参数

```js
// really bad
const handleThings = function(opts) {
  // No! We shouldn't mutate function arguments.
  // Double bad: if opts is falsy it'll be set to an object which may
  // be what you want but it can introduce subtle bugs.
  opts = opts || {};
  // ...
}

// still bad
const handleThings = function(opts) {

  if (opts === void 0) {

    opts = {};

  }
  // ...
}

// good
const handleThings = function(opts = {}) {
  // ...
}
```

## 8.箭头函数

8.1 当使用函数表达式时(传递一个匿名函数)，使用箭头函数

```js
// bad
[1, 2, 3].map(function (x) {

  return x * x;

});

// good
[1, 2, 3].map((x) => {

  return x * x;

});

// good
[1, 2, 3].map((x) => x * x;);
```

8.2 如果函数体只有一行并且只有一个参数，可随意省略大括号和小括号，并使用隐士返回；否则添加大括号和小括号，并使用return语句

```js
// good
[1, 2, 3].map(x => x * x);

// good
[1, 2, 3].reduce((total, n) => {
  return total + n;
}, 0);
```

## 9.构造函数

9.1 总是使用``class``，避免直接使用``prototype``

> class语法更简洁，更容易理解

```js
// bad
function Queue(contents = []) {

  this._queue = [...contents];

}
Queue.prototype.pop = function() {

  const value = this._queue[0];
  this._queue.splice(0, 1);
  return value;

}


// good
class Queue {

  constructor(contents = []) {

    this._queue = [...contents];

  }

  pop() {

    const value = this._queue[0];
    this._queue.splice(0, 1);
    return value;

  }

}
```

9.2 使用``extends``来实现继承

```js
// bad
const inherits = require('inherits');
function PeekableQueue(contents) {

  Queue.apply(this, contents);

}
inherits(PeekableQueue, Queue);
PeekableQueue.prototype.peek = function() {

  return this._queue[0];

}

// good
class PeekableQueue extends Queue {

  peek() {

    return this._queue[0];

  }

}
```

9.3 方法可以返回this来帮助方法链

```js
// bad
Jedi.prototype.jump = function() {

  this.jumping = true;
  return true;

};

Jedi.prototype.setHeight = function(height) {

  this.height = height;

};

const luke = new Jedi();
luke.jump(); // => true
luke.setHeight(20); // => undefined

// good
class Jedi {

  jump() {

    this.jumping = true;
    return this;

  }

  setHeight(height) {

    this.height = height;
    return this;

  }

}

const luke = new Jedi();

luke.jump()
  .setHeight(20);
```

9.4 自定义toString()方法是可以的，只需要确保它正常工作，不会造成负面影响

```js
class Jedi {

  contructor(options = {}) {

    this.name = options.name || 'no name';

  }

  getName() {

    return this.name;

  }

  toString() {

    return `Jedi - ${this.getName()}`;

  }

}
```

## 10.模块

- 10.1 在非标准模块系统中使用模块(import/export)

```js
// bad
const AirbnbStyleGuide = require('./AirbnbStyleGuide');
module.exports = AirbnbStyleGuide.es6;

// ok
import AirbnbStyleGuide from './AirbnbStyleGuide';
export default AirbnbStyleGuide.es6;

// best
import { es6 } from './AirbnbStyleGuide';
export default es6;
```

- 10.2 不要直接使用导入导出

```js
// bad
// filename es6.js
export { es6 as default } from './airbnbStyleGuide';

// good
// filename es6.js
import { es6 } from './AirbnbStyleGuide';
export default es6;
```

- 10.3 对具有类型定义的非ES6库使用TypeScript模块导入。检查[DefinitelyTyped](https://github.com/borisyankov/DefinitelyTyped)以获取可用的类型定义文件。

```js
// bad
/// <reference path="lodash/lodash.d.ts" />
var lodash = require('lodash')

// good
/// <reference path="lodash/lodash.d.ts" />
import lodash = require('lodash')
```

- 10.4 模块导入顺序

  - 具有类型定义的外部库

  - 带有通配符导入的内部TS模块
  - 没有通配符导入的内部TS模块
  - 没有类型定义的外部库

```js
// bad
/// <reference path="../typings/tsd.d.ts" />
import * as Api from './api';
import _ = require('lodash');
var Distillery = require('distillery-js');
import Partner from './partner';
import * as Util from './util';
import Q = require('Q');
var request = require('request');
import Customer from './customer';

// good
/// <reference path="../typings/tsd.d.ts" />
import _ = require('lodash');
import Q = require('Q');
import * as Api from './api';
import * as Util from './util';
import Customer from './customer';
import Partner from './partner';
var Distillery = require('distillery-js');
var request = require('request');
```

## 11.迭代器和生成器

- 11.1 不要使用迭代器，使用更受欢迎的高阶函数如``map()``，``reduce()``，而不是``for-of``循环。

```js
const numbers = [1, 2, 3, 4, 5];

// bad
let sum = 0;
for (let num of numbers) {

  sum += num;

}

sum === 15;

// good
let sum = 0;
numbers.forEach((num) => sum += num);
sum === 15;

// best (use the functional force)
const sum = numbers.reduce((total, num) => total + num, 0);
sum === 15;
```

## 12.属性

- 12.1 访问属性使用点表示法

```js
const luke = {
  jedi: true,
  age: 28,
};

// bad
const isJedi = luke['jedi'];

// good
const isJedi = luke.jedi;
```

- 12.2 使用变量访问属性时使用下标表示法

```js
const luke = {
  jedi: true,
  age: 28,
};

const getProp = function(prop) {

  return luke[prop];

}

const isJedi = getProp('jedi');
```

## 13.变量

- 13.1 始终使用``const``用于声明变量。不这样做会赋值到全局变量上。我们希望避免污染全局命名空间。

```js
// bad
superPower = new SuperPower();

// good
const superPower = new SuperPower();
```

- 13.2 每个变量使用一个const来声明

> 通过这种方式你不必担心``;``和``,``

```js
// bad
const items = getItems(),
    goSportsTeam = true,
    dragonball = 'z';

// bad
// (compare to above, and try to spot the mistake)
const items = getItems(),
    goSportsTeam = true;
    dragonball = 'z';

// good
const items = getItems();
const goSportsTeam = true;
const dragonball = 'z';
```

- 13.3 将const和let分组声明

```js
// bad
let i, len, dragonball,
    items = getItems(),
    goSportsTeam = true;

// bad
let i;
const items = getItems();
let dragonball;
const goSportsTeam = true;
let len;

// good
const goSportsTeam = true;
const items = getItems();
let dragonball;
let i;
let length;
```

- 13.4 在需要的地方定义变量，当时要放置在正确的位置

> ``let``和``const``是块作用域而不是函数作用域

```js
// good
function() {

  test();
  console.log('doing stuff..');

  //..other stuff..

  const name = getName();

  if (name === 'test') {

    return false;

  }

  return name;

}

// bad - unnessary function call
function(hasName) {

  const name = getName();

  if (!hasName) {

    return false;

  }

  this.setFirstName(name);

  return true;

}

// good
function(hasName) {

  if (!hasName) {

    return false;

  }

  const name = getName();
  this.setFirstName(name);

  return true;

}
```

## 14.提升

- 14.1 ``var``声明的变量会提升到范围的顶部，``const``和``let``不会提升

```js
// we know this wouldn't work (assuming there
// is no notDefined global variable)
function example() {

  console.log(notDefined); // => throws a ReferenceError

}

// creating a variable declaration after you
// reference the variable will work due to
// variable hoisting. Note: the assignment
// value of `true` is not hoisted.
function example() {

  console.log(declaredButNotAssigned); // => undefined
  var declaredButNotAssigned = true;

}

// The interpreter is hoisting the variable
// declaration to the top of the scope,
// which means our example could be rewritten as:
function example() {

  let declaredButNotAssigned;
  console.log(declaredButNotAssigned); // => undefined
  declaredButNotAssigned = true;

}

// using const and let
function example() {

  console.log(declaredButNotAssigned); // => throws a ReferenceError
  console.log(typeof declaredButNotAssigned); // => throws a ReferenceError
  const declaredButNotAssigned = true;

}
```

- 14.2 匿名函数表达式会提升其变量名，但不提升函数赋值

```js
function example() {

  console.log(anonymous); // => undefined

  anonymous(); // => TypeError anonymous is not a function

  var anonymous = function() {

    console.log('anonymous function expression');

  };

}
```

- 14.3 命名函数表达式提升变量名称，而不是函数名称或函数体。

```js
function example() {

  console.log(named); // => undefined

  named(); // => TypeError named is not a function

  superPower(); // => ReferenceError superPower is not defined

  var named = function superPower() {

    console.log('Flying');

  };

}

// the same is true when the function name
// is the same as the variable name.
function example() {

  console.log(named); // => undefined

  named(); // => TypeError named is not a function

  var named = function named() {

    console.log('named');

  }

}
```

- 14.4 函数声明提升了它们的名称和函数体。

```js
function  example（）{

  superPower（）; // =>飞行

  function  superPower（）{

    控制台。log（' Flying '）;

  }

}
```

## 15.比较运算符和相等

- 15.1 使用``===``和``!==``而不是``==``和``!=``

- 15.2 条件语句（如if语句）使用toboolean抽象方法强制计算其表达式，并始终遵循以下简单规则：

  - **Objects** evaluate to **true**
  - **Undefined** evaluates to **false**
  - **Null** evaluates to **false**
  - **Booleans** evaluate to **the value of the boolean**
  - **Numbers** evaluate to **false** if **+0, -0, or NaN**, otherwise **true**
  - **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

  ```js
  if ([0]) {
    // true
    // An array is an object, objects evaluate to true
  }
  ```

- 15.3 使用快捷方式

  ```js
  // bad
  if (name !== '') {
    // ...stuff...
  }
  
  // good
  if (name) {
    // ...stuff...
  }
  
  // bad
  if (collection.length > 0) {
    // ...stuff...
  }
  
  // good
  if (collection.length) {
    // ...stuff...
  }
  ```

## 16.块

- 16.1 多行时使用大括号或者换行

  ```js
  // bad
  if (test) return false;
  
  // ok
  if (test)
    return false;
  
  // good
  if (test) {
  
    return false;
  
  }
  
  // bad
  function() { return false; }
  
  // good
  function() {
  
    return false;
  
  }
  ```

- 16.2 如果你使用的是多行块，``if``块的右大括号与``else``放在同一行 。

  ```js
  // bad
  if (test) {
    thing1();
    thing2();
  }
  else {
    thing3();
  }
  
  // good
  if (test) {
    thing1();
    thing2();
  } else {
    thing3();
  }
  ```

- 16.3 如果您使用多行块与`if`和`else`，不要省略花括号。

  ```js
  // bad
  if (test)
    thing1();
    thing2();
  else
    thing3();
  
  // good
  if (test) {
    thing1();
    thing2();
  } else {
    thing3();
  }
  ```

## 17.注释

- 17.1 使用`/** ... */`多行注释。包括描述，指定所有参数的类型和值以及返回值。

  ```js
  // bad
  // make() returns a new element
  // based on the passed in tag name
  //
  // @param {String} tag
  // @return {Element} element
  const make = function(tag) {
  
    // ...stuff...
  
    return element;
  
  }
  
  // good
  /**
   * make() returns a new element
   * based on the passed in tag name
   *
   * @param {String} tag
   * @return {Element} element
   */
  const make = function(tag) {
  
    // ...stuff...
  
    return element;
  
  }
  ```

- 17.2 使用`//`单行注释。将单行注释放在注释主题上方的换行符上。在评论前面加一个空行。

  ```js
  // bad
  const active = true;  // is current tab
  
  // good
  // is current tab
  const active = true;
  
  // bad
  const getType = function() {
  
    console.log('fetching type...');
    // set the default type to 'no type'
    const type = this._type || 'no type';
  
    return type;
  
  }
  
  // good
  const getType = function() {
  
    console.log('fetching type...');
  
    // set the default type to 'no type'
    const type = this._type || 'no type';
  
    return type;
  
  }
  ```

- 17.3 使用`// FIXME:`注释的问题。

  ```js
  class Calculator {
  
    constructor() {
      // FIXME: shouldn't use a global here
      total = 0;
    }
  
  }
  ```

- 17.4 使用`// TODO:`注释解决问题的办法。

  ```js
  class Calculator {
  
    constructor() {
      // TODO: total should be configurable by an options param
      this.total = 0;
    }
  
  }
  ```

## 18.空格

- 18.1 使用软标签设置为2个空格。就是函数体内的代码缩进两个空格

  ```js
  // bad
  function() {
  
  ∙∙∙∙const name;
  
  }
  
  // bad
  function() {
  
  ∙const name;
  
  }
  
  // good
  function() {
  
  ∙∙const name;
  
  }
  ```

- 18.2 在主支撑前留出1个空间。

  ```js
  // bad
  const test = function(){
  
    console.log('test');
  
  }
  
  // good
  const test = function() {
  
    console.log('test');
  
  }
  
  // bad
  dog.set('attr',{
    age: '1 year',
    breed: 'Bernese Mountain Dog',
  });
  
  // good
  dog.set('attr', {
    age: '1 year',
    breed: 'Bernese Mountain Dog',
  });
  ```

- 18.3 在控制语句（左括号之前放置1个空间`if`，`while`等等）。在函数调用和声明中的参数列表之前不放置空格。

  ```js
  // bad
  if(isJedi) {
  
    fight ();
  
  }
  
  // good
  if (isJedi) {
  
    fight();
  
  }
  
  // bad
  const fight = function () {
  
    console.log ('Swooosh!');
  
  }
  
  // good
  const fight = function() {
  
    console.log('Swooosh!');
  
  }
  ```

- 18.4 用空格设置运算符。

  ```js
  // bad
  const x=y+5;
  
  // good
  const x = y + 5;
  ```

- 18.5 使用单个换行符结束文件。

  ```js
  // bad
  (function(global) {
    // ...stuff...
  })(this);
  ```

  ```js
  // bad
  (function(global) {
    // ...stuff...
  })(this);↵
  ↵
  ```

  ```js
  // good
  (function(global) {
    // ...stuff...
  })(this);↵
  ```

- 18.6 编写长方法链时使用缩进。使用前导点，强调该行是方法调用，而不是新语句。

  ```js
  // bad
  $('#items').find('.selected').highlight().end().find('.open').updateCount();
  
  // bad
  $('#items').
    find('.selected').
      highlight().
      end().
    find('.open').
      updateCount();
  
  // good
  $('#items')
    .find('.selected')
      .highlight()
      .end()
    .find('.open')
      .updateCount();
  
  // bad
  const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
      .attr('width', (radius + margin) * 2).append('svg:g')
      .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
      .call(tron.led);
  
  // good
  const leds = stage.selectAll('.led')
      .data(data)
    .enter().append('svg:svg')
      .classed('led', true)
      .attr('width', (radius + margin) * 2)
    .append('svg:g')
      .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
      .call(tron.led);
  ```

- 18.7 在块打开之后和块关闭之前留空行

  ```js
  // bad
  if (foo) {
    return bar;
  }
  
  // good
  if (foo) {
  
    return bar;
  
  }
  
  // bad
  const baz = function(foo) {
    return bar;
  }
  
  // good
  const baz = function(foo) {
  
    return bar;
  
  }
  ```

- 18.8 在块之后和下一个语句之前留空行。

  ```js
  // bad
  if (foo) {
  
    return bar;
  
  }
  return baz;
  
  // good
  if (foo) {
  
    return bar;
  
  }
  
  return baz;
  
  // bad
  const obj = {
    foo() {
    },
    bar() {
    },
  };
  return obj;
  
  // good
  const obj = {
    foo() {
    },
  
    bar() {
    },
  };
  
  return obj;
  ```

## 19.逗号

- 19.1 不要使用前置逗号

  ```js
  // bad
  const story = [
      once
    , upon
    , aTime
  ];
  
  // good
  const story = [
    once,
    upon,
    aTime,
  ];
  
  // bad
  const hero = {
      firstName: 'Ada'
    , lastName: 'Lovelace'
    , birthYear: 1815
    , superPower: 'computers'
  };
  
  // good
  const hero = {
    firstName: 'Ada',
    lastName: 'Lovelace',
    birthYear: 1815,
    superPower: 'computers',
  };
  ```

- 19.2 额外的尾随逗号

  > 这导致更清洁的git差异。此外，像Babel这样的转换器将删除已转换代码中的附加尾部逗号，这意味着您不必担心旧版浏览器中的[尾随逗号问题](https://github.com/excelmicro/typescript/blob/master/es5/README.md#commas)。

  ```js
  // bad - git diff without trailing comma
  const hero = {
       firstName: 'Florence',
  -    lastName: 'Nightingale'
  +    lastName: 'Nightingale',
  +    inventorOf: ['coxcomb graph', 'mordern nursing']
  }
  
  // good - git diff with trailing comma
  const hero = {
       firstName: 'Florence',
       lastName: 'Nightingale',
  +    inventorOf: ['coxcomb chart', 'mordern nursing'],
  }
  
  // bad
  const hero = {
    firstName: 'Dana',
    lastName: 'Scully'
  };
  
  const heroes = [
    'Batman',
    'Superman'
  ];
  
  // good
  const hero = {
    firstName: 'Dana',
    lastName: 'Scully',
  };
  
  const heroes = [
    'Batman',
    'Superman',
  ];
  ```

## 20.分号

- 20.1 加分号

  ```js
  // bad
  (function() {
  
    const name = 'Skywalker'
    return name
  
  })()
  
  // good
  (() => {
  
    const name = 'Skywalker';
    return name;
  
  })();
  
  // good (guards against the function becoming an argument when two files with IIFEs are concatenated)
  ;(() => {
  
    const name = 'Skywalker';
    return name;
  
  })();
  ```

## 21.类型强制转换

- 21.1 在语句的开头进行类型强转

- 21.2 字符串：

  ```js
  //  => this.reviewScore = 9;
  
  // bad
  const totalScore = this.reviewScore + '';
  
  // good
  const totalScore = String(this.reviewScore);
  ```

- 21.3 数字转换使用`Number`和`parseInt`。

  ```js
  const inputValue = '4';
  
  // bad
  const val = new Number(inputValue);
  
  // bad
  const val = +inputValue;
  
  // bad
  const val = inputValue >> 0;
  
  // bad
  const val = parseInt(inputValue);
  
  // good
  const val = Number(inputValue);
  
  // good
  const val = parseInt(inputValue, 10);
  ```

- 21.4 布尔

  ```js
  const age = 0;
  
  // bad
  const hasAge = new Boolean(age);
  
  // good
  const hasAge = Boolean(age);
  
  // good
  const hasAge = !!age;
  ```

## 22.命名约定

- 22.1 避免单个字母名称。用你的命名描述。

  ```js
  // bad
  function q() {
    // ...stuff...
  }
  
  // good
  function query() {
    // ..stuff..
  }
  ```

- 22.2在命名对象，函数和实例时使用小驼峰

  ```js
  // bad
  const OBJEcttsssss = {};
  const this_is_my_object = {};
  const c = function() {}
  
  // good
  const thisIsMyObject = {};
  const thisIsMyFunction = function() {}
  ```

- 22. 3在命名构造函数，类，模块或接口时使用大驼峰。

  ```js
  // bad
  function user(options) {
  
    this.name = options.name;
  
  }
  
  const bad = new user({
    name: 'nope',
  });
  
  // good
  module AperatureScience {
  
    class User {
  
      constructor(options) {
  
        this.name = options.name;
  
      }
  
    }
  
  }
  
  const good = new AperatureScience.User({
    name: 'yup',
  });
  ```

- 22. 4命名对象属性时使用下划线。

  ```js
  // bad
  const panda = {
    firstName: 'Mr.',
    LastName: 'Panda'
  }
  
  // good
  const panda = {
    first_name: 'Mr.',
    Last_name: 'Panda'
  }
  ```

- 22. 5在命名私有属性时使用前导下划线`_`。

  ```js
  // bad
  this.__firstName__ = 'Panda';
  this.firstName_ = 'Panda';
  
  // good
  this._firstName = 'Panda';
  ```

- 22. 6不要保存引用`this`。使用箭头函数或方法`bind`。

  ```js
  // bad
  function foo() {
  
    const self = this;
    return function() {
  
      console.log(self);
  
    };
  
  }
  
  // bad
  function foo() {
  
    const that = this;
    return function() {
  
      console.log(that);
  
    };
  
  }
  
  // good
  function foo() {
  
    return () => {
      console.log(this);
    };
  
  }
  ```

- 22.7 如果您的文件导出单个类，则您的文件名应该是该类的名称。

  ```js
  // file contents
  class CheckBox {
    // ...
  }
  export default CheckBox;
  
  // in some other file
  // bad
  import CheckBox from './checkBox';
  
  // bad
  import CheckBox from './check_box';
  
  // good
  import CheckBox from './CheckBox';
  ```

- 22.8 当默认导出是个函数时使用小驼峰。您的文件名应与您的函数名称相同。

  ```js
  function makeStyleGuide() {
  }
  
  export default makeStyleGuide;
  ```

- 22.9 导出单例/函数库/纯对象时使用大驼峰。

  ```js
  const AirbnbStyleGuide = {
    es6: {
    }
  };
  
  export default AirbnbStyleGuide;
  ```

## 23.访问器

- 23.1 不要使用属性来访问函数

- 23.2 如果你使用了访问器函数，请使用getVal()和setVal('hello')。

  ```js
  // bad
  dragon.age();
  
  // good
  dragon.getAge();
  
  // bad
  dragon.age(25);
  
  // good
  dragon.setAge(25);
  ```

- 23.3 如果时布尔值，请使用`isVal()`和`hasVal()`。

  ```js
  // bad
  if (!dragon.age()) {
    return false;
  }
  
  // good
  if (!dragon.hasAge()) {
    return false;
  }
  ```

- 23.4创建`get()`和`set()`函数是可以的，但要保持一致。

  ```js
  class Jedi {
  
    constructor(options = {}) {
  
      const lightsaber = options.lightsaber || 'blue';
      this.set('lightsaber', lightsaber);
  
    }
  
    set(key, val) {
  
      this[key] = val;
  
    }
  
    get(key) {
  
      return this[key];
  
    }
  
  }
  ```