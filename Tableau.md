# 	Tableau


### 参考文献

- 人人都是数据分析师：tableau应用实战

  <a  href='http://www.ituring.com.cn/book/1736'>data_source</a>

- Tableau商业分析一点通

  <a href='http://www.tableauhome.com.cn/'>data_source</a>

- <a href=' https://wiki.vip.corp.ebay.com/display/Tableau/Tableau+Developer+Knowledge+Base'>Tableau KT </a>  --待定

- <a href='https://www.w3cschool.cn/tableau/tableau_line_chart.html'>**计算字段**</a>

- 



### QA

1. <a href='https://www.cnblogs.com/sthinker/p/5965271.html'> what‘s is cube</a>
2. what's difference between Fact table and dim table?
3. Proper-nonour
   - YTD
   - YOY
   - WOW
4. 



## 第一章：入门

### Tableau工作区

- worksheet 工作表  ，又称visulazation 视图，是可视化分析的最基本单元
- dashboard 仪表盘 
- story 故事
- workbook 工作簿

### Tableau文件管理



## 第二章:场景应用

#### File attributes

| 文件类型 |          | 大小 | 内容                           |
| -------- | -------- | ---- | ------------------------------ |
| .twb     | 工作簿   | 小   | 可视化内容，但无源数据         |
| .twbx    |          | 大   | 所有信息以及资源               |
| .tds     | 数据源   | 小   | 数据源类型以及连接信息、数据源 |
| .tdsx    | 数据源   | 小   | 数据源                         |
| .tbm     | 书签     |      |                                |
| .tde     | 数据提取 |      |                                |

#### 数据角色

1. ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_49.png)

##### 度量和维度

1. ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_3.png)

2. 通过拖拽形式，更改维度或者度量![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_1.png)

3. ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_2.png)

4. 参数 

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_79.png)

5. 计算字段

6. 集合

7. 组    ---> <span style='background-color:lightblue'>**4至7 详情请看第六章**</span>

##### 字段类型转换

1. ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_4.png)



#### 创建视图

##### background 

一个完整的Tableau可视化产品由多个**仪表盘**构成，每个**仪表盘**由一个或者多个**视图**（工作表）按照一定的布局格式构成，因此**视图**是一个Tableau可视化产品**最基本的组成单元**。

<span style='background-color:lightblue'>**类似excel pivot table** </span>

**要在Tableau 中创建视图，首先需要新建一个数据源**

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_15.png)

##### Frame

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_5.png)

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_47.png)

- 页面卡 筛选卡 标记卡  

###### 页面卡

<video src='C:\Users\wenbluo\Desktop\wbluo\tableau\panel.mp4' controls='controls'>how to display this function</video>
注意事项：

1. 需要将列字段 放到页面卡上

2. 其次在选择

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_147.png)

**刚开始默认值是点移动，并没有线条；因此显示历史记录选择 全部，其次显示 （标记：只是点 ，轨迹：线条）**







- 行列功能区 试图区 
- 新建视图（工作表）  新建仪表盘 

##### 标记卡

- ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_7.png)

- ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_6.png)

- 大小

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_254.png)

  **if [Revenue Type]='Total' then 'Bold' else 'unbold' end**  --> bold flag definition

  **要区分Total & Detail 大小，这个时候可以将Bold  Flag 添加到大小中。**

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_255.png)

- 形状/shape

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_249.png)

  **只有自己选择图形类型为形状的时候，才有shape 出现！**

  <span style='background-color:lightblue'>-**->拓展如何自定义形状？**</span>

  - step1: 

    ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_257.png)

    首先在tableau 目录shapes ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_250.png)

  - step2:创建一个country 文件夹，存放图片 

    ps ：<span style='background-color:lightblue'> **图片大小建议  32*32** </span>

    ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_252.png)

  - 😁![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_253.png)

- 标签：

  - ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_78.png)

    **可以设置对齐方式以及字体大小** 

  - ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_117.png)

    <span style='background-color:lightblue'>**标签里面，不仅可以放度量单位，也可以放维度单位，并且对他计数**</span>

  - how to convert this type from left to right ?![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_135.png)

    **通过标记栏目，选择文本，设置格式**

    ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_136.png)

  - 

  

###### 工具提示

1. ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_8.png)
2. ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_153.png)
3. **也可以双击工具提示**，弹出对话框![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_248.png)



