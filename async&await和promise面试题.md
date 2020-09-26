**什么是Async/Await?**

 

　　async/await是写异步代码的新方式，以前的方法有回调函数和Promise。
　　async/await是基于Promise实现的，它不能用于普通的回调函数。
　　async/await与Promise一样，是非阻塞的。
　　async/await使得异步代码看起来像同步代码，这正是它的魔力所在。





```javascript
async function async1 () {
  console.log('async1 start')
  await async2();
  console.log('async1 end')
}
 
async function async2 () {
  console.log('async2')
}
 
console.log('script start')
 
setTimeout(function () {
  console.log('setTimeout')
}, 0)
 
async1();
 
new Promise (function (resolve) {
  console.log('promise1')
  resolve();
}).then (function () {
  console.log('promise2')
})
 
console.log('script end')
```

结果

```javascript
script start
async1 start
async2
promise1
script end
promise2
async1 end
setTimeout
```

解析：
先执行主代码块，所以第一行打印`script start`

`setTimeout()`是另一个宏任务，所以先放在宏任务队列。

然后执行`async1`，`async1`里先打印`async1 start`
执行`await async2` , 先执行`async2`，打印`async2`, 返回promise对象。`await`会阻塞`async1`后面的代码执行，所以先跳出来继续执行后面的代码。

然后执行`new Promise` 打印`promise1`
把`then`里面的函数加入微任务队列

打印`script end`
到这里第一个宏任务执行完毕，开始执行微任务`then`，打印`promise2`。

`then`执行完后，`await`才算是执行结束了，后面的代码不再被阻塞，所以打印`async1 end`
这时候继续执行第二个宏任务`setTimeout`，打印`setTimeout`

promise 面试题

Promise面试题[https://www.jianshu.com/p/4bb1521343ba]