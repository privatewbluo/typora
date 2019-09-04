#   1:instr ：instr(A,B,postition,occurance)

SELECT INSTR('hello world', 'l', 5) FROM DUAL;  左至右第五开始   ---oracle;
SELECT INSTR('hello world', 'l', -1) FROM DUAL;  右至左计数

<hr>


# 2:group  ：其实在aes (color \ fill ) 也是分组的一种形式

ggplot(Oxboys,aes(age ,height))+geom_line() 
ggplot(Oxboys,aes(age ,height,colour=Subject))+geom_line() 

<!--多了图例-->

ggplot(Oxboys,aes(age ,height,group=Subject))+geom_line() 

<img src='D:\Users\wbluo\Desktop\web html+css\pict1.png'>

<img src='D:\Users\wbluo\Desktop\web html+css\pict2.png'>



<img src='D:\Users\wbluo\Desktop\web html+css\pict3.png'>

如何为整体添加拟合曲线？

ggplot(Oxboys,aes(age ,height<u>,<em>group=Subject</em>)</u>)+geom_line() +geom_smooth(<strong>aes(group=1)</strong>>,method='lm')
​       



# 3:theme

<img src='D:\Users\wbluo\Desktop\web html+css\theme.png'>

# 4:ggplot(diamonds ,aes(cut))+geom_bar()+aes(fill='lightblue')

首先自定义含有 lightblue 字符的变量，然后将fill 映射到该变量中；由于这个变量为离散型，所以这个颜色为桃红色

                               <img src='D:\Users\wbluo\Desktop\R语言\123.png'>

# 5:histogram +density +geom_bar :

A、B：横坐标都是连续型变量，C:类别性变量 ； A:纵坐标是计数、B:纵坐标是频数/（总体*数组)



<h1> 6:geom_hist vs geom_bar </h1>

table(mpg$cyl)   ---无cyl =7 的数据

ggplot(mpg,aes(cyl))+geom_bar()  ---cyl=7 的条形图为空

ggplot(mpg,aes(as.factor(cyl)))+geom_bar()  ---横坐标无cyl=7 ；

<img src='D:\Users\wbluo\Desktop\web html+css\pict5.png'>

<img src='D:\Users\wbluo\Desktop\web html+css\pict4.png'>

ggplot(diamonds,aes(carat))+geom_histogram()
ggplot(diamonds,aes(carat))+geom_histogram(aes(y=..density..))

                                      <img src='D:\Users\wbluo\Desktop\web html+css\pict6.png'>


​           

                                      <img src='D:\Users\wbluo\Desktop\web html+css\pict7.png'>

 <h1> 7:difference between  ggplot_point and ggplot_jitter </h1>

  <p> ggplot(mpg,aes(cyl,hwy))+geom_point()</p>

<img src='D:\Users\wbluo\Desktop\web html+css\Rplot.png'>

ggplot(mpg,aes(cyl,hwy))+geom_point(position='jitter') 

 <!--等价于 ：ggplot(mpg,aes(cyl,hwy))+geom_jitter()-->

                                                                          <img src='D:\Users\wbluo\Desktop\web html+css\Rplot01.png'>

ggplot(mpg,aes(cyl,hwy))+geom_jitter()

ggplot(mpg,aes(cyl,hwy))+geom_jitter(width=0.2,height=0.5)<!--用width，height 控制离散点之间的间距-->