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

# 五、部分指令（DDL）

注：登录当前数据库之后，才可以运行 mysql 指令

```bash
# 登录
mysql -uroot -proot
# mysql -u用户名 -p密码
```

## 1、查看当前数据库

``` mysql
show databases;
-- SQL 语句以 ; 结尾
-- 默认安装号是有四个库，这四个不要动
-- information_schema
-- mysql              
-- performance_schema 
-- sys
```

## 2、创建数据库

```mysql
create database test01;

-- Query OK, 1 row affected (0.01 sec)
-- 数据库名称一版不要修改
```

## 3、删除数据库

```mysql
drop database test01;

-- Query OK, 0 rows affected (0.01 sec)
```

## 4、使用数据库

```mysql
use 库名;
```

## 5、查询当前使用的数据库

```mysql
select database();
```

## 6、约束

可以限制表的插入、更新和删除等，例如：id、日期等；

主键不能重复。

### 1）设置主键 `primary key`

① 初始时设置主键

```mysql
create table test01(
	id int(11) primary key,
	-- mysql 中没有 String 所以使用 varchar(这个里面定义字符的长度)
	name varchar(30)
);

insert into test01 (id, name) values(2, "哈哈哈");
```

② 后面指定主键

```mysql
create table test02(
	id int(11),
	-- mysql 中没有 String 所以使用 varchar(这个里面定义字符的长度)
	name varchar(30),
	age int,
	primary key(id)  -- 指定主键
)

INSERT INTO test02(id, name, age) VALUES(2, '呵呵呵', 1);
```

③ 对已建成的表设置主键

```mysql
create table test03(
	id int(11),
	-- mysql 中没有 String 所以使用 varchar(这个里面定义字符的长度)
	name varchar(30),
	age int
)

INSERT INTO test03(id, name, age) VALUES(2, '呵呵呵', 1);

-- 修改表，设置主键,设置主键时，上面创建的一定要存在 id
alter table test03 add primary key (id);
```

④ 直接在 navicat  中设置

![image-20240526225055560](https://not-have.github.io/file/images/image-20240526225055560.png)

### 2）自增约束 `primary key auto_increment`

注：自增约束，主要是配合主键使用，防止主键为空、重复。

```mysql
create table test04(
	id int(11) primary key auto_increment,
	-- mysql 中没有 String 所以使用 varchar(这个里面定义字符的长度)
	name varchar(30)
)

insert into test04(name) values("哈哈哈");
-- 每次自增都是给 id + 1
insert into test04(name) values("啊啊啊");
insert into test04(name) values("呵呵呵");

-- 删除 不影响自增顺序
delete from test04 where id = 3;
insert into test04(name) values("啦啦啦啦");
```

### 3）唯一约束 `unique`

```mysql
create table test05(
	id int(11),
	-- mysql 中没有 String 所以使用 varchar(这个里面定义字符的长度)
	name varchar(30) unique
)

insert into test05(id, name) values(1, "哈哈哈");
-- 报错：1062 - Duplicate entry '哈哈哈' for key 'test05.name'
insert into test05(id, name) values(2, "哈哈哈");
```

### 4）非空约束 `not null`

```mysql
create table test06(
	id int(11),
	-- mysql 中没有 String 所以使用 varchar(这个里面定义字符的长度)
	name varchar(30) not null
)

-- 报错：1364 - Field 'name' doesn't have a default value
insert into test06(id) values(1);

-- 正确
insert into test06(id, name) values(1, "呵呵呵");
-- 只指定了 name 不为空，但是可以重复
insert into test06(id, name) values(1, "呵呵呵");
```

### 5）默认值 

```mysql
create table test07(
	id int(11),
	-- mysql 中没有 String 所以使用 varchar(这个里面定义字符的长度)
	sex char(1) default "女"
)

insert into test06(id) values(1);

insert into test06(id, sex) values(1, "男");
```

### 6）外键约束

注：多表之间的一种关联关系的一种限制。

```mysql
-- 商品
create table tb_goods(
	id int primary key,
	name varchar(20),
	description varchar(100)
)

-- 订单（订单表关联了商品）
create table tb_order(
	id int primary key,
	time datetime,
  goodId int,
	-- 设置外键：constraint 外键名 foreign key (当前表中的列名) references 表(主键) 
	constraint relation_order_goods foreign key (goodId) references tb_goods(id)
)


/*
被引用的表被成为父表 tb_goods
引用别人的表称为子表 tb_order

*/

insert into tb_goods(id, name, description) values(1, "小米", "手机");
insert into tb_goods(id, name, description) values(2, "锤子", "手机");
insert into tb_goods(id, name, description) values(3, "魅族", "手机");

-- 1452 - Cannot add or update a child row: a foreign key constraint fails (`test1`.`tb_order`, CONSTRAINT `relation_order_goods` FOREIGN KEY (`goodId`) REFERENCES `tb_goods` (`id`)) 给子表添加一个父没有的
insert into tb_order values(1, "2024-05-27", 3);
insert into tb_order values(2, "2024-05-27", 3);

-- 子表可以随意删
delete from tb_order where id = 2;

-- 父表被引用的数据不能删
delete from tb_goods where id = 1;
-- 1451 - Cannot delete or update a parent row: a foreign key constraint fails (`test1`.`tb_order`, CONSTRAINT `relation_order_goods` FOREIGN KEY (`goodId`) REFERENCES `tb_goods` (`id`))
delete from tb_goods where id = 3; -- 有引用
```

![image-20240527001831151](https://not-have.github.io/file/images/image-20240527001831151.png)

# 六、查询





39
