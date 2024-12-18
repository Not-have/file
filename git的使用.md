# 一、创建一个空白分支

## 1、创建新分支

```bash
git branch 分支名
```

## 2、切换到新分支

```bash
git checkout 分支名
```

注：查看分支下文件：dir

## 3、删除原来的文件

```bash
git rm -rf .
```

## 4、提交到本地

```bash
git commit -m "提交的信息"
```

## 5、推到仓库

```bash
git push origin 分支名
```

# 二、github提送失败



## 1、进入根目录下的 .git

## 2、打开config

## 3、在里面加入以下参数

```bash
[http]
	postBuffer = 5244288800
```

# 三、修改本地和远程分支名

## 1、修改本地

```bash
git branch -m 原分支名 修改之后的分支名 
```

## 2、删除远程分支（远端无此分支 则跳过该步骤）

```bash
git push --delete origin 要删除的远端分支名
```

## 3、将重命名后的分支推到远端

```bash
git push origin 修改之后的分支名
```

## 4、把修改后的本地分支与远程分支关联

```bash
git branch --set-upstream-to origin/修改之后的分支名
```

注：之后就可以提交代码了。

# 四、github密钥生成

## 1、生成

```bash
ssh-keygen -t ed25519 -C "邮箱"
```

把生成的 <font color=red>.pub</font>> 文件放入github中

![image](https://user-images.githubusercontent.com/57132948/186974136-58323630-62b0-4dec-8c0c-aab8b596b452.png)

## 2、让信任本机

```bash
 # 一直回车
 ssh -T git@github.com
```

![image](https://user-images.githubusercontent.com/57132948/186974281-a384149b-5971-4b66-a4a2-f03737de76a6.png)

# 五、常用指令

## 1、查看本地分支

```bash
git branch
```

## 2、删除本地分支

```bash
git branch -D 分支名

# 删除当前分支外的所有分支
git branch | xargs git branch -d
```

## 3、查看远程分支

```bash
git branch -a
```

## 4、删除远程分支

```bash
git push origin --delete 分支名
```

## 5、清理远程仓库在本地的分支记录

```bash
git remote prune origin
```

## 6、更新本地分支

```bash
git remote update origin -p
```

# 六、修改分支名

## 1、先把远端的分支拉到本地

## 2、修改拉到本地的分支名字

```bash
git branch -m 旧分支名 新起的分支名
```

## 3、上传新改的分支名到远端

```bash
git push origin 新起的分支名
```

## 4、关联本地修改名字之后分支与远端的新分支

```bash
git branch --set-upstream-to origin/新起的分支名
```

显示一下内容代表关联成功：

![image-20211012111733501](https://gitee.com/Green_chicken/picture/raw/master/markdown/image-20211012111733501.png)

## 5、删除远端的分支

```bash
git push --delete origin 旧分支名
```

显示下面的代表远端分支 删除成功：

 ![image-20211012112039763](https://gitee.com/Green_chicken/picture/raw/master/markdown/image-20211012112039763.png)

注：以上的操作其实 就是新建一个分支并拷贝旧的分支内容到新的分支，然后 删除旧的分支。

# 七、Git GUI Here汉化

https://blog.csdn.net/sinat_26811377/article/details/106226034

# 八、github关联本地

## 1、拉去代码

```bash
git clone 仓库地址
```

## 2、每次提交前，先获取最新代码

```bash
git pull --rebase origin main（分支名）
```

# 九、git程序使用失败的解决

https://www.cnblogs.com/tianhengblogs/p/9142691.html

https://zhidao.baidu.com/question/322727885.html

https://blog.csdn.net/TionSu/article/details/99629507

# 十、使用vsc合并代码时是的选项

Accept Current Change		接受当前更改
Accept Incoming Change	 接受传入的更改
Accept Both Change		     接受两种改变
Compare Change			      比较变化


# 十一、删除 package-lock.json

```bash
git rm --cached package-lock.json


```

