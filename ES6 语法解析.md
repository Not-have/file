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

### 5）暂时性死区

![image-20240116155145401](https://not-have.github.io/file/images/image-20240116155145401.png)

# 四、模板字符串 - 标签模板字符串



```javascript
/**
 * 标签模板字符串
 * @param m 第一个参数依然是模块字符串中整个字符串，只是被切成多块,放到了一个数组中
 * @param n 第二个参数是模块字符串中，第一个
 * @param x 第三个参数是模块字符串中，第二个
 * 模板中传入了多少，就会有多少个参数，依次去获取值
 */
function fun(m, n, x) {
    console.log(m, n, x); // [ '你好', '拉拉', '。' ] 第一个参数 第二个参数
}

const one = "第一个参数";
const two = "第二个参数";
fun`你好${one}拉拉${two}。`;
```

注：React 中 style-components 就是使用了`标签模板字符串`。

# 五、展开运算符

```javascript
const names = ["哈哈", "啊啊啊", "呵呵"];
const name = "哦哦哦";
const info = {name: "小明", age: 16};

/**
 * 1、函数调用
 */
// 方法一
function fun1(...args) {
    console.log(args);
}

// fun1.apply(null, names);
fun1(...names);
fun1(name);

// 方法二
function fun2(x, y, z) {
    console.log(x, y, z);
}

fun2(...names);
fun2(...name);

/**
 * 2、构造数组
 */
const newNames = [...names, ...name];
console.log(newNames);

/**
 * 3、构建对象字面量
 * 后面添加的覆盖前面的
 */
const obj = {...info, address: "中国"};
console.log(obj);
obj.age = 18;
console.log(info.age);
```

# 六、Symbol

注：使用 Symbol 生成一个独一无二的值，来解决对象中属性名的冲突问题。

```javascript
/**
 * Symbol 是一个函数
 * 他会生成一个唯一的值
 * 使用 Symbol("test").description 可以获取到描述（也就是 Symbol() 中传入的内容）
 */
const sl1 = Symbol();
const sl2 = Symbol();
const sl3 = Symbol("test");

console.log(sl3.description); // test

const obj = {
    [sl1]: '哈哈哈'
};

obj[sl2] = "啊啊啊";

Object.defineProperty(obj, sl3, {
    enumerable: true,
    configurable: true,
    writable: true,
    value: "呵呵呵"
});

/**
 * 获取
 * 不能使用 . 语法
 */
console.log(obj[sl2]);

/**
 * 使用 Symbol 作为 key 的属性名，在遍历 Object.keys 等中是获取不到这些 Symbol 值的
 */
console.log(Object.keys(obj)); // 获取不到的
console.log(Object.getOwnPropertyNames(obj)); // 获取不到的

/**
 * 真实的获取 和 遍历
 */
console.log(Object.getOwnPropertySymbols(obj));

const slKeys = Object.getOwnPropertySymbols(obj);

for (const key of slKeys) {
    console.log(obj[key]);
}

/**
 * Symbol 创建一个相同的 key
 * Symbol.for(key)
 */

const sl4 = Symbol.for("same");
const sl5 = Symbol.for("same");
console.log(sl4 === sl5);
// 获取key
const sameKey = Symbol.keyFor(sl4);
console.log(sameKey);
```

# 七、Set 的基本使用

注：类似于数组，但是里面的数据是不能重复的（他是新增的数据结构）。

## 1、基本使用

```javascript
/**
 *  set类似于数组，但是里面的数据是不能重复的
 */
const arr = new Set();

arr.add(10);
arr.add(10); // 这个 10 在 Set 中是不存在的
arr.add(20);
arr.add(30);
arr.add(40);
arr.add(60);

// 他是存在两份的，因为 对象不是一个（分别是两个内存地址）
arr.add({age: 10});
arr.add({age: 10});
//但是下面这样写，就会只存在一个了，因为指向了同一个内存地址
const obj = {age: 16};
arr.add(obj)
arr.add(obj)

console.log(arr);
```

## 2、使用 Set 去重

```javascript
const obj = {age: 16};

/**
 * 使用 Set 的特性进行去重
 */
const arr = [11, 22, 33, 44, 55, 55, {age: 18}, {age: 18}, obj, obj];

const removeDuplicates = new Set(arr);
const newArr = [...removeDuplicates]; // 修改输出
console.log(newArr); // [ 11, 22, 33, 44, 55, { age: 18 }, { age: 18 }, { age: 16 } ]
```

## 3、size 属性

```javascript
const arr = [11, 22, 33, 44, 55, 55];

const setArr = new Set(arr);

// size 属性
console.log(setArr.size); // 6
```

## 4、delete 删除元素

数组.delete(要删除的元素);

注：他没有索引，所以无法传入索引。

```javascript
const arr = [11, 22, 33, 44, 55, 55];

const setArr = new Set(arr);

setArr2.delete(11)

console.log(arr);
```

## 5、has 是否存在

```javascript
const arr = [11, 22, 33, 44, 55, 55];

const setArr = new Set(arr);

/**
 * has 是否存在，存在返回 true，不存在返回 false
 */
console.log(setArr.has(22));
```

## 6、clear 清除 Set

```javascript
const arr = [11, 22, 33, 44, 55, 55];

const setArr = new Set(arr);

setArr.clear();
```

# 八、WeakSet

注：

① WeakSet 只能存放对象；

② 对对象是个弱引用;

```javascript
const arr1 = new WeakSet();

const obj1 = {
    age: 16
};
const obj2 = {
    age: 18
};
const obj3 = {
    age: 20
};

/**
 * add 添加元素
 */
arr1.add(obj1);
arr1.add(obj2);
arr1.add(obj3);

/**
 * delete 删除元素
 */
arr1.delete(obj1);

/**
 * has 是否包含
 */
console.log(arr1.has(obj2));

console.log(arr1);

/**
 * 应用场景
 */
const personSet = new WeakSet();

class Person {
    constructor() {
        personSet.add(this);
    }

    running() {
        if (!personSet.has(this)) {
            throw new Error("不能通过非构造方法创建出来的对象调用 running 方法");
        }
        console.log("跑步", this);
    }
}

const p = new Person();
p.running();

// 这里等于 null 的话 const personSet = new WeakSet(); 会直接销毁掉
// p = null;

p.running.call({name: "小明"});
```

# 九、Map 的使用

注：用来存储映射关系的（也就是键值对）。

```javascript
const obj1 = {
    name: "里斯"
};

const obj2 = {
    age: 18
};

/**
 * js 中不允许对象为 key
 */
const info = {
    [obj1]: "啊啊啊",
    [obj2]: "哈哈哈"
};

console.log(info); // { '[object Object]': '哈哈哈' }

/**
 * Map 允许 key 为对象
 */
const mapTest1 = new Map();
mapTest1.set(obj1, "啊啊啊");
mapTest1.set(obj2, "哈哈哈");
mapTest1.set(1, "呵呵呵");

console.log(mapTest1);

// 也可使用下面的方式创建
const mapTest2 = new Map();
mapTest2.set(obj1, "啊啊啊");
mapTest2.set(obj2, "哈哈哈");
mapTest2.set(2, "呵呵呵");

console.log(mapTest2);

/**
 * Map 的属性
 * set
 * get 传入 key 获取对应的 value
 * has 判断某个 key 是否存在
 * delete 删除指定 key 的元素
 * clear 清除
 */
console.log(mapTest2.get(2));
console.log(mapTest2.has(2));
mapTest2.delete(2);
console.log(mapTest2);
mapTest2.clear();
console.log(mapTest2);

/**
 * 遍历
 */
mapTest1.forEach((item, index) => {
    console.log(item, index);
})

for (const item of mapTest1) {
    const [key, value] = item;
    console.log(item, key, value);
}
```



