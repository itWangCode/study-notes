
<http://es6.ruanyifeng.com/#docs/promise>

- promise新建后会立即执行

```js

let promise = new Promise(function(resolve, reject) {
  // 这个函数会立即执行
  console.log('Promise');
  resolve();
});

promise.then(function() {
  console.log('resolved.');
});

console.log('Hi!');

// Promise
// Hi!
// resolved

```

- Promise.prototype.then()

- then方法返回的是一个新的Promise实例（注意，不是原来那个Promise实例）

```js
getJSON("/posts.json").then(function(json) {
  return json.post;
}).then(function(post) {
  // ...
});
```

- 上面的代码使用then方法，依次指定了两个回调函数。第一个回调函数完成以后，会将返回结果作为参数，传入第二个回调函数。

- Promise.resolve()

有时需要将现有对象转为 Promise 对象，Promise.resolve方法就起到这个作用。

```js
const jsPromise = Promise.resolve($.ajax('/whatever.json'));
jsPromise.then();


Promise.resolve('foo')
// 等价于
new Promise(resolve => resolve('foo'))
```

- Promise.reject()

Promise.reject(reason)方法也会返回一个新的 Promise 实例，该实例的状态为rejected。

```js
const p = Promise.reject('出错了');
// 等同于
const p = new Promise((resolve, reject) => reject('出错了'))

p.then(null, function (s) {
  console.log(s)
});
// 出错了
```

- 实现mergePromise

```js

const timeout = ms => new Promise((resolve) => {
  setTimeout(() => {
    resolve();
  }, ms)
});

// ajax1 是一个执行promise的函数
const ajax1 = () => timeout(1000).then(() => {
  console.log('1')
  return 1;
});

const ajax2 = () => timeout(1000).then(() => {
  console.log('2')
  return 2
});

const ajax3 = () => timeout(1000).then(() => {
  console.log('3')
  return 3
});

const mergePromise = ajaxArray => {
  let result = []
  let promise = Promise.resolve()
  ajaxArray.forEach(function (item) {
    promise = promise.then(item)
    // promise.then 返回的是一个 新的Promise实例
    result.push(promise)
  })
  return Promise.all(result)
};

mergePromise([ajax1, ajax2, ajax3]).then(data => {
  console.log('done')
  console.log(data) //[1,2,3]
});
```