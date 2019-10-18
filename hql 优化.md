[TOC]



##问题：描述

报表artNova降舱降价相关投诉及应收款 (数据集id:6562)报表推送失败，原因是报表运行时间过长，导致邮件推送失败；

#### 问题code;

![](D:\Users\wbluo\Desktop\tmp\code.png)

select a.orderid,b.sequence,b.cost,g.flightagencyname,a.quantity,
case when   r.fare is not null then r.fare else b.printprice end as printprice
,b.price
from   ods_fltorderdb.o_orders as a join  ods_fltorderdb.o_flight  as b  on a.orderid =b.orderid and b.d=date_sub(from_unixtime(unix_timestamp()),1)
join  ods_fltorderdb.o_flightextend as c on a.orderid =c.orderid and c.d=date_sub(from_unixtime(unix_timestamp()),1)
join     dim_fltdb.dimfltagency as  g on a.flightagency=g.flightagency
left join ods_AccFltDB.v_TicketCheckUsedTaskInfo t on t.orderid=a.orderid and t.sequence=b.sequence 
left join 
​       (
​      select e.orderid,ticketno,fare,passengername,dport,aport,row_number() over(partition by e.orderid,dport,aport,passengername order by RequestTime desc)r 
​       from ods_fltairtickets_accfltdb.P_TicketInfoRequest  e 
​	   where  e.d>='2018-01-01'
   )r on r.orderid=a.orderid and r.ticketno=t.ticketno and r.r=1

where a.d=date_sub(from_unixtime(unix_timestamp()),1)
and upper(a.flightclass)='N'
and c.saletype='TravelPackage' 
and b.price>b.cost 
and a.orderdate>='2018-01-01'

#####问题点： 数据倾斜（不患寡而患不均）

              出现在聚合函数+分组排序+主键关联
    
    常见解决方案：
    
    set hive.groupby.skewindata=true;

　hive.groupby.skwindata=true 当选项设定为 true，生成的查询计划会有两个 MR Job。第一个 MR Job 中，Map 的输出结果集合会随机分布到 Reduce 中，每个 Reduce 做部分聚合操作，并输出结果，这样处理的结果是相同的 Group By Key 有可能被分发到不同的 Reduce 中，从而达到负载均衡的目的；第二个 MR Job 再根据预处理的数据结果按照 Group By Key 分布到 Reduce 中（这个过程可以保证相同的 Group By Key 被分布到同一个 Reduce 中），最后完成最终的聚合操作。

####更新历程 第一次尝试  去掉两个left join 

select a.orderid,b.sequence,b.cost,g.flightagencyname,a.quantity,
b.printprice
,b.price
from   ods_fltorderdb.o_orders as a join  ods_fltorderdb.o_flight  as b  on a.orderid =b.orderid and b.d=date_sub(from_unixtime(unix_timestamp()),1)
join  ods_fltorderdb.o_flightextend as c on a.orderid =c.orderid and c.d=date_sub(from_unixtime(unix_timestamp()),1)
join     dim_fltdb.dimfltagency as  g on a.flightagency=g.flightagency
where a.d=date_sub(from_unixtime(unix_timestamp()),1)
and upper(a.flightclass)='N'
and c.saletype='TravelPackage' 
and b.price>b.cost 
and a.orderdate>='2018-01-01'

![](D:\Users\wbluo\Desktop\tmp\task_1.png)





#### 更新历程 第二次尝试  去掉o_flightextend

select a.orderid,b.sequence,b.cost,g.flightagencyname,a.quantity,
b.printprice
,b.price
from   ods_fltorderdb.o_orders as a join  ods_fltorderdb.o_flight  as b  on a.orderid =b.orderid and b.d=date_sub(from_unixtime(unix_timestamp()),1)
--join  ods_fltorderdb.o_flightextend as c on a.orderid =c.orderid and c.d=date_sub(from_unixtime(unix_timestamp()),1)
join     dim_fltdb.dimfltagency as  g on a.flightagency=g.flightagency
where a.d=date_sub(from_unixtime(unix_timestamp()),1)
and upper(a.flightclass)='N'
--and c.saletype='TravelPackage' 
and b.price>b.cost 
and a.orderdate>='2018-01-01'

![](D:\Users\wbluo\Desktop\tmp\task_2.png)

#### 更新历程 第三次尝试  去掉dim_fltdb.dimfltagency

