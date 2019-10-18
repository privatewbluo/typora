# Spark

## windows 搭载

<a href='https://blog.csdn.net/do_yourself_go_on/article/details/73129408'>spark</a>

- 安装maven ，需要将bin 文件放置到path 中；
- 安装scala ， 目前只有32x 所以不用担心，因为放到program file (x86)

- <a href='https://blog.csdn.net/do_yourself_go_on/article/details/73129408'>tutorial_one</a>
- <a href='https://blog.csdn.net/zy_zhengyang/article/details/78581283'>tutorial_two</a>
- 当发现ng command not found

## Import project 

## Spark-sql

<a href='https://spark.apache.org/docs/2.2.0/sql-programming-guide.html'>tutorial</a>

1. ![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_29.png)

   - 我已经set spark.sql.shuffle.partition=365,为什么底层文件还是365k

   - 需要添加distribute by 

     ![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_31.png)

     

2. 

## QA

- ![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_3.png)

  当出现某个partition 缺失时候，可以

  refresh table dw_clsfd.CLSFD_ECG_HIT;

  ![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_25.png)

  如果refresh 依旧报错，只能等table 写好，再去read;

- ![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_6.png)

  如何查找该cfg 

- set spark.sql.shuffle.partitions=20;  --这个参数是干撒子的哟？

  ![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_24.png)

  --》跟hive reduce 有什么区别？

- distribute by clsfd_ad_id  与 hive 上的有什么区别吗？





## Function

- windows function with having

  spark 支持 类似（ td 中的qualify ) ,然后hql 并不支持该语法；

![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_4.png)

**-->hive  并不支持**![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_5.png)





## Cli

command line interface 

1. chercules='ssh  hercules-lvs-cli.vip.ebay.com'

2. 查看confg 配置信息  ：set;   <span style='background-color:lightblue'>**---与hive 相同**</span>

   ![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_20.png)

   -->如何更改配置？

   

3. ll  *spark*

   ![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_1.png)

   每一个都是spark 执行客户端,进入spark-sql 命令行

   ![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_2.png)

4. ```shell
   /export/home/sg_adm/spark-sql-yarn -f /export/home/b_clsfd/ad_cube_daily_wenbluo_tmp.spark --conf  clsfd_start_dt='2019-07-01'  --conf  clsfd_end_dt='2019-07-01' >tmp_log_19_06.log 2>&1 &
   
   
   --hive 
   
   nohup hive -f /export/home/b_clsfd/ad_cube_daily_wenbluo_tmp.hql --hiveconf  clsfd_start_dt='2018-09-01'  --hiveconf  clsfd_end_dt='2018-10-01' >tmp_log_09.log 2>&1 &
   
   ```

   

5. 

   

   ```SQL
    SELECT
         CLSFD_INVC_ID
       , eventDateTimeLocal
       , CLSFD_SITE_ID
       , experiments as experimentArray
       , ROW_NUMBER() OVER(PARTITION BY CLSFD_INVC_ID ORDER BY eventDateTimeLocal desc) as RNK
       FROM clsfd_working.clsfd_odin_orderlog_experiment_event_nl_daily_inc
       -- select only the create event, where Order/feature is created as 1st place, and closest to the user experience
   --        WHERE lower(order.orderStatus) = 'created'
           HAVING RNK = 1
               
   CREATE TEMPORARY VIEW VW_CLSFD_ORDER_EXPRMNT_W
   AS
   SELECT
     CLSFD_FTR_ID
   , CLSFD_INVC_ID
   , CLSFD_SITE_ID
   , CAST(eventDateTimeLocal AS date)  AS INVC_CRE_DT
   , SUBSTR(CAST(eventDateTimeLocal AS STRING),12,8) AS INVC_CRE_TM
   , experiments.experimentId AS EXPRMNT_NAME
   , experiments.variation    AS BCKT_CD
   FROM clsfd_working.VW_CLSFD_ORDER_EXPRMNT_W_MP_RAW_test
   LATERAL VIEW EXPLODE(experimentArray) arr AS experiments
   ;
   cache TABLE VW_CLSFD_ORDER_EXPRMNT_W;
   
   
   set mapreduce.job.queuename=hdlq-other-default;
   set hive.groupby.orderby.position.alias =True;
   select 
   'eCG Total' as CLSFD_RPT_SITE_NAME,
   'eCG Total' as CLSFD_CLSTR_NAME,
   ACTUAL_TARGET,
   CLSFD_SUM_DT,
   9000044 as CLSFD_SITE_ID,
   sum(METRIC_VALUE) as METRIC_VALUE
   from p_clsfd_t.motor_monthly_sc
   where metric_type='Visits' 
   and ACTUAL_TARGET='Actual'
   group by 1,2,3,4,5
   ) VST_LY
   on VST.CLSFD_SITE_ID = VST_LY.CLSFD_SITE_ID
   and add_months(VST.CLSFD_SUM_DT+interval '1' day, -1)=add_months(VST_LY.CLSFD_SUM_DT+interval '1' day, -1)+interval '1' year
   
   ```

   

   




