<a href=https://wiki.vip.corp.ebay.com/display/DW/Teradata+Basic+concepts>	https://wiki.vip.corp.ebay.com/display/DW/Teradata+Basic+concepts</a>

### Teradata Help

<a href='http://www.teradatahelp.com/search/label/INDEX'> link</a>

#### Index

1. Secondary index (SI)  & Primary index (PI)

   ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_22.png)

   They are usually defined on columns which are more frequently used rather than PI column in joining condition.

   It just creates a **subtable** for defined secondary index , which contains there columns
   **-secondary index value**
   **-secondary index rowid (hashed value for SI)**
   **-hash value for primary index(pointing to target amp)**

   --structure

   This  subtable is created on each amp  which in turn points to **target AMP.**   Hence Secondary index can be TWO amp operation for USI and MANY amp operation for NUSI.  So whenever a SI column is used in a query,  based on that column  optimizer quickly reaches subtable by way  of  row hash generated for SI value and using PI row id corresponding to it,  .it would reach target amp.

   ##### QA

   what means PI column？what different to the non-clustered index ? 

   **PI primary key?** <a href='http://www.teradatawiki.net/2013/08/Teradata-Primary-Index.html'> link</a>

   ##### PI functions:

   - data distribution 
   - fastest way to retrieve data
   - joins

2. Partitioned Primary Index (PPI)

   Feature:

   - which allows access of portion of data of large table,thus ,reducing the overhead of scanning the table 

   - two -type ： case_n ,range_n

     ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_1.png)![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_2.png)

     ##### QA

     1. PPI also comes with some disadvantages like:

        - Another disadvantage is that when another table (without PPI) is joined with PPI table on PI=PI condition. If one of the tables is partitioned, the rows won't be ordered the same, and the task, in effect, becomes a set of sub-joins, one for each partition of the PPI table. **This type of join is sliding window join ** --> data skew?

     2. Using dbc.indices clause to show all indexes   -->**sql server: sys.indexes** ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_4.png)

        help index tbl_name;

    

3. Sparse Index 

   ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_3.png)

4. 

#### Statistics

1. Help  statistics  on db.tblname   --> **sp_helpstats  / sys.stats**  

2. Collect statistics on db.tblname (column) using  sample 20 percent/full stats  

   --> **dbcc show statistics  db.tblnames( column1)**

   Collect statistics  index (index-name) column (column-name) on <tablename>

3. Clause

   **bdc.columnstats, dbc.indexstats and  dbc.muticolumnstats**

4. **collect statictics ;explicit  to collect statics; ** --2019/04/01

    --sql server:select where join  

   --td ：

   ```sql
   COLLECT STATS VT_PI_SESSION_${siteid} COLUMN(CLSFD_SITE_ID,CLSFD_SUM_DT,GA_PRFL_ID );
   ```

   

5. View data distribution 

   ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_18.png)

   

   ##### QA

   - what’s information recorded ? in collect statistics clause?
   - how update statistics?
   - 

#### Macro

#### Data type  priority

1. char >int 

2. ​    ACTUAL_TARGET VARCHAR(22) CHARACTER SET LATIN CASESPECIFIC, 

   ACTUAL_TARGET = 'Actual'  **-->系统自动删除空格**

   ​       experience CHAR(20) CHARACTER SET LATIN NOT CASESPECIFIC,

   trim(experience )='1'  

#### load type

#### Constraint condition

1. 

#### Join

1. ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_12.png)<br>

   ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_13.png)

2. ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_14.png)

3. intersect &except  

   ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_54.png)

   ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_55.png)

   | test_0403  id | age  |
   | ------------- | ---- |
   | 1             | 12   |
   | 2             | 22   |
   | 3             | 32   |

   | test_0402 id | age  |
   | ------------ | ---- |
   | 1            | 12   |
   | 2            | 32   |
   | 3            | 22   |

   ```sql
   select * 
   from test_0403    ##	id	age  
                           1	12
   intersect 
   select * 
   from test_0402;   ###
   
   select  * 
   from test_0403
   except 
   select * 
   from test_0402;   ##2	22
                     ##3	32
   因此可以用except 比较两张的表的差异
   ```