###### 详细信息

  background：

  依据拖放的字段对dashborad 进行分解细化

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_174.png)

其实下面操作同样可以<span style='background-color:orange'>**细化分解dashboard**</span>

<span style='background-color:lightblue'>**将字段拖拽到 颜色、大小、标签等分类中；**</span>

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_173.png)

###### QA:

1. 怎么没有工具提示了？![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_214.png)

2. 

   

##### 筛选器

- 展示筛选器

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_9.png)

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_10.png)

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_50.png)

- 多种筛选方式

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_10.png)

  这样是会展示出前三甲的用电情况

- <span style='background-color:orange'>**离散型和连续性 -->对于时间参数 作为筛选器 有什么不同？**</span>

  连续性变量：则会显示<span style='color:green'>**绿色**</span>

  离散型变量：则会显示<span style='color:blue'>**蓝色**</span>

  <video src='C:\Users\wenbluo\Desktop\wbluo\tableau\mv_16.mp4'>dd</video>

- 

##### 智能显示

**ctrl+1 -->筛选图片**

##### 度量名称和度量值

- 如何将多个行度量放到一个数据轴中，而不是分面![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_11.png)

  

- ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_12.png)

##### 创建仪表盘

1. 创建仪表盘也是用拖拽的方法，将**建好的工作簿（视图）**拖放到右侧排版区，并按照一定的布局排版好，**最后添加操作完成的互动设置**

##### 保存工作成果

 文件类型：

| 文件类型 |          | 大小 | 内容                           |
| -------- | -------- | ---- | ------------------------------ |
| .twb     | 工作簿   | 小   | 可视化内容，但无源数据         |
| .twbx    |          | 大   | 所有信息以及资源               |
| .tds     | 数据源   | 小   | 数据源类型以及连接信息、数据源 |
| .tdsx    | 数据源   | 小   | 数据源                         |
| .tbm     | 书签     |      |                                |
| .tde     | 数据提取 |      |                                |

#### 创建仪表盘

- 详情请看第九章

#### QA

- 工作表，仪表盘，新建故事 三者关系

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_52.png)

- 如何设置字体大小

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_53.png)

  将graphtype overview 放到文本  就可以展示出 10319559 数字

  <span style ='background-color:lightblue'>设置字体格式就行</span>：![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_54.png)

  

- 关闭工具提示

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_55.png)

- 设置图例（legend)  

  <span style ='background-color:lightblue'>**隐藏和显示图例**</span>

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_57.png)

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_56.png)

  

- 前面这个仓库图标是什么意思？![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_60.png)

  **表示筛选器联动，其他工作表也依赖这些筛选器**

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_125.png)

  

- 怎么sitetype ,metric subject 没有显示出来？![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_61.png)

  <span  style='background-color:lightblue'>**通过：取消显示标题，****就可以隐藏了**</span>

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_73.png)

  

- 如何在仪表盘添加筛选？

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_62.png)

  

  <p  style='text-align:center;background-color:lightblue'><strong>**修改 filer** </strong></p>
![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_171.png)
  
<p style='text-align:t'![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_171.png)

-->以及筛选器如何设置成联动？（多个工作表之间共享筛选器）



![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_66.png)

-->如何将筛选器放在图行上边，并且有下拉选项？

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_169.png)

<span style='background-color:lightblue'>**红色框区域：添加一个水平区域，然后将筛选器拖拽到该区域**</span>

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_170.png)

<span style='background-color:lightblue'>**选择表框下三角，然后悬着下拉列表**</span>



--->如何设置级联筛选（单个工作表，多个筛选，如何显示部分值）

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_67.png)

解决方案：

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_68.png)

然后选择小方框就行

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_69.png)



-->仪表盘联动之‘动作筛选’

<a href='http://www.a-site.cn/article/1657275.html'> web</a>









- ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_64.png)

  都实时数据，我底层数据都删掉了，还没有变化？

  哪里有更新按钮？

- 数据表格：如何设置渐变色

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_80.png)

- 对于连续性日期函数，如何精确到只看到某一天

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_81.png)

  ​                                                         **如下选择：**

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_82.png)

- 

  

## 第三章：数据连接和管理

#### Tableau的数据架构

1. Connection(数据连接层)

   - ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_13.png)

2. Data Model(数据模型层)

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_14.png)

   

3. VizQL(数据可视化层)

