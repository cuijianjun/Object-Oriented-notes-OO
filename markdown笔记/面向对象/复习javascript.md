#Javascript重点复习

##javascript基础组成

- javascript由几部分组成？
> ECMAscript、DOM、BOM

##数据类型

####数据类型有哪些？

- 基本数据类型
number、string、boolean、null、undefined

- 复杂数据类型
object

####ECMAscript内置对象有哪些？
- Array、Math、Date、Object、String、Number、Boolean、Function、Error、RegExp

####如何判断数据类型？
- typeof无法判断null类型的数据，反过来说typeof判断object类型的数据可能存在误判
- typeof可以判断Function类型的对象

##基本类型与引用类型的赋值问题

####基本类型
- 赋的是具体的值

####引用类型
- 赋的是对象的地址

## 运算符

####算数运算符+%
- 算数运算、字符串链接、把数据类型转化为number
- 取余数

####逻辑运算符 &&　｜｜　！
- &&
从左往右依次把数据转化为boolean类型。如果为false则返回对应的数据，如果一直没有找到则返回最后一个值
- ||
从左到右依次把数据转化为boolean类型。如果为true则返回对应的值，如果一直没有找到，则返回最后一个值
- ！
把数据转化为boolean类型的值，然后取反

####相等运算符　== === ！=  ！==
- ===类型和值必须相等
- == 会先进行数据的转化，然后进行比较（后面会提到）
 
####三元运算符？：
- 问号前面的表达式为true ，执行冒号前面的表达式，否则执行后面的表达式

##布尔类型的转换

####如何把数据转化为布尔类型？
- ！！Boolean

####哪些数据类型在转化时数据类型为false

- 0 、undefined、null、NaN、""
- 所有对象转化为boolean类型时都为true

##语句

####分支语句
- if else  |switch case

####循环语句
- for,while 、do while、for in 

####break 和continue作用是什么？
- break：结束循环
- continue :结束当前循环，进入下一循环

##函数

####创建方式

- 函数声明式
- 函数表达式
- Function

####参数
- 对函数中重复执行的代码中的不同部分的抽象提取
- 形参是用来接收实参传进来的值

####返回值
- 如果没有return 则返回undefined 如果有 则返回相对应的值

####arguments
- 一个代表实参的对象
- 可以通过下标的方式获取实参
- 可以通过length属性获取实参个数

####this
- 谁调用指向谁

####错误抛出
- throw自定义错误抛出

#+号
- 如果两边含有字符串或者对象，那么转化为string后相加
- 除此外，两数相加，转化为number后相加

#-号
- 两数转化为number后相减

#==运算符规则比较
> 约定：空数据类型表示null和undefined两种数据类型

- 任何数据和NaN相比，结果都为false
- null等于undefined
- null和非空类型相比结果都为false
- undefined和非空类型相比结果都为false
- 数字和非空类型相比，先转化为数字在相比
- 布尔和非空类型相比，先转化为数字在相比
- 对象与对象，直接比较地址
- 对象与字符串相比，先转化为字符串在进行比较

类型|类型|规律
---|---|---
NaN|任意类型|false
null|undefined|true
null|非空类型|false
数字|非空类型|转化为数字在比较
布尔|非空类型|转化为数字在进行比较
对象|对象|比较地址
对象|字符串|转化为字符串进行比较
