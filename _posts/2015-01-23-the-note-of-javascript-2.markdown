---
layout: post
slug: hello-world
title: 《javascript权威指南》读书笔记(2)—语句
type: booknote
book: javascript权威指南
categories:
- 笔记
tags:
- javascript
- 读书笔记
- 编程语言
---
> 以下是关于《javascript权威指南》中表达式和运算符部分的整理，比起静态语言，动态语言在处理表达式进行类型转换方面的机制更为细致和复杂，理解“+”、“==”和eval()等这些关键运算符的作用原理个人感觉还是比较重要的

## 原始表达式
<!--more-->
- 常量
- 关键字
- 变量

## 对象和数组的初始化表达式

var sparseArray=[1,,,5];//三个元素是undefined

## 函数定义表达式

var square=function(x){return x*x;}

## 属性访问表达式

- exp.identify
- exp[expression]

在"."和“[”之前的表达式总会先计算，如果是null或undefined就抛出异常

如果运算结果不是对象，则将其转换成对象

如果命名的属性不存在，整个属性访问表达式的值后就是undefined

.identifier的方式只适用于要访问的属性名是合法的标识符,如果是保留字或数字必须使用方括号

## 调用表达式

{% highlight js%}
f(0)
Math.max(x,y,z)
a.sort()
{% endhighlight%}

## 对象创建表达式

new Object()

new Point()

js首先创建一个新的空对象，然后，通过传入指定的参数并将这个新对象当做this的值来调用一个指定的函数。这个函数可以使用this来初始化这个新建对象的属性。那些被当成构造函数的函数不会返回一个值，并且这个新创建并被初始化后的对象创建表达式的值

## 运算符概述

大多数运算符都是由标点符号表示的，比如“+”和“=”

另外是由关键字表示的，如delete和instanceof

- 操作数的个数：单目、双目、三目
- 操作数类型和计算结果
    - “+”和“<”等可能有多种结果
- 左值
    - 表达式只能出现在赋值运算符的左侧
    - 变量、对象属性和数组元素都是左值
    - 自定义的函数不能返回左值
- 运算符的副作用
    - 如果给一个变量或属性赋值，那么那些使用这个变量或属性的表达式的值都会改变，"++","--","delete"类似
- 运算符的优先级
    - 优先级高的运算符的执行总是先于优先级低的
- 运算符的结合性
    - 左结合或右结合
- 运算顺序
    - 一般是从左向右
    - 只有在任何一个表达式具有副作用而影响到其他表达式的时候，其顺去才看上去有所不同

    

## 算术表达式

- “+”：优先考虑字符串连接，否则进行算术加法,

{% highlight js%}
1+2 //=3
"1"+"2"//"12"
"1"+2//"12"
1+{}//"1[object Object]"
true+true //2
2+null //2:null转成0后做加法
2+undefined //NaN,undefined转成NaN
{%endhighlight%}

-  “*”,"/","-","%" 都是转成数字，转不成的转成NaN，6.5%2.1的结果是0.2
- 一元运算符"+","-","++","--"都是将对象转成数字
- 位运算符
    - 会将NaN，Infinity和-Infinity转成0


## 关系表达式

- 相等和不等运算符 

    javascript中的比较是引用的比较而不是值的比较

    - "=": 得到或赋值
    - "=="：相等
    - "==="：严格相等
    - "!=": 对"=="求反
    - "!==": 对“===”求反，严格不相等
    - 如果一个值是null，另一个是undefined，则他们相等
    - 如果一个值是数字，另一个是字符串，先将字符串转成数字，然后用转换后的值进行比较
    - 如果一个值是true，则将其转成1进行比较
    - 如果一个值是对象，另一个是数字或字符串，通过toString()或valueOf()获得原始值
    - 其他类型的比较都不相等

- 比较运算符
    
    - <
    - >
    - <=
    - >=
    - 0和-0相等
    - Infinity比任何数字都大
    - -Indinity比任何数字都小
    - 其中一个若是NaN,比较操作总返回false
    - 字符串的比较也只是两个字符串中的字符的数值比较，是区分大小写的，大写的ASCII都小于小写的ASCII字母
{%highlight js%}
1+2 //3
"1"+"2" //"12"
11<3 //false
"11"<"3" //true
"11"<3 //false
"one"<3 //false ,one转成NaN 
{%endhighlight%}

- in运算符
    - in运算符希望他的左操作数是一个字符串或可以转成字符串，希望他的右操作数是一个对象。如果右侧对象拥有一个名为左操作数的属性名，那么返回true
{%highlight js%}
var point={x:1,y:1}
"x" in point //true
"z" in point //false
"toString" in point //true
var data=[7,8,9];
"0" in data //true:数组包含元素0
1 in data //true:数字换成字符串
3 in data //false:没有索引为3的元素
{%endhighlight%}

- instantof 运算符
    - instanceof运算符希望左操作数是一个对象，右操作数标识对象的类。如果左侧的对象是右侧的实例，返回true，否则返回false
{%highlight js%}
var d=new Date()
d instanceof Date //true
d instanceof Object //true
d instanceof Number //false
var a=[1,2,3] 
a instanceof Array //true
a instanceof RegExp //false

{%endhighlight%}
    - 理解instanceof必须先理解原型链

## 逻辑表达式

- 逻辑与 &&
    - &&有时也成“短路”，可能不会计算右操作数

{%highlight js%}
if(a==b)stop();
(a==b)&&stop();
{%endhighlight%}    

- 逻辑或 ||
    - 可能不会计算右操作数的值
- 逻辑非 ！
- 假植
    
    - false
    - null
    - undefined
    - 0
    - -0
    - NaN
    - “”


## 赋值表达式

可以将赋值和检测方法一个表达式中
    
    （a=b）==0

- 带操作的赋值运算
    - +=
    - *=
    - -=
    - /=
    

## 表达式计算

js可以解释运行由js源码组成的字符串，js通过全局函数eval()来完成这个工作

{%highlight js%}
eval("3+2") //5
{%endhighlight%}

- eval()是一个函数，但是已经被当运算符看待了
- eval()只有一个参数
- 使用了调用它的变量作用域和环境，一个函数可以用如下代码声明一个局部函数

{%highlight js%}
eval("function f(){return x+1;}")
{%endhighlight%}

- 全局eval()
    - 具有改变局部变量的能力
    - 任何解释器不允许对eval()赋予别名
- 严格eval()
    - 可以查询或更改局部变量，但不能在局部作用域中定义新的变量或函数，将eval列为保留字

## 其他运算符

- typeof运算符
    - 在switch语句中比较有用
- delete运算符
    - 是一元操作符，用它来删除对象属性或数组元素
    - 不能删除通过var声明的元素
   
{%highlight js%}
var o={x:1,y:2}
delete o.x
"x" in o //false
var a=[1,2,3]
delete a[2]
2 in a //false
a.length //3
{%endhighlight%}

- void运算符
    - void是一元操作符，它出现在操作数之前，操作数可以是任意类型，操作数会照常计算，但忽略计算结果并返回undefined
    
- 逗号运算符
    
