# React全栈学习

## 1. 语言特性

### 1. let const 关键字

以前的时候JavaScript变量`var`默认是全局的。

![JpALRI.png](https://s1.ax1x.com/2020/04/14/JpALRI.png)

在ES6中新提出的`let`关键字则使这个缺陷得到了修复。

![JpEuo4.png](https://s1.ax1x.com/2020/04/14/JpEuo4.png)

同时还引入的概念是`const`，用来定义一个常量。常量一旦定义则不可修改（也不能被重新定义，即变量名不能重复），但是如果是引用类型的，那么可以改变它的属性。

![JpEUTe.png](https://s1.ax1x.com/2020/04/14/JpEUTe.png)

### 2. 函数
#### 箭头函数

箭头函数是一种更简单的函数声明方式，可以把它看作是一种语法糖，箭头函数永远是匿名的。

```js
> let add = (a, b) => {return a + b;}

> // 当后面是表达式时，还可以简写为
> let add1 = (a, b) => a + b;

> // 等同于
> let add2 = function(a, b) {
    return a + b;
}

> // 在回调函数中应用
> let numbers = [1, 2, 3];
> let doubleNumbers = numbers.map((number) => number *2);
> console.log(doubleNumbers);
[ 2, 4, 6 ]
```

#### this在箭头函数中的使用

在工作中经常会遇到这样的问题，就是`this`在一个对象方法中嵌套函数，而这样操作后`this`会指向`global`对象。

```js
let age = 2;
let kitty = {
    age: 1,
    grow: function() {
        setTimeout(() => {
            console.log(this.age);
        }, 100);
    }
}

kitty.grow();
-> 1
console.log(age);
-> 2
```

现在有了箭头函数之后，可以很轻松地解决这个问题。

#### 函数默认参数

ES6没有出现之前，面对默认参数都会让人感到很痛苦，不得不采用各种hack，比如书：`values = values || []`。现在一切都变得轻松很多。

![JpEb0U.png](https://s1.ax1x.com/2020/04/14/JpEb0U.png)

#### Rest参数

当一个函数的最后一个参数有`...`这样的前缀，它就会编程一个参数的数组。

![JpEz11.png](https://s1.ax1x.com/2020/04/14/JpEz11.png)

它和arguments有如下区别：

- ① Rest参数只是没有指定变量名称的参数数组，而arguments是所有参数的集合。
- ②arguments对象不是一个真正的数组，而Rest参数是一个真正的数组，可以使用各种方法，比如sort，map等。有了这两个理由，是时候告别arguments，拥抱可爱的Rest参数了。

### 3. 展开操作符

`...`操作符被称为展开操作符，它允许一个表达式在某处展开，在存在多个参数（用于函数调用），多个元素（用于数组字面量）或者多个变量（用于解构赋值）的地方就会使用。

#### 用于函数调用

如果在之前的JavaScript中，想让函数把一个数组依次作为参数进行调用，一般会这样做：

```javascript
function test (x, y, z) {};

var args = [1, 2, 3];

test.apply(null, args);
```

当有了ES6的展开运算符,可以简化这个过程.

```JavaScript
function test (x, y, z) {};

let args = [0, 1, 2];

test(...args);.
```

#### 用于数组字面量

在之前的版本中,如果想创建含有某些元素的新数组,常常会用到splice、concat、push等方法：

```javascript
var arr1 = [1, 2, 3];
var arr2 = [4, 5, 6];
var arr3 = arr1.concat(arr2);

console.log(arr3);

let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];
let arr3 = [...arr1, ...arr2];
console.log(arr3);
```

#### 对象的展开运算符

```JavaScript
let mike = {name: 'mike', age: 50};
mike = {...mike, sex: 'male'};
console.log(mike);
```

对象的展开其实已经被提上了日程，只不过它是ES7的提案之一，它可以让你以更简洁的形式将一个对象可枚举的属性复制到另一个对象上。

### 4. 模版字符串

在ES6之前的时代，字符串的拼接总是一件令人不爽的事情，但是在ES6来临的时代，这个痛处也要被治愈了。

```javascript
var name = 'viking';
var a = 'My name is ' + name + '!';

console.log(a);

// 多行字符串
var longStory = 'This is a long story, '
    + 'this is a long story, '
    + 'this is a long story.';

console.log(longStory);

// ES6
let name = 'viking';
let a = `My name is ${name}`;
let longStory = `This is a long story,
    this is a long story,
    this is a long story.`;
console.log(a);
console.log(longStory);
```

### 5. 结构赋值

结构语法可以快速从数组或者对象中提取变量，可以用一个表达式读取整个结构。

#### 结构数组

```JavaScript
let foo = ['one', 'two', 'three'];
let [one, two, three] = foo;
console.log(`${one}, ${two}, ${three}`);
```

#### 结构对象

```JavaScript
let person = {name: 'viking', age: 20};
let {name, age} = person;
console.log(`${name}, ${age}`);
```

结构赋值可以看作是一种语法糖，它受Python语言的启发，可以提高效率。

### 6. 类

众所周知，在JavaScript的世界里是没有传统类的概念的，它使用原型链的方式来完成继承，但是声明的方式看起来总是怪怪的，所以ES6提供了class这个语法糖，让开发者可以模仿其他语言类的声明方式，看起来更加明确清晰。需要注意的是，class并没有带来新的结构，而只是原来原型链方式的一种语法糖。

```JavaScript
class Animal {
    // 构造函数
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    shout() {
        return `My name is ${this.name}, age is ${this.age}`;
    }
    // 静态方法
    static foo() {
        return 'Here is a static method';
    }
}

const cow = new Animal('betty', 2);
// console.log(cow.shout());
// console.log(Animal.foo());

class Dog extends Animal{
    constructor(name, age=2, color='black') {
        // 在构造函数中可以直接调用super方法
        super(name, age);
        this.color = color;
    }
    shout() {
        // 在非构造函数中不能直接使用super方法
        // 但是可以采用super(). + 方法名调用父类方法
        return super.shout() + `, color is ${this.color}`; 
    }
}

const jackTheDog = new Dog('jack');
jackTheDog.shout();
```

### 7.模块

在ES6之前，JavaScript并没有对模块做出任何定义，于是先驱者们创造了各种各样的规范来完成这个任务。伴随着Require.js的流行，它所推崇的AMD格式也成了开发者的首选。在这之后，Node.js诞生了，随之而来的是CommomJS格式，再之后browserify的诞生，让浏览器端的开发也能使用这种格式。直到ES6的出现，模块这个观念才真正有了语言特性的支持。

```JavaScript
// hello.js
// 定义一个命名为hello的函数

function hello() {
    console.log('Hello ES6');
}

// 使用export导出这个模块
export hello;

// main.js
// 使用import加载这个模块
import { hello } from './hello';

hello();
```

上面的例子使用import和export关键字完成模块的导入和导出。

但是存在一个问题，当直接用vscode运行时会报错。

```
[Running] node "c:\Users\username\Desktop\react\Test\main.js"
c:\Users\username\Desktop\react\Test\main.js:3
import { hello } from './hello';
^^^^^^

SyntaxError: Cannot use import statement outside a module
    at Module._compile (internal/modules/cjs/loader.js:881:18)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:962:10)
    at Module.load (internal/modules/cjs/loader.js:798:32)
    at Function.Module._load (internal/modules/cjs/loader.js:711:12)
    at Function.Module.runMain (internal/modules/cjs/loader.js:1014:10)
    at internal/main/run_main_module.js:17:11

[Done] exited with code=1 in 0.121 seconds
```