4. 

   ##### QA

   - what‘s   production join ?  **-->笛卡儿积**    

     Cartesian Production Join

5. 

#### Alias:

```sql
select extract(year from clsfd_sum_dt )    as   "year" ,count(1) as num
from p_chewu_t.CLSFD_DAILY_ADREV_SC
group by 1   --因为year date  跟内置函数冲突，所以需要打双引号！  ——》sql server [year]

#参照时间戳一课题

###2019/05/05  命名可以直接调用。
select   clsfd_site_id as gap ,gap + 23 as gap_2, gap_2-gap
from clsfd_access_views.CLSFD_MP_REV_SUM 

```

#### DDL/DML

1. Difference between Create table (copy) and Create table (select)?

   - **CREATE   TABLE** check_COPY **AS** check123 **WITH** no data ;

     --->拓展：

     ```sql
      
      create table p_chewu_t.CLSFD_DAILY_ONSITEREV_SC as  P_CLSFD_T.CLSFD_DAILY_ONSITEREV_SC with no data;
      
      insert into p_chewu_t.CLSFD_DAILY_ONSITEREV_SC 
      select  * 
      from   P_CLSFD_T.CLSFD_DAILY_ONSITEREV_SC
      where clsfd_sum_dt>='2018-01-01';
      
      
     ```

     <span style='background-color:lightblue'>**既保留原始表的所有表结构信息 pi ,defalut 等等，又只抽取部分数据集；**</span>

     

   - **CREATE    TABLE** Check_SELECT **AS** ( sel * **FROM**   check123 ) **WITH** no data ;

     **拓展临时表：Volatile  建立索引并且保留数据**

     - ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_48.png)

     - ```sql
       create multiset volatile table test_0403  (id int ,age int) primary index (id)  on commit preserve rows;
       ```

   - ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_15.png)

     <strong><span style='color:red;font-size:20px'>前者完全跟check123结构一样，约束一样；</span><span style ='background-color:lightblue'>后者会有缺失，并且PI索引是以第一个字段建立索引</span></strong>：

     ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_52.png)

     

     ```sql
     ---拓展sql  server :
     select  * into check_copy  from check123  
     ---只有表结构，没有索引，没有主键等；
     ```

     

   - ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_34.png)

     **注意添加pi特点** 

2. how to alter some columns date type ?

   <a hre f='http://blog.sina.com.cn/s/blog_a5aced010101fn5s.html'>更改字段数据类型</a>

   - 并没有alter  功能  直接 alter table  column  datetype 
   - 步骤如下：
     1. 先对旧表备份  **rename table table_name to  table_name_bak;**
     2. create multset table  table_name 
   - 恢复数据 ： insert into table_name  select* from table_name_bak
   - 删除备份数据： drop table table_name_bak

3. 新增字段：

   **多个字段，每个字段之前都有一个add 命令**

   ```sql
   ALTER TABLE emp_data  ADD educ_level CHAR(1), ADD insure_type SMALLINT ;
   ```

4. 删除字段

   

5. how to judge whether object it exists?

   **select * from <span style="color:red">dbc.tables</span>** 

   **where databasename ='P_chewu_T'**

   **and tablename='CLSFD_TRFC_SUM_RPT_MKT'**

   **---拓展是否可以写控制循环？**

6. How to measure the table size?

   ```sql
   select * 
   from dbc.allspace
   where lower(databasename) ='p_chewu_t'
   and  tablename like ('%mkt%')
   ```

7. How to measure database space ?

   ```mysql
   help database p_chewu_t
   ```

8. How to truncate table 

   **delete p_chewu_t.CLSFD_TRFC_SUM_RPT_MKT_bak all** 

   --> truncate table  p_chewu_t.CLSFD_TRFC_SUM_RPT_MKT_bak