select a.orderid,b.sequence,b.cost--,g.flightagencyname
,a.quantity,
b.printprice
,b.price
from   ods_fltorderdb.o_orders as a join  ods_fltorderdb.o_flight  as b  on a.orderid =b.orderid and b.d=date_sub(from_unixtime(unix_timestamp()),1)
--join  ods_fltorderdb.o_flightextend as c on a.orderid =c.orderid and c.d=date_sub(from_unixtime(unix_timestamp()),1)
--join     dim_fltdb.dimfltagency as  g on a.flightagency=g.flightagency
where a.d=date_sub(from_unixtime(unix_timestamp()),1)
and upper(a.flightclass)='N'
--and c.saletype='TravelPackage' 
and b.price>b.cost 
and a.orderdate>='2018-01-01'

![](D:\Users\wbluo\Desktop\tmp\task_3.png)



#### 更新历程 第四次尝试  调整join 表顺序：

![](D:\Users\wbluo\Desktop\tmp\join.png)



经过前四次尝试：无明显变化；

new :查询12

  总共323 个map,87个reduce![](D:\Users\wbluo\Desktop\tmp\test_1.png)

old : 查询13

  总共290个map,87个reduce![](D:\Users\wbluo\Desktop\tmp\2.png)

发现还是没有多大改进，于是从执行过程查看问题点；



#### 更新历程 第五次尝试 explain 查看执行计划

    由old:查询13 可以知道目前最大的map+reduce 过程
    
     是Stage-Stage-7: Map: 889  Reduce: 327 
    
     Stage-Stage-8: Map: 2337  Reduce: 1009

于是通过查看执行计划，看着两个明细过程是什么：

    ![](D:\Users\wbluo\Desktop\tmp\1.png)

 发现在join 关联时候 竟然有个隐形转换 都将orderid 转化为 double 类型；

from   ods_fltorderdb.o_orders as a join  ods_fltorderdb.o_flight  as b  on a.orderid =b.orderid and b.d=date_sub(from_unixtime(unix_timestamp()),1)
join  ods_fltorderdb.o_flightextend as c on a.orderid =c.orderid and c.d=date_sub(from_unixtime(unix_timestamp()),1)
join     dim_fltdb.dimfltagency as  g on a.flightagency=g.flightagency

<p style='color:red;'>left join ods_AccFltDB.v_TicketCheckUsedTaskInfo t on t.orderid=a.orderid and t.sequence=b.sequence </p>

于是查看o_orders 表 以及TicketCheckUsedTaskInf表 中orderid 数据类型：

    ![](D:\Users\wbluo\Desktop\tmp\task_4.png)

最后发现是投诉表中mainorderid 为字符串

 ![](D:\Users\wbluo\Desktop\tmp\3.png)

![](D:\Users\wbluo\Desktop\tmp\convert.png)

 <p style='text-align:center;'>优先级：timestamp>string>(double>bigint>int>smallint)>boolean</p>

<span style='color:orange'>通过优先级可以看到：string 可以转化为double, bigint 也可以转化为double ；所以系统都隐形转化UDFtoDouble!</span>

#### 最后一次调优：

将订单表 强制转化为string然后再关联 join

原先脚本：

![](D:\Users\wbluo\Desktop\tmp\old_one.png)

新脚本：

![](D:\Users\wbluo\Desktop\tmp\task_5.png)

ps:本次调优仅仅是从代码角度，并不设置hive 执行环境调整参数；



<p style='text-align:center; font-size:20px'>spark 运行测试下：</p>

11/30周五测试：

old code ：

![](D:\Users\wbluo\Desktop\tmp\spark_1.png)

 new code: 

![](D:\Users\wbluo\Desktop\tmp\task_6.png)

12/03周一测试：

  ![](D:\Users\wbluo\Desktop\tmp\task_7.png)

#### 总结

 1：前四次尝试，在涉及多表关联时候，注意表关联顺序（大表放在后面），以及MR个数特点；

 2：通过查看执行计划，知道数据类型优先级，以及内部隐形转化；

#### 拓展

ReduceSinkOperator ：逻辑运算操作符：

 1：很简单，ReduceSinkOpertor之前的在Map执行，ReduceSinkOperator之后的在Reduce执行，ReduceSinkOperator的作用是把数据从Map发到Reduce.

 2：MAP端： from ,where , 

​       Reduce端：join ,group by ,count,distinct,having ;

##参考文献：

- https://www.cnblogs.com/kongcong/p/7777092.html
- https://blog.csdn.net/qq_34941023/article/details/71189842
- http://bigdata.51cto.com/art/201703/535606.htm
- https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Types#LanguageManualTypes-AllowedImplicitConversions
- https://yq.aliyun.com/ziliao/431843?spm=a2c4e.11155472.blogcont.17.6f4a5e9dJePhVl