## 面向对象第四天

####面向对象的三大特征

- 封装
> 把一堆相关性的变量和函数组织到一起
- 继承
> 原型
- 多态
> 对象的动态变化

####for in

- for in用来遍历一个对象可枚举的属性
- 需要注意：该对象继承的属性也能够被遍历出来

```
 Object.prototype.it = '科技';
        var obj = { val: 1 };

        // 打印 val 和 it
        for ( var key in obj ) {
            console.log(key);
        }
```

####静态成员与实例成员

- 静态成员（类成员）:添加给类（构造函数）自己的属性和方法

    -  什么情况下会用到静态成员：
    1. 如果一些属性和类的关联性比较大，那么可以考虑作为静态属性存在
    2. 如果一些方法具有通用性，那么可以考虑将其作为静态方法存在
> Person.MAX_AGE = 200;
- 实例成员:添加给实例的属性和方法

1. 

```
function Person( name, age ) {
            // 这是添加给将来的实例的，所以这是实例成员
            this.name = name;
            this.age = age    
        }
```
2. 

```
添加到原型中的属性与方法，本意是让实例使用的，所以也可以理解为是实例成员
        Person.prototype.run = function () {
            console.log('run');
        };
```        

#### Object 原型上的几种方法

- hasOwnProperty

> 作用:判断一个对象是否含有（自己）的某个属性
语法：对象.hasOwnProperty(要判断的属性)
返回值：boolean
  
```
实例代码
    var obj = { a: 1 };
            console.log(obj.hasOwnProperty('a'));  // true
            console.log(obj.hasOwnProperty('b'));  // false
            console.log(obj.hasOwnProperty('constructor'));  // false
            console.log(Object.prototype.hasOwnProperty('hasOwnProperty'));  // true
```

- propertyIsEnumerable

> 作用:判断一个对象是否（自己）含有某种属性，并且判断该属性是否可枚举；这个属性是双重判断，通常称这个方法为hasOwnProperty的加强版
语法:对象.propertyIsEnumerable(要判断的属性名);
返回值：boolean

```
<code>
var obj2 = { a: 1 };
        console.log(obj2.propertyIsEnumerable('a'));  // true
        console.log(obj2.propertyIsEnumerable('b')); // false
        console.log(obj2.propertyIsEnumerable('constructor'));  // false
        console.log(Object.prototype.propertyIsEnumerable('hasOwnProperty'));  // false
```

- isprototypeOf

> 作用：判断一个对象是不是另一个对象的原型对象
语法:被判断的对象.isprototypeOf(对象)
返回值:boolean

```
<code>
console.log( Object.prototype.isPrototypeOf( obj2 ) );  // true
        console.log( Object.prototype.isPrototypeOf( Object ) );  // true
        console.log( Function.prototype.isPrototypeOf( Function ) ); // true
        console.log( Function.prototype.isPrototypeOf( Math ) );  // false，
        因为Math只继承Object.prototype
```

- toString

>  作用:根据方法执行时内部的this指向，返回一个类似这样的字符串'[Object this对象的类型名称]'
通过这个字符串可以得知内置对象的类型
 使用技巧：为了让内部执行时，内部的this指向Math，所以把这个方法添加到Math自身，
 然后通过Math调用，这样toString执行时，内部this指向Math

```
<code>
Math.toStr = Object.prototype.toString;
        console.log(Math.toStr());
        Date.toStr = Object.prototype.toString;
        console.log(Date.toStr());
 另一种方法:this指向
        console.log(Object.prototype.toString.call(Math));
        console.log(Object.prototype.toString.call(Function));
```

#####自定义构造函数创建的对象统一返回'[object Object]'

####函数的几种属性

1. arguments代表实参的伪数组对象，
> arguments 有一个callee属性，该属性返回被调用的函数。
说白一点，callee就是返回函数自己。
2. caller 返回调用该函数的函数
3. length 形参的个数

