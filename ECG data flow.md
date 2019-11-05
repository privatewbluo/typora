Front-end data flow

<a  href ='https://wiki.vip.corp.ebay.com/display/DW/Google+UA'> https://wiki.vip.corp.ebay.com/display/DW/Google+UA</a>

## ECG sites

> <a href='https://www.ebayclassifiedsgroup.com/'>History</a>

![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_46.png)



mobile.de :德国卖汽车

![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_47.png)

既然

```sql
case when clsfd_site_id in (4001,4002,4003,4004,4011) then 4001
```

 为什么不直接用 clsfd_rpt_site_id?

<p style ='text-align:center'>原因如下:</p>
Belgium （ 2dehands &2dehands）:比利时：专门卖二手的网站：

![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_48.png)

```sql
case when clsfd_site_id in (1020,1021,1022,1023,1024) then 1021
```

Marktplaats: 

netherland(荷兰) classifieds site：咸鱼

the idea was to recycle castoffs and help customers find what‘s they want 

![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_49.png)

9000086：统称benelux (荷兰比利时卢森堡)

其中用clsfd_rpt_site_id 去区分 belgium (6000081)  ,荷兰(3001)



![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_75.png)

dba: Denmark (丹麦) ：5001  

It;s  a classified websites ,for users to buy or sell just about anything from smart phones, design furniture ,branded kid's clothing or even finding the perfect house or flat anywhere **in Denmark**



Bilbasen : Denmark : 6001 

![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_78.png)

eBay-K :9021

![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_79.png)



## GA Tables

<a href ='https://yoast.com/tag/google-analytics/'> Learning Google Analytics </a>

1. what's is scope ?

   <a href='https://www.lunametrics.com/blog/2016/11/30/understanding-scope-google-analytics-reporting/'> To learn what's the scope</a>

   ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_76.png)

2. 

   dw_clsfd.clsfd_ga_row

   features:

- contain all BigQuery raw  data .backend file stored with json format
- this table contain raw data from bigquery .stored  with sequence file format
  - staging table 
  - working table   
  - target table 

#### Hit&Session&Pageviews

1. <a href='https://support.google.com/analytics/answer/2731565?hl=en'>what's  session  in Analytics</a>

   session 表示打开窗口, Hit 表示点击次数

   sessions have hits, but hits don't have sessions

   ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_77.png)

   一次session 的有效期限：

   - 每次hit 往后延30min, or 新的一天作为session 结束
   - 

2. <a href='https://yoast.com/what-are-pageviews-in-google-analytics/'> pageviews</a>

   - what 's  pageviews? and unique pageviews?

     *A pageview (or pageview hit, page tracking hit) is an instance of a page being loaded (or reloaded) in a browser*  -->每次加载的一个页面(www.baidu.com)就是一次pageview

     

     

3. 

### Alation

​    <a href=https://wiki.vip.corp.ebay.com/display/KNOW/How+to+request+access+for+individual+or+batch+account+on+Hadoop+Clusters%3A+Ares%2C+Apollo%2C+Hercules%2C+Spades%2C+Artemis+from+BDP+including+Alation+users#HowtorequestaccessforindividualorbatchaccountonHadoopClusters:Ares,Apollo,Hercules,Spades,ArtemisfromBDPincludingAlationusers-AccesstoindividualuseraccountsonAres/Apollo>链接</a>

1. run  hql  on  Alation
2. 

#### Github

<a href='https://guides.github.com/activities/hello-world/'>Github</a>

### Linux key and public key

<a href= 'https://www.cnblogs.com/dalaoyang/p/9928806.html'> key</a>

### Proper nourn

1. Baston Host 

   Access to Hadoop clusters is allowed only through Baston Hosts

   <a href='https://wiki.vip.corp.ebay.com/display/KNOW/How+to+access+or+login+to+Hadoop+Client+%28Apollo%2C+Ares%2C+Artemis+cluster%29+through+Bastion+Hosts'>链接</a>

   SSH Bastion Access

   <a href='https://wiki.vip.corp.ebay.com/display/STO/Infrastructure+Bastion+Host'>链接</a>