#### 数据连接

1. 需要在下次使用时快速打开数据连接，可以将数据连接到已保存的数据源中，操作步骤为选择“数据”>“数据名称> "添加到已保存的数据源"

2. ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_98.png)

   

3. 对于一个excel有多个sheet 时候，<span style='background-color:lightblue'>**依旧要点击两次创建新的数据源**</span>,分别当作独立的data source ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_263.png)

##### 连接服务器数据源



1. ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_91.png)

   - <span style='background-color:orange'>**查看分段和初始sql 有何作用？**</span>

     ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_216.png)

     --><span style='background-color:red'>**不可以放入多个语句**</span>

     ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_235.png)

     先进一步汇总数据，然后再进行select 操作；

     ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_217.png)

   - <span style='background-color:orange'>**实现动态数据源**</span>![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_231.png)

     

   - ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_92.png)

     连接后发现没有数据库，<span style ='background-color:lightblue'>这个时候需要自己mannul输入数据库 p_clsfd_t</span>;

     录入后，可以跨其他数据库查询，or  关联  : p_chewu_t;

   - 链接到数据库后，添加表

     ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_93.png)

   - 新建sql 自定义语句

     <span style='background-color:orange'>**创建多个sql ,关联数据集**</span>

     ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_94.png)

   Ps :链接到teradata  完全符合该数据库  sql 标准；

   <span style='background-color:orange'>**注意：** **Tableau 不会检查语句有无错误。在连接时，此 SQL 语句直接发送到数据库**。</span>

   -->拓展

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_134.png)

   <p style='text-align:center;background-color:lightblue;font-size:1.5p'><strong>怎么添加多个数据源？</strong></p> 
这样就可以自定sql语句
   

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_137.png)



3. 数据提取和数据实时

   - 数据提取一般取多少数据？

     这个时候需要添加筛选器

     编辑需要的数据

     ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_130.png)

     

   - 数据实时

4. 

##### 筛选数据

background

直接使用数据源的全量数据，在<span style='background-color:lightblue'>**视图设计\工作表**</span>的时候可能会导致工作表响应缓慢。如果仅希望对部分数据进行数据分析，可以使用**数据源筛选器**

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_16.png)

-->拓展筛选器：

- 

##### 数据整合

background 

在实际分析过程中，数据可能来源多张数据表，有可能来自不同的文件或者服务器。Tebleau的数据整合功能实现同一数据源的多表联结、多个数据源的数据融合，以及针对元数据的行列转换

1. 多表的联结

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_17.png)

   <span style='background-color:lightgreen'>**ps：同一个数据源（excel )表，多个sheet 之间的关联**</span>

   <span style='background-color:lightblue;text-align:center;'>                **拓展excel 编译后的数据集合**</span>

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_101.png)

   - 首先比源数据多了一列： 记录数
   - 其次在工作表中多了一个维度：度量名称 （包含度量框中 ：所有度量值）

   

2. 多数据源的数据融合

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_18.png)

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_145.png)

   

   --->由上面的图形可以知道的信息：

   - 蓝色标记的motors 表示是该sheet的主数据源

     橙色标记的Key metrics 表示其他数据源，用到了 clsfd_sum_dt ，cluster/site 两个字段

     前面仓库的标志：

     ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_146.png)

     这种设置：可以联动到其他sheet 中

     --->拓展

     <video src='C:\Users\wenbluo\Desktop\wbluo\tableau\mv_7.mp4' controls='controls'>sd</video>
- 双击旅客姓名和销售额，可以查看旅客的总销售额
  
- 然后点击物流信息数据源时候，发现有一个链接
  
  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_198.png)
  
  <span style='background-color:lightgreen'>**说明两个数据源之间，已经通过顾客姓名融合** </span>
  
- 然后点击物流数据源中的 运输费用
  
- <span style='background-color:orange'>**点击智能显示，选择散点图**</span>
  
- 
  
- <span style='background-color:orange'>**方法一：**</span>
  

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_19.png)

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_20.png)

​     <span style='background-color:orange'> **方法二：**</span>![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_200.png)

3. 行列转换

   step_1:![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_21.png)

   系统会自动生成两列：数据透视表字段，数据透视表数值

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_102.png)

   


#### 数据加载

Tableau 有两种数据加载方式

1. 实时连接 

   - 强调实时性
   - 强调安全性
   - 底层数据源性能优越

