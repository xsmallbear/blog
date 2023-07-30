---
title: "JavaScript 笔记"
date: 2023-07-30
type: "post"
tags: ["JavaScript"]
showTableOfContents: true
---

# 写在前面

本笔记是([MDN Web Docs的JavaScript教程](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript))笔记

# 语法和数据类型

JavaScript 是区分大小写的,并使用 Unicode 字符集

`früh` 和 `Früh` 则是两个不同的变量

在 JavaScript 中,指令被称为语句(Statement),并用分号(;)进行分隔(不写也可以)。

## 变量和作用域

- ` var ` 声明一个变量。
- ` let ` 声明一个块作用域的局部变量。
- ` const ` 声明一个块作用域的只读常量。

用 `var` 或 `let` 语句声明的变量,如果没有赋初始值,则其值为 `undefined`。

### 作用域

- 全局变量 可被当前文件中任何地方的代码访问
- 局部变量 只能在当前函数内访问

### 变量提升

先使用变量稍后声明变量被成为变量提升,但是提升后的变量会返回undefined

- `var`声明的变量可以被正常用

```JavaScript
console.log(x === undefined); // true
var x = 3;
```

- `let`和`const`声明的变量会抛(ReferenceError)

```JavaScript
console.log(x); // ReferenceError
let x = 3; 
```

### 函数提升

函数声明会被提升,函数表达式不会被提升

```JavaScript
foo(); // "bar"
function foo() {
  console.log("bar");
}

baz(); // 类型错误：baz 不是一个函数
var baz = function() {
  console.log("bar2");
};
```

## 基本数据类型

- `Number` 整数或者浮点数
- `String` 字符串
- `BigInt` 大整数
- `Boolean` 布尔值
- `null` 表明null的特殊关键字
- `undefined` 特殊关键字,undefined表示变量未赋值时的属性
- `Symbol` 符号
- `Object` 对象

## 字面量

- 数组字面量 (Array literals) `["a", "b", "c"]`
- 布尔字面量 (Boolean literals) `true`和`false`。
- 数字字面量 (Numeric_literals) ` 3.14; -.2345789; -3.12e+12; -3.12*10^12`
- 对象字面量 (Object literals) `{name:"smallbear",sex:"man"}`
- RegExp 字面值 ``/ab+c/``
- 字符串字面量 (String literals) `hello World!`

# 流程控制与错误处理

## 语句块

由一对大括号界定的代码范围

`{const a = 10} //这就是个语句块`

## 条件判断语句

### if语句

```JavaScript
if(条件){
    语句1
}else{
    语句2
}

if(条件){
    语句1
}else if(条件){
    语句2
}
```

### switch语句

```JavaScript
switch(变量){
    case 值1:
        break
    case 值2:
        break
    default:
        没有匹配时执行的代码
}
```

### 异常处理语句

```JavaScript
try{
    代码
}catch(e){
    有异常执行的代码
}
finally{
    无论有无异常都会执行的代码
}
```

- throw语句 抛出一个异常,会被try语句捕获,可以抛出任何的表达式或者对象

- try块 捕获异常,如果其中的代码运行出现异常,跳转到catch快

- catch块 处理异常,被捕获的异常会在catch中进行处理,JavaScript只有一个catch快

- finally块 无论try块中是否存在异常,都会执行的部分

# 循环与迭代

## for

``` JavaScript
for(let i = 0; i < 10; i++){
    console.log(i)
}
```

## do...while

``` JavaScript
let i = 0
do{
    console.log(i++)
}while(i < 10)
```

## while

``` JavaScript
let i = 0
while(i < 10){
    console.log(i++)
}
```

## label
label提供了一个可以让其他语句跳转到此位置的标识符，可以被`break`和`continue`跳转到此

``` JavaScript
flag1:  //flag1就是一个label
    console.log("hello")
```

## break

- 中止循环 中止`while`，`do-while`，`for`(在多层循环中只能跳出一层)
- 跳出switch 被`case`匹配后跳出switch，避免执行之后的`case`
- 跳转到指定的label 在很复杂的多层循环中可以用

## continue

- 中止本次循环，立刻开始下一次循环
- 跳到一个被 label 标识的循环语句

## for...in

- 遍历对象所有可枚举的属性
- 将属性名（key）传入循环体中
- 遍历顺序确定

## for...of