9. How to insert data 

   1. insert 插入数据可以不按着底层表结构，顺序插入；

      ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_49.png)

   ​    **但是一定要指明分别插入那个fields,而且对应的数据类型要一直**

   <span style='color:red' >**其次 td仓库 可以实现 跨库插入：**</span>

   ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_50.png)

10. How to update 

   ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_46.png)

   --->different from sql_server

   ```sql
   update FCST 
   set weekly_MGT_forecast=metric_value 
   from p_clsfd_t.CLSFD_WEEKLY_FCST_SC FCST  join (select '2019-04-20' as week_end_date,'Benelux' as cluster_name,'Belgium' as site,'Total' as Revenue_type,5325000 as metric_value) as MFC 
   on  FCST.REVENUE_TYPE=MFC.Revenue_type and FCST.week_end_date=MFC.week_end_date and  FCST.cluster_name=MFC.cluster_name and FCST.site_name=MFC.site; 
   
   ```

   

11. volatile  table can't no be alter  after it 's has bulit!

    ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_53.png)

12. 同样适用

    merge into  

    using

    when matched  then  update 

    when not matched then insert 

13. 

    

#### Distinct & Group by 

1. ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_16.png)
2. ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_17.png)

#### Help &Show command

| DIRECTIONS                               | HIVE/Sql server                  |
| ---------------------------------------- | -------------------------------- |
| HELP Table                               | desc/ sp_helptext tbl 查看表结构 |
| Help index                               | sp_helpindex                     |
| help statistics                          | sp_helpstats                     |
| help View                                | sp_helptext                      |
| Help sql                                 | 内置函数                         |
| show function                            | UDF                              |
| Show objection(tbl/view/macro/procedure) | 查看DDL语句                      |

![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_41.png)

### Teradata system with TDload

background:

  copy table from different systems : 跨服务器抽取表

<a  href ='https://wiki.vip.corp.ebay.com/display/DW/Copy+tables+across+different+Teradata+system+with+TDLOAD'> reference </a>

![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_31.png)

### **Teradata Wiki**

<a href='http://www.teradatawiki.net/'>link</a>

### Teradata introduction 

<a href='https://courses.teradata.com/learning/BLADE_MS/legacy/29955_SQL_Intro/home.asp?AICC_SID=C778977M1041S&AICC_URL=http%3a%2f%2funiversity.teradata.com%2fplateau%2fPwsAicc&trackScore=0'>book</a>

### Teradatapoint

<a href='https://www.teradatapoint.com/teradata/explain-plan-in-teradata.htm'> link</a>

#### Sample function

- select * from table sample 5/.2 
- select top 2 *  from table 

#### ACID

![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_56.png)

### **Teradata tutorials-point**

<a href='https://www.tutorialspoint.com/teradata/teradata_architecture.htm'>English_website</a>&<a href='https://www.w3cschool.cn/teradata/teradata_macros.html'>W3Cschool</a>

1. Feature:MPP强调Massively Parallel Processing 大规模的并行运算
2. OLAP 层次的应用
3. 

#### Architecture

- ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_19.png)

  1. PE：一个PE也被称作为Vproc(Virtual processor) ,完成四项工作：

     - session control -->SQL Parse-->Optimizer-->Dispatcher(任务分布)
     - Dispatcher:分给各个AMP
     - Dispatcher阶段完成**hash value ,hash bucket ,hash-map** 工作

  2. node: node consists of its own operating system, CPU, memory, own copy of Teradata **RDBMS** software and disk space. -->集群形式的关系数据库

  3. Message passing layer:**BYNET**,is the **networking layer** in Teradata system. It allows the communication between **PE** and **AMP** and also between the **nodes**

  4. AMP(**访问模块处理器**):that actually **stores and retrieves the data.** 每个AMP与存储数据的一组磁盘相关联。 只有该AMP可以从磁盘读取/写入数据。

     - AMP与表的关系

       1. 每个AMP都含有表的基本结构（表名，列名，索引信息）

       2. 每个表的数据有可能分布到各个AMP,最理想的是 均匀分布，这样以更好的利用所有节点并行处理，减少数据的倾斜

          ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_25.png)

          这样我们请求table1数据请求，就可以用4个AMP协同工作，加快处理。假设Table1只分布在一个AMP，此时请求Table1会慢很多。

     - AMP与磁盘的关系

       一个AMP最多可以控制64个磁盘/节点node

