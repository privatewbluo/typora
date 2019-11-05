# Hive数据仓库

### 数据处理类型

![](C:\Users\Administrator\Desktop\携程文件夹\tmp\hql_18.png)

###hive与hadoop 有什么关系？

hadoop 是集群；

- Hive是基于Hadoop的数据仓库工具，可以对存储在**HDFS**上的文件数据集进行查询和分析处理
- Hive对外提供了类似于SQL语言的查询语言 HiveQL，在做查询时将HQL语句转换成**MapReduce**任务，在Hadoop层进行执行。
- 所以没有数据库那种 update更新操作，只有insert ,delete操作。
- hadoop=hdfs+mapreduce (高富帅+名人)

1. ![](C:\Users\Administrator\Desktop\携程文件夹\tmp\dw_4.png)

    

   ODS:operational data store:操作型数据库，一般数据是实时性，数据抽取频率很高(10min,半小时抽取一次)

   是接近landing(帖源层数据)，ods会保存历史数据

###什么是分布式？什么是集群

分布式：对于不同数据结构、不同数据功能的节点组成的系统；异构节点；

一个事物请求，会涉及多个节点

集群：对象相同节点

一个事物请求，只会涉及一个节点

共性：高吞吐量与高可用性

用一堆廉价PC去实现大规模稳定的并行数据处理

### hive架构

![](C:\Users\Administrator\Desktop\携程文件夹\tmp\hql_17.png)

用户接口：cli,wui（web user interface：通过浏览器访问）

cli:command-line interface:命令行的客户端 (shell)

jdbc：drive+url(连接数据库）+pwd+username



#### Meta store:元数据

<span style='background-color:lightblue'>**schema 与 data 分离** </span>

- 数据存放到hdfs中 文件；

  <span style='background-color:lightblue'>**Case one : 有schema， 无文件**</span>

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_13.png)

  1. 在hue file browser,可以看到hdfs上已经没有文件

     但是在数据库中依旧保存 元数据

  <span style='background-color:lightblue'>**Case two :有文件，无schema**</span>

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_14.png)

  therefore :

  ```shell
  create external  table wenbluo_test_2 like VST_MOTOR_DAILY_NEW_2019_05_10 location 'hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_1';
  
  load data inpath 'hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_1/clsfd_partition_date=2016-01-01' into table wenbluo_test_2 partition (clsfd_partition_date='2016-01-01');
  
  ####此时wenbluo_test_1 目录下directory clsfd_partition_date=2016-01-01 被 copy  到wenbluo_test_2 目录下；
  ```

  

- meta 存放的是 hive表结构与hdfs文件位置关系、大小关系

###执行原理

![](C:\Users\Administrator\Desktop\携程文件夹\tmp\hql_2.png)



解析器：解析SQL语句，(语法解析）查看是否报错，生成AST tree（抽象树）

编译器：生成HQL的逻辑执行计划，将抽象树转化为 operator tree (操作树）

优化器：对生成的逻辑执行计划，进行优化，最后转化为MR过程；

一般OTtree 有哪些操作： SELECT operator ,filter Operator, Join operator ,GroupBy operator,TableScan operator

<span style='color:orange'>因此跟关系数据库一样，也有优化功能</span>

input -->map-->shuffle-->reduce-->ouput

其中input和output都是hdfs文件，其中文件格式不唯一：可以是csv ,txt等格式

map：读取文件生成key-value:  其中key :一般是join 关联字段 生成组合键， value:是where +select 字段信息 +tag表标记信息

shuffle：执行阶段是将key依据key键hash取模 排序；

reduce：执行 join group by  聚合函数



#### Hive 不支持语法

1. update

2. delete 

   ```sql
   delete from wenbluo_test_10 where clafd_partition_date='2016-01-01';
   FAILED: SemanticException [Error 10294]: Attempt to do update or delete using transaction manager that does not support these operations.
   
   ```

3. 非等值连接

4. union 

#### Hive 特有语法

- create table +location +delimiter 

- bucket  :分桶表

- meta data  : 存放table 表结构以及 与hdfs 映射关系，hadoop 上的hdfs 仅仅是文件

   ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_45.png)

   - 查看schema 映射 hdfs :location
   - 统计table statistics 
   - stored formate 

   msck table  table_name  :

   msck repair table table_name:

- 复杂的数据类型：集合数据  （ map\ array \ struct )

- rlike  & like   : regular 

   ```sql
   select * 
   from wenbluo_test_10
   where country rlike '.*(Chicago|Ontario).*'
   
   ---等价于
   select * 
   from wenbluo_test_10
   where country like '%Chicago%'  or country like '%Ontario%'
   ```

   

- left semi join & map join

   - semi  join 

     - ```sql
       select  * 
       from tbl as a 
       where a.column1  in ( select * from tbl_2 as b )
       --- error  not supoort
       
       --- exists
       select * 
       from tbl as b 
       where exists  ( select 1  from tbl_2 as b where b.column1=a.column1)
       
       ---left semi join 
       
       select a.* 
       from tbl as a left semi join tbl_2 as b on a.column1=b.column1
       
       ---注意使用left semi join 时候 select 字段 只能是left table !!
       ```

       

   - map join 

     <span style='color:orange'> **优势在于 减少了shuffle 和reduce 中间过程**</span>

     ```shell
     select /*+mapjoin(b) */ s.* from tbl as  a join tbl_2 as b on a...=b..
     
     
     set hive.auto.convert.join=True;
     ##对于两张表join 时候，会自动区分大小表，而不需要显式，指定who Is big table !
     
     ```

     

- order by cluster by  distributed by  sort by 

   <a href='https://saurzcode.in/2015/01/hive-sort-order-distribute-cluster/'>**reference**</a>

   - order by <span style='background-color:yellow'>**global 排序**</span>，需要放到一个reducer 去操作。

     --> 因此当数据集很大的时候，会导致异常缓慢，因为一个reducer 在操作。

     --> 作用于 <span style='background-color:lightblue'>**reduce 端**</span>

   - sort by 是  <span style='background-color:yellow'>**局部排序**</span>，对每一个reducer 输出结果进行一次排序，这样可以提高后面的全局排序

     --> 优于order by 

     --> 最终结果集，并不是全局排序

     --> 作用于<span style='background-color:lightblue'>**reduce 端**</span>

     

   - distributed by  

     ---> 作用于<span style='background-color:lightblue'>**map端**</span>

     -->由于sort by 是局部排序，因此多个reduce 数据内容会有明细的重叠

     ---> 采用distribute by 这样可以在map 端输出结果后，统一键值的数据，会分发到同一个reduce 中

     ```sql
     select * 
     from tbl 
     distributed by site 
     sort by site 
     
     --- distribute by 一定要写在 order by 之前！
     --QA :
     --- 难道为了sort by 就单独研发一个distribute by ? 一定要组合运用？
        1:可以单独使用
          select * from tbl1  distribute by 1 ;
        
        2：分桶表的运用
           create table tbl ( clsfd_site_id int ,clsfd_rpt_name string ,metric_value bigint ) 
           partitoned by ( clsfd_sum_dt date )
           cluster by (clsfd_site_id ) into 32 buckets ;
           
        
     ```

   - **cluster by = sort by + distribute by**

   ### Application :<a href='https://deepsense.ai/optimize-spark-with-distribute-by-and-cluster-by/'>When Are They Useful?</a>

- group by rollup  /cube by /cube/grouping sets

- **data 与 schema 的分离，是hadoop 超越关系数据库的重要主张之一**

  <span style='background-color:lightblue'>**external table 生动解释了schema 与 data 的分离！**</span>

  external table 跟unix 创建 <span style='background-color:lightblue'>**soft link**</span> 一样的原理

- **external table  :外部表**

  --><span style='background-color:yellow'>本质跟shell 中 **soft link 一样**，**仅仅是创建了一个链接**</span>

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_70.png)

  - 可以看到create external table 也会创建table  目录
  
  - ```sql
    hive (dw_clsfd)> load data inpath  'hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_22' into table wenbluo_test_location;
    Loading data to table dw_clsfd.wenbluo_test_location
    Table dw_clsfd.wenbluo_test_location stats: [numFiles=4, totalSize=878]
    OK
    --通过load 函数 去加载数据
    --注意：会将wenbluo_test_22 底层文件直接mv 到wenbluo_test_location 
    --并不是copy
  --要想cp :需要 MR ！！！！
    ```

  - drop table wenbluo_test_location;
  
    ```sql
    select count(1) from wenbluo_test_location;
    FAILED: SemanticException [Error 10001]: Line 1:21 Table not found 'wenbluo_test_location'
  ---仅仅是删除了schema ，但是不删除data!!!
    ```

    ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_71.png)
  
  - 

#### hive 建表语句

- ![](C:\Users\Administrator\Desktop\携程文件夹\tmp\hql_1.png)

- ![](C:\Users\Administrator\Desktop\携程文件夹\tmp\hql_11.png)

- <span style='background-color:yellow'>**set hive.metastore.warehouse.dir** </span>

  存放table ,hdfs (分布式模式)or  file:// (本地模式).文件路径

  ```shell
  hive (dw_clsfd)> set hive.metastore.warehouse.dir;
  hive.metastore.warehouse.dir=/user/hive/warehouse
  
  ###默认的创建数据库 文件路径
  
  ```

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_57.png)

  当我们create database dw_clsfd 时候，hive 将会对应的创建一个目录 <span style='background-color:lightblue'>***/user/hive/warehouse/dw_clsfd.db**</span>   **.db 结尾的directory**

- ```shell
  show databases like '*clsfd*';
  ###查看所有有关clsfd db 的数据库
  ```

  ```shell
  use dw_clsfd;
  show tables;  ###展示所有tables
  show tables like '*reply*'  ###展示跟reply有关的
  ```

  

- 标准建表格式：

  --> 通过指令： show  create table tbl_name :查看ddl 语句

  --> 跟 sql server : sp_helptxt tbl_name /alt+f1

  ```sql
  CREATE [EXTERNAL] TABLE [IF NOT EXISTS] table_name
      [(col_name data_type [COMMENT col_comment], ...)]
      [COMMENT table_comment]
      [PARTITIONED BY (col_name data_type [COMMENT col_comment], ...)]
      /*分区表*/
      [CLUSTERED BY (col_name, col_name, ...)
      /*分桶表*/
      [SORTED BY (col_name [ASC|DESC], ...)] INTO num_buckets BUCKETS]
      [ROW FORMAT row_format] /*或者[ROW FORMAT DELIMITED]*/
      /*告知文件每一行有分隔符*/
      [FIELDS TERMINATED BY ',|\t|;|01|02']
      [COLLECTION ITEMS TERMINATED BY '2']
      [MAP KEYS TERMINATED BY '3']
      /*字段间是以，制表符，分号etc 形式分割*/
      /*其中01 02 对应的是ascii or 八进制  http://tool.oschina.net/commons?type=4   https://blog.csdn.net/yelangjueqi/article/details/52210290  对照表*/
      
      [STORED AS file_format]  /*指定文件存储格式   TextFile ,ORC, SequenceFile ,Parquet;  */
      [LOCATION hdfs_path]  /*指定文件存储路径 */
  ```

  -->拓展 ：

  <span style='background-color:lightblue'>**创建临时表 **</span>    -->  **并没有临时视图 之说**

  ```sql
  create temporary table wenluo_test_23 (
  clsfd_site_id           int         comment 'int',
  clsfd_pltfrm_id         int                   comment ' int',
  clsfd_dvic_id           int                    comment ' int',
  metric_value            bigint                 comment ' bigint',
  clsfd_sum_dt            date                    comment 'date'
  ) ;  ---关键字 temporary
  insert into wenluo_test_23
  select  * 
  from wenbluo_test_22;
  
  ```

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_40.png)

  1. <span style='background-color:lightblue'>**表最终存放到tmp目录下**</span>

  2. 当退出会话时候，table也跟着消失，类似sql server :#t1 临时表一样

     ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_43.png)

