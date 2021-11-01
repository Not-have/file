## 1、文字显示省略号

1）文字两行显示，超出部分显示省略号

```css
容器{
    width:200px;
    /* 高度要是两行文字的高 */
    height: 40px;
    border:1px solid red;
    overflow: hidden;
    -webkit-line-clamp:2;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-box-orient: vertical;
}
```

2）单行显示，超出显示省略号

```html
容器{
    /* 要给容器的宽 */
    width:200px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}
```

