# 一、函数的使用

## 1、函数作为参数

```javascript
// 将函数作为另一个函数的参数
function foo(fn){
    fn()
}
function bar(){
    console.log("将函数作为另一个函数的参数")
}
foo(bar)


function calc(num1, num2, calcfn){
    console.log(calcfn(num1,num2))
}
function add(num1, num2){
    return num1 + num2
}
function  sub(num1, num2){
    return num1 - num2
}

let num1 = 10, num2 = 20;
calc(num1,num2,sub)
```

## 2、函数作为返回值

1）简单的案例

```javascript
function foo(){
    function bar(){
        console.log("函数作为返回值")
    }
    // 返回bar函数
    return bar
}

const fn = foo()
fn() // 这个其实是在指执行bar函数
```

2）参数相加

```javascript
/**
 * 按理说，下面的fun执行结束，count就会销毁，但是因为闭包的原因，导致他不会进行销毁
 */
function fun(count){
     return function add(num){
        return count + num
    }
}

const fn2 = fun(3)
console.log(fn2(9)) // 12
console.log(fn2(11)) // 14
```

## 3、函数和方法的具体区别

函数：独立的function，称之为是一个函数

方法：当我们的一个函数属于某一个对象时，我们称这个函数为这个对象的方法

## 4、数组中的函数

### 1）filter的使用

```javascript
let arr = [11,22,55,44,66]
/**
 * filter() 过滤的使用(他是方法)
 * 他的返回值是个布尔值,同时也会返回一个 数组
 * filter()的参数是个函数，该函数的参数如下：
 * item 每个元素
 * index 每个元素的下标
 * Array 传入的整个数组
 */
const newArr = arr.filter(function (item, index, Array) {
    if(item % 2 === 0){
        return true
    }
})
console.log(newArr) // [ 22, 44, 66 ]
```

### 2）map 映射

```javascript
let arr = [11,22,55,44,66]
/**
 * map 映射
 * 他有返回值
 */
let newArr = arr.map(function (item, index){
    return item + 1
})
console.log(newArr)
```

### 3）forEach 迭代

```javascript
let arr = [11,22,55,44,66]
arr.forEach(function (item, index){
    console.log(item,index)
})
```

### 4）find 查找元素

```javascript
let arr = [11,22,55,44,66]
/**
 * find 查找
 * 他的返回值时布尔值
 * 他会返回一个元素
 */
let newArr = arr.find(function (item){
    if (item === 22) return item
})
console.log(newArr)
```

### 5）findIndex 查找对象所对的索引值

```javascript
let arr = [ 11, 22, 55, 44, 66 ]
const findIndex = arr.findIndex(function (item){
    if (item === 22) return true
})
console.log(findIndex)
```

### 6）reduce 累加

```javascript
let arr = [ 11, 22, 55, 44, 66 ]
/**
 * reduce 累加
 * 他的参数是个函数 和 初始值
 * 该函数的参数 如下：
 * preValue 上一次的返回值
 * item 当前值
 * 初始值 可以按照需求传入,默认不穿的时候为0
 */

let newArr4 = arr.reduce(function (preValue, item){
    // 返回一个10，就代表他的第二次来的时候preValue就为十了
    // return 10
    return preValue + item
}, 0)
console.log(newArr4) // 198
```

# 二、JS闭包

## 1、闭包的初步认识

1）闭包是指有权访问另一个函数作用域中的变量的函数（也就是能够读取其他函数内部变量的函数）

2）作用：函数内部的变量，只能在当前函数内部使用，而你要在函数外部使用函数内部的变量，这个时候，你就要使用闭包来完成

文档：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures

```javascript
function makeFunc() {
    let name = "你好"
    function displayName() {
        console.log(name)
    }
    return displayName
}
/**
 * 在makeFunc()执行完之后，按理说  它里面的内容是要销毁的（也就是 name 本来是要销毁的）
 * 但是在执行displayName这个函数的时候，你依然可以访问到本来要销毁的变量时，依然可以访问到
 * 在下面调用（displayName）的时候，依然可以访问到makeFunc函数里面的变量，这个时候，他就形成了闭包
 * 闭包的组成：函数 + 可以访问的自由变量
 */
let myFunc = makeFunc()
//myFunc就是displayName这个函数
myFunc();
```

## 2、闭包的定义

① 闭包跟函数最大的区别在于，当捕捉闭包的时候，它的 自由变量 会在补充时被确定，这样即使脱离了捕捉时的上下文，它也能照常运行；

② 闭包在实现上是一个结构体（整体），它存储了一个函数和一个关联的环境（相当于一个符号查找表）；

③ 闭包是由两个东西组成，一个可以用来返回的函数，一个外层的自由变量，只有能在返回的这个函数里，访问到外层的这个自由变量，这两个东西结合在一起，就组成了严格意义上的闭包；



