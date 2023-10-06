# 一、字面量

1、属性的简写

key 和 value 一致时，就可以按照下面这样写了

```javascript
const name = '里斯'
const age =22

// const  obj = {
//     name: name,
//     age: age
// }

const obj = {
    name,
    age
}

console.log(obj)
```

2、方法的简写

```javascript
const name = '里斯'
const age =22

const obj = {
    name,
    age,
    // 原始写法
    foo: function (){
        console.log(this) // {name: '里斯', age: 22, foo: ƒ, fun1: ƒ}
    },
    // 对象中方法的简写
    fun(){
        console.log(this) // {name: '里斯', age: 22, foo: ƒ, fun1: ƒ}
    },
}

console.log(obj.foo())
console.log(obj.fun1())
```

3、添加属性（计算属性名）

```javascript
const name = '里斯'
const age =22

const obj = {
    name,
    age,
    // 计算属性名
    [name + '定义']: '哈哈哈'
}

// 原始的写法
obj[name + '定义1'] = '呵呵呵'

console.log(obj) // {name: '里斯', age: 22, 里斯定义: '哈哈哈', 里斯定义1: '呵呵呵'}
```

# 二、解构赋值

## 1、数组的结构

```javascript
const names = ['呵呵', '哈哈', '啊啊']
// 读取数组的值
// console.log(names[0])
// console.log(names[2])

// 只解构后面两个的元素，因为 解构是按照顺序来写的
const [, name1, name2] = names
console.log(name1, name2) // 哈哈 啊啊

// 之解构出一个元素，后面的放到一个数组中
const [name0, ...rest] = names
console.log(name0, rest) // 呵呵  ['哈哈', '啊啊']

// 解构一个默认值
const [namea, nameb, namec, named = '默认值'] = name
console.log(named) // 默认值
```

## 2、对象的解构

```javascript
const obj = {
    age: 11,
    name: '哈哈',
    sex: '男'
}

// 对象的解构，对象的解构没有顺序，但是 一定要写准确的 key
const {name, age, sex} = obj
console.log(name, age, sex) // 哈哈 11 男

// 修改解构出来的命名
const {name: newName} = obj
console.log(newName) // 哈哈

// 解构时给对象中没有的属性 默认值
const { address = '火星' } = obj
console.log(address) // 火星
const { address: newAddress = '水星' } = obj
console.log(newAddress) // 水星
```

## 3、解构的使用

```typescript
interface Obj {
    age: number, name: string, sex: string
}
interface IProps extends Obj{}
const obj: Obj = {
    age: 11,
    name: '哈哈',
    sex: '男'
}
function fun({
    age,
    name,
    sex
}: IProps){
    console.log(age, name, sex)
}
fun(obj)

function arr(){
    return [1, 2, 3]
}
const [, b, c] = arr()
console.log(b, c)
```

# 三、var、let、const 的区别

## 1、const

```javascript
// const 本质上是传递的值不可修改
// 但是如果传递的是一个引用类型（内存地址），但是可以通过引用找到对应的对象，区修改对象内部的属性，这个是可以的
const obj = {
    name: '哈哈'
}

// obj = {} //  Assignment to constant variable.
obj.name = '呵呵'
```

![image-20230426232421825](https://not-have.github.io/file/images/image-20230426232421825.png)

## 2、let / const 作用域

1）let

![image-20230426234648342](https://not-have.github.io/file/images/image-20230426234648342.png)

a 变量不是在代码执行到第 6 行的时候，别创建出来的；而是在执行上下文的词法环境中被创建的，只是 js 引擎规定，他在第 5 行时，是不可以别访问的。（也就是说在第 5 行时已经创建，只是不能访问）

注：执行上下文中会创建词法环境。

![image-20230426235258409](https://not-have.github.io/file/images/image-20230426235258409.png)

作用域提升：在声明变量的作用域中，如果这个变量可以在声明之前被访问，那么我们可以称之为作用域提升，在这里，它虽然被创建出来了，但是不能被访问，我认为不能称之为作用域提升。

注：let、const 没有作用域提升，但是在执行上下文的时候，已经被创建出来了。

## 3、let、const 和 window 的关系

1）var 与 window 的关系

<font color=red>在全局通过var来声明一个变量，事实上会在window上添加一个属性，但是let、const是不会给window上添加任何属性的。</font>

```javascript
var a = 1

// var 定义的这个 a 会放到 window 里面
console.log(window.a)
```

注：下面这样写是可以修改 var 定义的变量，所以在日常开发中，使用 *var* 声明变量往往会成为 bug 的温床。

用 var 定义的变量 和 window.Xx 定义的变量，其实并不是在一个对象中，而修改他们中的一个时，另一个也修改，只是 为了保持他们之间的相等性。

```javascript
var a = 1

window.a = 2

console.log(window.a)
```

## 4、块级作用域

![image-20230516235703833](https://not-have.github.io/file/images/image-20230516235703833.png)

### 1）ES5 作用域

```javascript
// // 声明对象中的字面量 （对象里面时键值对）
// var obj = {
//     name: '老王'
// }
//

/**
 * 代码块（block code）
 * 按理来说，你在代码块中定义了变量，他就 只能在当前代码块中使用
 * ES5 你使用了 var 定义了变量之后，你在代码块之前还能使用
 * ES5 中只有两个东西会形成作用域：全局作用域 / 函数作用域
 */
{
    // 表达式
    var foo = '哈哈哈'
}
console.log(foo)
```

### 2）ES6 作用域

```javascript
/**
 * 代码块（block code）
 * ES6 的代码块级作用域
 * let / const / function / class 声明的类型是有效
 *
 */
{
    // 表达式
    let foo = '哈哈哈'
    function fun() {
        return '老王'
    }
}
// console.log(foo) // foo is not defined
// 不同的浏览器有不同的实现，大部分浏览器为了兼容之前的代码，是让 function 没有代码块级作用域，如果你在一个纯的 ES6 浏览器上跑，他是提示 fun is not defined
console.log(fun()) // 老王
```

注：<font color=red>if、switch、for 都是代码块级作用域。</font>

### 3）应用场景

![image-20230517002753990](https://not-have.github.io/file/images/image-20230517002753990.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
</head>
<body>
    <button>按钮一</button>
    <button>按钮二</button>
    <button>按钮三</button>
    <button>按钮四</button>
    <script>
        const buts = document.getElementsByTagName('button')

        for (var i = 0; i < buts.length; i++) {
            // buts[i].onclick = function (){
            //     console.log(`点击第 ${i} 个按钮`) // 这样子定义下来，你不管点击那个按钮的时候，他都会显示 点击第 4 个按钮
            // }
            // 解决方案 如下：
            (function (n) { // 因为 function 是有块级作用域的
                buts[i].onclick = function () {
                    console.log(`点击第 ${n} 个按钮`) // 这样子定义下来，你不管点击那个按钮的时候，他都会显示 点击第 4 个按钮
                }
            })(i)
        }
    </script>
</body>
</html>
```

直接使用块级作用域解决：

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
</head>
<body>
    <button>按钮一</button>
    <button>按钮二</button>
    <button>按钮三</button>
    <button>按钮四</button>
<script>
    for (let i = 0; i < buts.length; i++) {
        buts[i].onclick = function (){
            console.log(`点击第 ${i} 个按钮`) // 这样子定义下来，你不管点击那个按钮的时候，他都会显示 点击第 4 个按钮
        }
    }
</script>
</body>
</html>
```