- create temporary view ,创建临时视图
  
  1. 在 hql 并没有临时视图，但是spark-sql 有！等价于 create temporary table .
  
     ```sql
     CREATE TEMPORARY VIEW VT_FULL_END_AD_LIST AS
     SELECT
     AD.CLSFD_AD_ID
     ,AD.CLSFD_SITE_ID
     ,hst.AD_STATUS_START_DATE AS SRC_END_DT /*USE AD EXPIRED DATE FOR POPULATION*/
     ,AD.SRC_CRE_DT 
     FROM  VT_ORGANIC_ADS AD
     INNER JOIN ${clsfdDB}.CLSFD_AD_STS_CHNG_HST HST
     ON ad.CLSFD_AD_ID = hst.CLSFD_AD_ID
     AND hst.AD_STATUS_END_DATE ='2099-12-31' /*Get the last ad status record*/
     AND (hst.AD_STATUS_ID IN (2,3,12))  /*Get  ad final status that  is expired, user_deleted, closed ads. exclude live ads*/
     WHERE  HST.CLSFD_SITE_ID IN  (${clsfd_site_id});
     
     CACHE TABLE VT_FULL_END_AD_LIST; 
     
     ---cache table 操作才是真正的缓存数据！！
     
     ```
  
     
  
- <span style='background-color:yellow'>**show tblproperties  dw_clsfd.wenbluo_test_10;**</span>
  
  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_55、.png)
  
  1. **最后一个modified , 以及修改时间**
  
- ```
  
  create table DW_CLSFD.VST_MOTOR_DAILY_NEW_2019_05_10
  ( clsfd_site_id           int  comment 'int ',
  clsfd_sum_dt            string comment 'string',
  clsfd_pltfrm_id         int  comment 'int',
  clsfd_dvic_id           int  comment 'int',
  metric_value            bigint  comment 'bigint') 
  partitioned by (clsfd_sum_dt string )
  LOCATION 'hdfs://ares-lvs-nn-ha/EDW_CLSFD/GA/VST_MOTOR_DAILY_NEW_2019_05_10/'
  ```

  

  

  ##此时motor_daily_new 为内部表（<span style='background-color:lightblue'>**managed_table:控制表**</span>)

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_85.png)

  文件存储方式为 <span style='background-color:lightblue'>**二进制 ： serialization** </span>

  

   <span style='background-color:lightgreen'>**拓展一**</span>:

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_10.png)
  
  建立分区表的时候， <span style='background-color:lightblue'>**分区字段不能显示存在建表语句中**</span>。分区字段，<span style='background-color:lightblue'>**会自动的添加到表中**</span>；
  

所以正确的见表语句
     ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_11.png)

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_15.png)

  

  - <span style='background-color:orange'>**external table and maganed table** </span>
  
    ```shell
    hive> create table wenbluo_test_4 like wenbluo_test_3 ;
    OK
    Time taken: 1.486 seconds
    hive> create external table wenbluo_test_5 like wenbluo_test_3;
    
    ```
  
    1. **都会在hdfs 下创建一个directory** ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_16.png)
    
    2. ```shell
       drop table wenbluo_test_4;
       OK
       Time taken: 1.197 seconds
       
       ```
    
     drop table wenbluo_test_5;
       OK
 Time taken: 0.169 seconds

   ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_17.png)
    
   当drop table 时候，external table 仅仅是删除metadata ,<span style='background-color:lightgreen'>**不会删除目录wenbluo_test_5 以及目录下的子文件**</span>；
    
   然后managed table <span style='background-color:lightgreen'>**则会删除metadata and dfs** </span>
    
```shell
 ---> schema 与 data 分离；
    
     wenbluo_test_5 ：先有文件 ；
    
     因此我们可以建立一个external table 【schema 】 去访问数据

  create external table wenbluo_test_30 like wenbluo_test_10 location 'hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_location';
   set hive.execution.engine=tez;
   set hive.prewarm.enabled=true;
   set hive.prewarm.numcontainers=10;
   select count(1) from wenbluo_test_30;
   _c0
0
  ---不可以直接读取文件！
  ---因为 wenbluo_test_30  是一张分区表。
  ---需要msrepair table wenbluo_test_30;

```

​       ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_90.png)

<span style='background-color:lightblue'>**当不是分区表时候，是直接可以读取文件信息！**</span>

![](C:\Users\wenbluo\Desktop\wbluo\hive\p_91.png)

```shell
3.
   load data inpath  'hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_3' into table  wenbluo_test_5;
```

​       hive> dfs -count hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_3;
​                  1            0                  0 hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_3
​       可以看到 wenbluo_test_3 目录下的文件<span style='background-color:yellow'>**都被mv 到** </span>wenbluo_test_5 目录下； 
​     hive> dfs -count hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_5;
​                  1            4                328 hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test5
​                    


    4.

sql
create external table wenbluo_test_8 (
clsfd_site_id           int         comment 'int',
clsfd_pltfrm_id         int                   comment ' int',
clsfd_dvic_id           int                    comment ' int',
metric_value            bigint                 comment ' bigint',
clsfd_sum_dt            date                    comment 'date'
) partitioned  by (clsfd_partition_date date comment 'date' )
location 'hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_1';

此语句特点是 external table location  wenbluo_test_1; <span style='background-color:orange'>**因此并不会建立directory**</span> 





  #### QA ： 大小写(uppercase)

  create table DW_CLSFD.VST_MOTOR_DAILY_NEW_uppercase (
  clsfd_site_id           int         comment 'int',
  clsfd_pltfrm_id         int                   comment ' int',
  clsfd_dvic_id           int                    comment ' int',
  metric_value            bigint                 comment ' bigint',
  clsfd_sum_dt            date                    comment 'date'
)
  partitioned  by ( CLSFD_PARTITION_DATE    date  comment 'date')
  location    'hdfs://ares-lvs-nn-ha/EDW_CLSFD/GA/VST_MOTOR_DAILY_NEW_uppercase';

  


此时文件路径是  'hdfs://ares-lvs-nn-ha/EDW_CLSFD/GA/VST_MOTOR_DAILY_NEW_uppercase' 

![](C:\Users\wenbluo\Desktop\wbluo\hive\p_49.png)



1. <span style='background-color:lightblue'>**hdfs : 文件路径注重大小写！**</span> :<span style='background-color:yellow'>**table 不区分大小写**</span>,**自动转化为小写**

   ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_80.png)

  2. <span style='background-color:lightblue'>**table 中的fields  并不注重大小写**</span>

  3. 但是： <span style='background-color:yellow'>**路径区分大小写**</span>

     ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_82.png)

  4. <span style='background-color:lightblue'>**分区字段都是小写**</span> 或者<span style='background-color:yellow'>**被隐形转化为小写**</span>

  

  

##### bucket 分桶表

1. ```sql
   create table DW_CLSFD.wenbluo_test_22 (
   clsfd_site_id           int         comment 'int',
   clsfd_pltfrm_id         int                   comment ' int',
   clsfd_dvic_id           int                    comment ' int',
   metric_value            bigint                 comment ' bigint',
   clsfd_sum_dt            date                    comment 'date'
   ) 
   clustered by (clsfd_site_id) into 4 buckets ###通过对clsfd_site_id 进行hash 取mode 4 分到四个桶中  --dfs 则为四个文件
 row format delimited 
   fields terminated by ',';
   
    set hive.enforce.bucketing=T; ---设置参数使HIVE识别分桶；
    --- 目的初始化一个正确的reduce 个数
    
   ```
   
   ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_38.png)
   
    <span style='background-color:lightblue'>**因为分成四个桶，则为四个文件，所以最后需要四个reduce ,生成四个文件**</span>
   
    ![p_39](C:\Users\wenbluo\Desktop\wbluo\hive\p_39.png)

```shell

    dfs -cat hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_22/000001_0
       > ;
   6001,2,10,2,2016-01-01
   4001,1,30,287072,2016-01-01
   4001,4,30,129507,2016-01-01
   4001,3,30,118246,2016-01-01
   4001,1,20,189312,2016-01-01
   ###000001_0 文件特点都是余数为1 ；
   dfs -cat hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_22/000003_0;
   4007,10,20,11757,2016-01-01
   4007,1,20,144632,2016-01-01
   4007,1,10,142319,2016-01-01
   4007,1,30,56643,2016-01-01
   ###000003_0 文件特点都是余数为3 ；
```
  - create table xxx  location as select   
  
    create table DW_CLSFD.clsfd_motor_active_user_weekly_tot
    location 'hdfs://ares-lvs-nn-ha/EDW_CLSFD/GA/clsfd_motor_active_user_weekly_tot'
    as 
    select
    clsfd_sum_dt,
    clsfd_site_id,
    week_id,
    active_users ,
    from_unixtime(unix_timestamp()) as datachange_lasttime
    from  dw_clsfd.clsfd_motor_active_user_weekly   
    
    

**注意事项：**

![](C:\Users\wenbluo\Desktop\wbluo\hive\p_67.png)



**合并小文件处理**

**dfs.block.size**

![](C:\Users\wenbluo\Desktop\wbluo\hive\p_58.png)

![](C:\Users\wenbluo\Desktop\wbluo\hive\p_60.png)

```sql
map :360个(15gb =15*4个block ), 通过查看set dfs.block.size; #268435456 :256mb;


select clsfd_Sum_dt,platform,count(distinct decypted_user_id) as num
from clsfd_pi_ad_view_nl
where 1=1
group by 1 ,2;
```

![](C:\Users\wenbluo\Desktop\wbluo\hive\p_59.png)



map:1861个





##### Truncate

- 清空表数据： truncate table tmp_tbl

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_6.png)

  即使清空数据，但是不改变底层元数据（文件路径）， <span style='background-color:lightblue'>**directory依旧还在**</span>;

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_109.png)

  <span style='background-color:lightblue'>**跟windows 系统清空文件夹一样，并不是delete文件夹**</span>

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_120.png)

  -->即使清空table ,底层directory 依旧还在，所以show partitions  仍旧还是有分区；

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_121.png)

  通过msck repair  也无法删除该partition ，只能通过正式的 alter table drop partition ,更改schema -->更改metadata

- 劣势

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_122.png)

  <span style='background-color:gray'>**background : table wenbluo_test_25;含有91个目录or partition ,然后只有2016/09/01 之后才有数据。**</span>

  因为truncate table 并不会更改schema or metadata 只是删除hdfs . 因此namenode 数据没有影响。

  导致 select  * 仍旧会起 91个 map 去处理小文件。

  

- ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_107.png)

  <span style='background-color:lightblue'>**对于external table wenbluo_test_25,无效**</span>
  
  
  
  
  
  

#### Alter table 

- 修改表名

  alter table wenbluo_test_10 rename <span style='background-color:lightblue'>**to** </span>wenbluo_test_20;

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_75.png)

  <span style='background-color:lightblue'>**类似window一样，更改目录名字**</span>

  <span style='background-color:red'>**watch out!!!**</span>

  1. alter table 对 managed_table 可以更改路径

  2. alter table 对 external_table 无效！

     <span style='background-color:red'>**external table 就像 shell 中的 ln -s 软链，仅仅更换名称，并不会更改映射的位置**</span>
     
     ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_76.png)

- 新增字段

  alter table wenbluo_test_10 <span style='background-color:lightblue'>**add columns** </span>country int ;

  -->如果新增多个columns 呢？

  alter table bucket_test_wenbluo_test add columns (test_column  int) ;

  alter table bucket_test_wenbluo_test add columns  (test_column1 int ,test_column2  int );

- 删除字段

  alter table wenbluo_test_10 drop country; -->error  <span style='background-color:lightblue'>**没有drop column 关键字**</span>

  只能通过下面方法：  <span style='background-color:yellow'>**replace columns**</span>

  ```sql
   ALTER TABLE emp REPLACE COLUMNS( name string, dept string);
  ```

- 修改字段名称

  alter table wenbluo_test_10 <span style='background-color:lightblue'>**change column** </span>country state int 

- 修改字段类型

  alter table wenbluo_test_10  <span style='background-color:lightblue'>**change  column** </span>state state int 

- 调整字段位置

  alter table wenbluo_test_10 <span style='background-color:lightblue'>**change column**</span> state <span style='background-color:lightblue'>**after|First**</span> zone ; 

- 新增分区

  alter table wenbluo_test_10  <span style='background-color:lightblue'> **add** **partition** </span>(clsfd_sum_dt='2019-01-01')  location '..../....'

- 删除分区

  alter table wenbluo_test_10 <span style='background-color:lightblue'>**drop partition** </span>(clsfd_sum_dt='2019-01-01')

