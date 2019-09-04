# R

## 2019/03/18

#### Change the Default Library in Rstudio

<a href='https://www.accelebrate.com/library/how-to-articles/r-rstudio-library'> Change the Default Library in Rstudio</a>  目的直接在R.3.4版本libra目录下，方便导入其他新的R版本中。	

1. Navigate to the ./library/base/R

   ![](C:\Users\wenbluo\Desktop\wbluo\R\r_1.png)

2. editor  Rprofile 

   ```R
   ## my custormer stuff
   mypaths<-.libPaths()
   mypaths<-c(mypaths[2],mypaths[1])
   .libPaths(mypaths)
   ```

3. ![](C:\Users\wenbluo\Desktop\wbluo\R\r_2.png)

4. ![](C:\Users\wenbluo\Desktop\wbluo\R\r_3.png)

5. ![](C:\Users\wenbluo\Desktop\wbluo\R\r_4.png)

   **remove.packages('Rserve')**

## 2019/03/20

#### R与Tableau

1. **please refer to 2019/04/26**

## 2019/04/11

1. rJava  reload  fail

   ```R
   library("rJava", lib.loc="~/R/win-library/3.5")
   Error: package or namespace load failed for ‘rJava’:
    .onLoad failed in loadNamespace() for 'rJava', details:
     call: fun(libname, pkgname)
     error: JAVA_HOME cannot be determined from the Registry
   ```

   first_step:

   - download Jkd

   - cfg java enviroment

   <a href='https://jingyan.baidu.com/album/6dad5075d1dc40a123e36ea3.html?picindex=1'>cfg jdk</a>

2. set operate

3. ```R
   length(union(names(data1),names(data2)))
   length(intersect(names(data1),names(data2)))
   length(setdiff(names(data1),names(data2)))
   length(setdiff(names(data2),names(data1)))
   
   ##集合运算 ： setdiff --> except 
   b=seq(1,10,3)
   diff(b)
   3,3,3,3
   ```

4. ```R
   b=round((rnorm(189,mean=100,sd=2)))  ### int((rnorm(189,mean=100,sd=2)))
   could not find function "int"   ###as.integer(b)  
   cumsum(a),cumsum(b)
   diff(b):后一位与前一位的差值
   a[which(a%%2==0)] which ：指针 
   paste(a,',',sep='') 
   ```

   

5. 

## 2019/04/16

<a href='C:\Users\wenbluo\Desktop\wbluo\R\question_1.R'>R_question</a>

1. Terminal 

   ![](C:\Users\wenbluo\Desktop\wbluo\R\r_5.png)

   

2. geom_text & geom_label

   <a href='http://www.sthda.com/english/wiki/ggplot2-texts-add-text-annotations-to-a-graph-in-r-software'>tutorial </a>

   ![](C:\Users\wenbluo\Desktop\wbluo\R\Rplot.png)

   ![](C:\Users\wenbluo\Desktop\wbluo\R\Rplot_2.png)

   require(ggrepel)![](C:\Users\wenbluo\Desktop\wbluo\R\Rplot_3.png)

3. 


## 2019/04/21

### txt 文件用excel 文件打开，如何指定encoding ？

<a href='https://superuser.com/questions/280603/how-to-set-character-encoding-when-opening-excel'>[How to set character encoding when opening Excel]</a>

![](C:\Users\wenbluo\Desktop\wbluo\R\r_6.png)

1. 指定文件保存encoding 格式

   ![](C:\Users\wenbluo\Desktop\wbluo\R\r_7.png)

2. read.csv 难道只能读csv 格式？ txt 格式就不能读取了

   read.table 又是什么读取函数？

   read.xlsx 是调取java ，经常会出现，outofmemory 当数据量很大的时候

   - 参考文献
     - <a href='http://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html'>ASCII ,UNICODE,UTF-8</a>
     - <a href='https://blog.csdn.net/kmd8d5r/article/details/79153548'>中文乱码</a>
     - <a href='https://stackoverflow.com/questions/22876746/how-to-read-data-in-utf-8-format-in-r'>[how to read data in utf-8 format in R?](https://stackoverflow.com/questions/22876746/how-to-read-data-in-utf-8-format-in-r)</a>

3. 

## 2019/04/24

today's tasks 

- apply家族  -->done
- merge left_join
- ascii unicode  utf-8   -->done
- tableau  数据源管理  -->done
- tableau  分面   -->done
- unix 后台运行 jobid  & fg bg  nohup  --done
- unix grep 的操作 --done 
- vim 的操作  --done 

### Notes

1. ANSI 

   ![](C:\Users\wenbluo\Desktop\wbluo\R\r_9.png)

2. ![](C:\Users\wenbluo\Desktop\wbluo\R\r_8.png)

   U+5973 :U+表示 unicode  5973 对应的是汉字(女)编码

   **hyperlink:**

   <p style ='text-align:center'><a href='http://www.chi2ko.com/tool/CJK.htm'>汉字对应的Unicode</a></p>

   

   

   

   

   

3. ASCII  Latin1 Unicode UTF-8

   - ASCII ：一个字节（byte)  =8 bits(位)  ： 00000000 -11111111 , 256中表达方式

     <span style='background-color:lightblue'>**但是ASCII 码一共规定了128个字符的编码**</span>，

   - GB2312 ：简体中文常见的编码方式，使用两个字节表示一个汉字：所以256*256=65536个汉字

   - Unicode 只规定了符号的二进制代码，却没有规定二进制代码如何存储（以多少个字节存储）

   - UTF-8是一种变长度存储方式，他可以使用1-4个字节表示一个字符（符号）

   