2. 数据提取

   step_1:

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_22.png)

   step_2:

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_23.png)

   step_3:

   保存数据以.tde格式

   step_4:

   当源数据发生改变时候，通过**刷新**数据提取保持数据更新

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_24.png)

   补充：

   向数据提取添加行

   - 从文件添加数据
   - 从数据源添加

##### QA 

1. 三种refresh 的区别？

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_205.png)

   - 第一种refresh 顾名思义：就是全部数据源的更新

   - 第二种refresh

     <span style='background-color:orange'>**并不会刷新数据集，仅仅是刷新dashboard 在工作表的更新操作**</span>

   - 第三种refresh 

     ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_207.png)

     ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_206.png)

     1. 可以通过查看历史记录，每次刷新更新数据集
     2. 拓展：<span style='background-color:lightblue'>**设置提取范围**</span>   <span style='background-color:orange'>**增量提取**</span>
        - ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_208.png)
        - ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_209.png)

   

2. 

#### 数据治理

- 替换数据源

  如果希望使用新的数据源来替换已有的数据，<span style='background-color:lightblue'>**而不希望新建工作簿**</span>。

  数据-->替换数据源

- 删除数据源

#### 总结：

- 创建数据源

  -->保存数据源

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_51.png)

- 新建工作表

- 创建视图

- 保存

- publish

#### QA

1. 连接到数据库，怎么添加自定义的sql 语句/或者新增数据

   step one:选择数据库

   step two:选择自定义sql 

2. 多个数据源？如何设置主次

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_144.png)

3. 如何关闭数据源

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_156.png)

4. 如何导出数据

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_177.png)

   <span style='background-color:lightblue'>**选择交叉表**</span>![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_178.png)

   <span style='background-color:orange'>**展示完整数据**</span>![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_234.png)

5. 

## 第四章：初级可视化分析

#### 条形图

1. 如何行列转换以及升\降序排列

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_25.png)

2. 如何堆积

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_10.png)

3. 如何设置填充颜色？

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_105.png)

   

4. 

##### QA

- ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_63.png)

  如何设置刻度线？

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_65.png)

  

- ![]()



#### 直方图

1. 堆积直方图

   background :对比13 年 14 年的销售情况

   方法一：

   采用组合图的形式：

   <video src='C:\Users\wenbluo\Desktop\wbluo\tableau\mv_2.mp4' controls='controls'>组合图</video>
方法二：
   
<video src='C:\Users\wenbluo\Desktop\wbluo\tableau\mv_3.mp4' controls='controls'>组合图</video>
   <span style='background-color:orange'>**亮点**</span>：

   - 并不是组合图形式（将14年拖拽到右侧纵坐标)，而是将14年的数据拖拽到13年左侧纵坐标

     - 系统则会生成两个值（度量名称 和度量值）

   - 通过功能栏，筛选分析中的<span style='background-color:orange'>堆叠标记</span>，取消堆叠

     默认堆叠时候，纵坐标总数值（14+13 year)

     ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_191.png)

   - ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_194.png)

     只有一个度量名称，怎么快速复制成两个？

     <span style='background-color:orange'>**通过按ctrl 键**</span>，然后将度量名称拖拽到 大小，即可  

   - 组合图

     <video src='C:\Users\wenbluo\Desktop\wbluo\tableau\mv_6.mp4' controls='controls'>dd</video>
特点：
     

右击利润轴，弹出菜单，选择【将标记移至底层】，这样折线图会在直方图前面。
     
类似**excel (至于底层)**
     
- 
  
2. 连续性变量**分桶**

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_26.png)

   最终分桶的字段，会放在维度上-->并在![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_27.png)

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_28.png)

   拓展高级应用：

   自定义分桶，通过**创建自定义字段**

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_29.png)

#### 饼状图

1. 合并小文件

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_30.png)

2. ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_118.png)

   <span style='background-color:lightblue'>**如何绘画饼**(**pie**)</span>

   - step1:将中心放置标签栏目

   - step2:再在标签栏选择饼状图

     ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_119.png)

   

3. ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_31.png)

   -->如何展示百分比

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_121.png)

#### 折线图

1. 同数轴展示

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_32.png)

2. 双组合图

   background:同一个sheet中分别用<span style='background-color:lightblue'>**两个不同的纵轴标记不同数据类型或者数据范围的折线图**</span>；

   

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_33.png)

   <span style='background-color:red'>**注意是拖到视图右侧**</span>

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_34.png)