#### Data Distribution

<a href='https://blog.csdn.net/wali_wang/article/details/50493077'>CSDN</a>&<a href='http://dwgeek.com/teradata-data-distribution-works-amps.html/'>Dwgeek.com</a>

- The data is **hashed using hash algorithm on Primary Index (PI)**. The hash algorithm produces **hash bucket value** and **row hash value.** Hash bucket value will be **having AMP’s numbers**. The BYNET sends the records to **respective AMP’s** based on Hashed values and AMP will intern store that in it’s associated disks.

  hash value  / hash bucket 

  当以取mode形式,value%6![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_28.png)

  ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_29.png)

  ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_30.png)

  <a href='https://blog.csdn.net/NevePioneer/article/details/3712920'> 数据分布机制</a>

  1. 依据PI，生成一个行哈希值 hash value  **-->HASHROW** 计算row的hash值
  2. hash bucket  **-->HASHBUCKET**根据hashrow计算hashbucket，代表了一个hashmap的入口
  3. hash map ： into AMP  **-->HASHMAP**根据hashbucket确定数据分布到哪一个AMP

  <span style='color:orange'>**提前查看字段分布情况**</span>

  ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_18.png)

- Data exchange 

  **teradata有两种数据交换一种为复制表(DUPLICATE)，另一种为重分布(REDISTRIBUTE)**

  

#### Tables

- create table 
  - **CREATE   TABLE** check_COPY **AS** check123 **WITH** no data ;

  - **CREATE    TABLE** Check_SELECT **AS** ( sel * **FROM**   check123 ) **WITH** no data ;

    

  **-->拓展 比较volatile  table  其中也有 with data  primary index  on commit preserve rows**

1. Permanent table

   create set/multiset table tbl_name ()

   ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_34.png)

   ##### QA

   - what ‘s difference between CET/MUTICET table?

     ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_36.png)

     ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_37.png)

   - 

2. Volatile table

   DDL: 

   create volatile table tbl_name( )

   ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_35.png)

   **--> select  * into #t1 from tbl1**  ( is comparable  to )

   - <span style='color:orange'>首先无需要添加db </span>, ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_43.png)

     <span style ='color:darkgreen'>**特点是系统默认的将表添加到 个人 user_id 下**</span>

   - 其次 <span style='color:orange'>on commit preservewith  rows</span>/：

     含义：每一次批处理后，保留临时表内的数据，并且保留表结构信息；

     **跟基本表一样，可以进行后期的insert +update**

     如果没有该语句：系统会默认添加 为  **on commit delete rows:**

     ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_44.png)

     **<span style='background-color:lightblue'>正如字面意思：每次交互处理结束后，会清空表数据 ，保留表结构</span>**

   - **最后，关闭会话（软件后），清除表（跟sql server 临时表一样），再次开启新的session，已经不存在**

   - **CREATE VOLATILE TABLE VT_TOT_METRIC_DIVIDE as**![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_47.png)

     

     - bridge & volidate table 

       ```sql
       SET QUERY_BAND = 'sock_timeout=120;recv_eofresponse=1;childThreadCount=1;sinkClass=HDFSTextFileSink;delimiterCharCode=1;null=\N;' FOR SESSION;
        create volatile table okr_active_user as 
        (
       SELECT BRIDGEIMPORT.* FROM PARALLEL_IMPORT
       (USING OUTPUT_COLS
       (
       'SITE varchar(255), 
       DT varchar(255), 
       APP_ACTIVE_USER varchar(255)
       '
       )configserver('bridge-gateway-mzt:1025')
       configname('mzt_dm_ares')
       dataPath( '/user/xianglwang/bridgeOutput/SLAM_OKR/APP_ACTIVE_USERS/*')
       failOnNoImportFiles('false')
       fileErrorLimit('0')
       ) bridgeimport 
       ) with data primary index ( SITE)  on  commit preserve rows ;
       
       ```

       

     - ->拓展：

   ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_42.png)

   <span style='background-color:lightblue'>当没有on commit preserve rows </span> 时候，一次事务（交互式）处理完成，则会清空表数据，保留表结构信息

   

   

   