2. 

### Three Clusters

1. Ares

2. Apollo  -->EBAY

3. Hercules   ---EBAY 

   <a href='https://wiki.vip.corp.ebay.com/display/DW/Hadoop+-+Hercules'> Hercules </a>

   1/2 被用于与adhoc query user:  业务方.

   hercules 用<a href ='https://docs.databricks.com/spark/latest/spark-sql/index.html#sql-language-manual'>spark sql </a>user: deps 

   

   <a href ='https://wiki.vip.corp.ebay.com/pages/viewpage.action?pageId=242289518#NewHireApollo/Ares-Overview '> [New Hire Apollo/Ares](https://wiki.vip.corp.ebay.com/pages/viewpage.action?pageId=242289518)</a>

4. ECG HADOOP :ECG自己的集群  在Dutch 荷兰

### Mozart

1. td2  teradata db2

## BigQuery

1. <a href='https://wiki.vip.corp.ebay.com/display/DW/Google+BigQuery+Extract+Process'>bigquery+extract+process</a>
2. <a herf='https://wiki.vip.corp.ebay.com/display/DW/Google+BigQuery+Transformation+on+Ares_Mozart'>BigQeury</a>
3. 

## Hadoop/Ares/Apollo

1. <a href='https://wiki.vip.corp.ebay.com/pages/viewpage.action?pageId=571352728'>wiki</a>
2. 吴瑕
3. **Apollo PHX** (to be retired after 2019 Q1)
4. **Apollo RNO** (to be used after 2019 Q1)
5. Hercules:spark平台运行spark sql

## ETL FRAMWORK

1. ADPO

2. <a href='https://wiki.vip.corp.ebay.com/display/DW/eCG+DW+Data+Flow'>data flow</a>

3. <span style='background-color:lightblue'>（E过程）</span>:API, local dataware house ,manual  data

   ETL SERVER (PHX01-23 LVS01-23)

   TD:teradata  <span style='background-color:lightblue'>（L的过程）</span>

   Hadoop :datacenter 

   cluster(herclus <span style='background-color:lightblue'>T 的过程</span>) -->cluster(ares /apollo/apollo reno[hermes :adhoc query]) <span style='background-color:lightblue'>（L 的过程）</span>

   

   <span style='background-color:lightblue'>**adtacenter : phx lvs reno** </span>

   

   OLAP :我们主要做的是Olap

   

   OLTP :ECG hadoop  荷兰cluster

   

   GA/BIGQUERY

   

   hadoop fs:<https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html>

   

   <a herf ='https://wiki.vip.corp.ebay.com/display/DW/New+Traffic+Tables+in+Hadoop'>Traffic_tables</a>

   <a href='https://wiki.vip.corp.ebay.com/pages/viewpage.action?pageId=571352728'>Hadoop+Herclus+Ares</a>

   

   ares  sql diretory

   <span style='background-color:lightblue'>**/export/home/b_clsfd/sql**</span>

   

   

   

   ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_71.png)

   **Apollo RNO** -->is best and can refer to bruce's wiki 

   

   

   cube:hdlq-other-default

   

   

   - single+table _extract_handler.ksh+ETL_ID

     ste :single table extract 

     - staging table 
     - working table   
     - target table 

4. L:single_table_load_handler.ksh

5. T:Targert_table_load_handler.ksh

   stt: transformation sparksql  target table  -->hercules

   ​                                                                          -->daily incremental  (table /ods /file )

   stm:merge file into  td (teradata) /ares /  reno

6. stt  与 stm 可以并行处理。

   - 

7. uow

## ADPO流程 -->herclus

phx019:

lvs023:ca ( batch ) 

lvs005:调用hql

apollo phx -->will be remove  and  migrate apollo  rno --> for analysis  / query 

### BI report :

1. Insight builders   -->ART/ ARTNOVA 
2. tableau 
3. quick strike   两个重要的report: **balance scorecard   weekly-scorecard**

![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_110.png)

- post an jira ticket :https://jirap.corp.ebay.com/browse/BIPL-13627  to get an access to publish on prod & qa 
- 

