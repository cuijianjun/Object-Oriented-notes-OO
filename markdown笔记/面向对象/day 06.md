## 面向对象第6天

#### setTimeout

> 异步执行

```
function callback() {
            console.log(111);
        }
        // setTimeout是全局下的一个函数,
        // 第一个参数要求是回调函数
        // 传入setTimeout的回调函数，是异步执行的(即：不是随着代码的顺序执行)。
        setTimeout(callback, 0);
        console.log(222);
        for ( var i = 0; i < 10; i++ ) {
            console.log(i);
        }
```