- 函数length属性的作用-------------------判断函数的形参和调用时传入的实参数目是否一致

```
function add(a, b) {
            if ( arguments.length !== add.length ) {
                throw '参数个数不符！';
            }
            console.log(a + b);
        }

        add(10,20);
        add(1);
```
 
4. name 函数的名字

```
<code>
function fn(a, b, c) {
            console.log(arguments);  // 这是推荐的使用方式
            console.log(fn.arguments);  // 这种方式已经被废除了
        }
        console.dir(fn);
        console.log(fn.name);
        console.log(fn.length);
        fn(1,2);
        
        <返回调用函数>
         function f1() {
                    console.log(f1.caller);
                }
        
                !function f2() {
                    f1();
                }();
        
                f1();
```

####in 运算符-----判断后面的对象是否有前面的属性或者方法

````
var obj = { aa: 11 };
        console.log( 'aa' in obj );
        console.log( 'bb' in obj );
        console.log( 'hasOwnProperty' in obj );
````

####delete 运算符

- 作用：删除对象的属性和方法
- 语法: delete 对象.属性名

```
var obj = { aa : 10, bb : 230, 1: 100, 2: 200 };
        delete obj.bb;
        delete obj[2];
```

####Function 
- Function创建函数的语法:new Function( arg_name1, arg_name2, arg_name3, functionBody )
前面可以定义任意数量的形参，最后一个参数代表函数的代码体。注意：这些参数必须是字符串的形式
- 返回值：一个新创建的函数实例。

```
<code>
 function add(a, b) {
            console.log(a + b);
        }

        var addObj = new Function('a', 'b', 'console.log(a + b);');
        addObj(10, 60);
```
**如果使用Function创建函数，很繁琐，一般不会采纳。
但是这种方式有一个亮点，就是会把字符串当做代码执行。**

####方法

1. eval可以直接把字符串当做代码执行
 - 语法：eval(字符串代码)   example: ``eval('console.log(123223)')``

2. JSON数据格式 ==>   '{ "name": "李四" }'