## ElasticSearch

1. 

## VDM (virtual data mart)

- 

## Odin

1. <a href=https://ecgwiki.corp.ebay.com/display/ANYL/Odin>odin</a>
2. ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_39.png)

## Kafka

1. <a href=https://www.ibm.com/developerworks/cn/opensource/os-cn-kafka/index.html>kafka</a>
2. kafka 是一个消息系统。活动流数据是几乎所有站点在对其网站使用情况做报表时都要用到的数据中最常规的部分。活动数据包括页面访问量（Page View）、被查看内容方面的信息以及搜索情况等内容。这种数据通常的处理方式是先把各种活动以日志的形式写入某种文件，然后周期性地对这些文件进行统计分析
3. ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_38.png)

## UC4

1. Putty 的配置 

   https://wiki.vip.corp.ebay.com/display/BatchPlatform/DW+UC4+SSH+Tunneling+Setup+and+Instructions

2. <https://wiki.vip.corp.ebay.com/display/DW/UC4+Common+Knowledge>

3. <a href='https://wiki.vip.corp.ebay.com/display/BatchPlatform/High+Level+Overview#HighLevelOverview-NewUC4AccountRequest'>high level    https://wiki.vip.corp.ebay.com/display/BatchPlatform/High+Level+Overview</a>

   https://wiki.vip.corp.ebay.com/display/BatchPlatform/Logging+Into+UC4+for+the+First+Time

4. prod环境：1000/1450

   dev环境：1400/4000

   username:wenbluo 

   password:NT

   <span style='background-color:lightblue'>**jobplan  目录：**</span>

   ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_74.png)

   - jobs & jobplans 的区别

     JOBS:A single step of your ETL processes.

     JOBPLAN: (ASA Container) You can organize your jobs with line tool to show these relationship and make sure them running in order.

   - 

5. how to transfer from dev to prod  --- release ?

   ![]()

6. search  the container 

   ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\uc_1.png)

7. 注意事项：

   - jobs and jobplans : 要title 名称一致
   - attributes  配置注意 hostname 
   - Variables & Prompts  and process 

8. ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_96.png)

   <span style='background-color:yellow'>**如何设置顺序调度？**</span>![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_88.png)

   <span style='background-color:yellow'>'**黄色是报错！**</span>

   ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_94.png)

   <span style='background-color:yellow'>'**绿色是正在运行！**</span>

9. ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_89.png)

   那如何ssh 该servername 去查看日志？

   <span style='background-color:yellow'>**ssh  lvsdpeetl005.lvs.ebay.com** </span>

   **pwd is **   <span style='background-color:lightblue'> **unix password**</span>

10. ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_90.png)

   通过读取日志： 知道application_id  然后去BDP ,查看具体信息

   ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_91.png)

11. 如何查看执行的是什么文件？&如何查看运行脚本？

    ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_98.png)

    

    ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_93.png)

    <span style='background-color:yellow'>**通过etl_id ：dw_clsfd.clsfd_ad_cube_daily_mp**</span>

    ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_95.png)

12. 如何任务失败后再调度？

    ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_88.png)

13. 如何从herclues cluster --> apollo rno  cluster？ stm 过程

    ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_101.png)

14. 


### arguments:

1. dual
2. 

## Release

- https://jirap.corp.ebay.com/secure/CreateIssue.jspa
  create DWRM ticket
  PDP - 周一三五的3.00PM之前
  

![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_84.png)

**revise your due data .**

  ECR-每天9.00PM之后，或者3.00PM之前 :

**注意rollout 时间： 要date-1**,**不然manifest 时候告知不到时间....**

    PDP >>>ECR

- Develop your code

- git hub release : https://wiki.vip.corp.ebay.com/display/HWDI/GitHub+essentials+for+APD+ETL+development

- <a href='https://wiki.vip.corp.ebay.com/display/DW/KnowledgeBase+for+ECG+DW+Developers'>how to name your tag & branch </a>

- manifest https://dssrm.vip.ebay.com/release/ (edited) 

  ![](C:\Users\wenbluo\Desktop\work\kylin cube\10.png)

