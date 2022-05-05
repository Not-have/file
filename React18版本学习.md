# 一、React-Router-v6的学习

文档：https://reactrouter.com/

## 1、下载

```bash
npm install react-router-dom
```

## 2、在App.js中引入界面和React-Router的方法

```javascript
import { BrowserRouter, Routes, Route, Link } from 'react-router-dom'
import Home from './pages/home/Index.jsx'
import Mine from './pages/mine/Mine.jsx'
function App() {
	return (
		<div className="App">
			<BrowserRouter>
				<div className="menu">
					<Link to="/home">about</Link>
					<Link to="/mine">mine</Link>
				</div>
				<Routes>
					<Route path="/home" element={<Home />} />
					<Route path="/mine" element={<Mine />} />
					<Route path="/" element={<Home />} />
				</Routes>
			</BrowserRouter>
		</div>
	)
}

export default App
```

注：① BrowserRouter 声明一个当前要用非hash模式的路由；

​        ② Link 指定路由跳转组件，to用来配置路由地址；

​        ③ Routes路由出口，路由对应的组件，会在这里 进行渲染；

​        ④ Route 指定路径和组件的对应关系，有几个理由，就写几个，path代表路径，element代表组件；

## 3、路由的核心组件

### 1）路由的两种模式（HashRouter、BrowserRouter）

