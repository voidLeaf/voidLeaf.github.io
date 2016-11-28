---
layout: post
slug: hello-world
title: 《javascript权威指南》读书笔记(3)—语句
type: booknote
book: javascript权威指南
categories:
- 笔记
tags:
- javascript
- 读书笔记
- 编程语言
---

## 表达式语句

如
{%highlight js%}
counter++;
delete o.x;
alert(greeting);
window.close();
Math.cos(x);
{%endhighlight%}
<!--more-->
## 复合语句和空语句

许多条语句联合在一起
{%highlight js%}
{
	x=Math.PI;
    cs=Math.cos(x);
    console.log("cos(pi)="+cx);
}
{%endhighlight%}

## 声明语句

### var

声明一个或多个变量，如：
{%highlight js%}
var i;
var j=0;
var p,q;
{%endhighlight%}

- 如果var出现在函数体内，它定义的是一个局部变量，其作用于就是这个函数
- var语句中的变量没有指定初始化表达式，那么这个变量的初始值为undefined

### function

两种定义写法：
{%highlight js%}
var f=function(x){return x+1;}//函数定义表达式
function f(x){return x+1;} //函数声明语句
{%endhighlight%}

- 函数声明语句常出现在javascript代码的顶层，也可以嵌套，但在嵌套时，只能出现在所嵌套函数的顶部
- 函数声明语句中的函数名是一个变量名，和通过var声明变量一样，函数定义语句中的函数被“显式”地提前了。使用var的话，只有变量声明提前了
- 函数声明语句创建的变量是无法删除的

## 条件语句

### if

{%highlight js%}
if(username==null)
username="John Doe";
{%endhighlight%}

- if和else采用就近匹配的原则

### switch

{%highlight js%}
swich(exp){
	case 1:
    break;
    case 2:
    break;
    case 3:
    break;
    default:
    break;
}
{%endhighlight%}  

- 允许case关键字跟任意的表达式
- 避免使用带有副作用的case
- switch语句首先计算switch关键字后的表达式，对于每个case实际上执行的是“===”，因此表达式和case的匹配不会做任何类型转换


## 循环语句

### while

### do/while

### for

### for/in

{%highlight js%}
for(variable in object)
statement;
{%endhighlight%}
如果object表达式等于一个原始值，会转换成与之对应的包装对象。js会依次枚举对象的属性来循环，然而在每次循环之前，js都会先计算variable表达式的值，并将__属性名__赋值给它

需要注意的是，只要for/in循环中variable的值可以当做赋值表达式的左值，它可以是任意表达式，每次循环都会计算这个表达式，如
{%highlight js%}
var o={x:1,y:2,z:3};
var a=[],i=0;
for(a[i++]in o);
{%endhighlight%}
数组是特殊的对象，枚举数组索引
{%highlight js%}
for(i in a) console.log(i);
{%endhighlight%}
for/in循环并不会遍历对象的所有属性，只有“可枚举”的属性才会遍历到

枚举的顺序 是按属性定义的先后顺序

如果对象的“原型链”上有多个对象，那么链上的每个原型对象的属性的遍历也是按特定顺序执行的，js的一些实现依照数字顺序来枚举数组属性，而不是某种特定的顺序。但当数组元素的索引是非数字或是稀疏数组时，按照特定顺序枚举


## 跳转

### 标签语句

由语句前的标识符和冒号组成：

identifer:statement

break和continue是js中唯一可以使用标签的语句。
{%highlight js%}
mainloop:while(token!=null){
	continue mainloop;
}
{%endhighlight%}
identifer必须是一个合法的标识符，不能是保留字，任何语句都可以有很多个标签



### break语句

后面可以跟语句标签，在break和语句标签之间不能换行，break无法越过函数边界。

### continue语句

 只能在循环体里运行
 
 在while循环中，在循环开始处指定的expression会重复检验，如果检验结果为true，循环体会从头开始执行
 
 在do/while循环中，程序的执行直接跳到函数结尾处，这时会重新判断循环条件，之后才会继续下一次循环
 
 在for循环中，首先计算自增表达式，然后再检测test表达式，用以判断是否执行循环体
 
 在for/in循环中，循环开始遍历下一个属性名，这个属性名赋给了指定的变量
 
 由于continue在两种循环中的行为表现不同，因此使用while循环不可能完美的模拟for循环

### return语句

函数终止执行，没有表达式返回undefined

### 分支主题

### throw语句

{%highlight js%}
throw expression;
{%endhighlight%}
throw显式地抛出错误

expression的值是可以任意的，可以抛出一个代表错误码的数字，或者包含可读的错误信息的字符串。当javascript解释器抛出异常的时候通常采用error类型和其子类型。一个error对象有一个name属性表示错误类型，一个massage属性用来存放传递给构造函数的字符串
{%highlight js%}
function factorial(x){
	if(x<0) throw new Error("x不能是负数");
    for(var f=1;x>1;f*=x;x--);
    return f;
}
{%endhighlight%}
当抛出异常时，js会立即停止当前正在执行的逻辑，并跳转至就近的异常处理程序。异常处理程序是用try/catch/finally语句的catch从句编写的。如果抛出异常的代码没有一条相关联的catch从句，解释器会检查更高层的闭合代码块，看他是否有相关联的异常处理程序。以此类推，直到找到一个异常处理程序位置。如果没有找到，js会把异常当做程序错误处理，并报告给用户

### try/catch/finally语句

try定义异常所在的代码块，catch从句跟随在try从句之后，当try块内某处发生了异常时，调用catch内代码逻辑。catch从句跟随finally块，后者放置清理代码
{%highlight js%}
try{

}catch(e){
//catch中的参数是和异常相关的值（比如error对象）。
//和普通的变量不同，catch语句中的标识符具有块级作用域
//只在catch语句中有定义
}
finally{
//用于清理
//如果使用了return，continue，break忽略
}
{%endhighlight%}

## 其他语句类型

### with语句

{%highlight js%}
with(object)
statement
{%endhighlight%}
将object添加到作用域链的头部，然后执行statement，最后把作用域链恢复到原始状态

尽可能不要使用with语句

对象嵌套很深的时候可以使用with将form对象添加至作用域链的顶层
{%highlight js%}
document.forms[0].address.value;
with(document.forms[0]){
	name.value="";
    address.value="";
    email.value="";
}
{%endhighlight%}

with语句并不能创建属性

### debugger语句

用来产生一个断点
