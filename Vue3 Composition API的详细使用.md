# 一、setup函数的参数

## 1、主要有两个参数

1）第一个参数：props

2）第二个参数：context

## 2、props

props非常好理解，它其实就是父组件传递过来的属性会被放到props对象中，我们在setup中如果需要使用，那么就可 以直接通过props参数获取

注：setup中不能再用this获取东西了。

① 对于定义props的类型，我们还是和之前的规则是一样的，在props选项中定义；

② 并且在template中依然是可以正常去使用props中的属性，比如message；

③  因为props有直接作为参数传递到setup函数中，所以我们可以直接通过参数来使用即可；

![image-20211128190300681](https://gitee.com/Green_chicken/picture/raw/master/20211128190302.png)

## 3、context

1）attrs：所有的非prop的attribute（属性）

![image-20211128215424656](https://gitee.com/Green_chicken/picture/raw/master/20211128215426.png)

2）slots：父组件传递过来的插槽（这个在以渲染函数返回时会有作用）



3）emit：当我们组件内部需要发出事件时会用到emit（因为我们不能访问this，所以不可以通过 this.$emit发出事件）



# 二、setup中API的使用

## 1、setup返回值的作用

1）setup的返回值可以在模板template中被使用

2）通过setup的返回值来替代data选项

3）返回值必须是一个对象

 ![image-20211129221912620](https://gitee.com/Green_chicken/picture/raw/master/20211129221913.png)

## 2、Reactive

  ![image-20211129222645476](https://gitee.com/Green_chicken/picture/raw/master/20211129222646.png)

<font color=red>注：</font>Reactive API必须传入的是一个对象或者数组类型。

## 3、ref

1）ref 会返回一个可变的响应式对象，该对象作为一个 响应式的引用 维护着它内部的值，这就是ref名称的来源；

2）它内部的值是在ref的 value 属性中被维护的；

<font color=red>注：</font> ① 在模板中引入ref的值时，Vue会自动帮助我们进行解包操作，所以我们并不需要在模板中通过 ref.value 的方式 来使用；

​         ② 在 setup 函数内部，它依然是一个 ref引用， 所以对其进行操作时，我们依然需要使用 ref.value的方式；在 setup 函数内部，它依然是一个 ref引用， 所以对其进行操作时，我们依然需要使用 ref.value的方式； 

 ![image-20211204170119254](https://gitee.com/Green_chicken/picture/raw/master/20211204170121.png)

## 4、readonly

readonly会返回原生对象的只读代理（也就是它依然是一个Proxy，这是一个proxy的set方法被劫持，并且不 能对其进行修改）

注：用readonly包裹响应式数据，发送给子组件，让其不能在子组件中，随意改变，只能在组件中，去改变响应的数据，如果非要在子组件中改变传过去的数据，他就会报错。

 ![image-20211204173238773](https://gitee.com/Green_chicken/picture/raw/master/20211204173239.png)

## 5、Reactive其他API的使用

具体的，请参考文档： https://v3.cn.vuejs.org/api/basic-reactivity.html#isproxy

1）isProxy

检查对象是否是由 reactive 或 readonly创建的 proxy；

2）isReactive

检查对象是否是由 reactive创建的响应式代理，如果该代理是 readonly 建的，但包裹了由 reactive 创建的另一个代理，它也会返回 true；

3）isReadonly

检查对象是否是由 readonly 创建的只读代理；

4）toRaw

返回 reactive 或 readonly 代理的原始对象（不建议保留对原始对象的持久引用。请谨慎使用）；

5）shallowReactive

创建一个响应式代理，它跟踪其自身 property 的响应性，但不执行嵌套对象的深层响应式转换 (深层还是原生对象)，只做浅层的响应式代理；

6）shallowReadonly

创建一个 proxy，使其自身的 property 为只读，但不执行嵌套对象的深度只读转换（深层还是可读、可写的）；

## 6、toRefs

将reactive 将对象中的所有属性都转成ref，并且是一个reactive的响应式的对象，才能作为转换。

```javascript
<template>
    <p>toRef的使用</p>
    <button @click="fun">点击</button>
    <p>{{ info.name }} —————— {{ info.age }}</p>
</template>

<script lang='ts'>
import { defineComponent, reactive, toRefs } from "vue";

export default defineComponent({
    name: "index",
    components: {},
    setup() {
        let info = reactive({
            name: "李四",
            age: 18,
        });
        // 直接修改他俩 需要 .value
        const { age, name } = toRefs(info);
        const fun = () => {
            // info.age++;
            age.value++;
        };
        return { info, fun };
    },
});
</script>
```

## 7、toRef

对其中一个属性进行转换，转成ref属性

```javascript
<template>
    <p>toRef的使用</p>
    <button @click="fun">点击</button>
    <p>{{ name }} —————— {{ age }}</p>
</template>

<script>
import { reactive, toRef } from "vue";

export default {
    name: "index",
    components: {},
    setup() {
        let info = reactive({
            name: "李四",
            age: 18,
        });
        let { name } = info;
        // 只转一个(这里不能在做一个结构了)
        let age = toRef(info, "age");
        const fun = () => {
            age.value++;
        };
        return { name, age, fun };
    },
};
</script>
```

## 8、ref的其他API

具体的，请参考文档：https://v3.cn.vuejs.org/api/refs-api.html#unref

### 1）unref