- 更改路径

  alter table wenbluo_test_10 partition (clsfd_sum_dt='2019-01-01' ) <span style='background-color:lightblue'>**set location** '..../new_clsfd_sum_dt'</span>

- 更改表属性

  ```sql
  alter table table_name set TBLPROPERTIES ('EXTERNAL'='TRUE');  //内部表转外部表 
  alter table table_name set TBLPROPERTIES ('EXTERNAL'='FALSE');  //外部表转内部表
  ---IS UPPERCASE:都要大写！
  ```

- 存储类型

  1. bucket table 

     ```
     alter table bucket_test_wenbluo
     cluster by 
     into 32 buckets;
     ```

     

- 

### hive 导入数据 &导出数据

- load

  load data local inpath "xxxx" into table log_text;  ---xxx表示文件路径；

  ```shell
  load data local inpath '/export/home/b_clsfd/00967_0.txt' into table wenbluo_test_1;
  Loading data to table dw_clsfd.wenbluo_test_1
  
  ###本地文件被 cp 到hadoop  中；
  ```

  load date inpath '/user/student.txt' overwrite into table log_text;

  overwrite:是重新覆盖数据；into：插入数据

  <span style='color:orange'>注意此时student.txt不在user文件夹下，而是移动了文件位置到log_text目录下</span>

  ```shell
load data inpath  'hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_05_27/clsfd_partition_date=2016-09-29' into table  dw_clsfd.wenbluo_test_05_27 partition(clsfd_partition_date='2016-09-29');
  ```

  
  
- location   --一般采用外部表时候

  create external table student4 like student  

  location '/user/atguigu';  --指定文件路径；

  好处：无需迁移文件夹

  ##### QA

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_96.png)

  1. 为什么没有分区？ wenbluo_test_9;

     **msck table wenbluo_test_9**

     ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_97.png)

     -->没有分区信息！

  2. msck repair table wenbluo_test_9;

     ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_98.png)

  3. 最后show partitions wenbluo_test_9; 有分区数据！

     

- create table XXX as select * from tbl

- CREATE TABLE DW_CLSFD.VST_MOTOR_DAILY_NEW_2019_05_09
  LOCATION 'hdfs://ares-lvs-nn-ha/EDW_CLSFD/GA/VST_MOTOR_DAILY_NEW_2019_05_09/'

  ##建表指定文件路径

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_69.png)
  
  ```sql
  create table DW_CLSFD.VST_MOTOR_DAILY_NEW_2019_07_24 like dw_clsfd.wenbluo_test_10 --external table
  ---
  ---Location:               hdfs://ares-lvs-nn-ha/EDW_CLSFD/GA/VST_MOTOR_DAILY_NEW_2019_05_10
  
  ```
  
  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_767png.png)
  
  <span style='background-color:lightblue'>**所以 create table like 主要copy fileds & partitions &store type** </span>  并不会<span style='background-color:yellow'>**copy table propertites**</span>
  
  ```sql
  create external table  dw_clsfd.external_table_like_bulid like DW_CLSFD.VST_MOTOR_DAILY_NEW_2019_07_24 ;
  OK   ---创建外部表
  Time taken: 1.228 seconds
  ```
  
  
  
  ```sql
  select count(1) from DW_CLSFD.VST_MOTOR_DAILY_NEW_2019_07_24;
  _c0
  0
  --仅仅是创建tabel schema 还没有将数据load 进去
  
  insert into VST_MOTOR_DAILY_NEW_2019_07_24  partition (clsfd_partition_date)   select * from wenbluo_test_10;
  --通过一次MR 将数据load 进去；
  
  --Location:               hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/vst_motor_daily_new_2019_07_24
  ```
  

create table  `clsfd_working.clsfd_ad_cube_daily`  like  `clsfd_working.CLSFD_AD_CUBE_DAILY_MIGRATE`  location 'viewfs://apollo-rno/sys/edw/working/clsfd/clsfd_working/clsfd/clsfd_ad_cube_daily';

<span style='background-color:lightblue'>**本以为clsfd_ad_cube_daily ,跟migrate table 一样，会是external table ，结果是内部表(managed table !)**</span>;

- 动态分区
  
  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_84.png)
  
  
  
  在 <span style='background-color:lightblue'>**hive-site.xml 文件**</span>会默认给分区命名：__HIVE_DEFAULT_PARTITION__
  
  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_114.png)
  
  
  
  -->拓展：
  
  - 什么情况下会是<span style='background-color:lightgreen'>**_ _hive_default_ _partition _ _**</span>
  
    正如蓝色框，NULL 以及 empy 以及 不可转义的字符，都会转化为--default--
  
    ```sql
    set hive.exec.dynamic.partition=true;
    set hive.exec.dynamic.partition.mode=nonstrict;
    insert overwrite   table dw_clsfd.wenbluo_test_40 partition(clsfd_site_id)
    select a.clsfd_sum_dt,
    a.clsfd_dvic_id,
    b.clsfd_pltfrm_id,
    sum(b.metric_value)as metric_value ,
    b.clsfd_site_id
    from dw_clsfd.wenbluo_test_15 as a  left  join dw_clsfd.wenbluo_test_10 as b on a.clsfd_site_id=b.clsfd_site_id and b.clsfd_site_id=6001
    where  a.clsfd_partition_date>='2019-08-19'
    and a.clsfd_site_id=4001
    group by 1,2,3,5;
    
    ---所以这张wenbluo_test_40 的partition 都是__HIVE_DEFAULT_PARTITION__
    ```
  
    
  
  - 
  
  
  
  ```sql
  set hive.exec.dynamic.partition=True;
  set hive.exec.dynamic.partition.mode=nonstrict;
  ---两个要如影随形
  ---第一个是申明动态分区
  ---第二个是strict 模式下，一个静态分区确认的情况下，其他分区可以是动态
  INSERT OVERWRITE TABLE ${clsfdworkingDB}.CLSFD_AD_CUBE_DAILY_TMP PARTITION(clsfd_site_id,clsfd_sum_dt)
  SELECT
  CLSFD_PLTFRM_ID
  ,CLSFD_DVIC_ID
  ,CLSFD_CATEG_REF_ID
  ,CLSFD_GEO_REF_ID
  ,API_FLAG
  ,CLSFD_PROXY_ID
  ,LISTING_TYPE
  ,FREE_FLAG
  ,ASK_PRICE_LC
  ,NEW_ADS_TOTAL
  ,NEW_ADS_WITH_REPLY
  --,NULL AS new_ads_with_second_reply
  ,CONNECT_ADS_TOTAL
  ,CONNECT_ADS_IN_24H
  ,clsfd_site_id
  ,clsfd_sum_dt
  FROM ${clsfdworkingDB}.CLSFD_AD_CUBE_DAILY_W
  WHERE clsfd_site_id IN (${clsfd_site_id})
  ```
  
  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_85.png)
  
  - **即使是写明了dynamic.partition; 仍要指定clsfd_partition_date** 
  - 

#### Insert overwrite/ into table 

create table wenbluo_test_14 (

clsfd_pltfrm_id         int                   comment ' int',
clsfd_dvic_id           int                    comment ' int',
metric_value            bigint                 comment ' bigint',
clsfd_sum_dt            date                    comment 'date'
) partitioned by (clsfd_site_id int comment 'int',clsfd_partition_date    date  comment 'date');



insert into table  wenbluo_test_14 partition (clsfd_partition_date,clsfd_site_id )
select clsfd_pltfrm_Id,
clsfd_dvic_id,
metric_value,
clsfd_sum_dt,
clsfd_partition_date,
clsfd_site_id
from  VST_MOTOR_DAILY_NEW_2019_05_10 where clsfd_partition_date ='2016-01-01'



![](C:\Users\wenbluo\Desktop\wbluo\hive\p_23.png)

###<span style='color:red'>**错误分区！**</span>  分区字段位置不能改变!



create table wenbluo_test_13 (

clsfd_pltfrm_id         int                   comment ' int',
clsfd_dvic_id           int                    comment ' int',
metric_value            bigint                 comment ' bigint',
clsfd_sum_dt            date                    comment 'date'
) partitioned by (clsfd_site_id int comment 'int',clsfd_partition_date    date  comment 'date');



insert overwrite  table  wenbluo_test_13 partition (clsfd_site_id ,clsfd_partition_date)
select clsfd_pltfrm_Id,
clsfd_dvic_id,
metric_value,
clsfd_sum_dt,
clsfd_site_id,
clsfd_partition_date
from  VST_MOTOR_DAILY_NEW_2019_05_10 where clsfd_partition_date >='2016-01-01'
and clsfd_partition_date<'2016-01-04'

![](C:\Users\wenbluo\Desktop\wbluo\hive\p_24.png)

```sql


insert into table wenbluo_test_15 partition(clsfd_site_id=4001, clsfd_partition_date='2019-08-19')
select 
clsfd_pltfrm_id,
clsfd_dvic_id,
metric_value,
clsfd_sum_dt
from wenbluo_test_13
where clsfd_site_id =4001
and clsfd_partition_date='2016-01-01';

---注意wenbluo_test_15 是二级分区： clsfd_site_id ,clsfd_partition_date 
---在 insert table  partition (clsfd_site_id =xx ,clsfd_partition_Date=xxx)
---写明固定分区后，在select 不用再写 clsfd_site_id ,clsfd_partition_date 
```

![](C:\Users\wenbluo\Desktop\wbluo\hive\p_93.png)

如果是<span style='background-color:lightblue'>**set  hive.exec.dynamic.partition=true; set hive.exec.dynamic.partition.mode=nonstrict;**</span>

则要写明clsfd_site_id, clsfd_partition_date;

hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_15![](C:\Users\wenbluo\Desktop\wbluo\hive\p_94.png)



总结： 

- insert into table  partition( col1,col2 ) 

  col1 ,col2 要跟建表语句的位置 一样，不能随意更改位置！

- select 语句，倒数第二个字段 必须是col1 ,最后一个字段是 col2

- <span style='color:red'>**通过insert into /overwrite  hdfs  文件备份 cp  ，而不是mv**  </span>

##### 拓展

1. ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_96.png)

   - 特点：hdfs 碰到.hive-staging 文件，并不会select 出来，

   - 执行insert into 语句

     ```sql 
     insert into table wenbluo_test_15 partition(clsfd_site_id=4001, clsfd_partition_date='2019-08-19')
     select 
     clsfd_pltfrm_id,
     clsfd_dvic_id,
     metric_value,
     clsfd_sum_dt
     from wenbluo_test_13
     where clsfd_site_id =4001
     and clsfd_partition_date>='2016-01-01';
     
     insert 操作指令时候，并不是往原始文件 00000_0 文件添加数据，而是生成新的copy 文件
     
     ```

   - 如果执行 insert overwrite 语句呢？

     ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_98.png)

     ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_102.png)
   
     ```SQL
     insert overwrite table wenbluo_test_15 partition(clsfd_site_id=4001, clsfd_partition_date='2019-08-19')
     select
     clsfd_pltfrm_id,
     clsfd_dvic_id,
     metric_value,
     clsfd_sum_dt
     from wenbluo_test_13
     where clsfd_site_id =4001
     and clsfd_partition_date>='2016-01-01';
     
     --可以看到有许多文件操作，会将底层文件 删除，再新增新的文件。
    --所以过多小文件，也会影响效率
     ```

     ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_99.png)
   
   - 如果有多个分区，insert overwrite 会影响到其他hdfs 文件吗？
   
     ```sql
     insert overwrite table wenbluo_test_15 partition(clsfd_site_id=4001, clsfd_partition_date='2019-08-20')
     select
     clsfd_pltfrm_id,
     clsfd_dvic_id,
     metric_value,
     clsfd_sum_dt
     from wenbluo_test_13
     where clsfd_site_id =4001
     and clsfd_partition_date>='2016-01-01';
     
     
     ```
   
     
   
   - 

#### Export data 

1. from hdfs to  hdfs 

   ```sql
   insert overwrite directory 'hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test/tmp_data'
   select year(clsfd_suM_dt) as year, month(clsfd_sum_dt) as month ,
   count(1)as num 
   from  wenbluo_test_10
   group by 1,2
   order by year, month ;
   
   ---key word : insert overwrite directory
   
   insert overwrite directory './wenbluo_test/tmp_data'
   ---'hdfs://ares-lvs-nn-ha/user/b_clsfd/wenbluo_test/tmp_data'
   select year(clsfd_suM_dt) as year, month(clsfd_sum_dt) as month ,
   count(1)as num 
   from  wenbluo_test_10
   group by 1,2
   order by year, month ;
   
   
   ```

   ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_78.png)

