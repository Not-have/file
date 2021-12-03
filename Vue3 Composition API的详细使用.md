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



# 二、setup的返回值

## 1、setup返回值的作用

1）setup的返回值可以在模板template中被使用

2）通过setup的返回值来替代data选项

3）返回值必须是一个对象

 ![image-20211129221912620](https://gitee.com/Green_chicken/picture/raw/master/20211129221913.png)

## 2、Reactive

  ![image-20211129222645476](https://gitee.com/Green_chicken/picture/raw/master/20211129222646.png)

<font color=red>注：</font>Reactive API必须传入的是一个对象或者数组类型。

