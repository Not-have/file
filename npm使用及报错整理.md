# 一、npm源切换

## 1.得到原本的镜像地址

```bash
npm get registry
```

未修改时得到下面的：

https://registry.npmjs.org

## 2、设成淘宝的源

```bash
npm config set registry http://registry.npm.taobao.org/
```

## 3、换成初始源

```bash
npm config set registry https://registry.npmjs.org/
```

