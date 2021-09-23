# 一、bind的使用

1、自己实现bind方法

```javascript
function fun() {
    console.log(this);
}

var obj = {
    name: "why"
}

function bind(func, obj) {
    return function () {
        // arguments传递过来的 所有参数
        console.log(arguments);
        return func.apply(obj, arguments);
    }
}

var bar = bind(fun, obj);

bar(); // obj对象
```

2、使用原生bind的方法

```javascript
function fun() {
  console.log(this);
}

var obj = {
  name: "why"
}

var bar = fun.bind(obj);

bar(); // obj对象
```

