# feature-1.1

js事件循环机制

总所周知，js是单线程的。
所有同一时间只能执行一个任务，那如果遇到一个长任务，就意味着我们需要等到它解决完才能进行下一个任务。所以为了解决这个问题，提供更好的用户体验，JavaScript使用了一种称为事件循环（Event Loop）的机制来处理异步操作。

什么是线程、进程

js的执行环境是浏览器或者Node.js。浏览器和node.js中涉及到线程、进程的概念。

进程是操作系统中资源分配的基本单位，每个进程都有自己的内存空间、文件描述符等资源。在现代计算机系统中，一个应用程序通常对应一个或多个进程。

一个进程中包含一个或者多个线程