3. Global Temporary table

   **-->select *  into ##t1 from tbl1**

4. Derived table  

    

#### Space types

1. Permanent  space
   - 各个AMP物理空间，磁盘
2. Spool space
   - 存储中间结果，用于查询
   - 临时表  volidate table
   - 除了ps 剩下的空间都是 spool space+temp space
3. Temp space

#### View

1. create view ，修改view 采用 **replace view**
2. replace view p_clsfd_v.CLSFD_TRFC_SUM_RPT_MKT as locking row for access  select * From p_clsfd_t.CLSFD_TRFC_SUM_RPT_MKT;
3. show view 查看ddl 语句；

#### Macros

- ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_20.png)

  **exec get_emp_salary(101)**

#### Stored Procedure

- ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_21.png)

  **call insersalary(12,12,112,12)**

#### Explain

- F6：快速查看执行计划；

- **Purposes**

  ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_32.png)

- 

- | types of   executions                             |                                                  explain | reference                                                    |
  | :------------------------------------------------ | -------------------------------------------------------: | ------------------------------------------------------------ |
  | all-maps retrieve step / Single-AMP retrieve step |                                        数据检索涉及到AMP |                                                              |
  | by way of all-row scan                            |                                        AMP retrieve type |                                                              |
  | by way of the primary index                       |                                        AMP retrieve type |                                                              |
  | by way of hash value                              |                              AMP retrieve type/join 阶段 |                                                              |
  | duplicated on all AMPS                            |                                            data exchange | for data join                                                |
  | redistributed by the hash mode                    |                                            data exchange | for data join                                                |
  | merge / hash /nested/ production/ exclusion join  |                                               joint type |                                                              |
  | confidence level                                  |                                             no/low/high/ | ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_33.png) |
  | with no residual condtions                        | All applicable conditions have been applied to the rows. |                                                              |

- ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_38.png)

- ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_39.png)

- ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_40.png)

- 

#### BTEQ

<a href='https://www.teradatapoint.com/teradata/temporary-table-in-teradata.htm'> link</a>

#### Fastload/FastExport/Mutiload

<a href='https://www.teradatapoint.com/teradata/temporary-table-in-teradata.htm'> link</a>

#### Others

##### show execution plan

- ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_6.png)

  **-->smart key：F6** 

  --> sql server : CTRL+L 

  ##### QA

  1. what's real execution plan ?  --> sql server：Ctrl+M
  2. It's have graphic display?

#### Functions

<a href='https://www.w3cschool.cn/teradata/teradata_date_time_functions.html'>link</a>&<a href='https://docs.teradata.com/reader/756LNiPSFdY~4JcCCcR5Cw/wc9YbaTQzZEiPMlnB~CawA'>Teradata document </a>

##### Character

- ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_8.png)

  --> length

- ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_7.png)

- ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_5.png)

- 替换 

  ```sql
  select oreplace('FRED 222','FRED','Bill')   ## bill 222
  ```

- 

##### Date

<a href='C:\Users\wenbluo\Desktop\wbluo\teradata\时间函数.sql'>sql</a>

date '2019/01/01'  -->报错： date literal  

date '2019-01-01'  -->right  转化为时间函数

