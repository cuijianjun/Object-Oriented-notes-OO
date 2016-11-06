##面向对象 第一天

####arguments

- 关键字，通过他可以通过下标的方式获得所有的实参
- length属性:用来获取实参的个数
- arguments，是数组，
- arguments 类似与这个样子：{  0: '实参1', 1: '实参2', length: 2 }

```
计算任意数据的和
        function count( ) {
            var result = 0;
            for ( var i = 0; i < arguments.length; i++ ) {
                result += arguments[i];
            }
            console.log(result);
        }
        count(1,2,3,4,5,234,3467,4564,234)
```
####伪数组

> 拥有length属性，
> 以下标储存数据的对象

####throw

```
写一个函数，函数接收一个参数并打印，如果参数的值是null或undefined，则报错。
        function con( par ) {
            if ( par == null ) {
                throw '参数错误！' + par;
            }
            console.log( par );
        }
        //con(null);
        //con(undefined);
        con(111);
        con('acb');
```

####工厂模式

> 该方法可以获取狗对象这种把创建对象封装起来的方式。

```
            function getDog(name, color) {
                var dog = new Object();
                dog.name = name;
                dog.color = color;
                dog.say = function say() {
                    console.log('旺旺');
                };
                return dog;
            }
            var aLaSiJia = getDog('阿拉斯加', '黑白相间');
            var jinMao = getDog('金毛', '黄色');
```

####构造函数

> 构造函数只是函数的另外一个称呼，和普通函数无异；
> 如果一个普通函数，专门是用来配合new关键字创建对象的，
> 那么这个函数就可以称之为构造函数。
> 如果一个函数想当做构造函数使用，那么有一个潜规则就是函数名的首字母大写。

```
        function Dog(name, color) {
            this.name = name;
            this.color = color;
            this.say = function say() {
                console.log('旺旺');
            };
        }
```

#### 类
- 就是对一些具有相同特征与特性的对象的抽象描述。
- 在ES6之前，可以把构造函数看作是类。

##### 实例的类型
- 实例的类型就是构造函数的名字。

####工厂函数与构造函数的区别

> 工厂函数与构造函数的区别：
工厂函数把通过构造函数创建对象的过程，进行了封装。
也就是说，工厂函数依赖构造函数。
工厂函数和构造函数起始都是普通函数，
只不过因为他们的作用不用，
从而产生了不用的称呼而已。

```
        // 构造函数，配合new关键字可以创建对象
        function Dog(name, color) {
            this.name = name;
            this.color = color;
            this.say = function say() {
                console.log('旺旺');
            };
        }

        // 工厂函数，就是把创建对象的过程进行了封装
        function getDog(name, color) {
            var dog = new Dog(name, color);
            return dog;
        }
```

####面向对象概念

>面向过程的概念：想到哪写哪，凡事亲力亲为，自己单干。面向对象的概念：利用对象解决问题，本质上就是对面向过程的封装。面向对象的好处是代码的重复利用，和函数很相似；面向对象可以认为是比函数封装更高一层的封装。   
js对面向对象编程思想提供的支持：可以编写构造函数，然后利用构造函数构建出各种对象，利用这些对象帮我们解决问题。

#####利用js创建一些人对象

```
// 构造函数，专门用来创建人对象
        function Person(name, age, sex) {
            this.name = name;
            this.age = age;
            this.sex = sex;
            this.eat = function () {
                console.log('必须吃！');
            }
        }

        // 工厂函数，把创建人对象的过程进行了封装
        function getPerson(name, age, sex) {
            return new Person(name, age, sex);
        }

        // 创建一个谢娜对象
        var xieNa = getPerson('谢娜', 18, '女');
        xieNa.eat();
```

####类的概念

> 类是对一些具有相同特征与特性对象的抽象描述   构造函数中，把某种具有相同特征与特性的对象进行了一个抽象描述,所以可以把js的构造函数看作是类(类可以认为是一个模子或者是模版,通过它可以创建出具有相同特征的对象)

####实例

> 通过构造函数创建出来的对象，就叫实例

- 实例和对象的关系

    - 没有可比行，都是对一个东西的称呼和描述，只不过角度不同罢了
    
####__proto__

> 1. 每一个对象都有一个__proto__属性。通过构造函数创建的实例对象，都会带有一个__proto__属性，这个属性的值与构造函数prototype属性的值一致，都存储着同一个对象的地址。
2. __proto__相当是一个记录着宝藏存放的地址的属性；所有的对象都有这个属性，每当我们访问一个对象的属性或方法，那么首先会在自身去找，找不到就去顺着__proto__找宝藏，宝藏没有再继续顺序__proto__找下一个宝藏，直到终点。
3. Person.prototype里面存储的东西，就是为了让实例共享，得到节省内存的目的

