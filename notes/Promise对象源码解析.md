## Promise原理的实现

## 介绍

Promise是JavaScript异步编程的一种流行解决方案。Promise的构造函数必须传入一个函数参数，该参数作为执行异步任务的函数，在传入后立即被调用，并将结果传入Promise对象下的两个方法resolve和reject中的一个。

## 知识点

### Promise状态

每一个Promise对象都存在三种状态：

1. PENDING：进行中，Promise对象的初始状态
2. FULFILLED：成功
3. REJECTED：失败

> Promise对象只能由PENDING状态转变为FULFILLED或者REJECTED，并且状态改变后就不能再改变。Promise对象状态的改变不由对象本身决定，而是由我们传入的异步任务最终完成的结果决定。



### promise.resolve方法



### promise.reject方法



### promise.then方法



### promise.catch方法



### promise.finally方法



### Promise.all方法



### Promise.race方法



### Promise.allSettled方法