# Tunning

SPARK

## CLI

Trail one :

check the code and find one part that would be optimized 

![](C:\Users\wenbluo\Desktop\work\spark tunning\p_13.png)



 we suggest that  the step , distribute by clsfd_invc_id % 40 , would be executed at  CREARE TEMPORARY VIEW before.

## The Original Process Time Costs

the original procedure had been assigned 1592 shuffle.partitions

![](C:\Users\wenbluo\Desktop\work\spark tunning\p_2.png)

![](C:\Users\wenbluo\Desktop\work\spark tunning\p_3.png)

set spark.sql.shuffle.partitions=40;

![](C:\Users\wenbluo\Desktop\work\spark tunning\p_4.png)

![](C:\Users\wenbluo\Desktop\work\spark tunning\p_5.png)





## The Optimizes Process Time Costs

![](C:\Users\wenbluo\Desktop\work\spark tunning\p_7.png)



![](C:\Users\wenbluo\Desktop\work\spark tunning\p_8.png)



![](C:\Users\wenbluo\Desktop\work\spark tunning\p_9.png)

![](C:\Users\wenbluo\Desktop\work\spark tunning\p_10.png)









#### QA:



![](C:\Users\wenbluo\Desktop\work\spark tunning\p_6.png)

```shell
cache table clsfd_working.VW_CLSFD_ORDER_EXPRMNT_W_MP_RAW_test;
Time taken: 199.756 seconds
```





![](C:\Users\wenbluo\Desktop\work\spark tunning\p_1.png)



![](C:\Users\wenbluo\Desktop\work\spark tunning\p_11.png)

##### ![](C:\Users\wenbluo\Desktop\work\spark tunning\p_12.png)











## 测试

- **zeta_dev_clsfd_working**  ：dev的db
- 



# Kylin

<a href='http://kylin.apache.org/cn/docs/'>kylin apache documnets</a>

## Concept

### Cube

<a href='http://kylin.apache.org/cn/docs/tutorial/create_cube.html' >how to create cube  </a>

1. derived column 

   <a href='http://kylin.apache.org/docs/howto/howto_optimize_cubes.html'>how to optimize cube </a>

   - “Normal” 添加一个普通独立的维度列，“Derived” 添加一个 derived 维度，derived 维度不会计算入 cube，<span style='background-color:lightblue'>**将由事实表的外键推算出**</span>。

   - ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_103.png)

   - Theoretically for N dimensions you'll  end up with 2^N dimension combintions.

   - Hierachies:

     1. combination pruning 

        content ,country ,city  : from 2^3 -->3 

        ![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_26.png)

        Year ,Quater ,month ,week,day   from 2^5 -->5

        

     2. derived optimzation :

        First, we can remove dimensions those do NOT necessarily have to be dimensions. For example, imagine a date lookup table where keeps cal_dt is the PK column as well as lots of deriving columns like week_begin_dt, month_begin_dt. Even though analysts need week_begin_dt as a dimension, we can prune it as it can always be calculated from dimension cal_dt, this is the “derived” optimization.
     
     3. Mandatory -->强制性
     
        if a dimension is specificed  as 'Mandatory' , then all  of the combinations withou such dimension can be pruned 