3. 如何设置组合图

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_71.png)

   **特点是：标记栏有两行总计**

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_72.png)

4. 如何添加多个折线图

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_107.png)

   <span style='background-color:lightblue'>**将metric_name 放到标记中的 颜色盘中即可**</span>

5. <a href='https://onlinehelp.tableau.com/current/pro/desktop/zh-cn/calculations_calculatedfields_lod_fixed.htm'>折线图</a>

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_184.png)

   重要的细节：

   计算平均值后 要添加快速表计算：汇总![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_185.png)

   

   **2168=1732+435**，累计求和 running_sum

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_186.png)

   

6. 


#### 散点图

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_157.png)

1. 连续性变量，放在列|行 ，<span style='background-color:lightblue'>**选择维度**</span>

#### 标靶图

1. 提前将可提供电量（预测值） 放到标记栏中

   

2. 两个参考线

   - 第一条参考线设为“线”，范围设为“每单元格”，值设为“总记（可提供电量，标签设为“无”）
   - 第二条参考线设为“分布”，范围也是“每单元格”，值设为总计（用电量）的60和80百分比，格式设为向下填充。

   

结果：

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_106.png) 





##### QA

- 如何通过筛选器添加折线图，

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_70.png)

- 坐标轴：如何再添加标题

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_74.png)

  <a href='https://onlinehelp.tableau.com/current/pro/desktop/zh-cn/formatting_editaxes.htm#Why'>tableau </a>

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_75.png)

  <span style ='background-color:lightblue'>**通过显示标题，就能显示横坐标**</span>

- 

## 第六章：高级数据操作

<a href='https://onlinehelp.tableau.com/current/pro/desktop/zh-cn/functions.htm'> Tableau  function tutorial</a>

#### 分层结构![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_39.png)

1. hierarchy是一种维度之间的自上而下的组织结构形式：

   年-月-日  省份-地区 季度-月-日etc

2. drill-up ,drill-down :上钻、下钻

3. 创建分层结构：

   - 拖拽方式：

     ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_35.png)

     ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_95.png)

     应用：

     - 当没有添加分层时候如下；

       ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_96.png)

       <span  style='background-color:lightblue'>**各个维度前面没有  +  - 号，实现drill down - roll  up**</span> 

     - ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_97.png)

       <span  style ='background-color:orange'>**当分组后，首先不改变数据源结构，并没有新增的字段（销售产品）**</span>

     - 

   - 右键菜单创建分层结构

     ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_36.png)

   - 

#### 组![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_38.png)

background: 分组是<span style='background-color:lightblue'>**对一个字段内的内容**</span>重新分类汇总；

1. 通过在视图中<span style='background-color:lightblue'>**选择维度成员**</span>创建组

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_111.png)

2. 通过右键菜单栏分组

    <span style='background-color:lightblue'>**提供模糊查找**</span>

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_37.png)

3. 

#### 集![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_40.png)

background

根据某些条件定义数据子集的自定义字段，可以理解为<span style='background-color:red'>**维度的部分成员**</span>,用于计算，参与计算字段的编辑。

1. 集的分类：

   - 常量集

     ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_113.png)

     **与创建组不一样，手工选择列维度（而组是选择 行维度）** 

   - 计算集

     - ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_114.png)
     - 最终显示为集内/集外 --维度数据

     ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_122.png)

     ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_213.png)

     ##### QA:

     1. 有什么作用?分集内、集合外？
     2. 

   - 合并集

     - ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_115.png)
     - 

     

2. 

#### 参数

应用场景：

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_124.png)

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_127.png)

<span style='background-color:orange'>**实现动态数据源**</span>![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_231.png)



1. ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_109.png)

2. <span style='background-color:lightblue'>**筛选器**</span>

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_232.png)

   **该参数作用： 通过user 自己选择是想看week_id 数据 还是 WEEK_YOY数据，**

   **依据筛选，展示listings 就不同；**

   

3. ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_123.png)

4. 如何添加参考线？or 参数区间？

   <span  style='background-color:lightgreen'>**点击横坐标轴，右击添加参考线**</span>![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_204.png)

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_202.png)

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_203.png)

5. <span style='background-color:orange'>**参数格式设置**</span>

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_233.png)

   

