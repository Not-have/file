# 一、this的作用

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/this

1、与其他语言相比，函数的  this  关键字在 JavaScript 中的表现略有不同，此外，在[严格模式](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Strict_mode)和非严格模式之间也会有一些差别。

2、在绝大多数情况下，函数的调用方式决定了  this 的值（运行时绑定）。this 不能在执行期间被赋值，并且在每次函数被调用时 this  的值也可能会不同。ES5 引入了 [bind](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind) 方法来设置函数的  this 值，而不用考虑函数如何被调用的。ES2015 引入了[箭头函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)，箭头函数不提供自身的 this 绑定（this 的值将保持为闭合词法上下文的值）。

```javascript
let obj = {
    name: "哈哈哈",
    eating: function () {
        /**
         * 这块不使用this，也是可以的
         * 但是如果 你 obj 改变了，下面就要改变，这样会很不方便（在开发过程中，要切记不要低代码开发）
         */
        console.log(obj.name + "吃东西")
    },
    running: function () {
        console.log(this.name + "运动")
    },
    studying: function () {
        console.log(this.name + "学习")
    }
}
obj.eating()
obj.running()
obj.studying()
```

可查看下面这个案例（引用于MDN）：

<iframe height="490" src="https://interactive-examples.mdn.mozilla.net/pages/js/expressions-this.html" title="MDN Web Docs Interactive Example" loading="lazy"></iframe>

# 二、this的作用域

## 1、在全局作用域下

<font color=red>注：在全局作用域下，this指向window</font>

```javascript
console.log(this)
console.log(window)
```

## 2、在node环境下，this的指向

1）在node下，指向空的对象

2）在node下执行（其实源码里面是call来实现的）