2. from hdfs to local directory 

   - ```shell
      hive -e  'select year(clsfd_suM_dt) as year, month(clsfd_sum_dt) as month ,
     count(1)as num
     from  dw_clsfd.wenbluo_test_10
     group by 1,2
     order by year, month ;' >./wenbluo_tmp$$;
     
     hive -e //hive -f >  
     
     ```

   - ```sql
     insert overwrite local directory './wenbluo_test/tmp_data'
     select year(clsfd_suM_dt) as year, month(clsfd_sum_dt) as month ,
     count(1)as num 
     from  dw_clsfd.wenbluo_test_10
     group by 1,2
     order by year, month ;
     
     --key word  local directory
     
     insert overwrite local directory './wenbluo_test/tmp_data'
     row format delimited fields terminated by '\t'
     select year(clsfd_suM_dt) as year, month(clsfd_sum_dt) as month ,
     count(1)as num 
     from  wenbluo_test_10
     group by 1,2
     order by year, month ;
     ---指定文件分隔符
     ```

     

3. hadoop fs -get/-getmerge  

#### Tunning

```sql
insert into table sales 
select * from history where action='purchased'
insert into table credits 
select * from history where action ='Returned';

---前者histoty 被遍历两次


from history 
insert into sales where action ='purchased'
insert into credits where action ='Returned';

---history 只被遍历一次！


```




#### QA

1. ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_2.png)

   当插入clsfd_sum_dt>='2019-01-01'

   <span style='background-color:orange'>**如何自动每天分区？**</span>

   ```sql 
   set hive.exec.dynamic.partition=true;
   set hive.exec.dynamic.partition.mode=nonstrict;
   
   insert overwrite table DW_CLSFD.VST_MOTOR_DAILY_TOTAL_NEW
   partition(clsfd_partition_date)  
   SELECT
   clsfd_site_id,
   clsfd_pltfrm_id,
   clsfd_dvic_id,
   metric_value,
   clsfd_sum_dt,
   clsfd_sum_dt as clsfd_partition_date
   from DW_CLSFD.VST_MOTOR_DAILY_NEW
   ;
   ```

   ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_3.png)

2. 

3. 

### hive分析窗体函数

满足lead,lag,sum,max,row_number(),ntile(),first_value,last_value等等

lead(rate,2)over(partiton by id ,department order by lastname):从下往上挪动2位

sum(rate)over(partition by id ,department order by lastname rows between unbounded preceding and current row): 累积求和

first_value:最小值

olap上分析函数： grouping sets,grouping_id,cube ,rollup(向上钻取)：

![](C:\Users\Administrator\Desktop\携程文件夹\tmp\olap_2.png)

![](C:\Users\Administrator\Desktop\携程文件夹\tmp\olap_3.png)

![](C:\Users\Administrator\Desktop\携程文件夹\tmp\olap_4.png)

### Hive 统计信息

1. set hive.stats.autogather=True;

    stored into Hive MetaStore.

   ps ：<span style='color:red'>Statistics are not gathered for `LOAD DATA` statements.</span>

2. <a href='https://cwiki.apache.org/confluence/display/Hive/StatsDev#StatsDev-NewlyCreatedTables'>Analyze table </a>

3. 

-->analyze table wenbluo_test_5 compute statistics ;

###无分区

![](C:\Users\wenbluo\Desktop\wbluo\hive\p_25png.png)

-->analyze table wenbluo_test_10 partition(clsfd_partition_date='2016-01-01') compute statistics;

-->拓展：

#### 合并小文件

<a href='http://www.openkb.info/2014/12/how-to-control-file-numbers-of-hive.html'>map small files</a>

- background:

  Hive table contains files in HDFS, if one table or one partition has too many small files, the HiveQL performance may be impacted.
  Sometimes, it may take lots of time to prepare a MapReduce job before submitting it, <span style='color:red'>**since Hive needs to get the metadata from each file.**</span>

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_30.png)

  - set  hive.input.format =hive.input.format=org.apache.hadoop.hive.ql.io.HiveCombineFormat

    It will combine all files together and then try to split, so that it can improve the performance if the table has too many small files.
    One old default value is org.apache.hadoop.hive.ql.io.HiveInputFormat which will split each file separately. Eg, <span style='color:red'>**If you have 10 small files and each file only has 1 row, Hive may spawn 10 mappers to read the whole table.**</span>

  - ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_32.png)

    可以看到文件是450mb , 需要两个block 
  
    ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_117.png)
  
    >  select count(1)
  >     > from dw_clsfd.clsfd_ecg_hit
    >     > where clsfd_site_id=1011
  >     > and  ecg_session_start_dt='2016-01-01'
    >     > and ga_prfl_id=61944097

    ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_31.png)

    set dfs.block.size=268435456  (256mb)

    set mapreduce.input.fileinputformat.split.maxsize =256000000 (256mb)

    set  hive.exec.reducers.bytes.per.reducer=1000000000(1G)

    map个数 由max( split size, block.size)决定

    因此read 该文件需要两个map

    <span style='color:red'>所以需要一个reduce 就行</span>.

    由于set hive.map.aggr =true.

     --> reduce  mode：mergepartitial 

    ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_33.png)

    --> set hive.map.aggr=Flase

    --> reduce mode :complete

    ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_34.png)
  
  - <span style='background-color:lightblue'>**通过控制reduce 个数控制 file个数**</span>
  
    ```sql
    set mapred.reduce.tasks=2；  ---只用两个reduce
    insert overwrite table  wenluo_test_24 
    select a.clsfd_site_id
    ,a.clsfd_pltfrm_id
    ,a.clsfd_dvic_id
    ,sum(b.metric_value) as num
    ,a.clsfd_sum_dt 
    from vst_motor_daily_new_2019_05_10 as a  join wenbluo_test_10 as b on a.clsfd_site_id=b.clsfd_site_id
    and a.clsfd_pltfrm_id=b.clsfd_pltfrm_id
    and a.clsfd_dvic_id=b.clsfd_dvic_id
    and a.clsfd_sum_dt=b.clsfd_sum_dt
    where a.clsfd_partition_date>='2016-01-01'
  and a.clsfd_partition_date<'2016-03-01'
    group by 1,2,3,4
  
    ```
  
    ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_42.png)
  
    
    
    ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_124.png)
    
    

##### Map numbers

bg: the number of  mapper s determines the number of the intermediate files, and the number of mapper is determined by below 3 fators.

1. hive.input.format (hiveinputformat)
2. File split size  (mapreduce.input.fileinputformat.split.maxsize)
3. MapR-FS   (set dfs.block.size;)

###单个分区

![](C:\Users\wenbluo\Desktop\wbluo\hive\p_21.png)

![](C:\Users\wenbluo\Desktop\wbluo\hive\p_22.png)

--> analyze table wenbluo_test_13 partition(clsfd_site_id=4001 ,clsfd_partition_date) compute statistics;

####多个分区

![](C:\Users\wenbluo\Desktop\wbluo\hive\p_26png.png)

![](C:\Users\wenbluo\Desktop\wbluo\hive\p_27png.png)

--->analyze table wenbluo_test_13 partition(clsfd_site_id ,clsfd_partition_date) compute statistics;

###所有分区的统计信息



--->analyze table wenbluo_test_13 partition(clsfd_site_id=4001 ,clsfd_partition_date) compute statistics for columns clsfd_pltfrm_id,clsfd_divc_id;

###统计部分字段信息



- cfg

  1. set hive.compute.query.using.stats = true;
  2. set hive.stats.fetch.column.stats = true;
  3. set hive.stats.fetch.partition.stats = true;
  4. set hive.stats.deserialization.factor=1 ;


#### QA 小文件危害

- ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_44.png)

- 解决方案：

  1. purge unneeded small file

  2. merge small file 

     

#### Hive 压缩方式

<span style='background-color:lightblue'>**background:hadoop job 通常是I/O 密集型为不是CPU密集型**</span>

1. 查看hive /hadoop 支持的编码器(codec)

   <span style='background-color:lightblue'>**set io.compression.codecs**;</span>

   GzipCodec,DefaultCodec,LzoCodec,Bzip2Codec,SnappyCodec

   因此我们可以采用的压缩方式：

   - Gzip
   - Bzip
   - Snappy
   - Lzo

2. Bzip :压缩率最高，但是消耗CPU也最高

3. Gzip:是相对Bzip中庸的压缩方式

4. Snappy & Lzo :相比前两者压缩率低，但是压缩/解压速度块，特别是解压过程。

文件的可拆分性：

   由于mr 读取数据是先split  <span style='background-color:lightblue'>**（set dfs.block.size;）**</span> --> map -->reduce

-  Gzip & Snappy ：<span style='background-color:yellow'>**文件不可拆分**</span>
-  Bzip & Lzo :可拆分

##### 参数配置

1. 开启中间压缩

    set hive.exec.compress.intermediate=true

2. 开启输出压缩

   hive.exec.compress.output=true

   

##### 二进制压缩 sequencefile 

- set mapred.output.compression.type;  --在mapred-site.xml

  有三种方式：record .none. block,默认以block



#### hive 存储格式

**Textfile sequencefile  RCfile  ORC  Parquet** 

<span style='background-color:yellow'>**ps : 当文件格式 与 schema 格式不一样的时候，schema是无效的** </span>

![](C:\Users\wenbluo\Desktop\wbluo\hive\p_72.png)

 正确做法：<span style='background-color:lightblue'>**启动一次MR**</span>

**insert into wenbluo_test_23 select * from wenbluo_test_6;**

wenbluo_test_23：文件格式RCfile![](C:\Users\wenbluo\Desktop\wbluo\hive\p_73.png)



wenbluo_test_6：默认的文件格式textfile![](C:\Users\wenbluo\Desktop\wbluo\hive\p_74.png)

- 当没有指定stored as XXX 时候，

  默认方式为：<span style='background-color:lightblue'>**textfIle  文本格式**</span>

  ```shell
  STORED AS
    INPUTFORMAT 'org.apache.hadoop.mapred.TextInputFormat'
    OUTPUTFORMAT 'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
  ```

  create table wenbluo_test_23 like wenbluo_test_22 stored as textfile ;

  

- 行式存储和列式存储

  ![](C:\Users\Administrator\Desktop\携程文件夹\tmp\hql_15.png)

  其中textfile+二进制文件都是行存储； 系统默认都是textfile格式；

  

- ORC：（optimized Row Columnar)

  ![](C:\Users\Administrator\Desktop\携程文件夹\tmp\hql_16.png)

  思路：将一张表，分成n个stripe块（256mb)，然后stripe块里面行信息，采用列存储格式；

  存储格式：orc>parquet>textfile;

  ```shell
   create table wenbluo_test_6 like wenbluo_test_5 stored as orc ;
  OK
  Time taken: 0.457 seconds
  hive> load data inpath  'hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_5' into table  wenbluo_test_6;
  FAILED: SemanticException [Error 30019]: The file that you are trying to load does not match the file format of the destination table. Destination table is stored as ORC but the file being loaded is not a valid ORC file.
  
  ##txt 文本没法load 到orc table 中！
  
  ###通过 create table as selecet 形式 
  create table wenbluo_test_7 stored as orc as select * from wenbluo_test_5;
  Query ID = b_clsfd_20190529044439_1eaead3b-2d84-4801-b5c4-fc51d7590fbf
  Total jobs = 1  
  
  第二种形式：
  
  insert overwrite table log_orc  select * from log_text
  
  注意当文件存储格式并不是 textfile 时候一定要走一个map +reduce过程，
  
  不然文件格式依旧是textfile, 而不是orc格式
  
  ```

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_18.png)

  - orc  文件比较大 比 wenbluo_test_5 下txt 文本
  - wenbluo_test_7 下的dfs 文件命名默认以 00* 形式；





- use ods_fltorderdb; show tables; ---用于查看该数据仓库下有哪些表

