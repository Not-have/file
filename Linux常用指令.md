## 1、关机

```bash
halt             ### 停止机器
halt -p          ### 关闭机器
halt --reboot    ### 重启机器
```

## 2、压缩文件

```bash
tar -zcvf dist.zip /dist
tar zxf xxx.tar.gz  ==>解压xxx.tar.gz文件
tar zcf xxx.tar.gz xxx ==>压缩xxx文件为xxx.tar.gz
```

## 3、切换用户

```bash
# 切换
su root
# 修改root用户密码
sudo passwd root
# 切换用户
sudo su
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
# 修改文件名
mv 原先的名字 要修改的名字
```

## 6、查看当前目录

```bash
pwd
```

## 7、nginx

### 1）默认配置

- 页面展示位置

```bash
/usr/share/nginx/html
```

- 配置文件位置

```bash
/etc/nginx
```

## 8、常用集合

以下是一份**Linux常用指令及核心知识**的快速参考指南，适合日常操作和系统管理：

### 1）文件/目录操作

| **命令** | **作用**              | **常用参数**                               |
| -------- | --------------------- | ------------------------------------------ |
| `ls`     | 列出目录内容          | `-l`(详情), `-a`(包括隐藏), `-h`(可读大小) |
| `cd`     | 切换目录              | `cd ~`(主目录), `cd -`(返回)               |
| `pwd`    | 显示当前路径          |                                            |
| `mkdir`  | 创建目录              | `-p`(级联创建, e.g. `mkdir -p a/b/c`)      |
| `cp`     | 复制文件              | `-r`(复制目录), `-i`(覆盖提示)             |
| `mv`     | 移动/重命名           | `mv file1 file2` (重命名)                  |
| `rm`     | 删除文件              | `-r`(目录), `-f`(强制删除) ⚠️慎用!          |
| `touch`  | 创建空文件/更新时间戳 |                                            |

### 2）文件查看与编辑

| **命令** | **说明**                            |
| -------- | ----------------------------------- |
| `cat`    | 快速查看小文件                      |
| `less`   | 分页查看大文件 (支持搜索 `/关键词`) |
| `head`   | 查看开头几行                        |
| `tail`   | 查看末尾几行                        |
| `nano`   | 简单文本编辑器                      |
| `vim`    | 高效编辑器 (需要学习)               |

### 3）权限管理

```
chmod u+x file.sh    # 给用户添加执行权限
chmod 755 script.sh  # rwxr-xr-x (用户:rwx, 组:r-x, 其他:r-x)
chown user:group file # 修改文件所有者/组
```

- **权限符号**：`r`(读-4), `w`(写-2), `x`(执行-1)
- 示例：`-rwxr-xr--` = 用户可读写执行，组可读执行，其他仅读

### 4）系统监控

| **命令**  | **功能**                      |
| --------- | ----------------------------- |
| `top`     | 实时进程监控 (类似任务管理器) |
| `htop`    | 增强版 `top` (需安装)         |
| `df -h`   | 查看磁盘空间                  |
| `free -h` | 查看内存使用                  |
| `ps aux`  | 列出所有进程                  |

### 5）网络操作

```
ping baidu.com          # 测试网络连通性
ifconfig / ip addr      # 查看IP地址 (新系统用 `ip`)
netstat -tuln           # 查看端口监听状态
wget [URL]              # 下载文件
curl -O [URL]           # 下载/测试API (e.g. `curl ifconfig.me` 查公网IP)
```

### 6）包管理（示例）

| **系统**      | **命令**                           |
| ------------- | ---------------------------------- |
| Debian/Ubuntu | `apt update` → `apt install nginx` |
| CentOS/RHEL   | `yum install httpd`                |
| Arch/Manjaro  | `pacman -S firefox`                |

### 7）神器工具

| **命令** | **场景** |
| -------- | -------- |
| `grep`   | 文本搜索 |
| `find`   | 文件查找 |
| `awk`    | 文本处理 |
| `sed`    | 流编辑器 |
| `tar`    | 压缩解压 |

### 8）核心知识

1. **根目录结构**：

   - `/bin` 基础命令
   - `/etc` 配置文件
   - `/home` 用户目录
   - `/var/log` 系统日志
   - `/root` root用户目录

2. **管道与重定向**：

   ```
   cat file.txt | sort > sorted.txt   # 排序后保存
   command 2> error.log               # 错误输出到文件
   ```

3. **环境变量**：

   ```
   echo $PATH         # 查看PATH变量
   export PATH=$PATH:/new/path   # 临时添加路径
   ```

4. **服务管理** (Systemd)：

   ```
   systemctl start nginx    # 启动服务
   systemctl enable nginx   # 开机自启
   ```

### 9）新手必知

- **慎用** `rm -rf /` 或 `rm -rf *` (可能删光一切！)

- 多用 `Tab` 自动补全文件名/命令

- 忘记命令时：

  ```
  man ls          # 查看手册
  ls --help       # 快速帮助
  ```

  