![image-20220411234853146](https://cdn.jsdelivr.net/gh/Not-have/picture/202204112349384.png)

① HashRouter（哈希模式） 带 # 号

② BrowserRouter 不带 # 号（使用了H5的history.pushState API来实现）

### 2）Link的使用

作用 ：用于指定导航链接，完成路由跳转

语法说明： 组件通过to属性指定路由地址，最终会渲染为a链接元素

 ![image-20220419185102653](https://cdn.jsdelivr.net/gh/Not-have/picture/202204191851428.png)

### 3）Routes

提供一个路由出口，满足条件的路由组件会渲染到组件内部，定义path和组件的对应关系

 ![image-202204191852013](https://cdn.jsdelivr.net/gh/Not-have/picture/202204191852013.png)

### 4）Route

作用 ：用于指定导航链接，完成路由匹配（也就是定义url）

语法说明： path属性指定匹配的路径地址，element属性指定要渲染的组件

![image-20220419190009913](https://cdn.jsdelivr.net/gh/Not-have/picture/202204191900358.png)

## 4、编程式导航

### 1）在App.js中引入组件并定义url

![image-20220419221921461](https://cdn.jsdelivr.net/gh/Not-have/picture/202204192219245.png)

### 2）在组件 或 页面中使用

```javascript
// 1、引入
import { useNavigate } from "react-router-dom"
export default function Home(){
	// 2、执行
	const navigation = useNavigate()
	const goJump = () => {
		navigation("/mine")
	}
	return <div>
		首页
		<div>
			{/* 3、跳转的方式 */}
			<button onClick={() => navigation("/mine")}>点击跳转</button>
			<button onClick={goJump}>点击跳转</button>
		</div>
	</div>
}
```

 ![image-20220419222348370](https://cdn.jsdelivr.net/gh/Not-have/picture/202204192223088.png)

### 3）传参

#### ① searchParams传参

a、传参

```javascript
// 1、引入
import { useNavigate } from "react-router-dom"
export default function Home(){
	// 2、执行
	const navigation = useNavigate()
	/**
	 * navigation 调用时 可配置是否要记录历史
	 * replace 为true 时不记录
	 */
	const goJump = () => {
		navigation("/mine?id=11111&name=拉斯", { replace: true })
	}
	return <div>
		首页
		<div>
			{/* 3、跳转的方式 */}
			{/* <button onClick={() => navigation("/mine")}>点击跳转</button> */}
			<button onClick={goJump}>点击跳转</button>
		</div>
	</div>
}
```

b、取参

```javascript
import { useSearchParams } from "react-router-dom"
export default function Mine(){
	// 获取路由传参
	const [ params ] = useSearchParams();
	const id = params.get("id")
	const name = params.get("name")
	return (
		<div>
			<p>我的</p>
			<p>id为{ id }</p>
			<p>name为{ name }</p>
		</div>
	)
}
```

#### ② params传参

a、在路由url中加入参数键

![image-20220423211732929](https://cdn.jsdelivr.net/gh/Not-have/picture/202204232117567.png)

b、在A页面传参

```javascript
// 1、引入
import { useNavigate } from "react-router-dom"
export default function Home(){
	// 2、执行
	const navigation = useNavigate()
	/**
	 * navigation 调用时 可配置是否要记录历史
	 * replace 为true 时不记录
	 */
	const goJump = () => {
		navigation("/mine/11111", { replace: true })
	}
	return <div>
		首页
		<div>
			{/* 3、跳转的方式 */}
			<button onClick={goJump}>点击跳转</button>
		</div>
	</div>
}
```

c、在B页面取参

```javascript
import { useParams } from "react-router-dom"
export default function Mine(){
	// 获取路由传参
	const params = useParams()
	return (
		<div>
			<p>我的</p>
			<p>id为{ params.id }</p>
		</div>
	)
}
```

## 5、二级路由

### 1）在App.js中配置二级路由

```javascript
import { HashRouter, Routes, Route, Link } from 'react-router-dom'
import Home from './pages/home/Index.jsx'
import HomeOne from './pages/home-one/index.js'
import HomeTwo from './pages/home-two/index.js'
function App() {
	return (
		<div className="App">
			<HashRouter>
				<div className="menu">
					<Link to="/home">about</Link>
				</div>
				<Routes>
					{/* 谁的二级路由，就写在谁的里面 */}
					<Route path="/home" element={<Home />}>
						{/* 定义二级路由是  不加斜杠 */}
						<Route path="home-one" element={<HomeOne />} />
						<Route path="home-two" element={<HomeTwo />} />
					</Route>
					<Route path="/" element={<Home />} />
				</Routes>
			</HashRouter>
		</div>
	)
}
export default App
```

### 2）在一级路页面的写法

```javascript
// 1、引入
import { Outlet } from "react-router-dom"
export default function Home(){
	return <div>
		首页
		<div>
			<hr />
			{/* home下有二级路由，所以在这写二级路由的出口 */}
			<Outlet />
		</div>
	</div>
}
```

### 3）二级路由中的界面

```javascript
function HomeOne() {
	return <div>home的子界面1</div>
}
export default HomeOne
```

 ![image-20220423233734397](https://cdn.jsdelivr.net/gh/Not-have/picture/202204232337473.png)

### 4）默认选中的二级路由

![image-20220423233952333](https://cdn.jsdelivr.net/gh/Not-have/picture/202204232339128.png)

## 6、404页面配置

1）pages下新建一个404.js文件

```javascript
function NoFound() {
	return (
		<div>
			<h1>404</h1>
		</div>
	)
}
export default NoFound
```

2）在路由配置文件中的写法

![image-20220423234458883](https://cdn.jsdelivr.net/gh/Not-have/picture/202204232345670.png)

## 7、匹配当前选中项，要把Link替换成 NavLink

![image-20220424001054792](https://cdn.jsdelivr.net/gh/Not-have/picture/202204240010623.png)

# 二、hook的学习

注：hook只能在函数式组件中使用；

## 1、useState的使用

1）简单使用

① useState的参数式初始值

② useState返回的时一个数组，可以结构出来，但是结构类型时固定的

a、第一个元素是 值

b、第二个元素，是一个函数，是用来修改初始值的（基于原值计算，得到新值）

c、num、setNum是一对的 是绑定在一起的，setNum只能修改num，不能修改别的

```javascript
import { useState } from "react";
function App() {
    const [num, setNum] = useState(0)
    return (
        <div>
            <button onClick={() => setNum(num + 1)}>{num}</button>
        </div>
    )
}
export default App
```

2）useState下组件的更新过程

① 首次渲染，组件内部代码会执行，同时useState也会执行，这个时候 会进行初始值的赋值

② 调用第二个元素（setNum），整App都会重新执行

③ 第二次渲染之后，得到的num值，就是1了，模板会重新渲染，因为react会有一个数据状态记录

 ![image-20220427002349021](https://cdn.jsdelivr.net/gh/Not-have/picture/202204270023287.png)

3）使用规则

① useState 函数可以执行多次，每次执行互相独立，每调用一次为函数组件提供一个状态

② 不能嵌套在if/for/其它函数中（react按照hooks的调用顺序识别每一个hook）

```javascript
import { useState } from "react";
function App() {
    console.log("组件更新");
    /**
     * 不能按照斜面这样写（因为这个 和 react内部的运行机制有关）
     */
    if(true) {
        const [num, setNum] = useState(0)
    }
    return (
        <div>
            <button onClick={() => setNum(num + 1)}>{num}</button>
        </div>
    )
}
export default App
```

4）回调函数的参数

![image-20220505231210870](https://cdn.jsdelivr.net/gh/Not-have/picture/202205052312902.png)

## 2、useEffect

注：是为了处理函数组件的副作用产生的

​        函数中除了主作用的，都是副作用（理解成 一个函数只做一件事）

1）基本使用

```javascript
import { useState, useEffect } from 'react'
export default function App() {
	const [count, setCount] = useState(0)
	/**
	 * 1、做组件更新，副作用函数 会一直更新
	 * 2、useEffect里面第一个参数，是一个回调函数
	 */
	useEffect(() => {
		document.title = '清理副作用' + count
	})
	return (
		<div>
			<button onClick={() => setCount(count + 1)}>{count}</button>
		</div>
	)
}
```

注：常见的副作用：

​                                 ① 数据请求

​								 ② 手动修改dom

​								 ③ localstorage等数据存储

2）执行时机

① 默认无依赖项

会在组件初始化的时候 执行一次，数据更新，他会再次执行

② 第二个参数（添加为空数组）

会在组件渲染的时候，执行一次，但是 数据变化，就不再执行

③ 给第二个参数中加入执行依赖项

```javascript
import { useState, useEffect } from 'react'
export default function UseEffectPager() {
	const [count, setCount] = useState(0)
	/**
	 * 1、做组件更新，副作用函数 会一直更新
	 * 2、useEffect里面第一个参数，是一个回调函数
	 */
	useEffect(() => {
		document.title = '清理副作用' + count
		// count 改变 上面的就会执行
	}, [count])
	return (
		<div>
			<button onClick={() => setCount(count + 1)}>{count}</button>
		</div>
	)
}
```

注：在useEffect里面使用了，就应该出现在依赖项数组中

3）清理useEffect的副作用

给useEffect里面写一个定时器，当你销毁组件的时候，这个定时器 并没有销毁

```javascript
import { useState, useEffect } from 'react'
function Com() {
	useEffect(() => {
		let time = setInterval(() => {
			console.log('定时器')
		}, 1000)
		// 如果 不这样写，这个计时器 就算组件销毁了，他也会以一直执行
        // 这个函数 也就是做一个销毁操作
		return () => {
			clearInterval(time)
		}
	}, [])
	return <div>组件</div>
}

export default function UseEffectSideEffect() {
	const [show, setShow] = useState(true)
	return (
		<div>
			<button onClick={() => setShow(!show)}>组件显示隐藏</button>
			{show ? <Com /> : null}
		</div>
	)
}
```











案例

1）获取滚动条距离

```javascript
/**
 * 获取滚动条距离
 */
import { useState } from 'react'
export default function useWindowScroll() {
	const [x, setX] = useState(0)
	const [y, setY] = useState(0)

	window.addEventListener('scroll', () => {
		const top = document.documentElement.scrollTop
		setX(top)
		const left = document.documentElement.scrollLeft
		setY(left)
	})
	return [x, y]
}
```

2）同步修改本地数据

```javascript
import { useState, useEffect } from 'react'
/**
 * 设置localStorage
 * @param {*} key localStorage键
 * @param {*} defaultValue localStorage值
 * @returns
 */
export default function useLocalStorage(key, defaultValue) {
	const [message, setMessage] = useState(defaultValue)
	/**
	 * message或者key变化 都会存
	 */
	useEffect(() => {
		window.localStorage.setItem(key, message)
	}, [key, message])
	/**
	 * message 是值
	 * setMessage 在外面设置值
	 */
	return [message, setMessage]
}
```

![image-20220505225826737](https://cdn.jsdelivr.net/gh/Not-have/picture/202205052258417.png)