4. ```R
   Sys.getlocale() ###查看系统区域设置
   [1] "LC_COLLATE=English_United States.1252;LC_CTYPE=English_United States.1252;LC_MONETARY=English_United States.1252;LC_NUMERIC=C;LC_TIME=English_United States.1252"
   
   x3='严'
   Encoding(x3)   ###系统告知我们 x3的编码方式 UTF-8
   [1] "UTF-8"
   Encoding(x3)<-'GBK2312'  ###当我们指定x3字符串的编码方式是GBK2312时候
   x3
   [1] "ä¸¥" ##乱码
   
   Sys.setlocale('LC_CTYPE', locale = "English_United States.1252") 
   Sys.setlocale('LC_CTYPE', locale = "Chinese") 
   
   ##如果是默认Chinese :则编码方式GBK2132
   ```

   

5. ```R
   read.xlsx('C:/Users/wenbluo/Desktop/data/2018hew/2018.xlsx',sheetIndex = 1,encoding = 'UTF-8')
   ##  encoding="unknown" 系统默认是GBK2132
   ##  当知道是utf-8时候，我们指定编码方式
   
   
   ```






### Apply家族

file:<a href='C:\Users\wenbluo\Desktop\wbluo\R\apply.R'>apply_process</a>

link :<a href='https://blog.csdn.net/u012108367/article/details/80774977'>apply家族</a>

instance：<a href='https://nsaunders.wordpress.com/2010/08/20/a-brief-introduction-to-apply-in-r/'>example</a>

![](C:\Users\wenbluo\Desktop\wbluo\R\r_10.png)



- lapply --> sapply ( 新增一个参数 simplify=T)

- sapply -->vapply (新增参数，**FUN.VALUE FUN.VALUE **设置行名称)

  ```R
  l <- list(a = 1:10, b = 11:20)
  # fivenum of values using vapply
  l.fivenum <- vapply(l, fivenum, c(Min.=0, "1st Qu."=0, Median=0, "3rd Qu."=0, Max.=0))
  ```

  

## 2019/04/26

### R与tableau intergration

- <a href='https://community.tableau.com/thread/236068'>Tutorial </a>

- 在TableauDesktop中，可使用四个函数将R表达式传递给外部服务并获取结果。

  这些函数是：<span style='background-color:lightblue'>**表计算函数**</span>

  SCRIPT_BOOL  -->返回boolean 

  SCRIPT_INT   -->返回Integer

  SCRIPT_REAL  --->返回实数

  SCRIPT_STR   --->返回string 

  -->拓展 what is tablecalculation function ?

   <a href='https://onlinehelp.tableau.com/current/pro/desktop/zh-cn/functions_functions_tablecalculation.htm'> **TableCalculation Function**</a>

  -->what is tablecalculation

  <a href='https://onlinehelp.tableau.com/current/pro/desktop/zh-cn/calculations_tablecalculations.htm'>**TableCalculation**</a>

- 

- 

##  2019/06/04

R connect to Teradata

1. ```
   sqlSave(connect,data_1)
   select * 
   from p_chewu_t.data_1
   ```

   ![](C:\Users\wenbluo\Desktop\wbluo\R\r_11.png)

```
sqlSave(connect,data_1,tablename='iris_test',rownames = F)
sqlDrop(connect,'iris_test')
sqlSave(connect,data_1,tablename='iris_test',rownames = F,addPK = T)

```

![](C:\Users\wenbluo\Desktop\wbluo\R\r_12.png)

**#默认以第一个创建主键**

##addpk 参数：

logical. Should rownames (if included) be specified as a primary key?

##可以看出<span style='color:red'>无法指定哪个field 作为 PK</span>

-->

```
CREATE multiSET TABLE p_chewu_t.iris_test ,NO FALLBACK ,
     NO BEFORE JOURNAL,
     NO AFTER JOURNAL,
     CHECKSUM = DEFAULT,
     DEFAULT MERGEBLOCKRATIO
     (
      SepalLength FLOAT,
      SepalWidth FLOAT,
      PetalLength FLOAT,
      PetalWidth FLOAT,
      Species VARCHAR(255) CHARACTER SET LATIN NOT CASESPECIFIC)
PRIMARY INDEX ( Species );

###先创建框架，然后再insert  这样就可以创建自定义Pk
```

```SQL
sqlQuery(connect ,"  insert into p_chewu_t.iris_test (PetalWidth,Species)
select top 10   PetalWidth,Species
         from p_chewu_t.iris_test_2
         ")
         
```

总结：

- sqlsave 自动创建表结构，然后load 数据
- 