-->**select cast ('2019/01/01' as date),cast ('2019-01-01' as date)**   都对； 

2019/01/01

- 通过底层系统表： sys_calendar.caldates  ：一个字段cdate<span style='background-color:lightblue'>【2100-12-31  1900-01-01】</span>

- <span style='color:orange'>sys_calendar.calendar  week_of_year 是从0-52;</span>

  ```sql
  select  *
  from sys_calendar.CALENDAR
  where calendar_date='2019-01-01'
  
  week_of_year 返回的是0 ，表示不完整的一周；
  ```

  ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_9.png)

  **拓展：**

  也可以通过 weeknumber_of_year():

  ```sql
   select weeknumber_of_year(current_date),
   weeknumber_of_year(current_date - interval '3' day) , #周天作为新的一天
   current_date - interval '3' day   #2019/03/24  周天
   
   
  ```

- ```
  QUARTERNUMBER_OF_YEAR :计算quarter
  ```

- ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_23.png)

  ```sql
  extract (year from clsfd_sum_Dt) as "year",
  ```

- ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_24.png)

  ```sql
  select current_date + INTERVAL '1' MONTH;
  (Date+ 1)
  2017-11-25
  
  select current_date - INTERVAL '1' MONTH;
  (Date- 1)
  2017-09-25
  
  
  ####new 2019-05-05
  
  select  day_of_month,date-day_of_month+1 as p1p,date -interval  '2'  day as p2
  from  SYS_CALENDAR.CALENDAR
  where  1=1
  and  calendar_date=date
  
  ###不需要参数 interval 而是直接运算 
  
  
  ```

| Next_Day         |                      |
| ---------------- | -------------------- |
| Last_Day         | Eomonth              |
| TO_date          |                      |
| TO_UNIXTIMESTAMP |                      |
| ADD_MONTHS       |                      |
| INTERVAL         |                      |
| MONTHS_BETWEEN   | 时间间隔月数         |
| TRUNC            | 返回该日期第一周时间 |
| Round            |                      |

![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_26.png)

<span style='background-color:lightblue'>**返回该周第一天（周日）**</span>

```sql
select  trunc(date'2019-04-15','day'),trunc(date '2019-04-25','Month'),add_months(trunc(date '2019-04-25','Month') ,1)

/*	1	4/14/2019	4/1/2019	5/1/2019 */
--->拓展求每月月初的数据

第二种方法：
SELECT 
ADD_MONTHS(CAST((DATE '2018-12-11' )/100 * 100 + 1 AS DATE), -2),
ADD_MONTHS(CAST((DATE '2018-12-11' )/100 * 100 + 1 AS DATE), 0),
/* ADD_MONTHS((DATE '2018-11-24'  + interval '1' day), -2),
 ADD_MONTHS(CAST((DATE '2018-11-24' )+ 1 AS DATE), -2),*/
CAST((DATE '2018-11-24' )/100 * 100 +1 AS DATE),
(DATE '1900-01-01' )/100 * 100 + 1,
(DATE '1900-01-02' )/100 * 100,
(DATE '1900-01-03' )/100 * 100   ###向下取整

10/1/2018	12/1/2018	11/1/2018	101	100	100

 

```

![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_27.png)

##### unixstamp：

```sql
/*select  date ('2019/04/06')  - date ('2019/03/03')*/

select (date '2019-04-06' - date '2019-03-03') day ;  ## 34
select (date '2019-04-06' - date '2019-03-03') month ;  ## 1
select (date '2003-08-15' - date '2003-01-01') year ; ## 0 
```

##### Aggregation

- ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_10.png)

- ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_11.png)