6. <video src='C:\Users\wenbluo\Desktop\wbluo\tableau\mv_9.mp4' controls='controls'>sdd</video>

7. 



#### 分桶

<video src='C:\Users\wenbluo\Desktop\wbluo\tableau\mv_10.mp4' controls='controls' >dd</video>
### 总结

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_112.png)

**维度，度量，集，参数**

- 

#### 计算字段

##### 别名（alias)

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_266.png)

<span style='background-color:lightblue'>**所以可以解释为什么 metric_name 会有两个 private live listings**</span>

1. ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_41.png)

2. ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_42.png)

3. <span style='background-color:lightgreen'>**注释： //**</span>

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_175.png)

4. ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_221.png)

   <span style='color:red'>Total Tables  是依据维度计算的，而不是计算整个表的 clsfd_table_name </span>

5. 

   

##### 特殊函数

1. 表计算

   - 快速表计算

     1. ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_43.png)
     2. 

2. 百分比函数问题

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_155.png)

   原本percentage :计算是简单相加（error)

   需要手工添加计算字段 （percentage2) ,<span style='background-color:lightblue'>**分子相加和/分母相加和**</span>

3. 

###### 字符串函数

- mid+ find 

  **MID([CLSFD_TABLES],1,find([CLSFD_TABLES],'.',1)-1)**

- split+int 函数

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_110.png)

- 

###### 日期函数

###### 数值运算

- 累计求和  running_Sum(sum (profit))
- ZN:if Null then Zero
- index () :返回当前行数
- size ()：返回分区中的行数

##### 详细级别表达式 （LOD 表达式）

**Level of Detail Expressions** 

总结：

Include  和 exclude 创建形成的计算字段**只能当作度量使用**，而fixed函数创建形成的计算字段可用作与**维度或者度量**

可以多个dimensiion 

<span style='background-color:orange'>**{include [dim1,dim2]:sum([dim])}**</span>

- INCLUDE

  计算人工服务接听量时候，是以员工工号分组 计算

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_183.png)

  <span style='background-color:lightblue'>**可以知道整体组成绩 是41.462 ，但是单个员工的成绩 是38.4667**</span>

- EXCLUDE

  EXCLUDE 详细级别表达式对于<span style='background-color:lightblue'>**“占总计百分比”或**“**</span>与总体平均值的差异”方案非常有用

  

  {EXCLUDE [地区]:sum([销售额])}

  汇总销售额时候，<span style='background-color:lightblue'>**group  by  不考虑区域维度**</span>

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_189.png)

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_190.png)

  

- FIXED

  1.  FIXED 详细级别表达式不考虑视图详细级别

     {FIXED [地区]:sum([销售额])}  /// 以区域计算销售额

     ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_188.png)

  2. 

#### 变换

1. 变换日期型字段

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_45.png)

2. **Excel分列**![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_46.png)

#### 参考线&参考区间

1. ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_48.png)

   两种添加参考线方式 

   - 点击**数据窗口**旁边的 **分析窗口**

   - **点击坐标轴添加**

     ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_197.png)

     

2. <video src='C:\Users\wenbluo\Desktop\wbluo\tableau\m_5.mp4'  controls='controls'>ss</video>

3. ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_237.png)

   如何在 app listings 添加垂直参考线，类似App replies ，这样减少一个个去核实site 是否有最新数据

   <video src='C:\Users\wenbluo\Desktop\wbluo\tableau\mv_15.mp4' controls='controls' >sd</video>

4. 

#### 表计算 以及表计算函数

- -->拓展 what is tablecalculation function ? 表计算函数
- <a href='https://onlinehelp.tableau.com/current/pro/desktop/zh-cn/functions_functions_tablecalculation.htm'> **TableCalculation Function**</a>
- -->what is tablecalculation  表计算
- <a href='https://onlinehelp.tableau.com/current/pro/desktop/zh-cn/calculations_tablecalculations.htm'>**TableCalculation**</a>

##### 寻址分区

- 寻址

  执行表计算所针对的其余维度称为**寻址字段**，可<span style='background-color:orange'>**确定计算方向,**</span><span style='background-color:lightblue'>**是表结构**</span>

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_44.png)

- 分区

  计算分组方式的维度称为**分区字段**,<span style='background-color:lightblue'>**是对计算对象进行分组的维度字段**</span>

共性：都是维度，主要针对接下来要讲的 **特殊维度**