- desc tablename :可以查看表结构 

  -->desc formatted tablename : 查看表结构以及表路径 <span style='background-color:lightblue'>**（比desc 指令更加明细）**</span>

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_20.png)

  -->desc extended tablename: 查看表结构以及表路径   ( 与desc formatted 几乎无区别)

  以及统计信息：

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_19.png)

  --> <span style='background-color:lightblue'>**Show create table** </span>

  1. show create table dw_clsfd.wenbluo_test_7;
  2. hive (dw_clsfd)> show create table wenbluo_test_view_7;

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_95.png)

  

  

  

- ![](C:\Users\Administrator\Desktop\携程文件夹\tmp\hql_3.png)

  可以看到hive 表的特点：hive表-->（hdfs目录）文件夹

  ​                                        hive数据-->hdfs许多小文件（没有专门制定数据存储格式），可以是txt (\t ，-等分隔符表示)，csv;

### Hive 数据类型

- 复杂数据类型
  
  ![](C:\Users\wenbluo\Desktop\wbluo\hive\pict_222.jpg)
  
  1. structure
     - 数据类型可以变换多端 ，类似R 语言 中的 list  
     - C 语句
     - address.street  
  2. Map
     - deductions['states']
  3. array
     - MAX(hit_cd[20]) AS hashed_user_id
  
  ```sql
  create table employees( 
  
       name string ,
  	 salary flaot,
  	 subordinates array<string>,
  	 deductions  map <string , flaot>,
  	 address  struct<street:string ,city:string ,status:string ,zip:int>)
  	 row format delimited   ---  一定要写在前面 
  	 fileds terminated by '\001'  --对应的字符是A  ： 实则： row format delimitedfileds terminated  by '^A'
  	                              -- 列字段 
  	 collection items terminated by '\002'  --对应的字符是B  实则：  row format delimitedcollection items terminated by ^B;
  	                                -- array数据类型之间
  	 map keys terminated by '\003' --对应的字符是C
  	                             ---map 数据类型之间 
  	 lines terminated by '\n' ---行与行之间采用换行符
  	 ;
  
  	 ---对于集合数据类型，定义table 时候 采用 <>
  ```
  
  
  
- 一般数据类型
  1. string
  2. data
  3. int bigint 
  
- Data implicit  convert   //  data priority 



### Hive数据模型

- 内部表(Managed_table )

  1. 跟数据库中的基本表一样，只是hive每个table都对应一个文件夹（目录），所有该表数据都存放到该目录中

  2. ![](C:\Users\Administrator\Desktop\携程文件夹\tmp\hql_5.png)

     ![](C:\Users\Administrator\Desktop\携程文件夹\tmp\hql_4.png)

     当create table 时候没有指定字段间间隔符时候，系统默认无；

     因此在创建t5时候 采用<span style='color:orange'>row format delimited ##表示行间隔符号是什么 ,fields terminated by ',' ##表示字段之间是采用,分隔符</span>

- 分区表(parition table )

  ​    类似关系数据库中的分区表/或者水平拆分表

  ​    create table partition_table (sid int ) partitioned by (gender string)  

  ​    insert into partition_table( gender='M') select * from sample_tbl where gender='M'

  ​    alter table partition_table add partition (gender='M') partition (gender='M')

  ​    alter table partition_table drop partition (gender='M')  ,partition (gender='F')

  ​    在表文件夹下：产生分区的小文件夹![](C:\Users\Administrator\Desktop\携程文件夹\tmp\hql_6.png)

  优点：通过查询WHERE指定减少io，从全表扫描转化为部分文件扫描就行。

- 二级分区表

  ![](C:\Users\Administrator\Desktop\携程文件夹\tmp\hql_12.png)

  目录特点：创建二级目录/文件夹

  ![](C:\Users\Administrator\Desktop\携程文件夹\tmp\hql_13.png)

- 外部表 (external table )

  外部表只是一个过程，加载数据和创建表同时完成，并不会移动到数据仓库目录中，仅仅是与外部数据建立了一个连接。

  create external table  external_student (sid int ,sname string ,age int) 

  row format delimited fields terminated by ','

  <span style='color:orange'>location '/input'</span>;   ---一定要指定文件夹

  这个external_student表并不会创建一个目录，而仅仅是调用input文件夹下的数据

- 视图

  最大优势：简化复杂查询，并不是物理表；

  ![](C:\Users\Administrator\Desktop\携程文件夹\tmp\hql_7.png)

  ![](C:\Users\Administrator\Desktop\携程文件夹\tmp\hql_8.png)

- bucket 桶表 --水平拆分

  利用hash值再模运算取余，将相同结果数据放到一张表中

  create  table  buckt_table (sid int ,sname string ,age int)

  clustered by (sname) into 5 buckets --通过对sname进行hash 分桶运算，并且分成五个桶；

  row format delimited fileds terminated by ',' 

  <span style='color:orange'>load data local path '/user/flt_ts/tmp.txt' overwrite table bucket_table </span>; ---错误写法

  

  正确书写：

  ​     set hive.enforce.bucketing=T; ---设置参数使HIVE识别分桶；

  ​     load data local path '/user/flt_ts/tmp.txt' overwrite table tmp_table;

  ​     insert overwrite table buckt_table  select * from tmp_table;

  特点

  1. 与分区表一样，在目录下会有多个小文件夹

     ![](C:\Users\Administrator\Desktop\携程文件夹\tmp\bucket1.png)

  2. 一般用于抽样取数：分层抽样； ![](C:\Users\Administrator\Desktop\携程文件夹\tmp\bucket2.png)

  3. 分区表后又分桶：--在hive分区表中，分区中的数据量过于庞大时，建议使用桶。

     ![](C:\Users\Administrator\Desktop\携程文件夹\tmp\dw_5.png)

     

     参考文献：

     1. <a href='https://www.cnblogs.com/kouryoushine/p/7809299.html'> 分桶表</a>

排序函数



- | 排序函数      | map  | reduce | 描述                                                         |
  | ------------- | ---- | ------ | ------------------------------------------------------------ |
  | order by      | ×    | √      | 全局排序,最后放到一个reduce中                                |
  | sort  by      | ×    | √      | 局部排序,每个reduce阶段数据排序                              |
  | distribute by | √    | ×      | 制某个特定字段分区然后分配到reducer，通常是为了进行后续的聚集操作 |
  | cluster by    | ×    | √      | =distribute by+sort by  ：当排序为相同字段时候；无法指定排序； |

  ![](C:\Users\Administrator\Desktop\携程文件夹\tmp\distribute_by.png)

###其他常用函数

1. 空字段赋值： NVL(string1,replace_with) : 类似coalesce
2. 



###  streamtable  mapjoin，表的控制顺序

- SELECT /*+ STREAMTABLE(a) */ a.val, b.val, c.val FROM a JOIN b ON (a.key = b.key1) JOIN c ON (c.key = b.key1)

- select  /*+ MAPJOIN(b) */  * from smalltable as a join bigtable as b on a.id=b.id 

  <span style='color:orange'>set hive.auto.convert.join=true </span>;---系统默认会设置mapjoin;

- **从左至右读取表关联顺序**

### Map+reduce

- 分而治之的思想，一个大任务拆分成多个小任务（map),并执行后，合并结果（reduce)
- 

#### Reduce

哪些会在reduce端执行：

1. join

   ![](C:\Users\Administrator\Desktop\携程文件夹\tmp\reduce_1.png)

2. order by 

3. count(distinct id)

   ​    替换成： select count(1) from (select id from tbl  group by id ) as a 

   ​    目的：新增reduce个数，将一个reduce（count distinct) 任务分散给  多个reduce 去读取id ,最后用多个reduce去重

   ​    虽然时间会长一些，但是总比一个reduce导致任务跑不出来强一些；

4. group by 

---> <span style='background-color:lightblue'>**特点： 有多少**个reduce 就有多少个file ！</span>

```shell
hive (dw_clsfd)> set mapred.reduce.tasks;
mapred.reduce.tasks=1   ###控制一个reduce 操作
hive (dw_clsfd)> insert into wenbluo_control_reduce_tasks
               > select * from wenbluo_test_10;
               
               
```

![](C:\Users\wenbluo\Desktop\wbluo\hive\p_56.png)

####Map



### 数据倾斜：（不患寡而患不均）

1. reduce节点大部分执行完毕，但是<span style='color:orange'>有一个或者几个reduce节点运行很慢</span>（任务进度长时间维持在99%（或100%）），导致整个程序的处理时间很长，这是因为某一个key的条数比其他key多很多（有时是百倍或者千倍之多）。 ---Key键分布不均匀

2. SQL语句本身就有数据倾斜

3. 不同数据类型（UDFtoInt)

4. 常见的调优方式：

   解决MapReduce数据倾斜思路有两类：

   - 一是reduce 端的隐患在 map 端就解决

     set hive.map.aggr=T; 

   - 二是对 key 的操作，以减缓reduce 的压力

     set hive.groupby.skewindata=T;

     select count(distinct id) -->group by 替换，将一个reduce任务，分散给多个reduce来操作；

     

   - 采用mapjoin 

     1. <span  style='color:orange'>Hive join原理</span>，可以分为common join （Reduce阶段完成join) 和Map join (Map 阶段完成join)

        Commoin join阶

        common join :

        ![](C:\Users\Administrator\Desktop\携程文件夹\tmp\map_join_1.png)

        ![](C:\Users\Administrator\Desktop\携程文件夹\tmp\hql_9.png)

        Map join阶段

        是针对大表与小表关联情形。【针对2表关联时候】

        假设a表为一张大表（亿级别），b为小表，并且<span style='color:orange'>hive.auto.convert.join=true</span>,那么Hive在执行时候会自动转化为MapJoin。

        在map端join ;

        SELECT /*+ MAPJOIN(b) */ a.key, a.value
FROM a JOIN b ON a.key = b.key
        
        ![](C:\Users\Administrator\Desktop\携程文件夹\tmp\hql_10.png)

        首先在本地生成一个local task 读取比较小的表dept，然后将表写入Hash Table Files ，<span style='color:orange'>上传到HDFS的缓存中</span>，然后启动一个map作业，每读取一条数据，<span style='color:orange'>就与缓存中的小表进行join操作</span>，直至整个大表读取结束.

        <span style='color:orange'> 优势在于 减少了shuffle 和reduce 中间过程</span>

     2. Group by 聚合：

        - set hive.map.aggr=true:在map端聚合，减少在reduce阶段聚合的压力

          通过查看执行计划：--相当于combiner;

          可以看到reduce operator tree:

          mode:complete    ---当没有设置map端聚合时候

          mode:mergepatitial ---当设置map端聚合

        - set hive.groupby.skewindata=true;

          特点会生成两个mr工作；

        

     3. count(distinct )：

        - 特点:将所有数据分配到一个reduce上；导致一个reduce运行非常慢

        - 解决方案：采用group by 去重

          特点对于数据集很大的时候(内存过载，导致无法运行），将去重工作分配给多个reduce处理，虽然会多一个步骤，但是效率会很高；

          

        - select substr(day,1,4) year,count(*) PV,count(distinct cookieid) UV,count(distinct ip) IP,count(distinct userid) LOGIN 
     from dms.tracklog_5min a  
          where substr(day,1,4)='2015'
          group by substr(day,1,4)
     
          调整方案：

          ![](C:\Users\Administrator\Desktop\携程文件夹\tmp\hql_14.png)

     4. streamtable: 流式表

        在<span style='color:orange'>多表级联时</span>，一般大表都写在最后，因为写在最后的表默认使用stream形式加载，其他表的结果缓存在内存中。 
   可以使用**/\*+ STREAMTABLE(a) \*/** 来标示具体哪个表使用stream形式。在表关联时，使用该标识来指出大表，<span style='color:orange'>能避免数据表过大导致占用内存过多而产生的问题</span>。
     
        SELECT /*+ STREAMTABLE(a) */ a.val, b.val, c.val FROM a JOIN b ON (a.key = b.key1) JOIN c ON (c.key = b.key1)

     5. 合并小文件：

        - set hive.input.format=org.apache.hadoop.hive.ql.io.<span style='color:orange'>combineHiveInputFormat</span>;

          读取文件时候先合并小文件；

          因为小文件过多导致map 个数会很多。

        - 控制map 个数:

          控制map <span style='color:orange'>切片大小</span>：

          set hive.mapreduce.input.fileinputformat.split.maxsize=256000000;

          set dfs.block.size;

          控制一下maxsize与blocksize关系：

          当maxsize>blocksize:减少map个数

          当maxsize<blocksize:增加map个数

          

     6. 并行执行：

        set hive.exec.parallel;--查看是否开启并行运算

        set hive.exec.parallel.thread.number=8;--控制并行线程个数

     7. 压缩：

        存储格式：orc>textfile格式



### Tunning 

- map+reduce 

- big table & small table 

  1. hive假设查询中最后一个表是最大的表，从左至右。

     在对每行记录进行链接操作时候，它会尝试将其它表缓存下来。然后扫描最后一张tbl

  2. mapjoin or streamtable 

- 

### log

concept:

​     Hive既然是运行在hadoop上，最后被翻译成mapreduce程序，通过yarn来执行。所以我们如果想解决Hive中出现的错误，需要分成几个过程

- Hive自身翻译成为MR之前的解析错误
- Hadoop文件系统的错误
- YARN调度过程中的错误

#### **.staging  中间目录**

1. set hive.exec.stagingdir;

   hive.exec.stagingdir=.hive-staging

   ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_68.png)