- ```sql
  with pro_1 as 
  (
  select  1  as num1,null as num2,0 num3
  )
  select num1,coalesce(num2,0),num1/nullifzero(num3),nullifzero(num2),nullifzero(num3)
  from pro_1
  ```

  ![f](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_57.png)

  -->**zeroifnull ()**  : null 赋值为0

  --><span style='background-color:red'>**td 也支持coalesce** </span>

  ```sql
  SELECT clsfd_sum_dt,clsfd_site_id,
  coalesce(SUM(clsfd_cas_rev_usd), -99 )
  FROM P_CLSFD_T.CLSFD_METRIC_PLTFRM_SUM_ODS
  WHERE clsfd_sum_dt >= '2019-06-09' AND clsfd_site_id IN (2,1021,3001)
  GROUP BY 1,2 ORDER BY 2,1;
  
  ```

  ```sql
  select zeroifnull ( null) 
  # 0
  ```

  ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_58.png)

  **sqlserver  的coalesce** 

- Random :

  1. random  value

     select random(1,10) as num 

     it will return range(1,10) a value

  2. random sample 

     select * from tbl sample 10 ; 

     it will returns random  10 rows

- 

##### Operation

- ```sql 
  select 10/2 ,10/4
  --5  2  :向下取整
  ```

- 四舍六入五看双

##### Charsets

##### Windows

1. ![](C:\Users\wenbluo\Desktop\wbluo\teradata\picts\teradata_45.png)

2. 窗体函数：

   ```sql
   select RUN_DT, 
   CLSFD_SITE_ID,
   CLSFD_TABLES, 
   clsfd_sum_dt,
   DIMENSION_GROUP,
   METRIC_NAME ,
   sum(1) over () as num1,
   sum(1) over(partition by RUN_DT,CLSFD_SITE_ID,
   CLSFD_TABLES, 
   DIMENSION_GROUP,
   METRIC_NAME ) as num2
   FROM DQRC.CLSFD_ADPO_SUM_tbls_log 
   where run_dt>='2019-04-16'
   ```

   sum(1)over ：返回的是总行数据（跟维度无关） ##300

   sum(1) over(partition by RUN_DT,CLSFD_SITE_ID....):返回的是与维度有关的汇总数据 #15

   ```sql
   select RUN_DT, 
   CLSFD_SITE_ID,
   CLSFD_TABLES, 
   DIMENSION_GROUP,
   METRIC_NAME ,
   sum(1)   ##15
   FROM DQRC.CLSFD_ADPO_SUM_tbls_log 
   where run_dt>='2019-04-16'
   group by 1,2,3,4,5
   ```

   与下面sql 的区别在于： 第一个sql 返回300 行明细数据，然后num_1,num_2是汇总数据

   类似

   ```sql
   select a.*,b.num2
   from   
   （select * from tbl1 ) as a left join (select  .....,sum(1) as num2 from  group ....) as  b  on a....=b...
   ```

   

3. show most recent 5 checksum results for each table +site combo

   ```SQL
   select  RUN_DT, 
   CLSFD_SITE_ID,
   CLSFD_TABLES, 
   DIMENSION_GROUP,
   METRIC_NAME ,
   sum(total_segments)over (partition by CLSFD_SITE_ID,
   CLSFD_TABLES, 
   DIMENSION_GROUP,
   METRIC_NAME order by run_dt  desc rows between  current row and 4  following ) as latest_5_days_total_segments,
   sum(seg_qualify_cosine_value)over (partition by CLSFD_SITE_ID,
   CLSFD_TABLES, 
   DIMENSION_GROUP,
   METRIC_NAME  order by run_dt   desc  rows between  current row and 4 following ) as latest_5_days_seg_qualify_cosine_value,
   sum(seg_qualify_total_gap)over (partition by CLSFD_SITE_ID,
   CLSFD_TABLES, 
   DIMENSION_GROUP,
   METRIC_NAME order by run_dt   desc   rows between  current row and  4  following ) as latest_5_days_seg_qualify_total_gap
    from over_function
   ```

   - 窗体函数：重要参数 rows between and  current row / 1 following /  1  unbonded preceding  
   - order by run_dt   desc 一定需要，不然机器不知道怎么相加

