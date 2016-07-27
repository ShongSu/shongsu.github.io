---
layout: post
title: SQL to MongoDB Mapping Chart / SQL与MongoDB对照表
date: 2016-07-26 20:29
categories: [blog ]
tags: [MongoDB, ]
description:
---

##	术语 & 概念

下表对比了SQL和MongoDB的术语和概念。


| SQL   |  MongoDB |
|-------|----------|
| database  | database  |
| table  | collection |
| row	| document or BSON document |
|column |	field |
|index	| index |
|table joins	| embedded documents and linking |
|primary key: Specify any unique column or column combination as primary key.| primary key: In MongoDB, the primary key is automatically set to the `_id` field. |
|aggregation (e.g. `group by`)	| aggregation pipeline |


###	示例

下表对比了SQL和MongoDB的语句，我们假设本例中的表满足以下几点：

！SQL表名为 `users`

！MongoDB集合名为 `users`，包含以下文档格式：

	{
		_id: ObjectId("509a8fb2f3f4948bd2f983a0"),
		user_id: "abc123",
		age: 55,
		status: 'A'
	}


##	Create & Alter

下表对比了SQL和MongoDB中与表相关的操作语句。

	CREATE TABLE users (
	    id MEDIUMINT NOT NULL
	        AUTO_INCREMENT,
	    user_id Varchar(30),
	    age Number,
	    status char(1),
	    PRIMARY KEY (id)
	)


隐式的创建第一个`insert()`操作。如果没有显式声明主键，将自动生成主键key `_id`。

	db.users.insert( {
	    user_id: "abc123",
	    age: 55,
	    status: "A"
	 } )

然后，你也可以显示的创建一个集合：

	db.createCollection("users")


集合不描述或指定文件的结构,即在集合级别上，没有结构更换。
然而在文档级别，`update()`操作可以使用`$set`操作符向既存的文档中添加新的字段。


	ALTER TABLE users
	ADD join_date DATETIME


	db.users.update(
	    { },
	    { $set: { join_date: new Date() } },
	    { multi: true }
	)


在文档级别，`update()`操作可以使用`$unset`操作符删除文档中的某个字段。


	ALTER TABLE users
	DROP COLUMN join_date

	db.users.update(
	    { },
	    { $unset: { join_date: "" } },
	    { multi: true }
	)

创建索引

	CREATE INDEX idx_user_id_asc
	ON users(user_id)

	db.users.createIndex( { user_id: 1 } )

创建有序索引

	CREATE INDEX
	       idx_user_id_asc_age_desc
	ON users(user_id, age DESC)

	db.users.createIndex( { user_id: 1, age: -1 } )

删除数据表

	DROP TABLE users

	db.users.drop()


###	Insert操作

下表对比了SQL和MongoDB中与向表中插入数据相关的操作语句。

	INSERT INTO users(user_id,
	                  age,
	                  status)
	VALUES ("bcd001",
	        45,
	        "A")


	db.users.insert(
	   { user_id: "bcd001", age: 45, status: "A" }
	)

###	Select操作

下表对比了SQL和MongoDB中与从表中读取数据相关的操作语句。

