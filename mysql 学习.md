# 一、下载

[mysql](https://downloads.mysql.com/archives/installer/)

[navicat](https://drive.google.com/file/d/1n1q_CI_vQ0aLJCQs3sDWh3TXgz5pAfXi/view?usp=drive_link)

# 二、navicat 链接 mysql

## 1、navicat 报 2059的解决

![image-20231004154653759](https://not-have.github.io/file/images/image-20231004154653759.png)

### 1）启动 mysql 

```bash
# net start 查看所有正在运行的服务
# net start MySQL80 中 MySQL80 为 mysql 在我电脑中的名称
net start MySQL80
```

启动成功：

 ![image-20231004155703114](https://not-have.github.io/file/images/image-20231004155703114.png)

### 2）执行指令

```bash
mysql -hlocalhost -uroot -p
```

![image-20231006211754398](https://not-have.github.io/file/images/image-20231006211754398.png)

```bash
alter user 'root'@'localhost' identified by 'root' password expire never;
```

![image-20231006212256472](https://not-have.github.io/file/images/image-20231006212256472.png)

```bash
alter user 'root'@'localhost' identified with mysql_native_password by 'root';
```

![image-20231006212315263](https://not-have.github.io/file/images/image-20231006212315263.png)

![image-20231006212431164](https://not-have.github.io/file/images/image-20231006212431164.png)

进行以上的操作，你就链接成功了。

## 2、mysql 不是内部或外部命令

mysql -hlocalhost -uroot -p 报 'mysql' 不是内部或外部命令，也不是可运行的程序 或批处理文件

![image-20231006211644370](https://not-have.github.io/file/images/image-20231006211644370.png)

```
C:\Program Files\MySQL\MySQL Server 8.0\bin
```

注：默认安装地址

# 三、mysql

## 1、创建表

```mysql
create table t_book(
	id int,
	-- mysql 中没有 String 所以使用 varchar(这个里面定义字符的长度)
	name varchar(30),
	author varchar(10),
	price double
);
```

![image-20231006215222347](https://not-have.github.io/file/images/image-20231006215222347.png)

## 2、增删改查

![image-20231006220601208](https://not-have.github.io/file/images/image-20231006220601208.png)

# 四、java 链接数据库

## 1、下载 JBDC 驱动

地址：https://dev.mysql.com/downloads/connector/j/?os=26

![image-20231119220940508](https://not-have.github.io/file/images/image-20231119220940508.png)

## 2、插入数据

```java
package com.mysql.test01;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class Test01 {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        // 1、加载驱动
        Class.forName("com.mysql.cj.jdbc.Driver");
        /**
         * 2、获取链接
         * riverManager.getConnection 要三个参数
         */
        Connection conn = DriverManager.getConnection("jdbc:mysql://本机IP（127.0.0.1）:端口号（3306）/数据库名称?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai&allowPublicKeyRetrieval=true", "root", "root");
        // 3、创建会话
        Statement sta = conn.createStatement();
        // 4、发送 sql （也就是这里写 sql 语句）
        long i = sta.executeLargeUpdate("insert into t_book(id, name, author, price) values (1, '测试一', 'test', 22)");
        // 5、处理结果

        if (i > 0) {
            System.out.println("插入成gong");
        } else {
            System.out.println("插入 error");
        }
        // 6、关闭资源
        sta.close();
        conn.close();
    }
}
```

## 3、查询

```java
package com.mysql.test01;

import java.sql.*;

public class Test02 {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        // 1、加载驱动
        Class.forName("com.mysql.cj.jdbc.Driver");
        /**
         * 2、获取链接
         * riverManager.getConnection 要三个参数
         */
        Connection conn = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/test1?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai&allowPublicKeyRetrieval=true", "root", "root");
        // 3、创建会话
        Statement sta = conn.createStatement();
        // 4、发送 sql （也就是这里写 sql 语句）
        // ResultSet 理解为结果集合
        ResultSet resultSet = sta.executeQuery("select * from t_book");
        // 5、判断是否有记录
        while (resultSet.next()) {
            System.out.println(resultSet.getString("name"));
        }
        // 6、关闭资源
        sta.close();
        conn.close();
    }
}
```

