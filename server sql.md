##SQL SERVER

### 约束性

if object_id('cst_tbl') is not null  drop table cst_tbl;
create table cst_tbl (id int identity(1,1)  unique not null,
​                      name char(20)  ,
​					  age smallint check( age between 18 and 20)  not null   
​					  constraint pk_id  primary key  nonclustered (id)   ); ----完整性约束（主码+唯一性+条件+非空+外码）
​					  

添加主键有三种方式： 两种在定义表结构时候+alter table add constraint pk primary key nonclustered(id)

<strong style='color:orange'>ps : 非聚集索引不可在建表语句建立，只能单独create index idx_id on table (column1)</strong>

### 插入数据

insert into cst_tbl values('wbluo',18);
insert into cst_tbl (name ,age )
select ' wbluo',18
union all
select ' wbluo ',18;

insert into cst_tbl values(' wbluo',18);
insert into cst_tbl values(' wbluo ',18);
insert into cst_tbl values('我的名字叫美丽',18);
insert into cst_tbl values('我叫jack',18);
insert into cst_tbl values('',20);
insert into cst_tbl (age) values(20); ---指定插入字段；部分插入
insert into cst_tbl values('wbluo vs wbluo&love',20);

拓展：利用存储过程插入数据： 

 ![](D:\Users\wbluo\Desktop\tmp\procedure.png)

### 函数差异比较

select * ,charindex(' ',name) as p_1,patindex('%美丽%',name) as p_2,charindex('%美丽%',name) as p_3  ---charindex vs patindex
,case when isnull(name,'') ='' then '空值' else name end as name_2  ---对于空值/NA值赋值
,replace(name ,'wbluo','ctrip') as name_3 --replace 与stuff 的区别;
,stuff(name,1,2,'&') as name_4 ---前两个字节替换
,stuff(name ,1,1,'?') as name_5 ---前一个字节替换
from cst_tbl

![](D:\Users\wbluo\Desktop\tmp\difference.png)

### 快速创建空表

if object_id('cst_tbl_2') is not null  drop table cst_tbl_2;
<strong style='color:lightblue'>select top 0 * into cst_tbl_2   from cst_tbl where 1=1 ---创建空表；</strong>
insert into cst_tbl_2 values('我叫robert',18);
insert into cst_tbl_2 values('我叫rose',18);
insert into cst_tbl_2 values('我的名字叫美丽',20);
insert into cst_tbl_2 values('我叫jack',19);


select * from cst_tbl_2
select * from cst_tbl

### merge 的应用  实现delete update insert 三大功能；可以自由组合不一定三者一同出现；

merge cst_tbl as a 
using cst_tbl_2 as b on a.name =b.name 
when matched and a.age<>b.age then   ---当匹配时，更新不同数据
update set a.age=b.age 
when not matched then    ---当缺失时，插入缺失数据
insert (name ,age) values (b.name,b.age)
when not matched by source then delete ; ---删除主表没匹配到的记录;

### 复合索引 索引键字段顺序 是否影响索引引用?

if object_id ('tempdb..#a_soft') is not null drop table #a_soft
select * 
into #a_soft
from db_cmsdata..SoftPhoneRecords (nolock)
where calltime>='2019-01-01'

alter table #a_soft alter column recordid  bigint  not null  ---调整字段属性
alter table #a_soft add   primary key   (recordid) ---添加主键
create index idx_d on #a_soft (d,calltype) ;

select d, calltype
from #a_soft
--where TIME_IN>='2019-01-16'
WHERE 1=1 
AND d>='2019-01-16'            ---where 语句中 d 与calltype 语句谁先后无关；不影响使用索引；
and calltype in ('c','p','s')  --采用了非聚集索引idx_d index seek

![](D:\Users\wbluo\Desktop\tmp\idx_1.png)

select d, calltype
from #a_soft
where calltime>='2019-01-16'
and calltype in ('c','p','s') ---直接采用聚集索引 因为idx_d  没有calltime 这个键值

![](D:\Users\wbluo\Desktop\tmp\idx_2.png)

select d, calltype
from #a_soft
where calltime>='2019-01-01'
and d>='2019-01-16' ---直接采用聚集索引 因为idx_d  没有calltime 这个键值

![](D:\Users\wbluo\Desktop\tmp\idx_3.png)

select d, calltype
from #a_soft
where 1=1
and d>='2019-01-16' ---直接采用非聚集索引 index_d index seek

