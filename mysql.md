#[mysql分区]
create table t( 
    id int 
)engine innodb 
partition by hash(id) 
partitions 4;


目的：为了了解mysql单表分区方法，特此作为学习笔记记录一下。

一。准备表，创建一个学生表，包含主键sid和名称sname字段

create table students(
sid int(5) primary key,
sname varchar(24)
);


二。建立分区，按照主键ID的值进行设定分区规则如下：

alter table students partition by range(sid)
(
partition p0 values less than (10001),
partition p1 values less than (10003),
partition p2 values less than (10005),
partition p3 values less than maxvalue
);

删除一个分区
ALTER TABLE students drop PARTITION p0

更多请看https://www.2cto.com/database/201805/742843.html

