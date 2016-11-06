##面向对象第五天

####作用域链

> * 变量所有的有效区块(运行环境)，统称作用域链
* 环境：一个东西所依赖东西，就可以认为是他的环境
* 执行环境：代码执行时所依赖的环境，函数被调用一次，就会产生一个执行环境

####广义闭包

-   可以访问非自身变量的函数，从这个角度来讲，所有的函数都是闭包(1)
-   使用了非自身变量的函数，就是闭包(2)
 
```
var globalA = 10;
        // 这个函数，可以使用非自身的变量，那么这是一个闭包。
        function fn() {
            console.log(globalA);
        }
```

####狭义闭包

- 可以访问非自身局部变量的函数，从这种角度来讲，函数内的函数才是闭包(1)
- 使用了非自身局部变量的函数(2)

```
var globalA = 10;
        // 这个函数，只能使用全局变量和自身的局部变量，所以不是闭包。
        function fn() {
            console.log(globalA);
        }
        /*------------------------------------------------------*/
        (function () {
            var val = 10;
            // 这个函数可以使用自调函数中的局部变量，所以是闭包
            function f() {
                console.log(val);
            }
        }());
```

####闭包的特点

- 可以在任何地方操作一个局部变量

```
 function fn() {
            var a = 1;
            return function () {
                console.log(a++);
            };
        }
        var f = fn();
        f();
        f();
```

####生命周期

> 一个生物从出生开始到死亡结束，中间存活的过程叫生命周期

####变量的生命周期

> 一个变量从定义开始，到释放结束，中间存活的过程就叫变量的生命周期

####全局变量的生命周期

> 从定义开始，到页面卸载结束，就是全局变量的生命周期

####局部变量的生命周期

> 一般情况下，从变量定义开始，到函数执行完毕结束，就是局部变量的生命周期

   
   
```
 function fn() {
             // 这个变量，从fn调用时定义，到函数执行完毕结束。
             var val = 10;
         }
         fn();
```

- 二般情况可以通过闭包实现

```
function fn() {
            var a = 1;
            // 当fn执行完毕后，因为匿名函数需要使用局部变量a,
            // 所以造成a变量没有释放。
            // 总结：闭包可以延长局部变量的生命周期
            return function () {
                console.log(a);
            };
        }

        var f = fn();
        f();
        f();
```   
 
>  因为f变量一直存储着一个闭包函数，js解析引擎无法得知将来是否还有调用f函数，所以f函数不会被释
放，那么对应的a局部变量也不会被释放，这样就会造成内存浪费，如果将来f函数不会再使用了，最好手动
给f 赋值为 null。

```
// 如果想要阻止a的释放，必须要借助闭包
        function fn() {
            var a = 1;
            return function () {
                return a;
            };
        }

        var f = fn();
        var f2 = fn();

        // f调用和f2调用后，得到的a值，是来源与同一个a的。
        console.log(f());
        console.log(f2());
```

####闭包的应用场景

```
 /*
        * 1 操作私有属性(目的，为了防止其他地方对该属性进行随意修改的隐患)
        * */

        var total = 0;
        total+=10;
        total++;
        console.log(total);

        /*-----------------------------------------------*/
        function getCounter() {
            var total = 0;
            return function () {
                console.log(total++);
            }
        }
        var counter = getCounter();
        counter();
        counter();
        * 防止变量被全局污染
```

####编辑一段可以缓存数据的代码

```
var cache = {};

        // 给缓存中添加数据
        function setCache(key, val) {
            cache[key] = val;
        }

        // 获取缓存中指定的数据
        function getCache(key) {
            return cache[key];
        }

        /*---------------------------------------------------------*/

        function getCacheObj() {
            var cache = {};

            // 给缓存中添加数据
            function setCache(key, val) {
                cache[key] = val;
            }

            // 获取缓存中指定的数据
            function getCache(key) {
                return cache[key];
            }

            return {
                setCache: setCache,
                getCache: getCache,
            }
        }

        var cacheObj = getCacheObj();
        cacheObj.setCache('val', 888);
        console.log(cacheObj.getCache('val'));
```
####return基本数据类型和对象的区别

1. 基本数据类型

```
function fn() {
            var a = 1;
            return a;
        }

        /*
         * fn执行结果是返回一个数值，
         * 那么a和a2分别存储返回数值的一个copy版本。
         * */
        var a = fn();
        var a2 = fn();
```

2. 对象

```
function fn() {
            var obj = {};
            return obj;
        }

        /*
        * fn执行结果是返回一个对象的地址，
        * 那么o和o2存储是同一个对象的地址。
        * */
        var o = fn();
        var o2 = fn();
```