1. 主要步骤

   <span style='background-color:lightblue'>**git branch --> git add +git commit --> git merge -->git tag -->git push**</span>

   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_125.png)

2. 两个重要路径

   **/ad_cube/DINT-CLSFD/etl/sql/dw_clsfd**

   更改prod 环境sql 文件

   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_126.png)

3. **/DINT-CLSFD/etl/onetime/DW_ECR_DWRM33049**

   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_127.png)
   
4. how  to check your file had been deployed?

   etl-server : lvsdpeetl001.lvs.ebay.com  

   cd /dw/etl/home/prod/sql/dw_clsfd

   cat ***.file

   

5. how schedule job 

   ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_92.png)

   **导出生成xml文件，然后提交jira ticket** 

### Teradata

### Agile development--敏捷开发

1. JIRA
   - 项目管理角度
   - 方便tracking
   - 人员安排
2. KanBan
3. High-level -->multitask-->Bi-report

### VDM

### C3

1. SERVER 访问数据

2. 服务方提供数据-->服务器集群C3-->etl server ssh   （可以访问外网吧）

   C3:（从外网拿数据,集群）

   --> api 数据 + 爬虫数据

   -->可以自己申请

3. Baston 跳板机 

   C3:固定    /mnt/shared/prod   文件路径

   scp: 文件互传指令；

4. <a href='https://wiki.vip.corp.ebay.com/display/DW/CLSFD+C3+Server+summary'>wiki</a>

5. 

## ECG---EU/CA/

## STARTHUB/E-COMMERCIAL

## BI report

1. metric type  
   - Actual & Target & Forecast
2. metric granularity :粒度
   - daily & weekly & monthly & quarterly & yearly
3. metric calculation
   - N - over - N(Year, Quarter, Month, Week) ; YTD; QTD
     1. YTD: 本年迄今为止
     2. MTD:当月
4. metric dimension 
   - Status
   - Platform
   - Device
   - Category

###  BI platform introduction

<a href='C:\Users\wenbluo\Desktop\wbluo\emails\BI platform introduction.msg'>BI platform Introduction </a>



## Metric Definition

#### Visits

1. 30mins内一次点击动作，算作一次点击量

#### Pageviews

1. 每一次浏览，打开新的界面就算一次count(1)

#### Listings

1. AD:广告

## ADPO

## Kylin

1. <a href=C:\Users\wenbluo\Desktop\wbluo\emails\Kylin KT.ics>KT details</a>
2. --根
3. 预计算(sum,avg,max)，存放结果到HBase,
4. Cube 思想，由Cubeid组成；自下而上的建模roll-up
5. 砍枝，减少cube数量
6. 雪花模型（snowflow model)+星形模型（star model)

## Bridge

1. <a href='https://wiki.vip.corp.ebay.com/display/EBAYAISOPS/Teradata-Hadoop+Bridge'>bridge_between_TD_Hive</a>

2. <a href='https://wiki.vip.corp.ebay.com/pages/viewpage.action?pageId=361094825'>bridge_between_TD_Hive_2</a>

3. instantiate ：例示

   - hive -->td:

     ```sql
     --14.10
     set query_band = 'sock_timeout=120;recv_eofresponse=1;dataPath=/user/tchuang/jobs2FromVV/*;sinkClass=HDFSTextFileSink;' for session;
     select count(*) from parallel_import(
     returns p_sgteam_t.jobs2
     using
       configserver('bridge-gateway-lvs:1025')
       configname('lvs_dm_ares')
     ) bridgeimport;
     ```

4. 如何将本地文件上传