2. aggregation group

   <a href='http://kylin.apache.org/blog/2016/02/18/new-aggregation-group/'>kylin-cube</a>

   ![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_7.png)

   ![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_8.png)

   more detailed please click <a href ='http://kylin.apache.org/blog/2016/02/18/new-aggregation-group/'>here</a>

3. <span style='background-color:lightblue'>**High Cardinality** </span>

   - a filed has many  unique value such as cardno ,user_id etc ,which is not adviced to aggregate 

     ![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_27.png)

4. Cube segment 

   ![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_11.png)

5. Cube bulid and job monitoring 

   <a href='http://kylin.apache.org/docs/tutorial/cube_build_job.html'>cube maintainance</a>

   - how to refresh cube  -->action - bulid -select date_range and submit

   - refer to monnitor page ,to check fresh process and status

     ![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_10.png)

6. How to use cude ?

   ![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_9.png)

   Kylin 的网页版为用户提供了一个简单的查询工具来运行 SQL 以<span style='background-color:lightblue'>**探索现存的 cube**</span>，验证结果并探索使用下一章中的 Pivot analysis 与可视化的结果集。

   ```sql
   select clsfd_sum_dt,
   sum(new_ads_total) as ads_total,
   sum(new_ads_with_reply) as ads_reply,
   max(connect_ads_total) as max_connect
   from clsfd_access_views.v_clsfd_ad_cube_daily_base
   where clsfd_suM_dt>='2019-06-01'
   group by clsfd_sum_dt
   ###因为没有cube 定义过max(connect_ads_total),所以sql 会报错
   ```

   

7. what's drived ?![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_21.png)

8. what's mandatory Cuboids 

   ![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_23.png)

9. How to editor cube ?

   when you save the cube ,<span style='background-color:lightblue'>**you can not rebulid it anymore**</span> ,i.e add new dimensions or add new measures 

   <span style='background-color:lightgreen'>**first: make this cube disabled ,and you can rebuild it again!**</span>

   ![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_13.png)

   ![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_12.png)

   ![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_14.png)

   <span style='color:purple'>**目前delete segment & Purge 都会清空cube  data  ,类似** </span>truncate table 

10. Cube  planner

   <a href='http://kylin.apache.org/cn/docs/tutorial/use_cube_planner.html'>auto to optimze </a>

   ![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_19.png)

11. Others 

   - function :measurement 

     TOP_N ：本质是sum+ order by desc 

     ```SQL
     SELECT kylin_sales.part_dt, seller_id
     FROM kylin_sales
     INNER JOIN kylin_cal_dt AS kylin_cal_dt
     ON kylin_sales.part_dt = kylin_cal_dt.cal_dt
     INNER JOIN kylin_category_groupings
     ON kylin_sales.leaf_categ_id = kylin_category_groupings.leaf_categ_id
     AND kylin_sales.lstg_site_id = kylin_category_groupings.site_id 
     GROUP BY 
     kylin_sales.part_dt, kylin_sales.seller_id ORDER BY SUM(kylin_sales.price) DESC LIMIT 20;
     ```

     Count_distinct :

     - 基于简单的sum ,max, min 建议采用 spark
     - 基于count_distinct ,建议采用mapreduce
     - 

   - how to kylin connect  hive ? or hadoop ?


### Cuboid

1. QA

   - ![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_15.png)

   - Kylin cube 的json  与 insight bulider  json 有什么不同？

     是否可以直接将 kylin cube 的json  直接copy  到 insight  bulider 上？

     ![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_17.png)

     

     show sitemap ? 有何用？

     ![](C:\Users\wenbluo\Desktop\wbluo\spark\sp_18.png)

     

     

2. fact table   & lookup table 

   -->to bulid data model 

   -->fact table as 

3. 




### Segment

1. what's is the segment ? and how to schedule it ?

   ![](C:\Users\wenbluo\Desktop\work\kylin cube\21.png)

### Model

![](C:\Users\wenbluo\Desktop\wbluo\hive\p_46.png)

- star schema  &  snowflake 









