---
layout: post
title: MongoDB CRUD Operations / MongoDB的增删改查操作
date: 2015-07-23 17:23
categories: [blog ]
tags: [MongoDB, ]
description:
---


<center>**------English (英文)------**</center>


###<b>Insert Documents</b>

1> Insert a Document

1.1> Create a document.
Define a variable `document` that holds a document to insert.

```
document=({"user_id" : "chen","password" :"chen123" ,"DOB" :
	"02/03/1992" ,"education" :"M.C.S." , "profession" : "DEVELOPER"})
```

1.2> Insert the document.
Pass the variable `document` to the `db.collection.insert()` to perform an insert.

```
db.myinfo.insert(document);
```

2> Directly Insert without Document.

```
db.myinfo.insert({"user_id" : "hao","password" :"hao123" ,"DOB" : "02/12/1990" ,
	"education" :"M.C.A." , "profession" : "Software consultant"});
```


###<b>Modify Documents</b>

MongoDB can use `update()` function to modify data.

`db.collection.update( criteria, objNew, upsert, multi )`

<b>criteria</b> : update conditions, similar to the statements after `where` in sql update.

<b>objNew</b> : update's objects or some update operations ($,$inc...), may ragard as the statements after `set` in sql update.

<b>upsert</b> : if the record doesn't exist, if you want to insert this new record, true is insert, default is false, don't insert.

<b>multi</b> : defaust is false, only update the first record it found. if it is true, update all records meet the conditions.


```
db.myinfo.update({"user_id" : "hao"},{"user_id" : "hui","password" :"hui123" ,
	"DOB" : "06/06/1990" ,"education" :"M.E.B." , "profession" : "MARKETING"});
```


###<b>Query Documents</b>

Select all documents in a collection

```
db.myinfo.find({}); // or
db.myinfo.find();
```

which is similar to SQL statement

```
Select * from myinfo;
```

Select the documents which "education" is "M.C.S"

```
db.myinfo.find({"education":"M.C.S."});
```

which is similar to SQL statement

```
Select * from myinfo where education="M.C.S.";
```

<b>Conditional operator:</b>

(>) - $gt

(<) - $lt

(>=) - $gte

(<= ) - $lte

For example 1,

```
db.testtable.find({age : {$gt : 22}})
```

is similar to SQL statement

```
Select * from testtable where age >22;
```

Example 2,

```
db.testtable.find({age : {$lt :24, $gt : 17}})
```

is similar to SQL statement

```
Select * from testtable where age > 17 and age <24;
```


`limit()` accept a integer parameter, to define the number of records selected from MongoDB

```
db.COLLECTION_NAME.find().limit(NUMBER)
```

`skip()` to ignore the number of records

```
db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)
```

`sort()`, 1 is ascending, and -1 is descending

```
db.COLLECTION_NAME.find().sort({KEY:1})
```


###<b>Remove Documents</b>

Use `remove()`
if you want to delete record where "user_id" = "chen" in "myinfo", you may use,

```
db.myinfo.remove( { "user_id" : "chen" } );
```

Delete all data
if you want to delete all records in "myinfo", you may use,

```
db.myinfo.remove({});
```

Use `drop()` to delete collections
if you want to delete the whole "myinfo", including data and documents, you may use,

```
db.myinfo.drop();
```

Use `dropDatabase()` to delete database
if you want to delete the whole database, you may use,

```
db.dropDatabase()
```



<center>**------中文 (Chinese)------**</center>




###数据插入

###数据修改

###数据查询

###数据删除
