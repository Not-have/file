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

```javascript
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