![](D:\Users\wbluo\Desktop\tmp\idx_4.png)

select d, calltype
from #a_soft
where 1=1
and calltype in ('c','p','s') ---直接采用非聚集索引 index_d index scan  

![](D:\Users\wbluo\Desktop\tmp\idx_5.png)

### implicit convert 转化是否影响索引使用

​    通过下面执行计划看出，隐形转化影响索引 从seek -->scan；#t1中的calltype 字符类型为varchar(10).

因此先对字段进行一个转化convert (int)；由于统计信息直方图或者密度图是依据varchar(10)类型统计的，

所以优化器无法使用统计信息；

![](D:\Users\wbluo\Desktop\tmp\implicit.png)

### nested loop关联

1. 是否可以通过执行计划知道外部表与内部表？

   ​                                                             外部表在上面；

   ![](D:\Users\wbluo\Desktop\tmp\loop_11.jpg)

   ![](D:\Users\wbluo\Desktop\tmp\loop_22.jpg)

2. 线条粗细表示数据集大小

3. 控制内部表的大小，减少io

### 聚合索引vs覆盖索引

type1:create index idx_id on incomecall_orders (orderid ,incomecallpurporselevel1)

type2:create index idx_id on incomecall_orders (orderid) include (incomecallpurporselevel1)

- 聚集索引和非聚集索引重要区别：

​        聚集索引叶子层：数据集；

​       非聚集索引叶子层：索引页+定位器（聚集键）

- type1:根部以及中间页都会记录 orderid ,incomecallpurporselevel1 字段信息；

  type2:根部以及中间只会记录orderid 字段信息；incomecallpurporselevel1 只会被记录在叶子层；

- 因为中间页也记录incomecallpurporselevel1 信息，因此type1的索引index_depth 比 type2大；

  通过指令 <strong style='color:orange'>sys.dm_db_index_physical_stats(db_id(),object_id('incomecall_orders'),null,null,default)</strong> 

<span style='color:orange'>覆盖索引的目的：减少书签查找（bookmark) {1:key键查找--聚集表 2：RID查找 --堆} </span>

### 倒序索引+字符串索引

create index idx_id_2 on wbluo_test (id desc)   ---diffrence between  desc or not desc

---倒序索引：![](D:\Users\wbluo\Desktop\tmp\tmp_4.png)

---字符串索引： 利用函数checksum 

![](D:\Users\wbluo\Desktop\tmp\check_sum.png)

### 控制流

1. if ...else :

   ---- if else 语句 +存储过程

   if year(getdate())<>year(dateadd(day,1,getdate()))
     print '不是最后一天'
   else  if 
    month(getdate())<>month(dateadd(day,1,getdate())) 
     print '不是当月最后一天'
   else print ' 同月同年'

   create procedure pro_select @type int 
   with recompile
   as 
   if @type =1 
    begin
     select name,count(1)
     from cst_tbl
     group by name
    end
   else 
     begin
     select *
     from cst_tbl
     end
   exec pro_select 2

2. if+while :循环流

   ----if +while 

   if object_Id('t1') is not null drop table t1;
   create table t1 (id int );
   declare @id int ;
   set @id=0;
   while @id<=10 
     begin 
     set @id=@id+1;
     insert into t1  values(@id);  
     end 

### where on  条件语句的区别：

​    三值：True ,False ,Unkown; 

1. ![](D:\Users\wbluo\Desktop\tmp\on_1.png)

   ![](D:\Users\wbluo\Desktop\tmp\on_2.png)

 等价于：<br> ![](D:\Users\wbluo\Desktop\tmp\on_3.png)

<strong  style='color:orange'>ON 语句不影响左表输出行数，只是对左边有一步判断操作后，再进行left join，影响行输出的仅仅是where 语句</strong>

 ![](D:\Users\wbluo\Desktop\tmp\on_4.png)



2： 导致left join 无效话语句：

  case one：on where 条件语句位置

 ![](D:\Users\wbluo\Desktop\tmp\on_5.png)

 ![](D:\Users\wbluo\Desktop\tmp\on_6.png)

  case two ：多表关联

 ![](D:\Users\wbluo\Desktop\tmp\on_7.png)

### case when新秀

