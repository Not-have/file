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

注：ref、Reactive都是深层的响应式。

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

3）使用ref获取DOM元素

![image-20211220221915821](https://gitee.com/Green_chicken/picture/raw/master/20211220221917.png)

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

如果我们想要获取一个ref引用中的value，那么也可以通过unref方法

如果参数是一个 ref，则返回内部值，否则返回参数本身； 

这是 val = isRef(val) ? val.value : val 的语法糖函数。

### 2）isRef

判断值是否是一个ref对象

### 3）shallowRef

创建一个浅层的ref对象

```javascript
<template>
    <p>{{info}}</p>
    <button @click="changeInfo">点击</button>
</template>

<script>
import { ref, shallowRef } from '@vue/reactivity'

export default {
    name: 'index',
    components: {

    },
    setup() {
        const info = shallowRef({ nmae: "哈哈哈" })
        const changeInfo = () => {
            info.value.nmae = "呵呵呵"
        }
        return { info, changeInfo }
    }
}
</script>
```

### 4）triggerRef

手动触发和 shallowRef 相关联的副作用

```javascript
<template>
    <p>{{info}}</p>
    <button @click="changeInfo">点击</button>
</template>

<script>
import { ref, shallowRef, triggerRef } from '@vue/reactivity'

export default {
    name: 'index',
    components: {

    },
    setup() {
        const info = shallowRef({ nmae: "哈哈哈" })
        const changeInfo = () => {
            info.value.nmae = "呵呵呵"
            triggerRef(info)
        }
        return { info, changeInfo }
    }
}
</script>
```

## 9、customRef

创建一个自定义的ref，并对其依赖项跟踪和更新触发进行显示控制

```javascript
/**
 * 实现一个防抖的ref
 * 下面这个一般写在hook里面
 */
import { customRef } from "vue"
/**
 * 自定义ref
 * customRef 里面两个参数
 * track 跟踪（决定什么时候 收集依赖）
 * trigger 决定什么时候触发所有的依赖,进行更新
 * 返回一个带有 get 和 set 的对象
 */
export default function (value, delay = 200) {
    let time = null
    return customRef((track, trigger) => {
        return {
            get() {
                // 收集依赖
                track()
                return value
            },
            // 设置新的值
            set(newValue) {
                // 处理数据
                clearTimeout(time)
                time = setTimeout(() => {
                    value = newValue
                    trigger()
                }, delay)
            }
        }
    })
}
```

在页面中的使用：

![image-20211212190831716](https://gitee.com/Green_chicken/picture/raw/master/20211212190833.png)

## 10、computed 计算属性

方式一：接收一个getter函数，并为 getter 函数返回的值，返回一个不变的 ref 对象； 
方式二：接收一个具有 get 和 set 的对象，返回一个可变的（可读写）ref 对象。

```javascript
<template>
    <p>{{and}}</p>
    <button @click="fun">修改</button>
</template>
<script>
import { computed, ref } from "vue"
export default {
    name: 'index',
    setup() {
        const str1 = ref("lalall111")
        const str2 = ref("hahaha")
        /**
         * 方法一：
         * 传入一个getter函数
         * computed 返回的是一个ref对象
         */
        // const and = computed(() => str1.value + str2.value)
        /**
         * 方法二：
         * 传入一个对象，包含 etter/setter
         * computed 返回的是一个ref对象
         */
        const and = computed({
            get: () => str1.value + str2.value,
            set(newValue) {
                str1.value = newValue
            }
        })
        const fun = function () {
            and.value = "哈哈哈"
        }
        return { and, fun }
    }
}
</script>
```

## 11、watchEffect

### 1）watchEffect 自动响应式依赖

注：会立即执行一次，然后改变变量，这里就能监听到

```javascript
<template>
    <div>
        <p>{{name}}</p>
        <p> {{age}}</p>

        <p><button @click="changeName">点击</button><button @click="changeNameAge">点击</button></p>
    </div>
</template>

<script lang='ts'>
import { defineComponent, ref, watchEffect } from "vue";
export default defineComponent({
    name: "index",
    components: {},
    setup() {
        const name = ref("小明");
        const age = ref(22);
        const changeName = () => (name.value = "小花");
        const changeNameAge = () => age.value++;
        /**
         * 里面是响应式对象
         */
        watchEffect(() => {
            console.log("name", name.value);
            console.log("age", age.value);
        });
        return { name, age, changeName, changeNameAge };
    },
});
</script>
```

### 2）watchEffect的停止监听的使用:

```javascript
<template>
    <div>
        <p>{{name}}</p>
        <p> {{age}}</p>

        <p><button @click="changeName">点击</button><button @click="changeNameAge">点击</button></p>
    </div>
</template>

<script lang='ts'>
import { defineComponent, ref, watchEffect } from "vue";
export default defineComponent({
    name: "index",
    components: {},
    setup() {
        const name = ref("小明");
        const age = ref(22);
        const changeName = () => (name.value = "小花");
        const changeNameAge = () => {
            age.value++;
            if (age.value > 25) {
                stop();
            }
        };
        /**
         * 里面是响应式对象
         * 返回一个函数，在外面调用，他就会停止监听
         */
        const stop = watchEffect(() => {
            console.log("name", name.value, "age", age.value);
        });
        return { name, age, changeName, changeNameAge };
    },
});
</script>
```

### 3）watchEffect清除副作用

注：在网络请求中，当数据更新了之后，应该使用新数据去请求，停掉之前的那个的时候用。

```javascript
<template>
    <div>
        <h2>{{name}}-{{age}}</h2>
        <button @click="changeName">修改name</button>
        <button @click="changeAge">修改age</button>
    </div>
</template>

<script>
import { ref, watchEffect } from 'vue';

export default {
    setup() {
        // watchEffect: 自动收集响应式的依赖
        const name = ref("why");
        const age = ref(18);

        const stop = watchEffect((onInvalidate) => {
            const timer = setTimeout(() => {
                console.log("网络请求成功~");
            }, 2000)

            // 根据name和age两个变量发送网络请求
            onInvalidate(() => {
                // 在这个函数中清除额外的副作用
                // request.cancel()
                clearTimeout(timer);
                console.log("onInvalidate");
            })
            console.log("name:", name.value, "age:", age.value);
        });

        const changeName = () => name.value = "kobe"
        const changeAge = () => {
            age.value++;
            if (age.value > 25) {
                stop();
            }
        }

        return {
            name,
            age,
            changeName,
            changeAge
        }
    }
}
</script>
```

### 4）watchEffect的执行时机

```javascript
<template>
    <h2 ref="text">哈哈哈</h2>
</template>

<script>
import { ref, watchEffect } from 'vue'

export default {
    name: 'index',
    components: {

    },
    setup() {
        let text = ref(null)
        watchEffect(() => {
            console.log(text.value)
        }, {
            /**
             * pre 默认值 在元素 挂载 或者 更新 之前执行
             * post 监听DOM时，可修改为这个值
             * sync 强制效果始终同步触发
             */
            flush: "post"
        })
        return { text }
    }
}
</script>
```

## 12、watch

### 1）监听单个数据源

① reactive的监听

```javascript
<template>
    <p>{{ info }}</p>
    <button @click="fun">点</button>
</template>

<script>
import { reactive, watch } from 'vue'

export default {
    name: 'index',
    setup() {
        const info = reactive({ name: "哈哈哈", age: 22 })
        /**
         * 第一种写法
         */
        watch(() => info.age, (newVal, oldVal) => {
            console.log(newVal, oldVal)
        })
        const fun = () => {
            info.age++
        }
        return { info, fun }
    }
}
</script>
```

② 对reactive对象的监听

```javascript
<template>
    <p>{{ info.age }}</p>
    <button @click="fun">点</button>
</template>

<script>
import { reactive, ref, watch } from 'vue'

export default {
    name: 'index',
    setup() {
        const info = reactive({ name: "哈哈哈", age: 22 })
        /**
         * 下面这样写 就可以获取成一个 普通的对象
         */
        watch(() => ({ ...info }), (newVal, oldVal) => {
            console.log(newVal, oldVal)
        })

        const fun = () => {
            info.age++
        }
        return { info, fun }
    }
}
</script>
```

③ ref的监听

```javascript
<template>
    <p>{{ num }}</p>
    <button @click="fun">点</button>
</template>

<script>
import { reactive, ref, watch } from 'vue'

export default {
    name: 'index',
    setup() {
        const num = ref(0)
        watch(num, (newVal, oldVal) => {
            console.log(newVal, oldVal)
        })

        const fun = () => {
            num.value++
        }
        return { num, fun }
    }
}
</script>
```

### 2）监听多个数据源

```javascript
<template>
    <p>{{ info.age }}</p>
    <p>{{ num }}</p>
    <button @click="fun">点</button>
</template>

<script>
import { reactive, ref, watch } from 'vue'

export default {
    name: 'index',
    setup() {

        let info = reactive({ name: "哈哈哈", age: 22 })
        let num = ref(0)
        // watch([info, num], (newVal, oldVal) => {
        //下面可以 挨个打印
        watch([info, num], ([newInfo, newNum], [oldInfo, oldNum]) => {
            console.log(newInfo, newNum, oldInfo, oldNum)
        })

        const fun = () => {
            info.age++
            num.value++
        }
        return { info, num, fun }
    }
}
</script>
```

### 3）watch的选项

```javascript
<template>
    <p>{{ info }}</p>
    <button @click="fun">点</button>
</template>

<script>
import { reactive, ref, watch } from 'vue'

export default {
    name: 'index',
    setup() {

        let info = reactive({ name: "哈哈哈", age: 22, child: { name: "呵呵" } })
        watch(info, (newVal, oldVal) => {
            console.log(newVal, oldVal)
        }, {
            deep: true, // 深度监听
            immediate: true // 立即执行（西药第一次 进来就打印）
        })

        const fun = () => {
            info.child.name = "啊啊啊啊啊"
        }
        return { info, fun }
    }
}
</script>
```

## 13、provide

注：修改数据的时候，请尽量做到单向数据流。

![image-20211223113736557](https://gitee.com/Green_chicken/picture/raw/master/20211223113739.png)

## 14、hooks的使用

![image-20211223171341205](https://gitee.com/Green_chicken/picture/raw/master/20211223171342.png)

### 1）封装一个修改title

```javascript
import { ref, watch } from 'vue'
export default function (title = "默认值") {
    const titleRef = ref(title)
    /**
     * watch 是惰性的
     */
    watch(titleRef, newValue => {
        document.title = newValue
    }, { immediate: true })
    return titleRef
}
```

使用：

```javascript
<template>
    <p>hooks 的使用</p>
    <p> <button @click="fun">修改Title</button> </p>
</template>

<script lang='ts'>
import { defineComponent } from "vue";
import userTitle from "./userTitle";
export default defineComponent({
    name: "index",
    components: {},
    setup() {
        function fun() {
            let title = userTitle("你好");
            setTimeout(() => {
                title.value = "测试";
            }, 3000);
        }
        return { fun };
    },
});
</script>
```

### 2）监听界面滚动位置

```javascript
import { ref } from 'vue'
export default function () {
    const scrollX = ref(0);
    const scrollY = ref(0);
    document.addEventListener("scroll", () => {
        scrollX.value = window.scrollX;
        scrollY.value = window.scrollY;
    });
    return { scrollX, scrollY }
}
```

### 3）监听鼠标位置

```javascript
import { ref } from 'vue'
export default function () {
    const mouseX = ref(0)
    const mouseY = ref(0)

    window.addEventListener("mousemove", event => {
        mouseX.value = event.pageX
        mouseY.value = event.pageY
    })
    return { mouseX, mouseY }
}
```



