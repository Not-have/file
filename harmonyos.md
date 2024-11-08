[doc](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/application-dev-guide-V5)

创建一个项目

![image-20241029234909142](https://not-have.github.io/file/images/image-20241029234909142.png)

# 一、基础结构认识

注：每一个页面，也就是一个组件。

```ets
@Entry // 标注当前模块入口
@Component // 组件，可以省略，但是组件的时候，可以写着
struct Style { // struct 默认是组件，所以 @Component 可以省略
  build() { // 必须有这个，因为他是用来构建组件的
    Text('测试文本').fontSize(20).fontColor('red').width('100%')
  }
}
```

# 二、元素

## 1、进度条

```ets
@Entry
@Component
struct Style {
  build() {
    Column() {
      // 进度条
      Progress({ value: 60 }).margin(20)
      Progress({ value: 50 }).margin(20)
      Progress({ value: 60, total: 300 }).margin(20).width(10).height(60)

      Progress({value: 30, type: ProgressType.ScaleRing})

      Progress({value: 30, type: ProgressType.Eclipse})

      Progress({value: 20, type: ProgressType.Capsule})
    }
  }
}
```

 ![image-20240810164638151](https://not-have.github.io/file/images/image-20240810164638151.png)

## 2、Text