5. ```sql
   
   SET QUERY_BAND = 'sock_timeout=120;recv_eofresponse=1;childThreadCount=1;sinkClass=HDFSTextFileSink;delimiterCharCode=1;' FOR SESSION;
   CREATE MULTISET VOLATILE TABLE VST_MOTOR_BRIDGE AS
   (
   SELECT BRIDGEIMPORT.* FROM PARALLEL_IMPORT(USING OUTPUT_COLS
   (
   'clsfd_site_id SMALLINT,
   clsfd_pltfrm_id int,
   clsfd_dvic_id int,
   metric_value bigint,
   clsfd_sum_dt DATE
   ')
   configserver('bridge-gateway-mzt:1025')
   configname('mzt_dm_ares')
   dataPath('hdfs://ares-lvs-nn-ha/EDW_CLSFD/GA/VST_MOTOR_DAILY_NEW_2019_05_10/clsfd_partition_date=*/00*')  ---所有的partition_date都导入进去  支持正则表达式
   failOnNoImportFiles('false')
   --hdfsURI('hdfs://ares-nn.vip.ebay.com:8020')
   fileErrorLimit('0')
   ) bridgeimport
   )
   WITH DATA PRIMARY INDEX (CLSFD_SITE_ID) ON COMMIT PRESERVE ROWS;
   
   ```

   ![](C:\Users\wenbluo\Desktop\wbluo\hive\p_8.png)

   有时候不是分区表的时候，会bridge异常，这个时候建议采用分区表insert

6. 

## Spark

1. <a href='C:\Users\wenbluo\Desktop\wbluo\emails\Training Session on Hercules Spark-SQL and ADPO standards.ics'>KT detalis</a>
2. 叶&岑晨
3. 文件adrr
   /dw/etl/home/prod/sql/dw_clsfd



## GA Data KT

1. <https://analytics.google.com/analytics/web/#/>   ： Google analyst website;

   lkp ： select * from clfsd_tables.clsfd_ga_prfl_lkp;

   view：作为一步筛选机制，exclude,作为数据展示

   然而我们拿到的是raw 数据，并不是view数据；

   a:acountid    w： p: profileid 

   <https://ga-dev-tools.appspot.com/query-explorer/>   

   :onsite query 

2. Bigquery  and GA :

   property -->project -->因为bigquery  收费机制，所以每个site 设置了一个project

   configure bigquery link 

   

   ：当数据latency ,我们发送邮件给Google;

3. session table:     user:definition :unique cookie 

   ##30min ，半小时内是否有过操作行为；

   clsfd_session_id ,  ###primary key   利用下面四个fields  组合生成hash value;

   ga_vstr_id,[vistor]

   ga_vst_id,

   ga_prfl_id, [profile]

   clsfd_sum_dt:

   

   landing page:（pagetype ,catory,loc_id)  : is_entrance /is_exit; 判断

   

4. Meridian standard

5. Bounce Rate ： 跳转率；/转化率；

6. SPEMD 表结构优化 :--> Frodo 项目

7. 

   

   

   

   

   

   

## Pet

<a href='https://wiki.vip.corp.ebay.com/display/DW/PET+Jobs'>Pet</a>

Abinitio+shell handlers + Scheduling tools +Crontab 

Crontab.wrapper.ksh

##### 总结

- /home/chewu/etl_home/bin
- 三大ksh  
- <a href='C:\Users\wenbluo\Desktop\work\ksh\crontab.ksh'>crontab.ksh</a>
- <a href='C:\Users\wenbluo\Desktop\work\ksh\dw_clsfd.job_runner_v2'>job_runner</a>
- <a href='C:\Users\wenbluo\Desktop\work\ksh\target_table_load_handler'>target_table_load_handler</a>
- 

### Shell handler

1. single_table_extract_handler.ksh
   - 需要cfg文件，并放到cfg目录下
   - 以及dat 文件，放到dat目录下
   - sql脚本，放到sql 目录下

2. single_table_load_handler.ksh
   - 需要cfg文件，并放到cfg目录下
   - 以及dat 文件，放到dat目录下
   - sql脚本，放到sql 目录下

3. target_table_load_handler.ksh

   <a href='C:\Users\wenbluo\Desktop\wbluo\others\work_pict\target_table_load_handler.txt'>target_load</a>

   - 仅仅需要配置 sql 脚本 放到sql目录下面

### SQL/file

