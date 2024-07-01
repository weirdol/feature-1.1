# feature-1.1

<h1>js事件循环机制 Event Loop</h1>

众所周知，js是单线程的。
所以同一时间只能执行一个任务，那如果遇到一个长任务，比如长达10秒钟，就意味着我们需要什么都不干，等它解决完才能进行下一个任务。所以为了解决这个问题，提供更好的用户体验，提出了异步任务。就是可以把这种长任务放入异步队列中，等同步任务处理完成之后，再来异步处理这些长任务。这个规则就叫做事件循环机制。

<h2>JS为什么被设计为单线程</h2>
<ul>
  <li> 浏览器环境。JavaScript 最早是在浏览器中运行的脚本语言，用于处理用户交互、操作 DOM 和更新界面。浏览器中的很多任务（如渲染页面、响应用户输入等）本身就是单线程的。如果 JavaScript 是多线程的，那么在不同线程同时修改 DOM 时会引发复杂的同步问题和竞争条件，从而导致不可预测的行为。</li>
  <li> 简单性和安全性。单线程模型使编写代码变得简单，因为开发者不用担心多个线程之间的数据访问冲突和死锁等复杂性问题，这降低了程序错误的概率，增加了代码的可维护性和可靠性</li>
  <li>事件驱动架构。JavaScript 的单线程模型配合事件循环机制，可以高效地处理异步操作，如 I/O、计时器、网络请求等。通过回调函数和事件队列，JavaScript 能够在不阻塞主线程的情况下执行异步任务，从而提高性能和响应速度。</li>
</ul>

自己的话总结。js最早是在浏览器是运行的。是用于处理用户交互、操作dom和更新界面，如果是多线性的，那么不同的线程如果同时来操作dom，会有引发复杂的同步问题和竞争条件，从而导致行为不可预测，并且单线程可以简化编程，让开发者不用担心多个线程之间数据访问和死锁等复杂性的问题。

<h2>步入正题。事件循环机制</h2>

简单来说，事件循环机制，就是用于处理js任务执行先后顺序的一种制度。

js任务分为俩种类型 同步任务和异步任务。异步任务又分为微任务和宏任务。

先执行同步代码，遇到异步宏任务则将异步宏任务放入宏任务队列中，遇到异步微任务则将异步微任务放入微任务队列中，当所有同步代码执行完毕后，再将异步微任务从队列中调入主线程执行，微任务执行完毕后再将异步宏任务从队列中调入主线程执行，一直循环直至所有任务执行完毕

执行顺序：同步任务 > 微任务 > 宏任务

<h3>同步任务有哪些</h3>

在 JavaScript 中，同步任务是指那些在执行时会阻塞后续代码的运行，直到当前任务完成为止。这些任务按照它们出现的顺序逐行执行，不会被其他任务打断。以下是一些常见的同步任务：

#### 1. **变量声明和赋值**
```javascript
let x = 10;
x = x + 5;
```
这些操作会立即执行，并且必须在继续执行下一行代码之前完成。

#### 2. **函数调用**
当一个函数被调用时，它将完全执行完毕，然后才返回控制权给调用者。
```javascript
function add(a, b) {
    return a + b;
}
let result = add(3, 4);
console.log(result); // 输出: 7
```

#### 3. **循环结构**
所有类型的循环（如 `for`、`while` 和 `do...while`）都是同步执行的。在循环体内的所有语句都会依次执行，直到循环条件不再满足为止。
```javascript
for (let i = 0; i < 5; i++) {
    console.log(i);
}
```

#### 4. **条件判断**
条件语句（如 `if...else`、`switch` 等）也是同步执行的，根据条件选择相应的代码块执行。
```javascript
let condition = true;
if (condition) {
    console.log("Condition is true");
} else {
    console.log("Condition is false");
}
```

#### 5. **异常处理**
`try...catch` 块中的代码也是同步执行的。如果在 `try` 块中抛出异常，控制权会立即转移到对应的 `catch` 块。
```javascript
try {
    let result = someFunction();
} catch (error) {
    console.error(error);
}
```

#### 6. **对象属性访问和方法调用**
对对象属性的访问和方法调用也属于同步操作。
```javascript
let obj = { name: "Alice", age: 25 };
console.log(obj.name); // 输出: Alice

obj.greet = function() {
    console.log(`Hello, my name is ${this.name}`);
};
obj.greet(); // 输出: Hello, my name is Alice
```

#### 7. **数组操作**
大多数数组操作，如添加、删除、修改元素等，也都是同步的。
```javascript
let array = [1, 2, 3];
array.push(4);
console.log(array); // 输出: [1, 2, 3, 4]
```

#### 8. **DOM 操作**
虽然 DOM 操作可能涉及浏览器的重绘或回流，但从 JavaScript 的角度来看，这些操作本身是同步的。
```javascript
document.getElementById('myDiv').innerText = 'Hello World';
```

总结
JavaScript 中的大部分基本操作，包括变量声明和赋值、函数调用、循环、条件判断、异常处理、对象属性访问和方法调用、数组操作以及 DOM 操作等，都是同步任务。这意味着这些任务会按顺序逐行执行，每个任务都必须在继续执行下一个任务之前完成。

<h3>微任务有哪些</h3>
常见的微任务
Promise 回调

Promise.resolve().then(...)
Promise.reject().catch(...)
MutationObserver 回调

用于监听 DOM 变化。
queueMicrotask()

手动将一个函数加入到微任务队列中。

<h3>宏任务有哪些</h3>
整体代码脚本：每当一个新的JavaScript文件或块被加载和执行时，它会作为一个宏任务。
setTimeout 和 **setInterval**：这些定时器函数会将回调函数添加到宏任务队列中。
I/O操作：例如从服务器获取数据的网络请求。
事件处理程序：用户交互事件（如点击、输入等）的处理程序也是宏任务。

```javascript
setTimeout(() => {
    console.log('setTimeout')
},0)
async function async1(){
    console.log('async1 start')
    await async2()
    console.log('async1 end')
}
function async2(){
    Promise.resolve().then(()=>{
        console.log('promise2')
    })
}
function async4(){
    console.log('async4 start')
    Promise.resolve().then(()=>{
        console.log('promise4')
    })
    console.log('async4 end')
}
async function apple(){
  console.log('apple start');
  async1()
  console.log('apple end');
  await async4()
  console.log('apple end again');
}
apple()
```
apple start
async1 start
apple end
async4 start
async4 end
promise2
async1 end
promise4
apple end again
<h2>什么是线程、进程</h2>

js的执行环境是浏览器或者Node.js。浏览器和node.js中涉及到线程、进程的概念。
进程是操作系统资源分配的基本单位，线程是CPU调度的基本单位。

<li>进程是操作系统中资源分配的基本单位，每个进程都有自己的内存空间、文件描述符等资源。在现代计算机系统中，一个应用程序通常对应一个或多个进程。</li>
<li>一个进程中包含一个或者多个线程</li>
<li>线程是进程中独立执行单元，它是操作系统进行调度的基本单位，每个线程都共享进程的内存空间和系统资源，但拥有自己的堆栈和寄存器，可以独立地执行特定的代码块</li>