###### 特殊维度

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_168.png)

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_181.png)

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_181.png)



##### Practice

- 表区域

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_164.png)

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_176.png)

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_182.png)

  

  

  

- 块区域

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_165.png)

- 特定维度

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_167.png)

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_168.png)

- 快速表计算

  

1. d

   ##### QA

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_158.png)



#### 快速表计算

1. 

## 第七章：高级可视化分析

#### Pareto(帕累托)

- 二八原则
- 



#### 盒须图

- <video src='C:\Users\wenbluo\Desktop\wbluo\tableau\mv_4.mp4'  controls="controls">sd</video>

- 箱线图 （min ，qrt1 , median ,qrt3,max )   outLier 

- 拓展：散点图+箱线图的组合



#### 瀑布图



![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_212.png)









#### 甘特图 （Gattn Char)

background:

甘特图来显示事件或活动的持续时间



case one : 活动的持续时间

<a href='https://onlinehelp.tableau.com/current/pro/desktop/zh-cn/buildexamples_gantt.htm?_ga=2.181690983.1548169796.1559792461-1365825564.1553056591'>follow_up</a>

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_229.png)

pict1：left ,可以知道总体各个类型 在不同快递方式下，发货的速度。---standard class 最花费时间

pict2: right,可以具体到周，每个ship mode  的发货速度

ps: 此时区分大小的是连续性变量  :avg（ordertoship )

<span style='background-color:orange'>**overtoship:datediff("day",[订单日期],[发货日期])**</span>

case two:

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_228.png)

可以知道比如site  :Austrila  table :CLSFD_ADXCHNG_ADUNIT_REV:第三天，第六天 （red ) 出现一天的时间延迟，第4、5、9、10 出现了1天以上（orange )的延迟.

<span style='background-color:orange'>**时间轴timesheet 形式，告知情况**</span>

ps :细节
![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_230.png)

通过markup 控制单元格大小（eg 1:表示一天宽度），再利用color去填充大小

summary :两个重要维度

1. 一个是列维度一般为连续性时间维度
2. 一个是控制甘特图大小的size  ：标记卡一栏

## 第八章：统计分析

#### Tableau 与 R  语言

background:

Tableau 可以利用四大表计算函数与R的脚本实现集成<--> 通过Tableau的表计算函数把待处理的数据以参数形式传递给R，R利用函数和包来处理数据或建立预测模型，然后把处理结果或者预测模型的输出结果通过表计算函数返回给Tableau ，最后Tableau对结果进行可视化展示和分析。

## 第九章：分析图表整合

### 仪表盘

**Interface**:

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_58.png)

**一个仪表盘最多显示4个以内的图标；**



#### 布局容器

background:仪表盘的基本构成单元，分为**水平和垂直**两种；

- 水平容器(横向从左至右布局)
- 垂直容器(垂直从上至下布局)

布局方式

- 平铺
- 浮动（float）

操作步骤

1. 准备好零件---工作表

2. ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_59.png)

3. 布局区域应该遵循如下操作规则

   <span style='background-color:lightblue'>**总结： 从上至下，从左至右**</span>

   - 添加水平容器
   - 在水平容器添加内容
   - 在最右侧添加垂直容器
   - 在垂直容器中添加内容

4. d

#### 交互操作

<span  style='background-color:lightblue'>**三种形式： 筛选器、突出显示、URL**</span>

background:

虽然在一个仪表盘内，同时可以查看多张工作表，从不同的角度去分析公司的经营情况，<span  style='background-color:lightblue'>**但是希望多表之间联动**</span>；

- ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_89.png)

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_90.png)

  - 筛选器

     即选择某张工作表上某个点或几个点时候，其他工作表也能显示某个点或某几个点的数据

  - 突出显示

    ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_261.png)

    1. **当选择2019年，其他三个dashboard 也突出显示**
    2. 

    ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_260.png)

  - URL

- <video src='C:\Users\wenbluo\Desktop\wbluo\tableau\mv_8.mp4' controls='controls'>dd</video>

- 

<span style='background-color:lightblue'>**快速生成联动**</span>

![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_262.png)

**当选择当作筛选器时候，系统就会自动创建操作,对该dashboard 所有workbook设置联动**

##### QA

- 怎么隐藏仪表盘中的工作表？

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_76.png)

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_77.png)