1. 需要将运行的脚本放到

   [/home/chewu/etl_home/sql](mailto:chewu@phxdpeetl019.phx.ebay.com:/home/chewu/etl_home/sql) 目录下

### JobRunner

  通过jobrunner跑定时任务，需要在路径下

<a href='C:\Users\wenbluo\Desktop\wbluo\others\work_pict\dw_clsfd.job_runner_v2.ksh'>job_runner</a>

cd  /dw/etl/home/prod/land/dw_clsfd/job_runner/wenbluo_test 

1. 创建目录（自定义 wenbluo_test) : 创建 cfg ,dat 两个目录

2. 最后在cfg 目录下，创建两个文件 (break_point.seq ,date_range.cfg)

   - 其中break_point.seq 记录每个任务里面，step_X运行到第X步。

     当整个运行成功，则会重置为0 ；

     **一定要有初始值**；

   - date_range 传参给${START_DT_FMT}, ${END_DT_FMT}

     **一定要有初始值；**  --不然后面sql 语句没法跑

     然后date_interval :当程序跑完之后，会重新 赋值给date_range 

   - 在  home/chewu/etl_home/cfg/jobs.dic.v2   文件添加 job :

     ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_15.png)

     文件命名要规范：必须要有前缀： dw_clsfd

3. wenbluo_test.DATE_INTERVAL  以及  wenbluo_test.STEP  前缀都要跟 目录名称 wenbluo_test一致；

4. dw_clsfd.clsfd_daily_mkt_sc_wenbluo :表示 job id 

   ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_18.png)

   ##### QA

   - dw_clsfd.clsfd_daily_mkt_sc_wenbluo  :我怎么查看该id 是那些任务，对应的sql 文件是什么?

     <span style='background-color:orange'>-->在/home/wenbluo/etl_home/cfg 查看 jobs.dict.v2 </span>

     cat jobs.dic.v2 | grep dw_clsfd.clsfd_daily_mkt_sc_wenbluo

     知道了 dw_clsfd.clsfd_daily_mkt_sc_wbluo.sql 文件

     <span style='background-color:orange'>-->最后/dw/etl/home/prod/land/dw_clsfd/job_runner</span>

     查看sql 语句；

5. td2 ：表示访问的mozart 服务器，<span style='background-color:lightblue'>在哪个环境执行</span>

6. dw_clsfd.clsfd_daily_mkt_sc_wenbluo.sql ：表示 sql 文件

**调度任务：**

/home/chewu/etl_home/bin/dw_clsfd.job_runner_v2.ksh wenbluo_test > /dev/null 2>&1 

### 执行一条指令

```shell
ksh target_table_load_handler.ksh dw_clsfd.clsfd_daily_mkt_sc_wenbluo td2 dw_clsfd.clsfd_daily_mkt_sc_wbluo.sql  start_date=2018-01-01 end_date=2019-03-27

QA :
    发现jobrunner 上面的时间参数 并没有改变：date_range.cfg
    ###因为并没有用到crontab 取调度任务，仅仅是一次临时性的adhoc
```

<span style='background-color:lightblue'>ps: 需要在 etl_home/chewu/bin  目录下运行</span>；

### Crontab 

<a href='C:\Users\wenbluo\Desktop\wbluo\others\work_pict\crontab.warpepr.ksh'>crontab_ksh</a>

```sql
---任务调度   利用crontab 设置时间调度
15 1 * * 1 /home/wenbluo/etl_home/bin/crontab.wrapper.ksh /export/home/wenbluo/etl_home/bin/target_table_load_handler.ksh dw_clsfd.clsfd_daily_mkt_sc_wenbluo td2 dw_clsfd.clsfd_daily_mkt_sc_wbluo.sql  start_date=2019/03/26 end_date=2019/03/27  > /dev/null 2>&1
###仅仅是调度一个文件，一个sql
```

```shell
00 20 * * * /home/chewu/etl_home/bin/crontab.wrapper.ksh /export/home/chewu/etl_home/bin/dw_clsfd.job_runner_v2.ksh wenbluo_test > /dev/null 2>&1
### 调度wenbluo_test  jobid 下所有的etl_id
```