![image-20220224235630762](https://gitee.com/Green_chicken/picture/raw/master/20220224235634.png)

## 3、<font color=red>this在函数中的使用</font>

注：<font color=red>this指向什么，跟函数所处的位置是没有关系的，跟函数 被调用的方式是有关系的；他是在运行时被绑定的</font>

```javascript
function fun(){
    console.log(this)
}
/**
 * 下面提供了三种调用方式
 */
fun()

let obj = {
    name:"哈哈",
    /**
     * foo现在指向上面的函数
     */
    foo: fun
}
obj.foo()

fun.apply("啊啊啊啊")
```

![image-20220227191556344](https://gitee.com/Green_chicken/picture/raw/master/20220227191558.png)

# 三、this的绑定规则

注：this的指向，与调用的位置（或方式）有关

## 1、默认绑定

注：独立函数调用，也就是在window下 函数名/变量名() 这样子去运行的；未加任何绑定（或者 .XXX() 去进行运行的绑定）

```javascript
/**
 * 独立函数调用，这个时候 指向window
 * 下面的案例，一块不太好看，你可以挨个去运行，查看this的指向
 */
// 案例1
function fun1() {
    console.log("fun1", this)
}

fun1()

/**
 * 下面都是 独立被调用
 */
// 案例2
function fun2() {
    console.log("fun2", this)
}

function fun3() {
    console.log("fun3", this)
    fun2()
}

function fun4() {
    console.log("fun4", this)
    fun3()
}

fun4()

// 案例3 *
let obj = {
    name: "哈哈哈",
    fun5: function () {
        console.log("fun5", this)
    }
}
// obj.fun5() // 这个有调用主题（他不是 独立的，这个时候，它指向obj）
const fun6 = obj.fun5
fun6() // 指向window

// 案例4
function fun7(){
    console.log("fun7", this)
}
let obj2 = {
    name: "哈哈哈",
    fun: fun7
}
// obj.fun5() // 这个有调用主题（他不是 独立的，这个时候，它指向obj）
const fun8 = obj2.fun
fun8() // 指向window

// 案例5
function fun9(){
    function bar(){
        console.log("fun9", this)
    }
    return bar
}

const fun10 = fun9()
fun10()
```

## 2、隐式绑定

注：通过某个对象，进行的函数调用

![image-20220227224234941](https://gitee.com/Green_chicken/picture/raw/master/20220227224236.png)

```javascript
// case1
function fun1(){
    console.log(this)
}
let obj1 = {
    age:222,
    foo: fun1
}
obj1.foo() // obj1
let bar = obj1.foo
bar() // window

// case2
let obj2 = {
    age:22,
    foo: function () {
        console.log(this, this.age + obj2.age)
    }
}
obj2.foo()

// case3
let obj3 = {
     name: "obj3",
     foo: function () {
         console.log(this)
     }
}
let obj4 = {
    name: "obj4",
    // 对象中的属性，指向函数
    foo: obj3.foo
}
obj4.foo()
```

## 3、显示绑定

call、apply、bind 改变函数体内的this指向

语法：函数.call()、函数.apply()、函数.bind()

### 1）call

![image-20220227225543659](https://gitee.com/Green_chicken/picture/raw/master/20220227225545.png)

① call立即执行

```javascript
let name = "你好"
let obj = { age: 22, name: "小明" }
function fun(){
    console.log(this, this.name)
}
fun.call(obj) // 立即执行
fun()
```

② call有多个参数时需要罗列（也就是按个书写）

```javascript
let name = "你好"
let obj = { name: "小明" }
function fun(age, gender){
    // 这个 是传入的
    this.age = age
    this.gender = gender
    console.log(this, this.name)
}

fun.call(obj, 222, "boy")
```

![image-20220227230554191](https://gitee.com/Green_chicken/picture/raw/master/20220227230555.png)

### 2）apply

① apply立即执行

```javascript
let name = "你好"
let obj = { age: 22, name: "小明" }
function fun(){
    console.log(this, this.name)
}
fun.apply(obj) // 立即执行
fun()
```

② apply的第二个参数是数组

```javascript
let name = "你好"
let obj = { name: "小明" }
function fun(age, gender){
    // 这个 是传入的
    this.age = age
    this.gender = gender
    console.log(this, this.name)
}
fun.apply(obj, [ 222, "boy" ])
```

![image-20220227230929616](https://gitee.com/Green_chicken/picture/raw/master/20220227230932.png)





### 3）bind

① bind不立即执行

```javascript
let name = "你好"
let obj = { age: 22, name: "小明" }
function fun(){
    console.log(this, this.name)
}
fun.bind(obj) // 不立即执行(他返回出来 是一个函数)
fun.bind(obj)() // 立即执行要jia 括号
fun()
```

② bind有多个参数时需要罗列（也就是按个书写）

```javascript
let name = "你好"
let obj = { name: "小明" }
function fun(age, gender){
    // 这个 是传入的
    this.age = age
    this.gender = gender
    console.log(this, this.name)
}
fun.bind(obj, 222, "boy")()
```

![image-20220227231141923](https://gitee.com/Green_chicken/picture/raw/master/20220227231143.png)

```javascript
function fun(){
    console.log(this)
}
/**
 * 1、bind的返回值 是一个函数
 * 2、bind是函数的方法
 * 3、传入的参数，就是绑定的this
 */
let newFun = fun.bind("传入的参数(也就是绑定this)，就是绑定的this")
newFun()
```

### 4）call、apply、bind使用场景

```javascript
let arr = [ 1, 2, 4, 6, 8, 11, 134, 0 ]
// console.log(Math.max(arr)) // NaN
/**
 * 这块不涉及this的绑定，所以穿空
 * 这块只能时apply，因为apply的第二个参数 是数组
 * Math.XXX 的apply只能写在类似于下面的这个位置
 */
console.log(Math.max.apply(null ,arr))
```

```javascript
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
    </head>
    <body>
        <input type="button" value="按钮">
        <br>
        <h2>内容</h2>
    </body>
</html>
<script>
    const but = document.querySelector("input")
    const content = document.querySelector("h2")
    /**
     * 这块就要使用 bind ，因为 你不想让他自动触发
     */
    but.onclick = function (){
        console.log(this) // 指向h2
    }.bind(content)
</script>
```

## 4、自己实现bind方法

```javascript
function fun() {
    console.log(this);
}

var obj = {
    name: "why"
}

function bind(func, obj) {
    return function () {
        // arguments传递过来的 所有参数
        console.log(arguments);
        return func.apply(obj, arguments);
    }
}

var bar = bind(fun, obj);

bar(); // obj对象
```

## 5、new绑定

```javascript
/**
 * new构造函数
 * 1、this = 创建出来的对象
 * 2、这个绑定过程，就是new绑定
 */
function Person(val1, val2){
    this.val1  = val1
    this.val2  = val2
}
let f1 = new Person("呵呵", "哈哈哈")
console.log(f1.val1, f1.val2)

let f2 = new Person("age", 25)
console.log(f2.val1, f2.val2)
```

 ![image-20220228231212002](https://gitee.com/Green_chicken/picture/raw/master/20220228231213.png)

