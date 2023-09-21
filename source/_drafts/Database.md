---
title: Database
tags:
- 数据库
---

## 数据库
数据库（database）是用来 <span class='custom-box custom-box-933'>组织、存储和管理</span>数据的仓库。
数据的来源：出行记录、消费记录、浏览网页，消息等。
数据的种类：文本数据、图像、音乐、声音等
用户可以对数据库进行增、删、改、查操作

## 常见数据库
* MySQL 数据库（使用最广，流行度最高，开源免费；Community free + Entrerprise charge）
* Oracle 数据库 (收费)
* SQL Server 数据库（charge）
* Mongodb 数据库（Community free + Entrerprise charge）

> MySQL/Oracle/SQL Server 为 <span class='custom-box custom-box-933'>传统型数据库</span>（又叫 <span class='custom-box custom-box-393'>关系型数据库或 sql 数据库</span>）设计理念相同，用法类似。
> Mongodb <span class='custom-box custom-box-933'>新型数据库，非关系型数据库，NoSQL 数据库</span>。在一定程序上弥补了传统数据库缺陷。

## 传统型数据库的数据组织结构
数据以什么样的结构进行存储，传统型数据库，数据的组织结构分为数据库（database），数据表（table），数据行（row），字段（field）这4个部分组成。

项目开发库中，表、行、字段的关系：
* 一般每个项目都对应独立的数据库
* 不同的数据存储在数据库不同的表中，如users,books
* 每个表中具体存储哪些信息，由字段决定。如 users:id,name,age 等
* 每一行一条数据。

## 数据库软件
* MySQL Server 用于数据存储与服务的软件；对应的安装包为 mysql-*.*.8-mocos****.dmg
* MySQL Workbench MySQL 可视化管理工具,相应的安装包为 mysql-Workbench-Community-***-macos-***.dmg

## MySQL
SQL(Structured Query Language) 结构化查询语言。用来访问和处理数据库的编程语言。以编程的形式操作数据库里的数据。
SQL 语言只能在关系型数据库中使用，Mongodb 这类非关系型数据库不支持 SQL 语言。

查询数据（select）、插入数据（insert into）、更新数据（update）、删除数据（delete）
where 条件
and/or 运算符
order by 排序
count(*) 函数

### SELECT
查询表中的数据，执行结果被存储在一个结果集中。
> select [列名1,列名2,...] from [表名]

select [列名] from [表名]

### INSERT INTO
向数据表中插入数据行
<span class='custom-box custom-box-933'>通过 values 向表中插入数据行。注意：列和值要一一对应，多个列和多个值之间用英文逗号分隔</span>
> insert into table_name(列1,列2,...) values (值1,值2,...)

### UPDATE
修改表中某行数据
> update [表名] set 列名=value where condition

### DELETE 
删除表中的行
> DELETE FORM [表名] WHERE CONDITION

### WHERE 子句
用于限定选择标准，在 SELECT,UPDATE,DELETE 语句中，皆可使用 WHERE 子句来限定选择标准。
> SELECT [列名] FROM [表名] WHERE 列 运算符 值

#### WHERE 子句中的运算符
|操作符|描述|
|---|---|
|=|等于|
|<>|不等于|
|>|大于|
|<|小于|
|>=|大于等于|
|<=|小于等于|
|BETWEEN|在某个范围内|
|LIKE|搜索某种模式|

#### AND 和 OR 运算符
在 where 子句中把两个或多个条件结合起来
AND 必须同时满足多个条件
OR 满足做任意一个条件

### ORDER BY 子句
根据指定的列对结果集进行排序。
ORDER BY 默认按照升序排序（ASC），使用 DESC 关键字可以对结果集进行降序排序。

### COUNT(*) 返回查询结果的总条数
> SELECT COUNT(*) FROM [表名]

### AS 为列设置别名
> SELECT COUNT(*) AS TOTAL FROM [表名]


## 在项目中操作 MySQL 数据库
```mermaid
 flowchart LR
 s1[安装 mysql 模块] --> s2[配置 mysql 模块]
 s2 --> s3[mysql 连接测试]
```

### 安装 mysql 模块
> npm i mysql

### 配置 mysql 模块化路由
```mermaid
 flowchart LR
 S1[导入 mysql 模块] --> S2[建立与mysql数据库的连接]
```

```JavaScript
// 导入 mysql 模块
const mysql = require('mysql')
// 建立与 mysql 数据库的连接
const db = mysql.createPool({
    host: '', // 数据库的 IP 地址
    user: '', // 登录数据库的帐号
    password: '', // 登录数据库的密码
    database: '' // 指定要操作哪个数据库
})
```