2. ```shell
   The Hive staging directory is a temporary directory used during processing, and the end data will be copied to the final destination upon completion. This is to avoid writing data live on the destination folder and causing issues when other users are reading while data is being written at the same time.
   ```

3. 

#### container 

#### yarn





### HDFS Metadata

HDFS Architecture 

![](C:\Users\wenbluo\Desktop\wbluo\hive\p_119.png)



ref : <a href ='https://www.cnblogs.com/jackchen-Net/p/6506321.html'> tutorail_one</a> & <a href='https://www.cnblogs.com/shitouer/archive/2013/01/07/2837683.html'>tutorial_two</a>

单机模式

伪分布式模式

完全分布式模式

- Namenode and Datanode 

  - Namenode  (master 节点)

    1. namenode 是整个系统的管理节点

       - 文件与数据块(block),对应关系
       - ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_118.png)

    2. 两个文件：fsimage :元数据镜像

       ​                  edits:操作日志文件

       读取文件时候都存放到RAM,后续也会存放到磁盘当中

    3. 

  - Datanode (slave 节点)

    1. 提供真实的数据

    2. HDFS默认的dfs.block.size=268435456(256mb)

    3. replication:默认为dfs.replication=3;

       ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_116.png)

  - 

  

  

background :

**在hadoop 集群中，需要配置的文件主要包含四个，分别是core-site.xml 、hdfs-site.xml、mapred-site.xml 和yarn-site.xml**   -->  <a href='https://zhuanlan.zhihu.com/p/25472769'> click me </a>

/apache/hadoop/conf

- set dfs.block.size=268435456 (256mb)  -->hdfs-site.xml

  -->hadoop 最小存储单元是块 **block**

- set dfs.replication:3  -->hdfs-site.xml 

### HDFS

- https://blog.csdn.net/puqutogether/article/details/41677725

- ```sql
  
  insert overwrite table dw_clsfd.clsfd_motor_active_user_weekly_tot
  select  clsfd_sum_dt,
  clsfd_site_id,
  week_id,
  active_users,
  datachange_lasttime
  from (
  select clsfd_sum_dt,
  clsfd_site_id,
  week_id,
  active_users,
  datachange_lasttime,
  row_number()over(partition by clsfd_site_id ,clsfd_sum_dt order by datachange_lasttime desc )as rk 
  from  dw_clsfd.clsfd_motor_active_user_weekly_tot
  )as a 
  where a.rk=1
  ```

  为什么在zeta 跑文件最终是：![](C:\Users\wenbluo\Desktop\wbluo\hive\p_125.png)

  在unix /shell cli 跑最终文件是：

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_126.png)

  

### Hive  Config File

/apache/hive/conf   <span style='color:red'>**-->通过$PATH可以查看路径**</span>

-->如何查看hive 版本：

/ebay/apache/hive/lib  查看jar包类型.

hive-exec-1.2.1000.2.4.2.57-1.jar

###说明版本是1.2

ps :<span style='color:red'>目前没有提供 hive version :指令功能</span>

![](C:\Users\wenbluo\Desktop\wbluo\hive\p_28.png)



#### .hiverc

background: 当cli 启动的时候，在提示符 出现之前，<span style='background-color:lightblue'>**先会执行这个文件。**</span>

--> <span style='background-color:lightblue'> **有点类似 bash  .bashrc or .profile** </span>

-->启动cli 时候，**系统会先读hive-site.xml 配置文件**，再读取.hiverc 文件，**设置相同变量赋值会被后者覆盖；**

文件路径：

- $HOME/.hiverc  --> <span style='background-color:lightblue'>**针对current user**</span>
- /etc/hive/conf   --> <span style='background-color:lightblue'>**针对所有user** </span>

<span style='background-color:yellow'>**通过set ;查看配置信息**</span>

[ARES]/export/home/b_clsfd ><span style='background-color:yellow'> **hive --service cli -v;**</span>

<span style='background-color:lightblue'>**可以查看 .hiverc 文件配置信息**</span>

![](C:\Users\wenbluo\Desktop\wbluo\hive\p_62.png)

1. <a href='C:\Users\wenbluo\Desktop\wbluo\hive\hive-site-xml'>hive-site.xml</a> <a href='https://blog.csdn.net/w13770269691/article/details/17232947'>中文详细解</a>

   - set hive.input.format:org.apache.hadoop.hive.ql.io.HiveInputFormat

     文件输入格式；

     One old default value is org.apache.hadoop.hive.ql.io.HiveInputFormat which will split each file separately. <span style='color:red'>**Eg, If you have 10 small files and each file only has 1 row, Hive may spawn 10 mappers to read the whole table.**</span>

     set hive.input.format=org.apache.hadoop.hive.ql.io.CombineHiveInputFormat;

     优势：读取文件之间，先合并小文件；

   - mapred.reduce.tasks ：-1

     Hive will automatically figure out what should be<span style='color:red'> **the number of reducers**</span>.

     hive.exec.reducers.bytes.per.reducer:1000000000 (1G)  :xx/1024/1024/1024

     

   - hive.mapred.mode:nonstrict

     The mode in which the hive operations are being performed. In strict mode, some risky queries are not allowed to run

     i.e :cartesian mode :笛卡儿积

   - hive.groupby.skewindata :False

     Whether there is skew in data to optimize group by queries

     ##aggregate function

   - hive.optimize.skewjoin:False

     Whether to enable skew join optimization

     hive.skewjoin.key:10000

     Determine if we get a skew key in join. If we see more
             than the specified number of rows with the same key in join operator,
             we think the key as a skew join key

     hive.skewjoin.mapjoin.min.split：33554432 (32MB)  : (1G 32个map)

     Determine minimum map split size 

     hive.skewjoin.mapjoin.map.tasks:10000

     Determine the number of map task in the follow up map join job for a skew join 

     

   - hive.map.aggr:True

   - <span style='background-color:orange'>**hive.optimize.cp**</span>:True  <span style='color:red'>#**做查询时只读取用到的列**</span>

     -->select * 并不影响

     -->与sql server 的区别

   - hive.join.cache.size :25000

     How <span style='color:red'>**many rows**</span> in the joining tables (except the streaming table) should be cached in memory

   - hive.exec.parallel :False

   - hive.merge.mapfiles ：True

     Merge small files at the end of a map-only job <span style='color:red'>#map end  合并小文件</span>

     hive.merge.mapredfiles:False

     Merge small files at the end of a map-reduce job <span style='color:red'>#map -reduce end 合并小文件</span>

     mapreduce.input.fileinputformat.split.maxsize   <span style='color:red'>**#split size to determine map number**</span>

     mapreduce.input.fileinputformat.split.minsieze

     For example, if one Hive table have one 1GB file, and the target split size is set to 100MB, 10 mappers MAY be spawned in this step.

     hive.merge.size.per.task :256000000 #256mb 

     Size of merged files at the end of the job  ：<span style='color:red'>#合并文件 when job end </span>

   - hive.exec.dynamic.partition:False   ###**动态分区 insert into tbl**

     hive.exec.dynamic.partition.mode :strict

     **in the strict mode,the user must specify at least one static partition** 

   - hive.execution.engine  ： MR/TEZ  

   - hive.stats.autogather:True <span style='color:red'>#自动搜集统计信息</span>

   - hive.merge.mapfiles:True

     Merge small files at the end of a map-only job

   - hive.optimize.countdistinct :False (H3.0)

   - hive.enforce.bucketing  <span style='color:red'>**## 分桶**</span>

   

2. set hive.cli.print.current.db=Ture;

   ```shell
   
   hive (dw_clsfd)> set hive.cli.print.current.db=TRUE;
   hive (dw_clsfd)> set hive.cli.print.current.db=false;
   hive> set hive.cli.print.current.db=true;
   hive (dw_clsfd)>
   
   ```

   

3. set hive.cli.print.header=True; 

   print out fields/column names;

4. 

### Hive Function

- desc function explode ;
- desc formatted table_name





### Hadoop CLI

-->如何查看hadoop 版本？

hadoop version

![](C:\Users\wenbluo\Desktop\wbluo\hive\p_29.png)

<a href='https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#cat'> **Tutorial** </a>

- -cat   ###输出文本

  ```shell
  hadoop dfs -cat hdfs://ares-lvs-nn-ha/EDW_CLSFD/GA/VST_MOTOR_DAILY_NEW_2019_05_10/clsfd_partition_date=2016-09-30/000984_0
  DEPRECATED: Use of this script to execute hdfs command is deprecated.
  Instead use the hdfs command for it.
  
  400111012165412016-09-30
  
  ```

- -count  ### 

  <p >Count the number of directories, files and bytes under the paths that match the specified file pattern</p>
The output columns with -count are: <span style='background-color:lightblue'>**DIR_COUNT, FILE_COUNT, CONTENT_SIZE FILE_NAME**</span>
  
```shell
  hadoop dfs -count hdfs://ares-lvs-nn-ha/EDW_CLSFD/GA/VST_MOTOR_DAILY_NEW_2019_05_10/clsfd_partition_date=2016-09-30
  1           34                930 hdfs://ares-lvs-nn-ha/EDW_CLSFD/GA/VST_MOTOR_DAILY_NEW_2019_05_10/clsfd_partition_date=2016-09-30
  ##一个目录，34个小文件，总共930bytes,file_name
  
  ##hadoop dfs -ls hdfs://ares-lvs-nn-ha/EDW_CLSFD/GA/VST_MOTOR_DAILY_NEW_2019_05_10/clsfd_partition_date=2016-09-30 | wc -l 
  
```

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_84.png)

- -cp  : copy src to destination 

   <span style='background-color:lightgreen'>**hdfs ->hdfs** </span>

  ```shell
  dfs -cp  hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_4/* hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_5;
  
  ###将wenbluo_test_4目录下的所有文件都cp  到wenbluo_test_5下；
  ###此时采用了特殊符号  *;
  
  dfs -cp  hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_4 hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_5;
  
  ##将目录wenbluo_test_4 copy 到wenbluo_test_5下；
  
  
  ```

- -discp

  
  
- -mkdir 

  ```shell
  hdfs dfs -mkdir hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_05_27/clsfd_partition_date=2001-01-01
  
  ```

- touchz   : <span style='background-color:lightblue'>**touch+zero**</span>

   ```shell
   dfs -touchz hdfs://ares-lvs-nn-ha/user/b_clsfd/wenbluo_test/touchz;
   ##create a file of zero length 
   ```

- -mv :迁移文件 from src to destination   <span style='background-color:lightblue'> **同样 -cp**</span>

   ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_100.png)

   文件重命名失败！

   <span style='background-color:lightblue'>**但是在bash shell竟然你可以执行，并且会覆盖原先的文件.... mv 也可以重命名；**</span>

   ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_101.png)

- -du -df

   1. -du: 查看文件大小  ：display usage

      ```shell
      hive (dw_cLsfd)> dfs -du -h    hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_15/;
      1.4 K  hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_15/clsfd_site_id=4001
      
      ```

   2. -df ：查看capacity ,free and  used space of the filesystem

      ```shell
      hive (dw_cLsfd)> dfs -df -h   hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/;
      Filesystem                Size     Used  Available  Use%
      hdfs://ares-lvs-nn-ha  194.0 P  126.7 P     67.0 P   65%
      
      ```

   3. 