--->拓展crontab.wrapper.ksh 文件

```

```



##### QA

1. home/chewu/etl_home/bin/dw_clsfd.job_runner_v2.ksh weekly_sc :运行weekly_sc目录下所有的任务；

   -->缺陷是：没法并行，只能从上至下依次   （重新整个流程 跑一遍）

   ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_20.png)

2. 如果是某一两个的任务失败；怎么运行？

   <span style='background-color:lightblue'>单独用ksh  tagert_table_load_handler.ksh  dw_clsfd.xxxx  td2 dw_clsfd.xxxx.sql  start_date=  end_date=</span>



### 查看日志：

1. 在 chewu/etl_home/log/dw_clsfd   目录下就会有对应的历史文件信息

   <span style='background-color:lightblue'>/home/wenbluo/etl_home/log/secondary/dw_clsfd</span>  在 log 目录下

   每一个job_id(etl_id) /task_id (weekly_sc_daily)的每次执行情况日志；

2. ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_17.png)

3. 也可以通过<span style='background-color:lightblue'>/home/wenbluo/etl_home/tmp/secondary/dw_clsfd</span>    在tmp 目录下

   特点只保留最近一次tmp 文件信息，一般保留的是：<span style='background-color:orange'>$start_date ,$end_date 等参数替换后sql </span>

   ## 总结

   <span style='color:red'>**任何临时性的job ，会放到前面两个目录下**</span>

4. <span style='background-color:lightblue'>**生产环境的tmp目录  (b_clsfd ) 账号  :uc4** </span>

   /dw/etl/home/prod/tmp/td2/dw_clsfd

5. 也可以通过<span style='background-color:lightblue'> /dw/etl/home/prod/land/dw_clsfd/job_runner/log</span>  在job_runner 目录下查看

   用了<span style='background-color:orange'>**job_runner.ksh 就的任务，就可以在该目录下查看执行日志**</span>

   ```shell
   cat /dw/etl/home/prod/land/dw_clsfd/job_runner/log/weekly_sc_check_20190505.log
   ```

6. /home/wenbluo/etl_home/cronlog <span style='background-color:lightblue'>**查看执行时间是多久**</span>

   <span style='background-color:lightgreen'>dw_clsfd.clsfd_cronjob.20190505.log</span>

   ```shell
   
    ###在  crontab.wrapper.ksh   #123行
    cat dw_clsfd.clsfd_cronjob.20190505.log | grep  weekly_sc
    dw_clsfd.job_runner_v2.ksh[space]weekly_sc_check,20190505-193003,20190505-204944,success
   
   ```

   

7. 

#### 查看与表相关的文件：

```shell
[chewu@phxdpeetl019.phx.ebay.com:/home/chewu/etl_home/tmp/secondary/dw_clsfd]grep -i p_clsfd_t.clsfd_reply_mth_sc *

/home/wenbluo/etl_home/tmp/td2/dw_clsfd

grep -in P_CLSFD_T.CLSFD_REPLY_MTH_SC  * | egrep -i 'create|insert|delete'
clsfd_bsc.bt.dw_clsfd.step2_bsc_reply.sql.tmp:1170:DELETE FROM P_CLSFD_T.CLSFD_REPLY_MTH_SC
clsfd_bsc.bt.dw_clsfd.step2_bsc_reply.sql.tmp:1175:INSERT INTO P_CLSFD_T.CLSFD_REPLY_MTH_SC
grep: nohup.out: Permission denied
clsfd_daily_reply.bt.dw_clsfd.step2_bsc_reply.sql.tmp:1169:DELETE FROM P_CLSFD_T.CLSFD_REPLY_MTH_SC
clsfd_daily_reply.bt.dw_clsfd.step2_bsc_reply.sql.tmp:1174:INSERT INTO P_CLSFD_T.CLSFD_REPLY_MTH_SC
x.bt.bsc_weekly_table_archive.sql.tmp:319:CREATE MULTISET TABLE CDC_SCRATCH.A_CLSFD_REPLY_MTH_SC AS P_CLSFD_T.CLSFD_REPLY_MTH_SC WITH DATA;

```