注意：
`find()`方法返回的数据中总是包含`_id`字段，除非你在映射中显式的说明它。下面某些SQL查询将使用`_id`字段，尽管该字段并没有在对应的`find()``方法中包含。

查询所有记录

	SELECT *
	FROM users

	db.users.find()

查询指定字段的所有记录

	SELECT id,
	       user_id,
	       status
	FROM users

	db.users.find(
	    { },
	    { user_id: 1, status: 1 }
	)

不显示查询结果的`id`字段

	SELECT user_id, status
	FROM users

	db.users.find(
	    { },
	    { user_id: 1, status: 1, _id: 0 }
	)

指定查询`status`为`A`的记录

	SELECT *
	FROM users
	WHERE status = "A"

	db.users.find(
	    { status: "A" }
	)

指定查询`status`为`A`的记录并指定输出

	SELECT user_id, status
	FROM users
	WHERE status = "A"

	db.users.find(
	    { status: "A" },
	    { user_id: 1, status: 1, _id: 0 }
	)

指定查询`status`不为`A`的记录

	SELECT *
	FROM users
	WHERE status != "A"

	db.users.find(
	    { status: { $ne: "A" } }
	)

指定查询`status`为`A`，并且`age`为`5`的记录

	SELECT *
	FROM users
	WHERE status = "A"
	AND age = 50

	db.users.find(
	    { status: "A",
	      age: 50 }
	)

指定查询`status`为`A`，或者`age`为`5`的记录

	SELECT *
	FROM users
	WHERE status = "A"
	OR age = 50

	db.users.find(
	    { $or: [ { status: "A" } ,
	             { age: 50 } ] }
	)

指定查询`age`大于`25`的记录

	SELECT *
	FROM users
	WHERE age > 25

	db.users.find(
	    { age: { $gt: 25 } }
	)

指定查询`age`小于`25`的记录

	SELECT *
	FROM users
	WHERE age < 25

	db.users.find(
	   { age: { $lt: 25 } }
	)

指定查询`age`大于`25`，并且小于等于`50`的记录

	SELECT *
	FROM users
	WHERE age > 25
	AND   age <= 50

	db.users.find(
	   { age: { $gt: 25, $lte: 50 } }
	)

指定查询`user_id`中间包含`bc`的记录

	SELECT *
	FROM users
	WHERE user_id like "%bc%"

	db.users.find( { user_id: /bc/ } )


指定查询`user_id`以`bc`开头的记录

	SELECT *
	FROM users
	WHERE user_id like "bc%"

	db.users.find( { user_id: /^bc/ } )

指定查询`status`为`A`，并且按照`user_id`升序排序

	SELECT *
	FROM users
	WHERE status = "A"
	ORDER BY user_id ASC

	db.users.find( { status: "A" } ).sort( { user_id: 1 } )

指定查询`status`为`A`，并且按照`user_id`降序排序

	SELECT *
	FROM users
	WHERE status = "A"
	ORDER BY user_id DESC

	db.users.find( { status: "A" } ).sort( { user_id: -1 } )

查询`users`表中的记录个数

	SELECT COUNT(*)
	FROM users

	db.users.count()
	or
	db.users.find().count()

查询具有`user_id`字段的记录个数

	SELECT COUNT(user_id)
	FROM users

	db.users.count( { user_id: { $exists: true } } )
	or
	db.users.find( { user_id: { $exists: true } } ).count()

查询`age`大于30的记录的个数

	SELECT COUNT(*)
	FROM users
	WHERE age > 30

	db.users.count( { age: { $gt: 30 } } )
	or
	db.users.find( { age: { $gt: 30 } } ).count()

查询`status`值不重复的记录

	SELECT DISTINCT(status)
	FROM users

	db.users.distinct( "status" )

查询`users`表中返回的第一条记录

	SELECT *
	FROM users
	LIMIT 1

	db.users.findOne()
	or
	db.users.find().limit(1)

查询`users`表中的前5条记录，跳过10条记录

	SELECT *
	FROM users
	LIMIT 5
	SKIP 10

	db.users.find().limit(5).skip(10)

X

	EXPLAIN SELECT *
	FROM users
	WHERE status = "A"

	db.users.find( { status: "A" } ).explain()


###	Update操作

下表对比了SQL和MongoDB中与数据更新相关的操作语句。

将`age`大于`25`的用户的`status`设置为`C`.

	UPDATE users
	SET status = "C"
	WHERE age > 25

	db.users.update(
	   { age: { $gt: 25 } },
	   { $set: { status: "C" } },
	   { multi: true }
	)

将`status`为`A`的用户的`age`加`3`.

	UPDATE users
	SET age = age + 3
	WHERE status = "A"

	db.users.update(
	   { status: "A" } ,
	   { $inc: { age: 3 } },
	   { multi: true }
	)

###	Delete操作

下表对比了SQL和MongoDB中与删除数据相关的操作语句。

删除status为D的字段

	DELETE FROM users
	WHERE status = "D"

	db.users.remove( { status: "D" } )

删除users表中的所有记录

	DELETE FROM users

	db.users.remove({})

##	SQL聚集操作

聚合管道允许MongoDB提供原生聚合功能,对应于许多常见的SQL数据聚合操作。

下表概述了常见的SQL聚合条件,功能,和概念以及相应的MongoDB聚合运算符:

| SQL 	  | MongoDB  |
|---------|----------|
|WHERE	  | $match   |
|GROUP BY	| $group   |
|HAVING	  | $match   |
|SELECT	  | $project |
|ORDER BY	| $sort    |
|LIMIT	  | $limit   |
|SUM()	  | $sum     |
|COUNT()	| $sum     |
|join	    | $lookup  |


###	示例

下表对比了SQL和MongoDB的语句，我们假设本例中的表满足以下几点：

！SQL定义了两个数据表，表名分别为 `orders` 和 `order_lineitem`，其中以`order_lineitem.order_id`和`orders.id`为主外键连接.

！MongoDB定义了一个数据集合名为 `orders`，包含以下文档格式：

	{
	  cust_id: "abc123",
	  ord_date: ISODate("2012-11-02T17:04:11.102Z"),
	  status: 'A',
	  price: 50,
	  items: [ { sku: "xxx", qty: 25, price: 1 },
	           { sku: "yyy", qty: 25, price: 1 } ]
	}

返回数据记录的数量：

	SELECT COUNT(*) AS count
	FROM orders

	db.orders.aggregate( [
	   {
	     $group: {
	        _id: null,
	        count: { $sum: 1 }
	     }
	   }
	] )

计算`orders`中`price`字段的总和：

	SELECT SUM(price) AS total
	FROM orders

	db.orders.aggregate( [
	   {
	     $group: {
	        _id: null,
	        total: { $sum: "$price" }
	     }
	   }
	] )

对于每个特定的`cust_id`，计算`price`字段的总和：

	SELECT cust_id,
	       SUM(price) AS total
	FROM orders
	GROUP BY cust_id

	db.orders.aggregate( [
	   {
	     $group: {
	        _id: "$cust_id",
	        total: { $sum: "$price" }
	     }
	   }
	] )

对于每个特定的`cust_id`，计算`price`字段的总和，并将结果根据`sum`进行排序：

	SELECT cust_id,
	       SUM(price) AS total
	FROM orders
	GROUP BY cust_id
	ORDER BY total

	db.orders.aggregate( [
	   {
	     $group: {
	        _id: "$cust_id",
	        total: { $sum: "$price" }
	     }
	   },
	   { $sort: { total: 1 } }
	] )

对于每个特定的`cust_id`，`ord_date`组合，计算`price`字段的总和，不包括时间日期：

	SELECT cust_id,
	       ord_date,
	       SUM(price) AS total
	FROM orders
	GROUP BY cust_id,
	         ord_date

	db.orders.aggregate( [
	   {
	     $group: {
	        _id: {
	           cust_id: "$cust_id",
	           ord_date: {
	               month: { $month: "$ord_date" },
	               day: { $dayOfMonth: "$ord_date" },
	               year: { $year: "$ord_date"}
	           }
	        },
	        total: { $sum: "$price" }
	     }
	   }
	] )

对于具有多条数据的`cust_id`字段，返回`cust_id`以及数据记录的个数：

	SELECT cust_id,
	       count(*)
	FROM orders
	GROUP BY cust_id
	HAVING count(*) > 1

	db.orders.aggregate( [
	   {
	     $group: {
	        _id: "$cust_id",
	        count: { $sum: 1 }
	     }
	   },
	   { $match: { count: { $gt: 1 } } }
	] )

对于每一个独特的`cust_id`，`ord_date`分组，计算其`price`字段的总和，并返回总和大于`250`的记录，不包括时间日期：

	SELECT cust_id,
	       ord_date,
	       SUM(price) AS total
	FROM orders
	GROUP BY cust_id,
	         ord_date
	HAVING total > 250

	db.orders.aggregate( [
	   {
	     $group: {
	        _id: {
	           cust_id: "$cust_id",
	           ord_date: {
	               month: { $month: "$ord_date" },
	               day: { $dayOfMonth: "$ord_date" },
	               year: { $year: "$ord_date"}
	           }
	        },
	        total: { $sum: "$price" }
	     }
	   },
	   { $match: { total: { $gt: 250 } } }
	] )

对于每一个独特的具有`status`为`A`的`cust_id`，计算它们`price`字段的总和：

	SELECT cust_id,
	       SUM(price) as total
	FROM orders
	WHERE status = 'A'
	GROUP BY cust_id

	db.orders.aggregate( [
	   { $match: { status: 'A' } },
	   {
	     $group: {
	        _id: "$cust_id",
	        total: { $sum: "$price" }
	     }
	   }
	] )


对于每一个独特的具有`status`为`A`的`cust_id`，计算它们`price`字段的总和，并只返回总和大于`250`的记录：

	SELECT cust_id,
	       SUM(price) as total
	FROM orders
	WHERE status = 'A'
	GROUP BY cust_id
	HAVING total > 250

	db.orders.aggregate( [
	   { $match: { status: 'A' } },
	   {
	     $group: {
	        _id: "$cust_id",
	        total: { $sum: "$price" }
	     }
	   },
	   { $match: { total: { $gt: 250 } } }
	] )

对于每一个独特的`cust_id`，计算与`orders`结合的`line_time`的`qty`字段的总和。

	SELECT cust_id,
	       SUM(li.qty) as qty
	FROM orders o,
	     order_lineitem li
	WHERE li.order_id = o.id
	GROUP BY cust_id

	db.orders.aggregate( [
	   { $unwind: "$items" },
	   {
	     $group: {
	        _id: "$cust_id",
	        qty: { $sum: "$items.qty" }
	     }
	   }
	] )

返回`cust_id`，`ord_date`分组的记录个数。排除时间的日期：

	SELECT COUNT(*)
	FROM (SELECT cust_id,
	             ord_date
	      FROM orders
	      GROUP BY cust_id,
	               ord_date)
	      as DerivedTable
	db.orders.aggregate( [
	   {
	     $group: {
	        _id: {
	           cust_id: "$cust_id",
	           ord_date: {
	               month: { $month: "$ord_date" },
	               day: { $dayOfMonth: "$ord_date" },
	               year: { $year: "$ord_date"}
	           }
	        }
	     }
	   },
	   {
	     $group: {
	        _id: null,
	        count: { $sum: 1 }
	     }
	   }
	] )
