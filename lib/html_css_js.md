# html_css_js

## 一、HTML部分


## 二、CSS 部分



## 三、JS 部分

- 前端有哪几种数据存储结构方式。
- typeof null 和 typeof [] 的结果是什么。
- 数组去重的方法（两种以上）。
- 封装一个函数 输入 'abcdefg'，输出 'gfedcba'。
- [A1,A2,B1,B2,C1,C2,D1,D2]和[A,B,C,D] ===> [A1,A2,A,B1,B2,B,C1,C2,C,D1,D2,D]代码实现。
- 将数组转成树结构。
- 修改数组的方法有哪些。
- 判断对象是否存在某个属性的方式有哪些。
- 如何遍历对象，有哪些方式。
- 如果通过数组去重的方式，如何将字符串转为数组，遍历数组的方式有哪些？
- 类的 super() 表示什么。
- 浏览器点击事件如何阻止默认事件，什么是冒泡。
- 什么是事件循环，微任务是怎么产生的。
- promise 如何进行多个异步请求，有什么区别。
- 手写一个 Promise allSettled 方法。
- 使用 JS 实现带并发的异步任务调度器。
- 常见的跨域方式。
- 如何理解闭包，使用闭包应该注意哪些。 
- esm 和 commonjs 的实现的原理和区别。
- 用过 proxy 吗，什么情况下用。
- Aop 有哪些应用场景，设计在项目中哪些地方。同理 EP 也是。


## TypeScript

- 引入了一个外部版本的库，然后没有对应的类型文件，你应该如何处理。
- react的类型文件一般从哪里引入。
- react 定义props类型，然后如何使用，那样写有何意义。
- 什么是联合类型，如何定义。
- interface 和 type的区别是什么。可不可以重复定义。



### 代码运行结果题目
1. 以下代码运行结果是
```js

function test() {
    console.log(1);
    setTimeout(() => {
        console.log(2);
    }, 1000)
}
test();
setTimeout(() => {
    console.log(3);
})
new Promise((resolve) => {
    console.log(4);
    setTimeout(() => {
        console.log(5);
    }, 100)
    resolve()
}).then(() => {
    setTimeout(() => {
        console.log(6);
    }, 0);
    console.log(7);
})
console.log(8);
// 1 4 8 7 3 6 5 2

```

2、下面代码运行结果是：2 4 3 1
```js
setTimeout(() => {
    console.log(1);
}, 1000)

new Promise(function (resolve) {
    console.log(2);
    for (var i = 0; i < 10000; i++) {
        i == 99 && resolve()
    }
}).then(() => {
    console.log(3);
})
console.log(4);

```