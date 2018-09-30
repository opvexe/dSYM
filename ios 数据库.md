## 数据库

#### [iOS数据库加密](https://blog.csdn.net/zrhloveswallow/article/details/51841185) 
```
加密：
打开非加密数据库
创建一个新的加密的数据库附加到原数据库上
导出数据到新数据库上
卸载新数据库

解密：
打开加密数据库
创建一个新的不加密的数据库附加到原数据库上
导出数据到新数据库上
卸载新数据库
```
#### [数据库迁移](https://www.jianshu.com/p/7473a72700a5)

#### [数据库多表查询](http://artjing.com/2016/10/16/%E4%BD%BF%E7%94%A8FMDB%E5%8F%8ACoreData%E5%AE%9E%E7%8E%B0%E5%A4%9A%E8%A1%A8%E6%9F%A5%E8%AF%A2/)

#### [数据库增删改查](http://artjing.com/2016/10/16/%E4%BD%BF%E7%94%A8FMDB%E5%8F%8ACoreData%E5%AE%9E%E7%8E%B0%E5%A4%9A%E8%A1%A8%E6%9F%A5%E8%AF%A2/)

## FMDB 线程安全
```
推荐使用
使用FMDatabaseQueue保证线程安全，
FMDatabaseQueue是一个串行队列，在多个线程来执行查询和更新时会使用这个类，
避免同时访问同一个数据
```
## 常用的sql语句
> 1.普通的条件查询 SELECT nickname,email FROM testtable <br> 
> 2.筛选查询 SELECT DISTINCT nickname,email FROM testtable // DISTINCT可以去掉查询的重复数据<br> 
> 3.限制查询行数<br>  
 > > 只返回查询的前两行 SELECT TOP 2 nickname,email FROM testtable<br>  
 	  返回查询到的前百分之20 SELECT TOP 20 PERCENT nickname,email FROM testtable<br>  
 	  查询符合条件的行数 select count(*)from testtable where %@ = '%@'<br>  
 	  查询所有符合的数据，但限制20条，或者是从10到20条的分页数据 <br>
 	  select  * from testtable where %@ = '%@' limit 20<br>
 	  select  * from testtable where %@ = '%@' limit 10，20<br>

>4.查询时给表起别名 
 >> 查询用户表和城市表中，cityid相同的数据的username和cityid两列数据<br>
  	 SELECT `username`,citytable.cityid<br>
	 FROM `usertable`,`citytable`<br>
	 WHERE usertable.cityid=citytable.cityid <br>
	 
>5.查询条件WHERE和排序
 >>// where后面可以接各种的运算符>、>=、=、、!>、!= AND
// 下面的排序方式是，先以age进行排序，再根据userid排序（当age相同的情况下，根据userid再排序）
SELECT * FROM `usertable` ORDER BY `age` DESC,`userid` ASC 

>6.多表连接查询
 >> 1，内连接 INNER JOIN
    select s.name,m.mark from student s inner join mark m on s.id=m.studentid <br>
    2.外左连接 LEFT JOIN
    // 将左边的表全部选出来，即便mark表中有些分数没有，也会被查出来，区别于内连接，就是存在左连接表中没有值得数据
select s.name,m.mark from student s left join mark m on s.id=m.studentid <br>
	3，外右连接 RIGHT JOIN
	// 将右边表的数据全部取出来，无论左表中有没有数据匹配，而后面的on条件语句，其实是相当于一个行的对应，相同的id对应一行。
select s.name,m.mark from student s right join mark m on s.id=m.studentid<br>
	4，全连接 FILL JOIN
	// 将右边表的数据全部取出来，无论左表中有没有数据匹配，而后面的on条件语句，其实是相当于一个行的对应，相同的id对应一行。<br>
	select s.name,m.mark from student s full join mark m on s.id=m.studentid<br>
	select s.name,m.mark from student s full join mark m on s.id=m.studentid

>// 下面是一个在实际项目中用到的查询语句
dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
    NSString * sql = [NSString stringWithFormat:@"select DISTINCT A.*, B.%@, B.%@  From %@ as A Left Join %@ as B On A.%@=B.%@  where A.%@ = '%@' and A.%@ = '%@' and A.id < %ld order by id desc limit %ld;",yx_customer_service_nickname,yx_customer_service_portrait_url,t_OSMessage,t_OSCustomInfo,yx_customer_service_id,yx_customer_service_id,yx_current_user_id,userId,yx_contact_id,contactId,(long)lastId,(long)pageCount]; 
    
>7.插入语句
 >>// 插入一条数据，四个字段
INSERT INTO Persons VALUES ('Gates', 'Bill', 'Xuanwumen 10', 'Beijing')
// 插入一条数据，只有两个字段
INSERT INTO Persons (LastName, Address) VALUES ('Wilson', 'Champs-Elysees')  

>8.删除语句
 >>// 如果任何条件语句都没有，相当于删除整个表的内容
DELETE FROM Person WHERE LastName = 'Wilson'  

>9.更新语句
 >>UPDATE Person SET FirstName = 'Fred' WHERE LastName = 'Wilson'
UPDATE Person SET Address = 'Zhongshan 23', City = 'Nanjing'
WHERE LastName = 'Wilson' 