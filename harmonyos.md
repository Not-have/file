[doc](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/application-dev-guide-V5)

创建一个项目

![image-20241029234909142](https://not-have.github.io/file/images/image-20241029234909142.png)

注：

① ArkTS 采用声明式开发规范。

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

# 二、UI

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

## 2、Button

```ets
@Entry
@Component
struct Index {
  @State message: string = 'Hello !';

  build() {
    Column() {
      Row() {
        Button(this.message,  { type: ButtonType.Normal, stateEffect: true }).borderRadius({topLeft: 10, bottomRight: 10})
        Button(this.message,  { type: ButtonType.Normal, stateEffect: false }).borderRadius(4)
        Button(this.message,  { type: ButtonType.Circle, stateEffect: true })
        Button(this.message,  { type: ButtonType.Capsule, stateEffect: true }).onClick(() => {
          console.log("点击")
        })
      }
    }
  }
}
```

![image-20241103002905181](https://not-have.github.io/file/images/image-20241103002905181.png)

## 3、Image 的使用

```eetc
@Entry
@Component
struct Index {
  @State message: string = 'Hello !';

  build() {
      Button(){
        Row() {
          Text(this.message)
          Image($rawfile('API.svg')).height(100).width(100)
        }
      }.width(150).height(150)
  }
}
```

![image-20241103004339650](https://not-have.github.io/file/images/image-20241103004339650.png)

## 4、相对布局

```etc
@Entry
@Component
struct Index {
  build() {
    RelativeContainer() {
      Text('子元素 01')
        .id('box-01')
        .backgroundColor(Color.Orange)
        .width(100)
        .height(100)
        .margin({ left: 50, top: 100 })
      Text('子元素 02')
        .backgroundColor(Color.Pink)
        .width(100)
        .height(100)
        .alignRules({
          left: { anchor: 'box-01', align: HorizontalAlign.Center },
          top: { anchor: 'box-01', align: VerticalAlign.Bottom }
        })
    }
  }
}
```

![image-20241119225313802](./images/image-20241119225313802.png)

## 5、栅格布局

[docs](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/arkts-layout-development-grid-layout-V5)

```etc
@Entry
@Component
struct Index {
  build() {
    GridRow() {
      GridCol({span: 11, offset: 1}) {
        Text('1').borderWidth(1).width('100%')
      }
      GridCol() {
        Text('2').borderWidth(1).width('100%')
      }
      GridCol() {
        Text('3').borderWidth(1).width('100%')
      }
      GridCol() {
        Text('4').borderWidth(1).width('100%')
      }
      GridCol() {
        Text('5').borderWidth(1).width('100%')
      }
      GridCol() {
        Text('6').borderWidth(1).width('100%')
      }
      GridCol() {
        Text('7').borderWidth(1).width('100%')
      }
      GridCol() {
        Text('8').borderWidth(1).width('100%')
      }
      GridCol() {
        Text('9').borderWidth(1).width('100%')
      }
      GridCol() {
        Text('10').borderWidth(1).width('100%')
      }
      GridCol() {
        Text('11').borderWidth(1).width('100%')
      }
      GridCol() {
        Text('12').borderWidth(1).width('100%')
      }
      GridCol() {
        Text('13').borderWidth(1).width('100%')
      }
      // 超出了，就会另起一个十二行
      GridCol() {
        Text('14').borderWidth(1).width('100%')
      }
    }
  }
}
```

![image-20241119233946294](./images/image-20241119233946294.png)

## 6、媒体查询

```ets
import { mediaquery } from '@kit.ArkUI';

@Entry
@Component
struct Index {
  @State col01Width: string = '20%';
  @State col02Width: string = '60%';
  @State col03Width: string = '20%';
  // 帮我们去监听屏幕
  smListener = mediaquery.matchMediaSync('screen and (width>=320vp) and (width<=600vp)')
  mdListener = mediaquery.matchMediaSync('screen and (width>=600vp) and (width<=840vp)')
  lgListener = mediaquery.matchMediaSync('screen and (width>=840vp)')

  // 组件的生命周期方法：组件即将显示 aboutToAppear
  aboutToAppear(): void {
    console.log("组件即将显示")
    this.smListener.on('change', (result) => {
      if (result.matches) {
        console.log('当匹配到 sm 屏')
      }
      this.col01Width = '100%';
      this.col02Width = '100%';
      this.col03Width = '100%';
    })

    this.mdListener.on('change', (result) => {
      if (result.matches) {
        console.log('当匹配到 md 屏')
        this.col01Width = '10%';
        this.col02Width = '80%';
        this.col03Width = '10%';
      }
    })

    this.lgListener.on('change', (result) => {
      if (result.matches) {
        console.log('当匹配到 lg 屏')
        this.col01Width = '20%';
        this.col02Width = '60%';
        this.col03Width = '20%';
      }
    })
  }

  build() {
    Flex({wrap: FlexWrap.Wrap}) {
      Column() {
        Text('左侧文本内容').backgroundColor(Color.Yellow).width(this.col01Width)
      }

      Column() {
        Text('中间文本内容').backgroundColor(Color.Gray).width(this.col02Width)
      }

      Column() {
        Text('右侧文本内容').backgroundColor(Color.Orange).width(this.col03Width)
      }
    }.width('100%')
  }
}
```

![image-20241120002005972](./images/image-20241120002005972.png)