ls -l `egrep -il 'CLSFD_TRFC_SUM_RPT_MKT|CLSFD_DAILY_MKT_SC|CLSFD_WEEKLY_MKT_SC' *`



 <span style='background-color:lightgreen'>**grep -in clsfd_reply_mth_sc * |egrep -i 'insert|delete|create'**</span>

##### 总结：

1. 查看jobrunner :

   - cjob -->cd weekly_daily_sc -->job_steps.tmp文件可以查看运行那些weeklyscore card中那些daily数据

   - cd cfg  cat date_range.cfg  查看<span style='color:orange'>下次运行时间范围</span>是多少

   - 也可以在cfg 下  :特点是该文件 包含的所有jobid的配置任务，**但是内容与cjob是一样的；** 

     grep -i 'weekly_sc_daily' jobs.dic.v2 |head 

2. ```shell
   
   /home/chewu/etl_home/cfg
   cat jobs.dic.v2  |grep weekly_sc_check
   weekly_sc_check.DATE_INTERVAL=+7D#-27D
   
   
   /dw/etl/home/prod/land/dw_clsfd/job_runner/log
   cat  weekly_sc_daily_check_20190505.log
    [ INFO ] JOB_ID_HOME: /dw/etl/home/prod/land/dw_clsfd/job_runner/weekly_sc_daily_check
    [ INFO ] JOB_ID: weekly_sc_daily_check IS RUNNING...
    [ INFO ] FETCH THE DATE RANGE FROM DATE FILE: /dw/etl/home/prod/land/dw_clsfd/job_runner/weekly_sc_daily_check/cfg/date_range.cfg
    [ DONE ] [ START_DT: 20190407    END_DT: 20190504 ]
    [ INFO ] END INTERVAL: [+7D] START INTERVAL: [-27D]
   
   ```

   

   

   ##### QA:

   - job_steps.tmp是临时生成文件

     当任务失败之后，cfg目录下有个 break_point.

     ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_22.png)

   - ksh /home/chewu/etl_home/bin/dw_clsfd.clsfd_daily_motor_sc.ksh "${START_DT_FMT}" "${END_DT_FMT}"

3. 查看日志

4. 查看邮件

   ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_25.png)

5. <span style='background-color:lightblue'>**单独运行某个job :**</span>

   <span style='background-color:lightblue'>**cbin 目录下**</span>：

   ksh target_table_load_handler.ksh dw_clsfd.clsfd_daily_adrev_sc td2 dw_clsfd.clsfd_daily_adrev_sc.sql start_date='2019/03/03' end_date='2019/04/02'

   <span style='background-color:lightblue'>**运行整个job**</span>

   更改文件weekly_sc 夹 下date_range.cfg文件，

   然后再利用crontab+job_runner_v2.ksh去调度

   ```shell
   nohup ksh /home/chewu/etl_home/bin/crontab.wrapper.ksh /export/home/chewu/etl_home/bin/dw_clsfd.job_runner_v2.ksh weekly_sc_comb > refresh_comb_2019_05_22.log 2>&1 &
   ```

   

### Archive

每周归档：(snapshot)

- A:表示current_week

- B:表示week-1

- C:表示week_2 

- 保留最近三周的历史数据

- path:<span style='background-color:lightblue'>/home/chewu/etl_home/sql/**bsc_weekly_table_archive.sql**</span>

  

### 常见错误

1. ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_26.png)

   时间参数格式不对，系统默认识别： 2019-03-01  而不是别 2019/03/01





## Proxy SwitchySharp Chrome

http://www.cnplugins.com/devtool/proxy-switchysharp/

### Regular hyperlink

1. https://wiki.vip.corp.ebay.com/display/DW/KnowledgeBase+for+ECG+DW+Developers
2. <https://wiki.vip.corp.ebay.com/display/DW/Onboard+Task+List>
3. <https://wiki.vip.corp.ebay.com/display/DW/Account+And+Access>
4. https://wiki.vip.corp.ebay.com/display/DW/Classifieds

   