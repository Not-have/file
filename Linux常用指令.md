## 1、关机

```bash
halt             ### 停止机器
halt -p          ### 关闭机器
halt --reboot    ### 重启机器
```

## 2、压缩文件

```bash
tar -zcvf dist.zip /dist
```

## 3、切换用户

```bash
# 切换
su root
# 修改root用户密码
sudo passwd root

```

## 4、拷贝文件到指定路径下

```bash
cp -r 文件夹 指定的路径
```

## 5、修改文件

```bash
# 进入文件
vi 文件名.文件类型后缀
# 退出并保存
:wq
# 退出不保存
:q!
# 创建文件夹
mkdir 文件名

```