- 怎么从图一设置成图二：

  <span style='background-color:lightblue'>**垂直分布变成水平分布**</span>

  图片一：![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_83.png)

  图片二：

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_84.png)

  <span style='background-color:lightblue'>**拖拽至关键KPI -- 电网基建，右侧阴影部分，就能实现水平分布**</span>！

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_85.png)

- 如何显示筛选器

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_86.png)

- <span style='background-color:lightblue'>**如何筛选器设置联动，在dashboard界面端**</span>

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_87.png)

- 如何显示横坐标 ？

  **点击列维度 --》显示标题**

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_126.png)

  

- 如何设置调节两个panel 大小时候，两张图能自动调节大小？![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_138.png)

  选择取消固定宽度

  ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_139.png)

  

- **如何知道该筛选器是哪个dashboard?**![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_219.png)

- 





## 第十章：分析成果共享

1. ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_103.png)
2. 注意事项
   - ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_104.png)
   - 
3. 

## 格式设置

1. ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_58.png)

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_131.png)

   -->是否有格式刷功能？

   

2. 固定宽度

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_143.png)

3. **滚动条如何设置联动**![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_148.png)

   没法设置联动，可以直接设置  --> **整个视图，这样就消去滚动条**

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_247.png)

   

4. **设置单元格大小**![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_149.png)s

5. **设置单元格内字体大小**

   <video src ='C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_video.mp4' controls='controls'>hi</video>

6. **设置dashboard各个面板**

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_150.png)

7. ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_151.png)

   该界面有两个问题：

   - 滚动条无法联动
   - 其实都是统计维度，可以放到一个表中

8. **调整视图大小**

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_195.png)

   当列维度很多时候，系统会自加滚动条，如何在没有滚动条情况下，能全部展示所有维度数据？

   <p style='background-color:lightblue;text-align:center;font-size=2p'>选择视图</p>
![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_196.png)
   

   
9. 设置网格线

   ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_218.png)

10. 如何设置纵坐标轴范围

    ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_220.png)

    **选择编辑轴，然后将包含零 取消  -->系统就自动生成纵轴范围**![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_251.png)

    

11. <span style='color:red'>**如何设置 √ × 符号**</span>![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_222.png)

    <video src='C:\Users\wenbluo\Desktop\wbluo\tableau\mv_12.mp4' controls='control'>legend</video>

12. legend  

    ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_223.png)

    如何显示或者隐藏图例？

    <video src='C:\Users\wenbluo\Desktop\wbluo\tableau\mv_11.mp4' controls='control'>legend</video>

13. **如何设置refresh date？**

    ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_224.png)

    ![tableau_243](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_243.png)

    选择标记卡中的 文本操作，设置文本格式

    

    

    

14. **更改别名**![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_227.png)

15. 设置数字格式：连续型or离散型

    字体颜色都不一样：<span style='background-color:lightblue'>**离散型是蓝色，连续性是绿色**</span>

    <video src='C:\Users\wenbluo\Desktop\wbluo\tableau\mv_14.mp4' controls='controls'>ss</video>

16. <span style='background-color:orange'>**如何调整列字段顺序**</span>

    ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_236.png)

17. 日期格式的选择

    ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_238.png)

    <span style='background-color:lightblue'>**当时间范围过长的时候，建议采用相对日期**</span>

    <video src='C:\Users\wenbluo\Desktop\wbluo\tableau\mv_17.mp4' control='controls'>data filter</video>

18. 设置表结构的最大 行字段个数

    ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_239.png)

    ![tableau_240](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_240.png)

19. 如何添加汇总行/列

    **选择分析**，**将合计拖拽到图标区**![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_244.png)

    <video src='C:\Users\wenbluo\Desktop\wbluo\tableau\mv_20.mp4'> tutorial</video>

20. **如何添加两种不同类型的颜色？**![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_258.png)

    

    ![tableau_259](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_259.png)

    ![tableau_260](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_260.png)

21. 

22. 如何设置网格线

    将下面的横坐标的线条去掉。

    ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_245.png)

    ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_246.png)

    

    

23. 如何添加title ,

    ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_264.png)

    自定义一个维度

    ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_265.png)

24. 

    ![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_225.png)

<video src='C:\Users\wenbluo\Desktop\wbluo\tableau\mv_13.mp4'  control='controls'>refresh_date</video>
![](C:\Users\wenbluo\Desktop\wbluo\tableau\tableau_226.png)











e