- 遍历可迭代对象(Array、Map、Set、arguments 等)
- 将属性名（value）传入循环体中
- 遍历顺序确定(Iterator protocol)
 
# 函数

## 定义函数

``` JavaScript
//定义了一个名为square的函数
function square(number) {
    return number * number;
}

//使用表达式的方式定义了一个名为square2的函数
const square2 = function(number){
    return number * number
}
```

## 作用域和函数堆栈

### 作用域

定义的函数默认具有全局的作用域,嵌套定义的函数的作用域则是包裹它的函数

### 递归

函数有三种方法可以调用自己
- 函数名
- arguments.callee
- 作用域下指向函数的变量名

``` JavaScript
let foo = function bar(){
    foo()               //递归调用自己
    arguments.callee()  //递归调用自己
    bar()               //递归调用自己
}
```

### 嵌套函数

在一个函数内定义另一个函数成为嵌套函数。嵌套函数可以访问其容器函数的参数和变量

- 内部函数只在其外部函数中可以访问
- 内部函数形成了一个闭包：它可以访问外部函数的参数和变量，但是外部函数却不能使用它的参数和变量

``` JavaScript

function fun1(a, b){
    let number1 = 1
    let number2 = 2
    //在这里无法访问 c,d,number3,number4
    function fun2(c, d){
        let number3 = 1
        let number4 = 2
        //在这里可以访问 a,b,number1,number2
    }
}

```

### 命名冲突

当发生两个参数或者变量同名时，更近的作用域享有更高的优先权；链的第一个是最里面的作用域，最后一个元素是最外层的作用域

## 闭包

闭包是JavaScript中最强大特性之一

闭包指的是在一个嵌套函数中，以某种方法让内部函数访问外部函数的作用域

``` JavaScript
let pet = function (name) {
    let getName = function () {
        return name
    }
    return getName
}
//调用pet时返回内部函数getName
const myPet = pet("Hello")
//这里调用了内部函数getName，闭包产生
console.log(myPet()) //运行结果 Hello
```

## arguments 对象

函数的实际参数会存在一个类似数组的arguments对象中

`arguments[i]`访问函数的参数

`arguments.length`表示参数的数量

> arguments 变量只是“类数组对象”，并不是一个数组

## 函数参数

### 默认参数

函数的默认参数是undefined，也可以自己指定某个参数的默认值
``` JavaScript
//这里指定b的默认参数为1 函数调用时如果只有一个参数，则b会赋予默认值1
function fuc(a, b = 1){
    return a * b
}
```

### 剩余参数

剩余参数将不确定数量的参数表示为数组

``` JavaScript
function test(a, ...b) {
    console.log("a", a)
    console.log("b", b)
}
test(1, 2, 3, 4, 5)
//运行结果
// a 1
// b [ 2, 3, 4, 5 ]
``` 

## 箭头函数

引入箭头函数的两个因素
- 使lambda更精简
- this

function定义的函数会重新定义自己的this值，其条件如下
- 非严格模式
  - 直接调用(function invocation):指向全局对象
  - 作为某个对象的方法调用(method invocation):指向该对象
  - 使用call或apply方法调用:显式地设置为传递给这些方法的第一个参数
- 严格模式
  - 直接调用(function invocation):设置为undefined

箭头函数捕获上下文中的this值作为自己的this值


# 数组集合和映射

## Array

### 创建Array

``` JavaScript
//创建一个有三个值的数组
const a1 = new Array(1,2,3)
const a2 = Array(1,2,3)
const a3 = [1,2,3]

//创建一个长度为3，但是没有任何元素的数组
const b1 = new Array(3)
const b2 = Array(3)
const b3 = []
b3.length = 3

//创建一个二维数组
const a = new Array(4)
for(let i = 0; i < 4; i++){
    a[i] = new Array(4)
    for(let j = 0; j < 4; j++){
        a[i][j] = "[" + i + "," + j +"]"
    }
}

//稀疏数组
const a4 = [1, 2, , , 5];
```

### 访问数组

数组使用`属性访问器`来访问
- array[value]

如果其中的值是一个非整型，那么会作为一个对象的属性创建

## Set

### 创建Set
``` JavaScript
const s = new Set()

s.set("one")
s.set("tow")
s.set("three")
```

### 修改Set
``` JavaScript
s.has(1)
s.delete("foo")
s.size
```

## Map