1：

 select  case when <strong style='color:orange'>exists </strong>( select 1
​                            from incomecall_orders as a 
​							where a.orderid =b.orderid 
​							and a.maincid=b.maincid
​							and datediff(d,a.time_in,b.time_in) between 0 and 9 
​							) then 'T' else 'F' end as [是否9天内重复来电],*
 from incomecall_orders as b 
 where 1=1

 select case when <strong style='color:orange'>b.processor in (select eid from #eid ) </strong>then 'T' else 'F' end as type
 from cst_tbl

2:

 select id ,name ,
 case when  patindex ('%r%',name)>0
​      then case when id <2 then 'T' end 
​	 <strong style='color:orange'> else 'F' end as type, </strong><strong style='color:red'> ---ELSE 是对patindex ('%r%',name)>0 否定； </strong> 
 case when   patindex ('%r%',name)>0 then 'T' else 'F' END AS TYPE_2
 from cst_tbl_2

![](D:\Users\wbluo\Desktop\tmp\on_8.png)

### 存储过程

 特点：编译、重新编译和重用执行计划：

  1：执行计划重用

​       弊端：第一次调用存储过程时使用低选择性的输入，那么编译器将会保存该输入最理想的执行计划；

即使后面调用中，输入一个高选择性的参数，也无法改变（index scan -->index seek 或者用别的索引）

​      解决方案： 对于简单执行语句，换而言之解析编译花较少时间的语句，可以采用recompile 

<strong style='color :lightblue'>整个语句集重新编译：</strong>

​     create procedure XXX  with recompile as XXXXX;

<strong style='color :lightblue'>部分语句集重新编译：</strong>

​     create procedure XXX   as XXXXX (option recompile)  yyyyyyy;

<strong style ='color :red'> 特点：每次调用它时创建新的执行计划---sql server 不在缓存中保存执行计划；</strong>

###统计信息

-  update statistics table (字段) with full scan;
-  update statistics table (字段) with sample 70 percent;
-  update statistics table ----表所有统计信息更新

###SARG

- search agrument :参数查找，是对字段的一个筛选操作
- 一般在where on  等删选阶段
- 常见问题：非参数索引导致的  索引无法使用
  1. 索引字段使用函数 upper(column) ,column+1 ,substring(column)
  2. 隐性转换 ， on 之间数据类型不一样： datetime >int>char
  3. 复合索引，但是部分字段限制；

### Unique Index

create index idx_id on wbluo_test(id) 
create unique index idx_name on wbluo_test(id)    --- difference between unique and index 

对于前者，<b>即使id 有重复值</b>，也可以创建索引；
但是用了unique，<b>会检索是否有重复值</b>，有的话不能创建也不能插入数据  [插入异常]

### alter

alter table #t1 alter colum name varchar(20)

update #t1 set name=name+'wbluo' where name ='A'  ----补充内容；

数据备份另一种思路：

update wbluo_test set orderid =12300,datachange_lasttime=getdate() where orderid =123 

### 关系数据库和分布式数据库

#### Nosql 

not only sql :不仅仅是sql, 去掉了关系数据库的数据之间的关联性

在分布式数据库中CAP原理CAP+BASE：：
C:Consistency:强一致性，A:Availability：可用性，P:Partition tolerance：分区容错性。
CAP理论的核心是：一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性这三个需求， 
最多只能同时较好的满足两个。
因此，根据 CAP 原理将 NoSQL 数据库分成了满足 CA 原则、满足 CP 原则和满足 AP 原则三 大类： 
CA - 单点集群，满足一致性，可用性的系统，通常在可扩展性上不太强大。（msyql/Oracle）； 
CP - 满足一致性，分区容忍性的系统，通常性能不是特别高。（Redis/mongdb)； 

AP - 满足可用性，分区容忍性的系统，通常可能对一致性要求低一些。（大多数网站**架构**选择如淘宝，京东）；

#### RDBMS

- 高度组织化结构化数据

- 结构化查询语言（SQL）

- 数据和关系都存储在单独的表中  -->强调范式建表

- 数据操纵语言，数据定义语言

  强调ACID:

  1. Aotomicity:原子性

  2. Consistency:一致性

  3. Isolation:独立性

     所谓的独立性是指并发的事务之间不会互相影响，如果一个事务要访问的数据正在被另外一个事务修改，只要另外一个事务未提交，它所访问的数据就不受未提交事务的影响。比如现有有个交易是从A账户转100元至B账户，在这个交易还未完成的情况下，如果此时B查询自己的账户，是看不到新增加的100元的

  4. Durability:持久性

  

