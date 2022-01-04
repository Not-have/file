# 一、认识vue-router

文档：https://next.router.vuejs.org/zh/

Vue Router 是 [Vue.js](http://v3.vuejs.org/) 的官方路由。它与 Vue.js 核心深度集成，让用 Vue.js 构建单页应用变得轻而易举。

用 Vue + Vue Router 创建单页应用非常简单：通过 Vue.js，我们已经用组件组成了我们的应用。当加入 Vue Router 时，我们需要做的就是将我们的组件映射到路由上，让 Vue Router 知道在哪里渲染它们。

# 二、使用

## 1、下载

```javascript
npm install vue-router@4
```

## 2、创建路由文件

注：在src——>创建router文件夹——>index.js文件

```javascript
import { createRouter, createWebHistory, createWebHashHistory } from 'vue-router';
// 1、引入组件
import Home from "../views/Home.vue"
import Router4 from "../views/Router4的使用/Index.vue"
/**
 * 2、配置路由映射关系
 */
const routes = [
    {
        path: '/home',
        component: Home
    },
    {
        path: "/router",
        component: Router4
    }
]
// 3、创建一个路由的对象
const router = createRouter({
    // 指定模式
    /**
     * createWebHashHistory 带 # 号
     * createWebHistory 不带 # 号
     */
    history: createWebHashHistory(),
    // 下面这个 可以写成ES6的简写 routers
    routes:routes
})

export default router
```

## 3、路由懒加载

```javascript
    {
        path: '/路由地址',
        name: '路由记录独一无二的名称',
        component: () => import("文件路径/文件名.vue")
    },
```

![image-20220102214822798](https://gitee.com/Green_chicken/picture/raw/master/20220102214824.png)

## 4、引入main.js

```javascript
import { createApp } from 'vue'
import App from './App.vue'
import router from './router/index'

const app = createApp(App);

app.use(router);
app.mount('#app');
```

## 5、设置路由出口文件(一般在App.vue里)

```javascript
<template>
	<router-view />
</template>
```

## 6、路由属性

name属性：路由记录独一无二的名称

meta属性：自定义的数据

## 7、在setup中获取路由

1）useRoute返回当前理由里面显示的对象

2）useRouter 返回的是全局的路由对象

# 三、router-link页面跳转

## 1、router-link跳转的写法

```javascript
<template>
    <div id="app">
        <router-link to="/home">Home</router-link>
        <router-link to="/router">Router</router-link>
        <!-- 出口 -->
        <router-view />
    </div>
</template>
```

## 2、router-link属性

1）to属性： 

是一个字符串，或者是一个对象  

2）replace属性： 

设置 replace 属性的话，当点击时，会调用 router.replace()，而不是 router.push()， router.push()有返回，另一个没

3）active-class属性：

设置激活a元素后应用的class，默认是router-link-active

4）exact-active-class属性： 

链接精准激活时，应用于渲染的  的 class，默认是router-link-exact-active

## 3、设置默认路由

```javascript
import { createRouter, createWebHistory, createWebHashHistory } from 'vue-router';
// 1、引入组件
import Home from "../views/Home.vue"
import Router4 from "../views/Router4的使用/Index.vue"
/**
 * 2、配置路由映射关系
 */
const routes = [
	{
        path: '/',
        component: Home
    },
    {
        path: '/home',
        component: Home
    },
    {
        path: "/router",
        component: Router4
    }
]
// 3、创建一个路由的对象
const router = createRouter({
    // 指定模式
    /**
     * createWebHashHistory 带 # 号
     * createWebHistory 不带 # 号
     */
    history: createWebHashHistory(),
    // 下面这个 可以写成ES6的简写 routers
    routes:routes
})

export default router
```

## 4、重定向

```javascript
import { createRouter, createWebHistory, createWebHashHistory } from 'vue-router';
// 1、引入组件
import Home from "../views/Home.vue"
import Router4 from "../views/Router4的使用/Index.vue"
/**
 * 2、配置路由映射关系
 */
const routes = [
	{
        path: '/',
        redirect:"/home"
    },
    {
        path: '/home',
        component: Home
    },
    {
        path: "/router",
        component: Router4
    }
]
// 3、创建一个路由的对象
const router = createRouter({
    // 指定模式
    /**
     * createWebHashHistory 带 # 号
     * createWebHistory 不带 # 号
     */
    history: createWebHashHistory(),
    // 下面这个 可以写成ES6的简写 routers
    routes:routes
})

export default router
```

# 四、动态路由基本匹配

## 1、路由里面的写法

![image-20220103213629261](https://gitee.com/Green_chicken/picture/raw/master/20220103213633.png)

## 2、路由跳转处的写法

![image-20220103213808147](https://gitee.com/Green_chicken/picture/raw/master/20220103213809.png)

## 3、在组件里面获取![image-20220103214755757](https://gitee.com/Green_chicken/picture/raw/master/20220103214756.png)

## 4、一次获取多个值

![image-20220103215302553](https://gitee.com/Green_chicken/picture/raw/master/20220103215304.png)

# 五、实现一个404找不到路径

注：提供一个404图片

 <img src="F:\vue3-ts-practise\src\assets\404.gif" alt="404" style="zoom:25%;" />

## 1、匹配找不到的路径

```javascript
    {
        // 这个使用正则，在找不到的时候，会走 *
        path: "/:pathMatch(.*)",
        name: "404",
        component: () => import("../views/404.vue")
    }
    /*
    // 第二种写法
    {
        // 这个使用正则，在找不到的时候，会走 *
        path: "/:catchAll(.*)",
        name: "404",
        component: () => import("../views/404.vue")
    }
    */
```

## 2、获取URL中任意匹配的内容

```javascript
<template>
    <div class="not-found">
        <img src="@/assets/404.gif" alt="">
        <!-- 获取输入的内容 -->    
        <p>{{$route.params.pathMatch}}</p>
    </div>
</template>
<style scoped>
.not-found {
    width: 100%;
    height: 100%;
    overflow: hidden;
}
.not-found img {
    width: 100%;
    height: 100%;
}
</style>
```

## 4、加星和不加的区别

![image-20220103221034303](https://gitee.com/Green_chicken/picture/raw/master/20220103221036.png)

# 六、二级路由

![image-20220103222818068](https://gitee.com/Green_chicken/picture/raw/master/20220103222819.png)

# 七、JS方式路由跳转

## 1、直接跳转

```javascript
<template>
    <div>
        <p>router的使用</p> 
        <router-link to="/router/one">一</router-link>
        <router-link to="/router/two">二</router-link>
        <button @click="fun">跳转</button>
        <!-- 展示二级路由 -->
        <router-view></router-view>
    </div>
</template>
<script>
import { useRouter } from "vue-router"
export default {
    setup(){
        const router = useRouter();
        function fun(){
            // 这个是路径
            router.push("/router/two")
        }
        return { fun }
    }
}
</script>
```

## 2、对象的写法

```javascript
<template>
    <div>
        <p>router的使用</p> 
        <router-link to="/router/one">一</router-link>
        <router-link to="/router/two">二</router-link>
        <button @click="fun">跳转</button>
        <!-- 展示二级路由 -->
        <router-view></router-view>
    </div>
</template>
<script>
import { useRouter } from "vue-router"
export default {
    setup(){
        const router = useRouter();
        function fun(){
            router.push({
                // 这个是路径
                path:"/router/two"
            })
        }
        return { fun }
    }
}
</script>
<style scoped>
/* 默认 router-link 选中时的class名 */
.router-link-active{
    color: red;
    margin: 0 20px;
}
</style>
```

## 3、query传参

1）传参界面

```javascript
<template>
    <div>
        <p>router的使用</p> 
        <router-link to="/router/one">一</router-link>
        <router-link to="/router/two">二</router-link>
        <button @click="fun">跳转</button>
        <!-- 展示二级路由 -->
        <router-view></router-view>
    </div>
</template>
<script>
import { useRouter } from "vue-router"
export default {
    setup(){
        const router = useRouter();
        function fun(){
            router.push({
                path:"/router/two",
                query:{
                    age:22
                }
            })
        }
        return { fun }
    }
}
</script>
<style scoped>
/* 默认 router-link 选中时的class名 */
.router-link-active{
    color: red;
    margin: 0 20px;
}
</style>
```

2）接收界面

```javascript
<template>
    <P>router 的二级路由2 </P>
</template>

<script>
import { useRoute } from "vue-router"
export default {
    name: 'Two',
    components:{
        
    },
    setup() {
        const route = useRoute();
        console.log(route.query.age);
    }
}
</script>
```