### mysql 连接测试
```JavaScript
db.query('SELECT 1', (err, results) => {
    if(err) return console.log(err)
    console.log(results) // [ RowDataPacket { '1': 1 } ] 数据库连接正确
})
```

### 查询数据
> 执行查询语句，`results` 结果为数组。
```JavaScript
// 查询 users 表中所有数据
db.query('SELECT * FROM users', (err, results) => {
    if(err) return console.log(err.message) // 查询失败
    console.log(results) // 查询成功，打印查询结果
})
```

### 插入数据
> sql 语句中 `英文 ?` 为占位符
> `results` 结果为一个对象
> `results.affectedRows` 影响行数大于 1，数据成功插入数据表
```JavaScript
// 要插入表中的数据对象
const {userName, password} = userInfo; //{userName: 'A', password: '123456'}
// 待执行的 SQL 语句，其中英文 ? 为占位符
const sql = 'INSERT INTO users(userName, password) VALUES (?,?)'
// 使用数组的形式，依次为 ? 占位符指定具体的值
db.query(sql, [userName, password], (err, results) => {
    if(err) return console.log(err) // sql 执行失败
    if(results.affectedRows > 1) console.log('数据插入成功')
})
```

#### 插入数据的便捷方式
> 数据的每个属性与数据表的字段 <span class='custom-box custom-box-933'>一一对应</span>，可以快捷方式插入
> 直接将数据对象当作占位符的值
```JavaScript
// 要插入表中的数据
const user = {userName: 'A', password: '123456'}
// 待执行的 SQL 语句，其中英文的 ? 为占位符
const sql = 'INSERT INTO users SET ?'
// 直接将数据对象当作占位符的值
db.query(sql, user, (err, results) => {
    if(err) return console.log(err.message) // 失败
    if(results.affectedRows === 1) console.log('插入数据成功')
})
```

### 插入数据
```JavaScript
// 更新的数据对象
const {id, userName, password} = userInfo; //{userName: 'A', password: '123456', id:3}
// 要执行的 SQL 语句
const sql = 'UPDATE users SET userName=?, password=? where id=?'
// 调用 db.query() 执行 SQL 语句的同时，使用数组依次为占位符指定具体的值
db.query(sql,[id,userName,password], (err, results) => {
    if(err) return console.log(err.message) // sql 执行失败
    if(results.affectedRows === 1) console.log('更新数据成功！')
})
```

#### 插入数据快捷方式
> 数据对象的每个属性和数据表的字段一一对应，可使用快捷方式插入
> UPDATE users SET ? WHERE id = ?
```JavaScript
// 要更新的数据
const user = {userName: 'A', password: '123456', id:3}
// sql 语句
const sql = 'UPDATE users SET ? WHERE id=?'
// db.query() 执行SQL语句的同时，使用数组依次为占位符指定具体的值
db.query(sql,[user, user.id], (err, results) => {
    if(err) return console.log(err.message) // sql 执行失败
    if(results.affectedRows === 1) console.log('更新数据成功！')
})
```

### 删除数据
> 在删除数据时，推荐根据 id 唯一标识，来删除对应的数据
> <span class='custom-box custom-box-933'>如果 sql 语句中有多个占位符，则必须使用数组为每个占位符指定具体的值，如果 sql 只有一个占位符，则不需要使用数组，指定该占位符的值。</span>
```JavaScript
// sql 语句
const sql = 'DELETE FROM users where id = ?'
// db.query() 执行 SQL 语句，为占位符指定具体的值
db.query(sql, 3, (err, results) => {
    if(err) return console.log(err.message) // sql 执行失败
    if(results.affectedRows === 1) console.log('数据删除成功')
})
```

### 标记删除
使用 DELETE 语句，会把数据真正的从表中删除。保险起见，<span class='custom-box custom-box-933'>推荐使用标记删除，来模拟删除的动作。</span>
> 所谓标记删除，就是在表中设置类似于 status 字段，标记该数据是否被删除。
用户执行删除动作时，执行 UPDATE 语句，将状态标记为删除即可.
```JavaScript
// 标记删除，使用 UPDATE 语句，替代 DELETE 语句，更新数据状态，并非真正删除
db.query('UPDATE users SET status = 1 WHERE id = ?', 3, (err, results) => {
    if(err) return console.log(err.message) // sql 执行失败
    if(results.affectedRows === 1) console.log('数据删除成功')
})
```