4. 进阶：<span style='background-color:lightblue'>**qualify 应用**</span>：

   ```sql
   select *
   FROM dqrc.CLSFD_ADPO_CHKSUM_log
   qualify rank() over(partition by CLSFD_SITE_ID, CLSFD_TABLES order by run_dt desc , run_TM desc) = 1 
   ```

   特点：求出最新一期明细数据  

   等价于 ：group by having 

   ```sql
   select  clsfd_pltfrm_id,clsfd_pltfrm_type,clsfd_pltfrm_desc,row_number()over(partition by clsfd_pltfrm_type order by  clsfd_pltfrm_id ) as rk 
   from   clsfd_tables.CLSFD_PLTFRM_LKP
   qualify rk <=2
    order by 2,1
    ;
    
    select  clsfd_pltfrm_id,clsfd_pltfrm_type,clsfd_pltfrm_desc,row_number()over(partition by clsfd_pltfrm_type order by  clsfd_pltfrm_id ) as rk 
   from   clsfd_tables.CLSFD_PLTFRM_LKP
   qualify  row_number()over(partition by clsfd_pltfrm_type order by  clsfd_pltfrm_id ) <=2
    order by 2,1
    
    
   ```

   -->优于 sql server :

   ```sql
   select *,rank() over(partition by CLSFD_SITE_ID, CLSFD_TABLES order by run_dt desc , run_TM desc) as rk 
   from dqrc.CLSFD_ADPO_CHKSUM_log
   where rk=1   ### 会报错，非要嵌套才能应用rk 字段
   
   select rk.*
   from (select *,rank() over(partition by CLSFD_SITE_ID, CLSFD_TABLES order by run_dt desc , run_TM desc) as rk 
   from dqrc.CLSFD_ADPO_CHKSUM_log) as rk
   where rk=1
   ```

   

5. 

# scope&bridge?

- TD-hadoop 

  <a href=https://wiki.vip.corp.ebay.com/display/MarketingScience/4+-+Teradata-Hadoop+Bridge>hadoop</a>

- TD-load

  <a href=https://wiki.vip.corp.ebay.com/display/DW/Copy+tables+across+different+Teradata+system+with+TDLOAD>TDload</a>

- instance :

  ```sql
  
  SET QUERY_BAND = 'sock_timeout=120;recv_eofresponse=1;childThreadCount=1;sinkClass=HDFSTextFileSink;delimiterCharCode=1;null=\N;' FOR SESSION;
  
  CREATE MULTISET VOLATILE TABLE  vt_clsfd_belgium_users AS
  (
  SELECT BRIDGEIMPORT.* FROM PARALLEL_IMPORT(USING OUTPUT_COLS
  (
  '
  clsfd_sum_dt date,
  clsfd_site_id INTEGER,
  WEEK_ID VARCHAR(3),
  buyers VARCHAR(256),
  active_users VARCHAR(256)
  ')
  configserver('bridge-gateway-mzt:1025')
  configname('mzt_dm_ares')
  dataPath('/EDW_CLSFD/GA/clsfd_active_user_weekly/0*')
  failOnNoImportFiles('false')
  fileErrorLimit('0')
  ) bridgeimport
  )
  WITH DATA PRIMARY INDEX (clsfd_sum_dt)
  ON COMMIT PRESERVE ROWS;
  ```

  

### Reference

1. <a href='https://docs.teradata.com/reader/jmAxXLdiDu6NiyjT6hhk7g/~Hy5lbZSS5zSwwnQv7yRWg'> Teradata Document</a>

### QA

- 什么是BETQ

- 什么是MARCO

- confidence level

- what is difference between SAMPLE  and Top function?

- Charset<a href='https://r12a.github.io/app-conversion/'> convert </a>

  

## QA

1. 怎么快速调用夺db  --> use  datebase_name

2. 怎么查看DB权限 ：

   CLSFD_ACCESS_VIEWS

   --》通过VDM查看db信息；

   3. TD建表注意事项



## **4舍6入5成双**