####原型属性的称呼

> 在js中，有两个属性都会指向原型对象，
1. 一个是prototype属性，我们称这个属性为显示原型(原型属性)；
2. 一个是__proto__属性，我们称这个属性为隐式原型(原型对象)；

####对象的属性查找规则：
1. 每当我们访问一个对象的属性或方法时，
2. 那么首先会在对象自身上找，
3. 找不到就去顺着__proto__找，
4. 没有再继续顺序__proto__找，
5. 直到终点。

####继承

- 一个对象可以使用另一个对象的东西，就叫继承
- 一个对象可以使用本不属于自己的东西，就叫继承
- 继承的本质就是代码的复用,js中的原型就是对继承特性的实现
- js中的原型就是为了让代码复用，所以原型就是js对继承这个特性的实现,一句话：在js中，原型就是继承
- 实例的__proto__属性，我们有时候会称它为实例的原型对象

####继承方式

> 下面记录的继承方式都是基于原型的，然后融入了一些程序猿自己的编程思想，从而引出了多种继承方式

-   默认的原型继承( 很常用 )

```
function Fn(){}
Fn.prototype.value=100;
var fn=new Fn();
```
-  覆写构造函数的显式原型( 很常用 ) 

```
function Fn(){}
Fn.prototype={
    value:100;
}
var fn=new Fn();
```

- 给显式原型混入属性( 很常用 ) 

```
function extend(o1,o2){
    for(var key in  o2){
        o1[key]=o2[key];
    }
}

var obj={add:function(a,b){console.log(a+b)}};
function Fn(){};
extend(Fn.prototype,obj);
extend(Fn.prototype,{value:100})
var fn=new Fn();
```

- Object.create( 很少用 ) 

```
var obj= {value:100};
var newObj=Object.create(obj);
```

- 借用Object.create方法覆写显式原型( 很少用 ) 
语法：Object.create( 被继承的对象 );
返回值：返回一个新对象，新对象继承 传入 到create方法的对象。
作用是指定创建一个新对象，并且指定新对象的继承的对象

```
var obj={value:100};
function Fn(){}；
Fn.prototype=Object.create(obj);
var fn = new Fn();
```

- 复合式原型继承( 很少用 ) 

```
function PrFn(){}
PrFn.prototype.value=100;
function Fn(){}
Fn.prototype=new PrFn();
//实例继承Fn.prototype,Fn.prototype继承了PrFn.prototype,最终实例间接继承了PRFn.prototype
var fn=new Fn();
```

####注意事项

- 构造函数有一个prototype属性，实例可以共享里面的东西，而构造函数自身无法访问里面的东西
- 构造函数添加的方法，只能自己调用
- 实例添加的方法只能自己调用
- 只有函数才拥有prototype属性，因为prototype属性的作用就是为了让实例共享，而只有函数才可以创造出实例，也就是说prototype只有放在函数上才会凸显出它的作用
- 任何对象都有__proto__属性
- 内置的属性不可枚举，无法用for in去遍历
- Person的实例可以使用obj里面的方法

```
可以考虑把obj对象里面的方法copy到Person的显式原型中，达到实例共享的目的，完成需求
for ( var key in obj ) {
            Person.prototype[key] = obj[key];
        }
```

- extend方法
> 该方法会第二个对象的属性copy到第一个对象
    本质上是把多个对象的属性依次copy到原型对象中
    

```
function extend(o1,o2){
    for(var key in o2){
        o1[key]=o2[key];
    }
}
```

####属性继承

> 这种代码复用的方式：有地方叫它 对象冒充，也叫属性盗窃，也叫属性继承，最简单明了好理解的叫法是：构造函数借用

```
需求：Person的实例可以使用Animal原型里面的方法
 function Person(name, age, sex) {
 // 借用Animal函数，给Person实例添加属性
  this.__Animal__ = Animal;
  // 调用Animal函数时，里的this指向了当前Person的实例
  this.__Animal__(name, age, sex);
  }
```

####静态成员与实例成员

> 添加给实例的属性或者方法，就叫实例成员
添加给类自己的属性或者方法叫静态成员（类成员）
    - 类原型上的方法，本意也是给实例使用的，所以也可以认为是实例成员
```
 // 这是构造函数，也是类
        function Person(name, age) {
            // 这里的name和age属性，因为将来要添加到实例身上，
            // 所以也称之为实例成员(实例属性)
            this.name = name;
            this.age = age;
        }       
``` 

- 直接添加到类上的属性或者方法叫静态成员或者类成员
    - 静态成员（类成员）只能由构造函数自己访问
    - 构造函数内添加的实例成员，也只有实例可以访问
> Person.maxAge=200;
    
    
    
    
    
    