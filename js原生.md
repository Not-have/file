## 1、判断json对象中 是否包含某个键

```javascript
let obj = { a: 1, b: 2, c: 3 };
// 判断这个 Object里面是否有这个 字段
console.log(Object.prototype.hasOwnProperty.call(obj, 'a'));//true
```

## 2、事件防抖

<font color=Turquoise1>注：</font> 

①本来事件的触发比较频繁,但是,我们只希望这无数次的事件触发中,有部分事件是有效的（如：用户有短暂的停止时才调用函数）。特别是在触发一次，就发一次请求，会有无数次的抖动。

如: 键盘事件：onkeydown,onkeyup,onkeypress,oninput，都是按一次键,触发一次.触发非常频繁。

②使用场景:

搜索框(百度搜索框，淘宝，京东等等),每次用户输入内容都需要发一次请求,从后端拿到关键字对应的内容.

特别是输入汉字时:

我们希望用户输入完成汉字时再触发(发送请求)，

但是,实际情况是:当用户输入一个字母时,就会触发一次事件(发送一次请求)

```javascript
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title></title>
    <style type="text/css">
        * {
            margin: 0;
            padding: 0;
        }
        ul {
            list-style: none;
        }
    </style>
</head>
<body>
    <input type="text" id="querystr">
    <input type="submit" value="百度一下">
    <ul id="search-list">
        <li>啊啊啊</li>
        <li>哈哈哈</li>
        <li>呵呵呵</li>
    </ul>
</body>
</html>
<script type="text/javascript">
    let myTimer = null;

    $("querystr").oninput = function () {
        //在每次输入时，先清除上一次的定时器，清除了定时器后，就不会发送请求了。
        //即本次输入和上次输入的间隔非常短（小于200ms）时，上次定时器的代码就不会执行，也就不会发请求了。
        //如果本次输入和上次输入之间的时间间隔大于100ms时，上次输入的内容就会发送请求。
        if (myTimer != null) {
            window.clearTimeout(myTimer);
        }
        myTimer = setTimeout(() => {
            let scriptDom = document.createElement("script");
            scriptDom.src = 'https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su?wd=' + this.value + '&json=1&p=3&sid=1438_24869_21080_18560_17001_25177_22160&req=2&bs=1%2B&pbs=1%2B&csor=2&pwd=1%3D&cb=f&_=1511334117083';
            document.body.appendChild(scriptDom);
            scriptDom.remove();
        }, 200);
    }

    function f(data) {
        console.log(data);
        let htmlStr = "";
        data.s.forEach((item) => {
            htmlStr += `<li>${item}</li>`;
        });
        $("search-list").innerHTML = htmlStr;
    }

    function $(id) {
        return document.getElementById(id);
    }
</script>
```

![image-20210224114334911](https://gitee.com/Green_chicken/picture/raw/master/img/20210224114337.png)

## 3、封装$符获取元素

```javascript
/**
 * 元素尺寸属性	说明
 * clientWidth	获取元素可视部分的宽度，即 CSS 的 width 和 padding 属性值之和，元素边框和滚动条不包括在内，也不包含任何可能的滚动区域
 * clientHeight	获取元素可视部分的高度，即 CSS 的 height 和 padding 属性值之和，元素边框和滚动条不包括在内，也不包含任何可能的滚动区域
 * offsetWidth	元素在页面中占据的宽度总和，包括 width、padding、border 以及滚动条的宽度
 * offsetHeight	元素在页面中占据的高度总和，包括 height、padding、border 以及滚动条的宽度
 * scrollWidth	当元素设置了 overflow:visible 样式属性时，元素的总宽度，也称滚动宽度。在默认状态下，如果该属性值大于 clientWidth 属性值，则元素会显示滚动条，以便能够翻阅被隐藏的区域
 * scrollHeight	当元素设置了 overflow:visible 样式属性时，元素的总高度，也称滚动高度。在默认状态下，如果该属性值大于 clientWidth 属性值，则元素会显示滚动条，以便能够翻阅被隐藏的区域
 */
/**
 * @description: 根据选择器获取dom
 * @param {*} str 选择器（必须加选择器类型，如id加 #XXX）
 * @return {*} 返回dom元素
 */
export function $(str) {
    if (str.charAt(0) == "#") {
        return document.getElementById(str.substring(1, str.length));
    } else if (str.charAt(0) == ".") {
        return document.getElementsByClassName(str.substring(1, str.length))[0];
    } else {
        return document.getElementsByTagName(str)[0];
    }
}

/*
 *                        _oo0oo_
 *                       o8888888o
 *                       88" . "88
 *                       (| -_- |)
 *                       0\  =  /0
 *                     ___/`---'\___
 *                   .' \\|     |// '.
 *                  / \\|||  :  |||// \
 *                 / _||||| -:- |||||- \
 *                |   | \\\  - /// |   |
 *                | \_|  ''\---/''  |_/ |
 *                \  .-\__  '-'  ___/-. /
 *              ___'. .'  /--.--\  `. .'___
 *           ."" '<  `.___\_<|>_/___.' >' "".
 *          | | :  `- \`.;`\ _ /`;.`/ - ` : | |
 *          \  \ `_.   \_ __\ /__ _/   .-` /  /
 *      =====`-.____`.___ \_____/___.-`___.-'=====
 *                        `=---='
 * 
 *            佛祖保佑       永不宕机     永无BUG
 */
```

## 4、正则获取文件名后缀

```javascript
let url = "XXXXXXXX/1-switch.svg";
console.log(url.replace(/(.*\/)*([^.]+).*/ig,"$2"));//1-switch
```