- -rm :remove delete file 

   ```shell
   dfs -rm -R  hdfs://hercules/user/hive/warehouse/zeta_dev_clsfd_working.db/clsfd_ad_cube_daily_w_wenbluo/.hive-staging_hive_*
       > ;
   --模糊匹配，删除调.hive-staging 中间目录
   
   ```

- -test :answer various quesions about <path>,with result via exit status

   -d : is a directory ,return 0

   -e :exists 

   -f :is the file?

   -s: the file is not empty 

- -text  >> -cat

- -getfattr

   dfs -getfattr -d -R hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_location; <span style='background-color:lightgreen'>**快速查看文件**</span>　　-d: 表示目录

   ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_106.png)

   

   如果想看有多少文件？

   dfs -count -h -v xxxxx     :  -h : human type to look data size , and -v : to show the header 

   ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_108.png)

   

- -getmerge :copy hdfs  to local 

-  -createSnapshot

    “hdfs dfsadmin -allowSnapshot <path>” :启动镜像

   1. Create a snapshot on a directory  :快照，镜像(mirror)
   2. 防止数据误操作

   

- -getfacl :查看文件权限

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_92.png)

- -put  :

  <span style='background-color:lightblue'>**from local file system to hdfs** </span>  : i.e  **from unix system to hadoop file system**

  ```shell
  hadoop dfs -put   /export/home/b_clsfd/test_1.txt hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_05_27;
  
  ###copy  关系，本地和hadoop 都有test_1.txt  文件
  ```

- -get:将hadoop 文件导到unix本地

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_89.png)

  ```shell
  hadoop fs -get hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_2/clsfd_partition_date=2016-01-01/000967_0_copy_1 ./00967_0.txt;
  
   hadoop fs -get hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_location/000003_0  ./get_test/test;
   
   ##get: Destination exists and is not a directory: /home/b_clsfd/get_test  error ： 没有get_test目录
  
  ```

  <span style='background-color:yellow'>**这个时候可以采用getmerge :可以系统创建目录**</span>

  ```shell
  hadoop fs -getmerge hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_location/000003_0  ./wenbluo_test/test;
   
   
  ```

  --><span style='background-color:yellow'>**输出结果到从hdfs -->本地环境**</span>

  **方法一：sql 语句**

  ```sql
  insert overwrite   directory './wenbluo_test'
  ---'hdfs://ares-lvs-nn-ha/user/b_clsfd/wenbluo_test/tmp_data'
  select year(clsfd_sum_dt) as year ,month(clsfd_sum_dt) as month,count(1) as num
  from wenbluo_test_location
  group by 1,2
  order by year,month;
  
  ---详细请看hive 导出数据
  ```

  

- -discp

  
  
- -chmod

  ```shell
  dfs -chmod 755 hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_05_27;
  ```

- -stat : stat information   用于统计文件信息；

  ```shell
  dfs -stat  hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_05_27/clsfd_partition_date=2016-09-30/000984_0;
  2019-05-27 09:35:07   ###默认是最后修改时间
  
   hadoop fs -stat %b-%o-%r  hdfs://ares-lvs-nn-ha/user/hive/warehouse/dw_clsfd.db/wenbluo_test_05_27/test_1.txt
  24-268435456-3  ### byte:文件大小，o :block (每一个block 大小 256mb {268435456/1024^2} ) ,%r :副本个数 
  
  ###block.size,可以通过hdfs-site.xml 查看 set dfs.site.block;
  
  dfs -stat %b-%o-%r hdfs://ares-lvs-nn-ha/EDW_CLSFD/GA/clsfd_ECG_HIT/clsfd_site_id=1011/ecg_session_start_dt=2016-01-01/ga_prfl_id=69144097/adjust=0/000136_0;
  476547919-268435456-3
  
  ####可以看到000136_0已经超出block.size
  ```
  
  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_79.png)

  

- hadoop fs -ls /sys/edw/working/clsfd/clsfd_working/clsfd/clsfd_pi_traffic 

   ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_47.png)

   由于clsfd_pi_traffic 是多级分区表：

   clsfd_site_id ,clsfd_sum_dt

   hive  -e "desc clsfd_working.clsfd_pi_traffic";

   ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_48.png)

   

   <span style='background-color:lightblue'>**要展示所有文件**</span>

   hadoop  fs <span style='background-color:lightblue'>**-ls -R** </span> /sys/edw/working/clsfd/clsfd_working/clsfd/clsfd_pi_traffic   ： 大写的R

    

### Hive CLI

通过 hive --help ：查看有那些指令

![](C:\Users\wenbluo\Desktop\wbluo\hive\p_61.png)

一般主要通过cli 形式调用hive 程序。

<span style='background-color:yellow'>**hive --service cli --help** </span>

```shell
files.
usage: hive
 -d,--define <key=value>          Variable subsitution to apply to hive
                                  commands. e.g. -d A=B or --define A=B
    --database <databasename>     Specify the database to use
 -e <quoted-query-string>         SQL from command line
 -f <filename>                    SQL from files
 -H,--help                        Print help information
    --hiveconf <property=value>   Use value for given property
    --hivevar <key=value>         Variable subsitution to apply to hive
                                  commands. e.g. --hivevar A=B
 -i <filename>                    Initialization SQL file
 -S,--silent                      Silent mode in interactive shell
 -v,--verbose                     Verbose mode (echo executed SQL to the
                                  console)
```

hive --service cli -v





<span style='background-color:yellow'>**hive> dfs -help;   查看hadoop cli 指令**</span>

<span style='background-color:yellow'>**hive> set ;**</span>  : **查看所有环境变量**

```shell
hive (default)> set hive.cli.print.header;
set hive.cli.print.header
hive.cli.print.header=true
```



- hive -e

  ```shell
  ###无需进入到hive cli 模式
  
  ###执行sql 语句 ：execute
  hive (dw_clsfd)> select 268435456/power(1024,2);
  OK
  _c0
  256.0
  
  ###拓展
  [ARES]/export/home/b_clsfd > hive -S -e 'select 268435456/power(1024,2)';
   _c0
  256.0  ###输出结果更加简洁
  
  --省去time taken
  
  ```

  

- hive -f 

  ```shell
  ###执行sql 文件 ：file
  hive -f wenbluo_tmp_2019_05_10  ###无参数
  ```

  

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_5.png)

  ```shell
  ###执行sql 文件 ：file
  hive -f  motor_tmp_2019_05_10_tmp.hql  --hiveconf clsfd_start_dt='2016-01-01' --hiveconf clsfd_end_dt='2016-04-01';  ###参数
  ```

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_4.png)

  ##### QA

  1. 参数名字可以换吗？<span style='background-color:lightblue'>**传参  与 hive -d  定义变量的区别**</span>；

     <span style='background-color:lightgreen'>**不可以！ **</span>  固定参数 ： **hiveconf  hivevar**

     ```shell
     [ARES]/export/home/b_clsfd > cat  arg_test_tmp.hql
     select $start_time;
     [ARES]/export/home/b_clsfd > hive -f arg_test_tmp.hql --hiveconf start_time='2019-04-02';
     ParseException line 1:7 cannot recognize input near '$' 'start_time' '<EOF>' in select clause
     
     
     ####错误输入
     select {hiveconf:$start_time}
     
     ####正确输入：
     select ${hiveconf:start_time}
     
     ```

     --><a href='https://blog.csdn.net/dax1n/article/details/80822755'>拓展 hiveconf 与 hivevar difference </a>

     ```shell
     [ARES]/export/home/b_clsfd > hive --hiveconf first_name=wenbluo --hivevar last_name=robert   ###后面session 都可以应用该两个变量
     hive> select ${last_name};
     FAILED: SemanticException [Error 10004]: Line 1:7 Invalid table alias or column reference 'robert': (possible column names are: )
     hive> hive $last_name;
     FAILED: ParseException line 1:0 cannot recognize input near 'hive' '$' 'last_name'
     
     ###说明hivevar :调用参数的时候，可以不用写 hivevar
     
     hive> select ${hiveconf:first_name};
     FAILED: SemanticException [Error 10004]: Line 1:7 Invalid table alias or column reference 'wenbluo': (possible column names are: )
     hive> select ${first_name};
     FAILED: ParseException line 1:7 cannot recognize input near '$' '{' 'first_name' in select clause
     
     ###说明hiveconf:调用参数的时候，一定要写明hiveconf;
     ###总结： 调用参数 一定要有花括弧{}  ，$first_name error !;
     
     hive> exit;   ###退出session 
     [ARES]/export/home/b_clsfd > hive;  ###再开启新的session
     hive> select ${hiveconf:first_name};
     FAILED: ParseException line 1:7 cannot recognize input near '$' '{' 'hiveconf' in select clause   ###该参数不存在。说明是局部变量；
     
     
     
     ```

     

  2. 

- reset

  ```shell
  set mapreduce.job.reduces=10;
  hive> set mapreduce.job.reduces
      > ;
  mapreduce.job.reduces=10
  hive> reset;  ###重置环境变量
  hive> set mapreduce.job.reduces
      > ;
  mapreduce.job.reduces=-1
  
  --> set ; ###可以查看所有的变量配置环境属性
  ```

- dfs -ls 查看文件目录

  ```shell
  hive> dfs -ls xiaoxhuang/user_id;
  Found 1 items
  -rw-r--r--   3 b_clsfd hdmi-technology      94260 2017-03-14 23:26 xiaoxhuang/user_id/user_id.txt
  
  ###如何查看user_id.txt文件？
  
  ###采用dfs -text命令
  hive> dfs -text  xiaoxhuang/user_id/user_id.txt;
  
  ##hive> !ls;  查询当前linux文件夹下的文件
  ##hive> dfs -ls /; 查询当前hdfs文件系统下  '/'目录下的文件
  
  ```

  --> <span style='background-color:lightgreen'>**hadoop  fs -ls 指令**</span>

  ```shell
   hadoop fs -ls hdfs://ares-lvs-nn-ha/EDW_CLSFD/GA/clsfd_ECG_HIT/clsfd_site_id=2011/ecg_session_start_dt=2016-08-24/
  Found 3 items
  drwxr-xr-x   - b_clsfd hdmi-employees          0 2019-03-22 21:30 hdfs://ares-lvs-nn-ha/EDW_CLSFD/GA/clsfd_ECG_HIT/clsfd_site_id=2011/ecg_session_start_dt=2016-08-24/ga_prfl_id=104111715
  drwxr-xr-x   - b_clsfd hdmi-employees          0 2019-03-22 21:30 hdfs://ares-lvs-nn-ha/EDW_CLSFD/GA/clsfd_ECG_HIT/clsfd_site_id=2011/ecg_session_start_dt=2016-08-24/ga_prfl_id=78942672
  drwxr-xr-x   - b_clsfd hdmi-employees          0 2019-03-22 21:30 hdfs://ares-lvs-nn-ha/EDW_CLSFD/GA/clsfd_ECG_HIT/clsfd_site_id=2011/ecg_session_start_dt=2016-08-24/ga_prfl_id=78947209
  
  ```

  

- source 

  ```shell
  ###执行脚本文件
  hive> source wenbluo.hql;
  OK
  clsfd_site_id   clsfd_pltfrm_id clsfd_dvic_id   metric_value    clsfd_sum_dt
  9021    2       10      682     2016-01-01
  3001    1       20      75554   2016-01-01
  201     4       30      3991    2016-01-01
  9031    3       20      153373  2016-01-01
  5001    1       20      2219    2016-01-01
  8103    1       10      25770   2016-01-01
  3001    3       30      29580   2016-01-01
  3001    1       30      50959   2016-01-01
  1021    1       30      13341   2016-01-01
  9011    1       10      34958   2016-01-01
  Time taken: 1.884 seconds, Fetched: 10 row(s)
  
  ```

- hive -d 定义变量

  ```shell
  
  [ARES]/export/home/b_clsfd > hive -d db=dw_clsfd;  ##然后进入hive cli
  hive> select *
      > from ${db}.VST_MOTOR_DAILY_TOTAL_NEW ###直接在hive cli 中引用；
      > limit 10;
  OK
  vst_motor_daily_total_new.clsfd_site_id vst_motor_daily_total_new.clsfd_pltfrm_id       vst_motor_daily_total_new.clsfd_dvic_id        vst_motor_daily_total_new.metric_value  vst_motor_daily_total_new.clsfd_sum_dt  vst_motor_daily_total_new.clsfd_partition_date
  
  
  hive -d clsfd_site_id=3001 -d  clsfdworkingDB=clsfd_working;  
  
  ##定义多个参数 ，注意要有多个 -d
  ##根 hive -f xx   --hiveconf  start_dt --hiveconf end_dt一样
  
  hive -f  motor_tmp_2019_05_10_tmp.hql  --hiveconf clsfd_start_dt='2016-01-01' --hiveconf clsfd_end_dt='2016-04-01';  ###参数
  ```