- JSON.parse:用来把Json数据转化为js对象
- JSON.stringify:用来把js对象转化为Json数据
- 在ie8之前，可以用过eval或者Function解析JSON数据
    * var JSONString = '{ "name": "李四" }';
   console.log(eval('('+JSONString+')'));
    * 返回的函数的代码体：return { "name": "李四" }
   console.log(Function('return ' + JSONString)()//自调);
        
--- 
###注意事项
1. 很多地方认为构造函数默认的显式原型对象的类型，是构造函数的名字。
即 Person.prototype 是 Person 类型的对象
------------------


##函数作用域

###预解析

>1. 在代码整体执行前，会先进行解析一部分，解析后代码会从上到下依次整体执行，但是预解析过的代
码不会重新执行。
2. js预解析会把声明的部分提前执行

- 声明的代码部分
    + 变量声明 通过var关键字定义的变量
    + 函数声明 通过function关键字声明的变量
    
```
<code>

        // 其中var a是声明部分。
        var a = 1;
        // 这里的语句整体都是声明部分。
        var b, c, d;
        // 其中var e, f, p是声明部分。
        var e, f = j = 2, p;
```    

###函数

####函数表达式

>   不是以function关键字定义的函数;或者函数嵌套在非函数的代码块中

- 特征

1. 不会被预解析
2. 函数名字可有可无
3. 函数名字只能在函数内部使用

```
    // 这是函数表达式
        var f = function () {};
        {
            // 在非函数的代码块中定义的函数是函数表达式
            function f() {
            }
        }

        // 这是函数表达式
        (function () {})();
        // 这是函数表达式
        !function () {}();
        // 传入的函数是函数表达式
        fn(function () {});
```

```
 /*
        * 一个函数声明式的语法，写在非函数的代码块中，理论上这是函数表达式。
        * 但是对于这种函数，浏览器会预解析它的名字。
        * */
        console.log(fn);
        {
            // 这是函数表达式
            function fn() {
            }
        }
        console.log(fn);
        /*
        * 预解析：
        * var fn;
        * 预解析之后：
        * console.log(fn);
        * fn = function fn() {}
        * console.log(fn);
        * */
```

####函数声明式

> 以function关键字开头定义，并且定义在全局,或者直接嵌套在另一个函数里
特征:
 1. 会被预解析
 2. 函数必须有名字
 
- 函数声明提升的特点:在函数声明的前面，可以调用这个函数 

> fn(); function fn() {console.log(1);}

####预解析的规则

- 预解析如果遇到重复的变量声明，那么忽略
- 如果遇到重复的函数声明，保留后面的函数

```
<code>
 fn();
        function fn() {
            console.log(111);
        }
        function fn() {
            console.log(222);
        }
        fn();

        /*
        * 预解析：
        * 第一个fn函数声明
        * 第二个fn函数声明，发现重名，保留后面的函数
        *
        * 预解析之后：
        * fn();
        * fn();
        * */
```

- 如果遇到变量与函数重名，留下函数
#####关于预解析时重名处理的特点：

    * 凡是遇到重名的变量声明，那么忽略；
    * 凡是遇到重名的函数声明，当前的函数覆盖之前的。

- 函数内部预解析

```
<code>
 /*
        * 一个函数声明式的语法，写在非函数的代码块中，理论上这是函数表达式。
        * 但是对于这种函数，浏览器会预解析它的名字。
        * */
        console.log(fn);
        {
            // 这是函数表达式
            function fn() {
            }
        }
        console.log(fn);
        /*
        * 预解析：
        * var fn;
        * 预解析之后：
        * console.log(fn);
        * fn = function fn() {}
        * console.log(fn);
        * */
```

####作用域 变量的有效范围

- 检测一个变量的作用域

> 在指定的区域内使用这个变量，如果不报错，说明这个变量的作用域包含此区域

- 函数作用域

> 只有函数能够划分变量的作用域，这种作用域的规则叫函数作用域

  1.  函数外面无法访问函数内的变量
    
  2.  代码块外面可以访问里面的变量
  
- 块级作用域

> 凡是代码块就可以划分变量的作用域，这种作用域的规则就叫块级作用域
ES6，对块级作用域做了支持，新增了两种定义变量的方式  **(let , const)**

1. {
               let arr = [];
               console.log(arr);
           }
           //console.log(arr);

2. {
               const obj = {};
               console.log(obj);
           }
           console.log(obj);

    

####全局变量与局部变量

> 全局变量和局部变量就是通过作用域的大小来对变量进行的种类划分。

- 全局变量:在任何地方都可以使用的变量叫做称为全局变量（函数外定义的变量）

- 局部变量:只有在指定地方可以使用的变量成为局部变量（函数内申明的变量）
 
####词法作用域(变量查找规则)
 
- 词法作用域(静态作用域)：如果在函数内访问一个变量，优先找局部变量和形参,如果没用找到，
去定义该函数的环境中查找，直到全局为止。 

- 动态作用域：如果在函数内访问一个变量，优先找局部变量和形参,如果没用找到，去调用该函数
的环境中查找，直到全局为止。
 
####块级作用域 函数作用域 词法作用域

- 之间的区别：
1. 块级作用域和函数作用域描述的是，什么东西可以划分变量的作用域
2. 词法作用域描述的是，变量的查找规则。

- 之间的关系：
1.  块级作用域 包含 函数作用域。
2.  词法作用域 与 块级作用域&函数作用域之间没有任何交集，
- 他们从两个角度描述了作用域的规则。
> ES6之前js采用的是函数作用域+词法作用域，ES6 js采用的是块级作用域+词法作用域 。

-------
注意事项

1. 函数执行时形参赋值的顺序（在预解析之前）

----------------------------------

 