- 

- hive shell 中使用shell 指令

  ```shell
  hive> ! pwd;
  /home/b_clsfd
  hive> ! cat wenbluo.hql;
  select clsfd_site_id,
  clsfd_pltfrm_id,
  clsfd_dvic_id,
  metric_value,
  clsfd_sum_dt
  from DW_CLSFD.VST_MOTOR_DAILY_TOTAL_NEW
  where 1=1 -- clsfd_sum_dt='2019-05-09'
  limit 10;
  hive>
  
  ###但是不支持 管道 功能！
  
  ```
  

  
- 

- 

  

### Hive msck 

- <span style='background-color:yellow'>**msck table tablename :检查metestore**</span>

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_97.png)

  **msck repair  table tablename**   <span style='background-color:orange'>**针对分区表**</span>

  hive 会检测hdfs 目录下存在但表的  metastore 不存在partition 元信息，更新到metastore 中；

- 问题：

  clsfd_working.clsfd_ad_cube_daily_migrate ,可以通过msck repair 指令，能更正映射关系吗？

  <span style='background-color:lightblue'>**因为现在 viewfs://apollo-rno/user/hive/warehouse/wenbluo/ad_cube  已经没有/tmp  目录**</span>

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_103.png)

  **可以看到msck repair table 并不能解决映射问题**
  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_104.png)

  --> 是否可以更改文件路径？

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_105.png)

  <span style='background-color:yellow'>  **it's well-done and feasible ! **</span>
  
- msck repair table : 仅仅更新metadata  与 hdfs 文件映射关系。

  1. metadata : 是logical stored, hdfs :是physical stored

  2. ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_111.png)

     

  3. 

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_110.png)

  1. background :

     可以看到metadata 有clsfd_partition_date=2016-09-29/2016-09-30 的逻辑分区，

     <span style='background-color:yellow'>**但底层物理并没有对应的文件！**</span>

     <span style='background-color:lightgreen'>**变更logical stored 并不更改 physical stored** </span>

     ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_112.png)

  2. 因此要做alter table drop partition 

     ```sql
     alter table VST_MOTOR_DAILY_NEW_2019_05_10  drop partition(clsfd_partition_date='2016-09-30');
     ```

     

  3. ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_113.png)

     summary  :结果2 &3 ,可以看到msck  指令是负责**(添加)**映射关系，并不能底层更改**(删除)**schema 

     schema :仅仅是告知有分区，但是具体文件有哪些，需要msck 告知
     
     

- partition table  vs  Non-Partition table 

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_115.png)

  - <span style='background-color:lightblue'>**对于没有分区表： 可以直接读取文件**</span>
  - 对于分区表：<span style='background-color:lightblue'>**需要msck repair table ，告知metadata** </span>有那些映射文件 

### Hive Enviroment Args

1. set hive.groupby.orderby.position.alias=TRUE;

   ```shell
   set hive.groupby.orderby.position.alias;
   hive.groupby.orderby.position.alias=True
   hive> set hive.groupby.orderby.position.alias=FALSE;
   hive> set hive.groupby.orderby.position.alias;
   hive.groupby.orderby.position.alias=FALSE
   
   ###是整个session 的配置，而不是单个sql 语句
   
   hive> set hive.groupby.orderby.position.alias=TRUE;
   hive> select clsfd_sum_dt,clsfd_pltfrm_id,count(1)
       > from dw_clsfd.vst_motor_daily_new
       > where clsfd_sum_dt>='2019-05-01'
       > group by 1,2;
   Query ID = b_clsfd_20190509234515_d806ebd9-5539-4925-baf9-7565489b96b1
   Total jobs = 1
   
   
   ####新的sql  语句依旧可以使用 position.alias ;
   hive> select clsfd_sum_dt,count(1)
       > from dw_clsfd.vst_motor_daily_total
       > where clsfd_sum_dt>='2019-05-01'
       > group by 1;
   Query ID = b_clsfd_20190509234831_118eda45-36ad-466a-8eb7-f164991a9983
   Total jobs = 1
   
   ```

2. ```shell
   hive> set mapreduce.map.memory.mb=9024;
   hive>  set mapreduce.map.memory.mb;
   mapreduce.map.memory.mb=9024
   
   hive> exit;  ###退出命令行时候；关闭session 
   [ARES]/export/home/b_clsfd >hive  ###开启新的hive session；
   hive> set mapreduce.map.memory.mb;
   mapreduce.map.memory.mb=2304   ###参数不再是9024；
   hive>  #
   
   
   
   ```


### Hive Engine

MR & TEZ & Spark

1.  set hive.execution.engine:MR 默认是以Mapreduce

   Chooses execution engine. Options are: `mr` (Map Reduce, default), `tez` ([Tez](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) execution, for Hadoop 2 only), or `spark` ([Spark](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Spark) execution, for Hive 1.1.0 onward) [1.1.0之后版本] .

2. <a href='https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez'>Hive on Tez</a>

   - ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_63.png)

     1. 可以看到执行tez 执行引擎后，total job =1 
     2. Tez: 在乌尔都语中含义是“快速”
     3. **<span style='background-color:lightblue'>Tez:自始至终都是专门为hive构建的执行引擎！</span>**
     4. 

     

   - ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_64.png)

     1. 系统默认的为mr 执行引擎 

     2. 传统的mr执行引擎，会将中间 过程(结果)写到hdfs ，这将导致磁盘的I/O性能的损失

        ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_65.png)

        两个临时目录：

        - .staging 
        - done_intermidate

   - ```SQL
     SELECT A.STATE,COUNT(1) AS NUM
     FROM A JOIN B ON A.ID=B.ID
     JOIN C ON A.ITEMID=C.ITEMID
     GROUP BY A.STATE
     ```

     ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_66.png)

     通过右边图形可以看到，TEZ执行引擎减少了HDFS的生成(I/O),**直接存放到内存当中**

   - 为了最大化Tez执行引擎的优势，设置配置

     | attributes                 | value | application        |
     | -------------------------- | ----- | ------------------ |
     | Hiveserver Heap size       | 16gb  | 默认值为1G         |
     | hive.prewarm.enable        | true  | 创建Tez container  |
     | hive.prewarm.numcontainers | **    | 调整container 个数 |
     | te                         |       |                    |
     |                            |       |                    |

     

3. <a href='https://cwiki.apache.org/confluence/display/Hive/Hive+on+Spark%3A+Getting+Started'>Hive on Spark</a>

4. 

#### tutorial

1. <a href=https://blog.csdn.net/levy_cui/article/details/51143181>Hadoop监控页面查看Hive的完整SQL</a>
2. 



### Hive explain

EXPLAIN [EXTENDED|CBO|AST|DEPENDENCY|AUTHORIZATION|LOCKS|VECTORIZATION|ANALYZE] query

<span style='background-color:lightblue'>-**->不区分大小写！**</span>

<a href='https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Explain'>LanguageManual+Explain</a>

- ```sql
  insert overwrite    table  wenbluo_test_21   partition (clsfd_partition_date='2019-06-10')
  select   clsfd_site_id  ,
  clsfd_pltfrm_id,
  clsfd_dvic_id  ,
  metric_value   ,
  clsfd_sum_dt   
  from wenbluo_test_10
  where 1=1
  and clsfd_partition_date='2016-01-01'
  ```

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_36.png)

  <span style='background-color:orange'>**会将中间结果（read table from wenbluo_test_10) 先存放到 wenbluo_test_21 clsfd_partition_date='2019-06-10'下**</span>.

- ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_37.png)

  stage-2 依赖于stage-1;

  <span style='background-color:orange'>**ps :set hive.exec.parallel=true; 提高并行度，不依赖的stage可以同时进行**</span>;

- ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_41.png)

  **`Sort order` indicates the number of columns in key expressions that are used for sorting**

  Each "`+`" represents **one column** sorted in ascending order, and each "`-`" represents a column sorted in descending order.  --><span style='background-color:lightblue'>**因为group  by src.key ,所以有一个+**</span>	

- explain extended 

- 

#### QA

1. 如何在cli 模式下，where 输入错误，没法子修改？![](C:\Users\wenbluo\Desktop\wbluo\hive\p_1.png)

2. 怎么查看文件？

   ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_7.png)

   ```shell
   [ARES]/export/home/b_clsfd > hadoop fs -cat  hdfs://ares-lvs-nn-ha/EDW_CLSFD/GA/VST_MOTOR_DAILY_NEW_2019_05_10/clsfd_partition_date=2016-09-30/000984_0
   400111012165412016-09-30
   
   ```




### Hue

- ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_9.png)

  hue ：竟然可以使用dfs -ls 

  ```sql
  SELECT
  INPUT__FILE__NAME,  ###竟然还有这种参数 ,两个下划线
  date_value AS clsfd_sum_dt
  , 4001 AS clsfd_site_id
  ,'Actual' AS Actual_target
  ,'App Email Replies' AS Metric_type
  --,SUM( metric_value01 ) Metric_value
  FROM mobile_de_metrics.mobile_de_metrics
  WHERE metrics_name IN('mails')
  AND dimension_value01 IN ('android','iphone','mob-iPhone','mob-android')
  AND granularity='daily'
  AND date_value BETWEEN '2019-05-19' AND '2019-05-26'
  --GROUP BY date_value,clsfd_site_id,Actual_target,Metric_type
  ;
  ```

  




### Hcatlog

### Reference

- <a href='https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC'>apache-hive wiki</a>
- 



### JVM

### Tips

#### Auto-Code

- 选择Tab ,会自动补全可能的关键字

  !![](C:\Users\wenbluo\Desktop\wbluo\hive\p_54、.png)

- show partitions wenbluo_test_10 ;

  show partitions wenbluo_test_10 partion (clsfd_sum_dt='2019-08-03');

- show databases;

  show databases like 'dw_clsfd'

  --> 假设已经在dw_clsfd ，如何切换到最上层？

  use default ;

- show tables ;

  show tables like 'wenbluo*';
  
- show databases  like 'clsfd_working'  : <span style='background-color:lightgreen'>**查看有哪些database & datamarket**</span>

#### Cli Help

- 查看有哪些函数
  show functions like '*date*'

  ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_87.png)

- 





### Odd

1. "Number of reduce tasks is set to 0 since there's no reduce operator": a problem?

   --仅仅只有map 

   your job will have the following steps: Split-->Map -> Shuffle -> Reduce

   The Map and Reduce steps are where computations (in Hive: projections, aggregations, filtering...) happen. **Shuffle is just data going on the network**, to go from the nodes that launched the mappers to the one that launch the reducers. **(datanode 之间的通信)**

   So if there is a possibility to do some "Map only" job and to avoid the "Shuffle" and "Reduce" steps, better: your job will be much faster in general and will involve less cluster resources (network, CPU, disk & memory).

   因此对于一些简单的sql ，可以只用到map 

   ```sql
   select * from myTable where daily_date='2015-12-29' limit 10;
   
   
   ```
   

### Markdown  test

- footnote

  [^footnote]:i like markdown 

  you can create footnotes like this [^footnote]

- 



### Math  Calculate 

- ```sql
  select 268435456/power(1024,2);
  OK
  _c0
  256.0
  
  
  ```

- 



### Full outer join 

![](C:\Users\wenbluo\Desktop\wbluo\hive\p_77.png)

特点：

1. 并不是以null 赋值
2. <span style='background-color:lightgreen'>**本质跟 catersian 很像，只是多了一个join 条件**</span>

<span style='background-color:lightblue'>**full outer join 采用  union all +group  by 替换**</span>![](C:\Users\wenbluo\Desktop\work\kylin cube\16.png)



### Recover file 

在core-site.xml 

fs.trash.interval=1440; :保存周期为1day : 24*60

![](C:\Users\wenbluo\Desktop\wbluo\hive\p_86.png)

dfs -ls -R hdfs://hercules/user/b_clsfd/.Trash/Current/;













