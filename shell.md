psReference 

1. 《Unix/Linux/Osx 中的Shell 的编程》
2. 《Linux Shell 命令行及脚本编程实例详解》

# Unix/Linux

**<span style='color:orange'>linux中一切皆文件</span>**

 ref:<a href=http://www.tutorialspoint.com/unix/unix-useful-commands.htm>common unix shell commands</a> &<a href=https://wiki.vip.corp.ebay.com/display/DW/Unix> wiki_unix</a> 

1. Practice:

   https://wiki.vip.corp.ebay.com/display/DW/Unix

2. **文件特点可以不指定 文件格式，不同的软件（notepad++,word,记事本 etc) 都可以打开**






# Linux

### background

- window vs linus vs unix
  1. window 内核是nt(new technology) ,linus 内核是shell ,后者开源；
  2. unix 是linus的父亲，其中linus是开源，是类unix操作系统

- slash 正斜杠(/),back-slash 反斜杠(\\)

- 虚拟机和操作系统

- 指令+  --help ，可以查看其他选项命令含义：

  mkdir --help 

  wc --help 

  -->拓展：<span style='background-color:lightblue'>man(manul) 指令</span>

  ```shell
  man wc 
  -l :line
  -w :words
  -c :bytes;
  -m :chars;
  
  ```

  man mkdir   man  rename :与上面等价

- 

### File attributes

<a  href='http://www.runoob.com/linux/linux-file-attr-permission.html'> W3CSCHOOL</a>

- ls -l :查看文件属性以及文件所属的用户和组别

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_16.png)

  备注：

  - ls 是简单有什么文件：(list simple )

  - ll  是列出文件具体信息（d l -  owner  size  date )

  - 其中可以一定规则输出结果：

    <a href='https://blog.csdn.net/vickey1018/article/details/47759719'>ls命令指令</a>

    后面的 a  l lt ltr : 都是 <span style='color:orange'>'**指定选项**'</span> ：  通过  “- ”  letter 操作；  **命令选项**

    <a herf='<https://www.rapidtables.com/code/linux/ls.html>'> For list_details</a>

    | ls -a        | 输出所有文件包含隐藏文件                                     |
    | ------------ | ------------------------------------------------------------ |
    | ls -l        | 输出明细信息                                                 |
    | ls -lt       | 以时间顺序输出（默认倒叙，从大到小）                         |
    | ls -ltr      | 以时间顺序倒序（从小到大）                                   |
    | ls  etl_home | 可以直接指定目录，而无需先cd  etl_home  再ls                 |
    | ls -i        | 返回节点node                                                 |
    | ls -h        | ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_138.png)h: human |

    标框部分显示为 隐藏文件:  文件前面有一个.![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_41.png)

  - ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_52.png)

  

  ​	**d:表示目录， - :表示文件, l :表示链接文档**

  1. 在linux中每个文件有所有者、所在组、其他组的概念
     - 所有者： 文件创建者
     - 当**某个用户**创建一个文件之后，这个文件的所在组就是该用户所在的组

  **ll test_5.sh  -->查看文件**

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_22.png)

  [rwx]:表示read ,write ,execute

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_17.png)

  [1-3]:user ： U  ,[4-6] group  : G ,[7-9] others  :O

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_57.png)

  ---》拓展如何ls 只看某文件类型

  ```shell
  [wenbluo@phxdpeetl019 sql_test]$ ls -l
  total 148
  -rwxrwxr-x 1 wenbluo wenbluo  19909 Mar 27 22:58 clsfd_daily_mkt_sc_bak.sql
  -rw-rw-r-- 1 wenbluo wenbluo      0 Mar 28 19:00 crontab_chewu
  -rw-rw-r-- 1 wenbluo wenbluo      0 Apr 12 00:52 CURRENT_DATE
  -rwxrwxr-x 1 wenbluo wenbluo 112148 Apr 15 23:36 dw_clsfd.job_runner_v2.ksh
  drwxrwxr-x 2 wenbluo wenbluo   4096 Apr 12 01:07 huangxiaoxia
  drwxrwxr-x 2 wenbluo wenbluo   4096 Apr 24 01:44 test
  drwxrwxr-x 2 wenbluo wenbluo   4096 Apr 16 00:13 wbluo
  ###只看文件
  [wenbluo@phxdpeetl019 sql_test]$ ls -l |grep ^-
  -rwxrwxr-x 1 wenbluo wenbluo  19909 Mar 27 22:58 clsfd_daily_mkt_sc_bak.sql
  -rw-rw-r-- 1 wenbluo wenbluo      0 Mar 28 19:00 crontab_chewu
  -rw-rw-r-- 1 wenbluo wenbluo      0 Apr 12 00:52 CURRENT_DATE
  -rwxrwxr-x 1 wenbluo wenbluo 112148 Apr 15 23:36 dw_clsfd.job_runner_v2.ksh
  
  ####只看目录
  [wenbluo@phxdpeetl019 sql_test]$ ls -l |grep ^d
  drwxrwxr-x 2 wenbluo wenbluo   4096 Apr 12 01:07 huangxiaoxia
  drwxrwxr-x 2 wenbluo wenbluo   4096 Apr 24 01:44 test
  drwxrwxr-x 2 wenbluo wenbluo   4096 Apr 16 00:13 wbluo
  
  ```

  ​     -->拓展 ：

  -   <a href='https://blog.csdn.net/chienchia/article/details/39098925'> 字符设备和块设备的区别</a>
  - 块设备：硬盘（有固定的chunks)，可以随意访问
  - 字符设备：键盘，提供字符流，不能随意访问

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_21.png)

  

  **wenbluo :表示文件所有者**

  **1049089 ：表示用户所在的组**

  

- ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_18.png)

- /tmp 文件：

  是Unix系统专门用于放置临时文件的目录，每次系统重启时候，其中的内容都会被清空。

  

- /dev/null :

  是Unix系统一个特殊文件，任何人都可以读取或写入。向该文件写入的任何东西都会消失，**就像一个巨大的黑洞；**

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_48.png)

- <span style='background-color:orange'>**.ksh  .sh what's difference** </span>?

  1. In Linux extensions generally have no purpose like they do in DOS/Windows so aren't required. If someone puts an extension on a file they're just trying to let people know what it is without having to open it or run the "file <file>" command on it.
  2. ksh (kore shell ) is a superset of sh with a lot of compatability
  3. ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_68.png)

  ```shell
  ksh target_table_load_handler.ksh dw_clsfd.clsfd_daily_mkt_sc_wenbluo td2 dw_clsfd.clsfd_daily_mkt_sc_wbluo.sql  start_date=2018-01-01 end_date=2019-03-27
  ```

  

- 

### Linux远程登陆

1. putty远程登陆linux服务器

2. <a href=http://www.runoob.com/linux/linux-remote-login.html>putty_tutorial</a> &<a href='https://blog.csdn.net/eastmount/article/details/52753135'>CSDN</a>

   Linux在服务器端应用的普及，Linux系统管理越来越依赖于远程,在各种远程登录工具中，Putty是出色的工具之一,然后Putty是最常见的一个SSH工具

#### <a href='https://wiki.vip.corp.ebay.com/display/DW/Access+eBay+Hadoop+with+Unix#AccesseBayHadoopwithUnix-getbastionaccess'>putty connect</a>

##### Tunnel

<span style='background-color:lightgreen'>**可以download MTputty  (可以一个窗口 管理多个putty)**</span>

- ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_71.png)

  SSH默认端口号 ：22

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_94.png)

- ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_70.png)

  31003:SOURCE Port : 表示隧道入口

  Phxdpeetl019.phx.ebay.com.22:表示隧道出口

  然后Tunnels:作用是远程主机有firewall 时候，可以创建tunnel 形式连接到主机上。

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_87.png)

  127.0.0.1:表示localhost  <span style='background-color:orange'>**不需要联网，都是本机访问**</span>.

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_86.png)

  <span style='background-color:orange'>**s设置每隔60s发送一次数据包，保持连接**</span>
  
  <span style='background-color:lightblue'>**以及在.ssh目录下，添加文件 conf  ServerAliveInterval** 2</span>
  
  
  
  ```shell
  ssh-keygen -R "you server hostname or ip"
  
  --to generate the new/valid key
  ```

##### Copy and paste

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_88.png)

   Compromise: 左键选择，右键粘贴

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_91.png)

**如何实现复制的时候 只复制有数据的，而不是跟图一一样，整行复制**

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_90.png)

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_92.png)



##### Apperance 

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_89.png)

设置字体大小以及输入提示符 

##### Back 备份

win+run   :regedit

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_93.png)

选择putty -->export ，

<span style='background-color:orange'>**保存注册表**</span>



##### 

- <span style='background-color:lightblue'>**通过跳板机baston 再远程SSH 到  phx019 服务器**</span>

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_72.png)

1. <a href='https://wiki.vip.corp.ebay.com/display/BatchPlatform/DW+UC4+SSH+Tunneling+Setup+and+Instructions'>wiki_putty_SSH_Tunnels</a> 

2. SSH(security shell )

   <a href='<https://www.cnblogs.com/ftl1012/p/ssh.html>'>ssh</a>

   - 远程连接主机的用于应用层和传输层的安全协议

     

3. 

### 公钥和私钥

#### <span style='background-color:lightgreen'>**SSH 通信**</span>   

winscp 本机传送文件  互信机制  ，集群内部之间是通过（ssh) ，cli 与集群之间是

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_156.png)

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_157.png)

<span style='background-color:lightgreen'>实际上，**OpenSSH 7.0**及以上版本默认**禁用**了`ssh-dss`(DSA)公钥算法,都才作用RSA方法</span>

1. RSA & DSA

- <a href='https://www.cnblogs.com/dalaoyang/p/9928806.html'> key </a>

  ```shell
  ssh-keygen ##生成公钥和密钥  ## key generate 
  cd /home/wenbluo/.ssh/
  [wenbluo@phxdpeetl019 .ssh]$ ll
  total 12
  -rw------- 1 wenbluo wenbluo 1675 Mar 21 00:34 id_rsa#密钥
  -rw-r--r-- 1 wenbluo wenbluo  415 Mar 21 00:34 id_rsa.pub ##公钥
  -rw-r--r-- 1 wenbluo wenbluo 2477 May  8 02:33 known_hosts
  
  ##打开服务器B，将刚刚在服务器A内复制的内容（公钥）追加到/root/.ssh/authorized_keys
  ##这样ssh  到服务器B 时候就无需再次输入密码；
  
  ssh-keyscan  github.corp.ebay.com
  ssh [-T/t] git@github.com  ：查看是否能ssh 通
  ssh -v git@github.com  :查看具体过程
  ssh-agent ：打开代理
  ssh-add /c/Users/wenbluo/.ssh/id_rsa :将密钥添加到代理中
  
  
  
  ```

  ssh -vT git@github.com

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_152.png)

  <a href='http://yijiebuyi.com/blog/4b5c272e7058cb331098250c8e98eb3e.html'>ssh-agent </a>

   <span style='background-color:lightgreen'>**eval $(ssh-agent)**</span> :开启agent

  background: 管理密钥，当需要验证时候，自动将public key 与private key 匹对，减少重复输入密码。

  <span style='background-color:lightgreen'>**ssh-add /c/Users/wenbluo/.ssh/id_rsa   :添加密钥 给agent**</span>

- ssh 之 kown_hosts 的作用

  

- ssh chewu@phxdpeetl019.phx.ebay.com

- ```shell
  ssh -L 9000:kylin.rno.corp.ebay.com:80 liapan@phxbastion100.phx.ebay.com
  ```

  

- 退出远程连接 ： exit

- ssh -4 -L 9000:kylin.rno.corp.ebay.com:443 wenbluo@phxbastion300.phx.ebay.com

- 如何让其他人可以免密ssh登陆我的机子

- 如何生成多个ssh-keygen ?

  1. <a href='https://blog.csdn.net/yanzhenjie1003/article/details/69487932'>多个git 账号</a>

  2. ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_159.png)

     ssh -T git@github.com 

     ssh -T git@github.corp.ebay.com 

     

  3. <a href='https://blog.csdn.net/qq_41979043/article/details/83046336'>一个git 多个仓库</a>

  4. 最终clone 也可以采用 https or ssh

     ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_160.png)

     <a href='https://blog.csdn.net/x6_9x/article/details/60873021'>ssh 通过http 代理</a>

     



##### hyperlink

1. <a href='https://www.cnblogs.com/softidea/p/6855045.html'>ssh -t </a>
2. <a href='C:\Users\wenbluo\Desktop\wbluo\shell\ssh_hive'>ssh_hive</a>



### Unix 创建软链

<a href=https://www.cyberciti.biz/faq/creating-soft-link-or-symbolic-link/>soft_symbolic_link</a>

1. l :表示link  **类似window的快捷方式** 

   创建连接方式：

   <span style='color:red'>连接到file1 并命名为link1</span>

   **$ ln -s file1 link1**

   **bin :表示 link 1（ 别名）**

   **-->表示 bin 实际对接路径**(<span style='background-color:lightblue'>**file1**</span>)

   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_26.png)

2. 

3. 

# Shell

#### 其他快捷方式：

1. 查看时间 : date    

   --> 拓展：<span style='background-color:lightgreen'>**date + ‘format’ 以特定格式输出**</span>

   date +'%Y%m%d-%H%M%S'

   20190415-215810

   ```shell
   [wenbluo@phxdpeetl019 ~]$ date +'%Y%m%d-%h%M%S'
   20190415-Apr1924
   [wenbluo@phxdpeetl019 ~]$ date '+%Y%m%d-%h%M%S'
   20190415-Apr1935
   ###两者等价，一般后者最好 这样git 上也能使用；
   ```

   --><span style='background-color:lightblue'>**shell 日期区分大小写**</span>：

   | %Y   | 年份(以四位数来表示) |
   | ---- | -------------------- |
   | %y   | 年份(以00-99来表示)  |
   | %d   | 以0-31表示           |
   | %D   | 含年月日             |
   | %m   | 月份(以01-12来表示)  |
   | %M   | 分钟(以00-59来表示)  |
   | %H   | 小时(00-23来表示)    |

   

   

2. 查看谁登陆 ： who 

3. 查看登陆用户名： whoami

#### Alias

#### 

1. <span style='color:orange;text-align:center'>临时设置，重启后无效</span>![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_44.png)

2. 如何长期有效？因为上面用alias方法 ，当再次重启时候 就没有了；

   通过编辑 /home/wenbluo  目录下的隐藏文件 ： vi  .bashrc

   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_45.png)

   <span style='color:orange'>然后重启，就能访问这些快捷键</span>；

   或者采用  <span style='background-color:lightorange'>.  $HOME/.bashrc</span> 

   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_59.png)

3. <span style='background-color:lightorange'>**unalias  ccfg** </span> 可以删除 别名；但是系统重启之后仍旧还有ccfg ,可以从bashrc根本删除

4. alias 可以查看有那些别名

5. <span style='background-color:lightblue'>**env** </span> 可以查看所有的系统变量

## .Profile

1. 环境变量$HOME $PATH $PS1$PS2 $CPATH

   以bash (bourn shell 为例子)：修改.bashrc 文件
   修改命令行提示符（cli command line  ：Ps1)

   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_65.png)

   ```shell
   
   [wenbluo@phxdpeetl019 ~]$ . .bashrc
   wenbluo hellp
   
   ```

   <span style='background-color:lightblue'>**此时提示符 变成wenbluo** </span>

   ##### FUN:

   <span style='background-color:orange'>**这个怎么设置？开机登陆**</span>

   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_74.png)

   

2. 开机读取文件：.profile  .bash_profile .bashrc

3. 当没有该文件时候，可以自己vi 创建属于自己的文件

4. 

#### 拓展

- 1. 

     what‘s difference between .<span style='background-color:lightgreen'>**.profile ,.bash_profile,.bashrc**</span>

     - .bash_profile or  .profile is read by login shell , along with .bashrc 

       --->登陆shell 以及.bashrc  读取.bash_profile , .profile

       --->子shell 只读取.bashrc

       --->

       ```shell
       .bash_profile` and `.bashrc` are specific to `bash`, whereas `.profile` is read by many shells in the absence of their own shell-specific config files.
       ```

       <span style='background-color:orange'>**经过测试：文件夹下文件都有的时候，系统默认（本机是bash) ,先读.bash_profile**</span>

       ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_101.png)

       ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_62.png)

       ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_63.png)

       -->拓展：

       - 在.bashrc 中设置了命名：

         ```shell
         alias ccfg='cd /home/wenbluo/etl_home/cfg'
         ```

         在.bash_profile中设置了命名：

         ```shell
         alias ccfg='cd $HOME/etl_home/dbc'
         ```

         <span style ='background-color:lightorange'>我们会发现开启shell 时候</span>，<span style='background-color:lightblue'>ccfg 是   /dbc</span>

         -->  <span style ='background-color:yellow'>**bash_profile  包含  .bashrc**</span>

         <span style='background-color:orange'>**-->说明每次开启shell ,系统都会读取.bashrc  以及 .bash_profile**</span>

         -->如果先写ccfg 再调用.bashrc文件会怎样？
       
         ```shell
         # .bash_profile
         alias ccfg='cd $HOME/etl_home/dbc'
         # Get the aliases and functions
         if [ -f ~/.bashrc ]; then
                 . ~/.bashrc
         fi
         
         [wenbluo@phxdpeetl019 ~]$ alias
         alias capollo='ssh Apollo-devour.vip.ebay.com'
         alias cares='ssh ares-devour.vip.ebay.com'
         alias cbin='cd /home/wenbluo/etl_home/bin'
         alias ccfg='cd /home/wenbluo/etl_home/cfg'
  alias cf='cd /home/wenbluo/etl_home/cfg'
         ```

         通过测试可以看到：

         <span style='background-color:orange'>**ccfg 最后被.bashrc的alias  覆盖**</span>

       - 

       ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_61.png)

       --->拓展：

       <span style='background-color:orange'>**How to check which shell am i using** </span>

       1. bash ,ksh ,csh ,tsh ...

          <span style='background-color:lightblue'>ALL shell scripts (Bourne, BASH, Korn, Posix sh)</span>

          - bash  is standard linux shell

            基本已经取代sh（bourne shell)
       
            ```shell
            [wenbluo@phxdpeetl019 bin]$ pwd
            /bin
            [wenbluo@phxdpeetl019 bin]$ ll sh
     lrwxrwxrwx 1 root root 4 Mar 28  2018 sh -> bash
            ```

          - bourne is standard unix shell

          - korn add numerous features to sh 

            广泛应用在所有*nix系统中，<span style='background-color:orange'>**只要能访问命令行，就能使用ksh**</span>

          - ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_100.png) 

            <span style='color:red'>**there is some difference  between  ksh  and bash/sh** </span>

       2. echo $0   or echo $SHELL
       
          ```shell
          
          [wenbluo@phxdpeetl019 ~]$ echo $0
          -bash
          [wenbluo@phxdpeetl019 ~]$ echo $SHELL
   /bin/bash
          ```
  
          
  
     
  
  

## 文件操作符号

### Test 指令

| -f file | 普通文件   |
| ------- | ---------- |
| -e file | 文件存在   |
| -x file | 可执行文件 |
| -s file | 不是空文件 |
| -r file | 可读文件   |
| -d file | 是一个目录 |
|         |            |
|         |            |

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_143.png)

### 字符串操作符

| -z        | 为空则为真                                    |
| --------- | --------------------------------------------- |
| -n        | 不为空则为真  ： not null                     |
| str1=str2 | 字符串相同                                    |
| str1<str2 | 字符串str1字段顺序排在str2 之前  (ascii 排序) |
|           |                                               |

ps:作比较的时候，< 需要转义  " `\`<"

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_144.png)

<span style='background-color:lightblue'>**[[]] :这个高阶版本，仅在bash \korn shell 使用。 []  任何shell 都可以使用**</span>；

### 算术操作符

| -eq      | 相等          |
| -------- | ------------- |
| -ne      | 不相等        |
| -lt、-le | 小于\小于等于 |
| -gt、-ge | 大于\大于等于 |

### 逻辑操作符

| &&   | [[ $num -ge 90 && $num -le 100 ]]  : [90,100]  :逻辑与       |
| ---- | ------------------------------------------------------------ |
| \|\| | [[ $num -ge 90 \|\| $num -le 60 ]] ： >=90   or  <=60 : 逻辑或 |
| !    | if [ !  -d /etl/home ]逻辑非                                 |



## Configuration

文本编辑器+脚本解释器(命令解析器)

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_1.png)

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_2.png)

- 1. 直接在电脑上打开那个文件夹，然后在文件夹空白处右键选择Git Bash here

  2. ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_5.png)

  3. 文件夹处理：

     - ls -l :查看目录下的文件：  list  files/subfolders

     - ls (list simple) 简单输出有哪些文件和目录 

       ll :  详细文件信息  （ d, - ,l , RWX ，time  owner )等等

       **快捷方式 ：ll**  

       ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_20.png)

       **wenbluo :表示文件所有者 ** 

       **1049089 ：表示用户所在的组**

       **389:表示文件大小**

       #### 拓展：

       图片中的total 为 8 是什么意思呢？

       - 该目录占用block块数 （sql server 是以8k ），

         <a href=https://blog.csdn.net/apache0554/article/details/44813485>blocksize</a>

       - ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_49.png)

         通过指令 getconf pagesize : 得到一个block size  为4k  4096byte

         （19909+4096*4）/4096=9  个blocks ，所以占据空间为36 kb

     - 退出文件夹： cd ..  -->返回上一层文件夹   （类似html5,返回上一层）

       1. cd: change  direcory 

       2. **cd ~ ： 返回到用户**  :<span  style='color:orange'>**$(whoami)**</span> ##wenbluo  注意是<span style='background-color:lightgreen'>**括号并不是花括号**</span>

          <span style ='background-color:lightorange'>cd :返回用户目录(主目录 $HOME)</span> ： **/home/wenbluo  ** <span style='background-color:lightorange'>与 cd ~等价</span>

          ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_34.png)

       3. cd .. :返回上一级

          cd  desktop/wbluo/111 : **直接跳转到该文件夹下**

          **ps:  如果通过指令 cd  wbluo  想返回上一级 ，报错；只能 cd ..**

          **总结：**

          1. **~/.. ：先执行返回用户目录，再返回根目录**

          ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_29.png)

          

       4. pwd: 返回完整路径

          ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_19.png)

          **pwd(print working directory) 命令是用于显示当前的目录。**

          -->R语言中的 getwd

     - 新建文件夹 mkdir 111

       ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_6.png)

         -->拓展删除文件夹

          **rmdir 123  124  125  :删除多个文件夹**

       ```shell
        mkdir -p dir_1/dir_2
       [wenbluo@phxdpeetl019 test]$ cp ./while_test ./dir_1/dir_2
       [wenbluo@phxdpeetl019 test]$ rm dir_1
       rm: cannot remove `dir_1': Is a directory
       ###因为dir_1 目录不为空
       
       ###方法一： rm -r dir_1 :则会删除整个目录 
       ###方法二：
       rmdir dir_1
       rmdir: failed to remove `dir_1': Directory not empty
       
       ###既然还没有该功能，看来只能rm -r 了；
       rmdir -r dir_1
       rmdir: invalid option -- 'r'
       Try `rmdir --help' for more information.
       
       ```

       -->**当文件夹不为空的时候，可以采用 rm -r**   (recursion)

       **<span style ='color:red'>注意文件路径要正确！！，而且不要轻易使用该指令</span>**![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_31.png)

       --><span style='background-color:lightgreen'>**创建目录A下，再创建一个目录B**</span>

        mkdir -p dat/secondary dat/extract dat/primary dat/td2 dat/td1

     - touch 创建文件：

       1. touch  test_8.sh / test_9.xlsx / test_10.r

       2. 还可以通过 echo 赋值方式 ：

          ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_36.png)

          > > 表示**传参**
          > >
          > > 当break_point.seq 文件不存在时候，创建文件，并赋值；

          

          ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_40.png)

          

     - wc:word count   

       ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_25.png)

       1. 2: 表示文本共两行数据  -l  (lines)
       2. 23 ：表示文本有23 个单词 -w  (words)
       3. 106：表示字节bytes  -c 
       4. test_8.txt ,表示文件名称
       5. wc --help 查看含义 
       6. “#”：表示注释 

### vim文本编辑器：

- <a href='https://www.ibm.com/developerworks/cn/education/aix/au-unixtips2/index.html' >vim </a>

- vi /vim 有三种模式

  1. 命令模式

     - 可以gg （返回首行）

        shift+g  （返回末行）

     - u  ：返回上一步

     - ctrl+r  :撤销

  2. 输入模式

     - 按  I 字母  进入编辑模式

  3. 末行模式

     - 按 ： 字符

     - 骚操作

       1. 查找： / work

          --> 如何<span style='background-color:yellow'>**按 n**  </span>可以下翻找下一个work , <span style='background-color:lightblue'>**按 N**</span> 查找上一个 work

       2. 替换：  

          - :s /u/you/g : 替换鼠标**所在行** 所有的u-->you  ##其中/u  slash block  一定要

          - :%s /u/you :替换**整篇**的u 为 you

            -->

            如果想控制替换，可以添加一个指令/c

            :%s /u/you/c ，<span style='background-color:lightblue'>**则会强制每个替换需要用户进行确认**</span>
       
       
       

- **恢复文件**


  vi在编辑某一个文件时，会生成一个临时文件，这个文件以<span style='background-color:lightblue'> **. 开头并以 .swp结尾**</span>。正常退出该文件自动删除，如果意外退出例如忽然断电，该文件不会删除，我们在下次编辑(vi file ,并不是vi .swp)时可以选择一下命令处理：

  　　O只读打开，不改变文件内容
  　　E继续编辑文件，不恢复.swp文件保存的内容
  　　R将恢复上次编辑以后未保存文件内容
  　　Q退出vi
  　　D删除.swp文件
  　　或者使用<span style='color:red'>**vi －r** </span>文件名来恢复未保存的内容  

  

- 

  1. :quit  /:exit    --><span style='background-color:lightblue'>**规范退出vim 编辑器**</span>  

  2. <span style='background-color:orange'>**vi 创建文件并且编辑文件**</span>

     

     ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_24.png)

     1. vi test_8.txt 

     2. i : 表示insert 

     3. esc :跳出插入

     4. Shift+ZZ: 退出 文本操作    **: wq (windows .quit)**  **保存文件并退出**

        有时候也会**通过 q！ 退出；**--><span style='background-color:lightblue'>**并不会保存文件**</span>

     5. 通过 ： 可以查找文件    :/  **(模糊查找，区分大小写)**

        ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_42.png)

     6. 快捷键：

        - dd 删除整行数据 

        - ctrl+f : front   翻页（下一页）

        - ctrl+b:back    翻页（上一页）

        - shift+g :快速到达末行

          --><span style='background-color:lightgreen'>**如果向快速定位都某一行？**</span>

          17 + shift +g

          <span style='background-color:lightblue'>**其实就是 17G** </span>

        - gg:到达首行

        - **u**   撤销上一步的操作

        - **Ctrl+r** 恢复上一步被撤销的操作

        - 全部复制：按esc键后，先按gg，然后ggyG    (Yank:拉取 Global)

        - 全选高亮显示：按esc键后，先按gg，然后ggvG或者ggVG

        - ^:当前行首行

        - $:当前行末尾

        - r:表示替换replace

        - y:表示复制(yank) ：<span style='background-color:lightblue'>**注意是复制到vi的缓冲区**</span>

          --->要想copy 到本机,只能cat 文件，然后复制

        - p:表示粘贴(paste)   -->这些快捷键只能使用与vim，没法复制到记事本上；

          -->可以采用xftp6将unix 文件上传到windows

          ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_64.png)

          -->如何解决indent 问题，因为paste 到remote terminal  结果文件全部错乱
        
          <a href='https://stackoverflow.com/questions/506075/how-do-i-fix-the-indentation-of-an-entire-file-in-vi'>vi indent </a>
        
          1. = :表示indent 
        
             - gg=G : 所有行缩进
           - =g:当前行下所有行都缩进
             - ==：当前行缩进
        
          2. 为了避免paste 时候出现乱行
        
             :set paste
        
             <span style='background-color:lightblue'>**This will turn off the auto-indent, line-wrap, etc. features that are messing up your paste.**</span>
        
          

  <span style='color:red'>wc  -l  word.txt  test_8.txt  : 统计两个文件行数</span>

  - mv 移动文件夹/**重命名**

    mv a.txt /b/c.txt  : 将a文本移到b文件夹下，并重命名为c文本

    mv a.txt c.txt :将a 文件**重命名**为 c文件
  
    ---<span style='background-color:orange'>拓展 rename</span> ：
  
    - unix系统有两种rename编程语言 一个是C，一个是perl
  
    - 通过man rename command 查看type  (rename --help)   rename -V
  
    - c 语言下：
  
      <a href='https://www.cnblogs.com/khldragon/p/3591137.html'>c_rename_tutorial</a>
  
      <a href='https://www.cnblogs.com/amosli/p/3491649.html'>per_rename_tutorial </a>
  
      rename  old-c new-c filename 
  
    ```shell
      [wenbluo@phxdpeetl019 test]$ ll
    total 0
      -rw-rw-r-- 1 wenbluo wenbluo 56 Apr  2 21:55 wbluo_1.txt
    -rw-rw-r-- 1 wenbluo wenbluo 48 Apr  2 01:32 wbluo_2.txt
      -rw-rw-r-- 1 wenbluo wenbluo 48 Apr  2 01:36 wbluo_3.txt
      -rw-rw-r-- 1 wenbluo wenbluo 14 Apr  2 01:37 wbluo_4.txt
      [wenbluo@phxdpeetl019 test]$ rename wbluo f *
      ##将所有wbluo 都特换成 f
      [wenbluo@phxdpeetl019 test]$ ll
      total 0
      -rw-rw-r-- 1 wenbluo wenbluo 56 Apr  2 21:55 f_1.txt
      -rw-rw-r-- 1 wenbluo wenbluo 48 Apr  2 01:32 f_2.txt
      -rw-rw-r-- 1 wenbluo wenbluo 48 Apr  2 01:36 f_3.txt
      -rw-rw-r-- 1 wenbluo wenbluo 14 Apr  2 01:37 f_4.txt
    ```
  
    
  
    困惑：rename是否支持正则表达式：
  
    ```shell
      [wenbluo@phxdpeetl019 test]$ rename 'f|w' wbluo *
    [wenbluo@phxdpeetl019 test]$ ll
      total 0
    -rw-rw-r-- 1 wenbluo wenbluo 14 Apr  2 01:37 F_4.txt
      -rw-rw-r-- 1 wenbluo wenbluo 48 Apr  2 01:36 w_3.txt
    -rw-rw-r-- 1 wenbluo wenbluo 56 Apr  2 21:55 wbluo_1.txt
      -rw-rw-r-- 1 wenbluo wenbluo 48 Apr  2 01:32 wbluo_2.txt
    ### 结论是不支持！
    ```
  
      ---拓展：如何大小写替换：
  
      sed (stream editor)
  
      <span style='background-color:lightblue'>**基本格式： sed pattern filename filename2 ....**</span>
  
      - 仅仅是在输出替换，并没有底层更改名称
  
      - 大写转小写:  echo "ABCDS" | sed 's/[A-Z]/\l&/g'
  
    - 小写转大写: echo "abcds" | sed 's/[a-z]/\u&/g'
  
      - 取出固定位置数值
  
        ```shell
        [wenbluo@phxdpeetl019 test]$  date '+%Y%m%d' |sed 's/\(....\)\(..\)\(..\)/\1 \2 \3/g'
        2019 04 16
        ###把模式放入括号内会将其匹配的内容保存到寄存器中，在这里寄存器是1，2，3，可以采用\1 \2 \3来引用
        ###类似hive  regexp_extract(columname,'(\\d+)(12)',1)
        
        $ echo '2019-01-02'  | sed 's/\(....\)\(..\)\(..\)/\1 \2 \3/g' | read YEAR_EXP MONTH_EXP DAY_EXP
        $ echo $YEAR_EXP
        2019
        
  
        ```

      - ```shell
      sed 's/[ir]/11/g' wbluo_1.txt
        echo '11 am 11obe11t'
        [wenbluo@phxdpeetl019 test]$ cat wbluo_1.txt  | sed 's/[ir]/11/g'
        echo '11 am 11obe11t'
        [wenbluo@phxdpeetl019 test]$ cat wbluo_1.txt
      echo 'i am robert'
        
  
        sed 's/[ir]/11/g' wbluo_1.txt  wbluo_2.txt
      echo '11 am 11obe11t'
        echo '11 am Robe11t'
        ```
      ```
      
      ```
  
    - 
  
    tr (translate:switch)
  
    - cat t.txt |tr ‘abc’ ‘xyz’  
      - cat file| tr [a-z] [A-Z] ###小写转换为大写
      - cat file |tr -d ’snail‘  ##**删掉所有snail 字符串**
    - cat file | tr -s “:‘’ ‘\n’ ###冒号替换成换行符
  
  - 
  
  
  
- 查看文件内容： $ **cat** 文件名.文件类型   -->catenate :**表示连接文件并输出到前端；**
  
- cat <span style='background-color:lightblue'>**一次输出多个文件**</span>
  
  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_128.png)
  
  拓展：<a href=https://blog.csdn.net/hzraymond/article/details/8181287> 文件查看指令</a>
  
  1. less test  ///   退出形式按 ： q
  
     特点：
  
   - 不像cat test ，全部输出
       - 而是像vi 一样,可以<span style='background-color:lightgreen'>**翻页（page up\page dn)** </span>,便与查看，但又不会修改文件
     - 也可以模糊匹配: /clsfd_site_id
  
  2. head 
  
     head -20 dw_clsfd.step2_clsfd_daily_sc_mkt_mtrc.sql  返回前20行数据
  
  3. tail
  
     tail -20 dw_clsfd.step2_clsfd_daily_sc_mkt_mtrc.sql 返回后20行数据
  
  4. more 
  
     显示所有信息，但是可以通过输入enter键，翻页
  
     <span style='background-color:lightgreen'> **ps -aux | more      优于  ps  -aux**   </span>
  
     --> 也可以通过 <span style='background-color:yellow'>**d(down)/ b (back)** </span>：快捷键 快速翻页
  
  5. top 5 or  tail 5: 
  
     - 查看文件前几行：
  
       head -n 10 file.txt  <span style='background-color:lightorange'> #head 默认值 为10</span>   
  
       <span style='background-color:lightorange'>--> head file.txt   ## head -2  file.txt   不写 -n ：number也行；</span>
  
       tail -n 10 file.txt  
  
     - 查看目录前几行：
  
         ll -| head 
     
  6. column ：格式化输出
  
     ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_129.png)
  
     - 其他参数 -c : controls table's column width 
  
       ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_130.png)
  
     - 
  
  <span style='background-color:orange'>总结：**less>>head==tail>>more >>cat** </span>
  
- 删除目录下的文件； $ **rm** 文件名.文件类型    remove file 
  
- 运行shell文件:  $ **sh** 文件名.sh
  
  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_7.png)
  
- 改变权限的命令：
  
  chmod 755 abc.sh :表示rwxr-xr-x
  
  chmod  a+r abc.sh :表示所有用户添加读的权限
  
  **-->也可以 chmod  +r abc.sh**  
  
  **-->撤销读权限 chmod -r abc.sh**
  
  **只能分批次赋予用户权限（U,G,O)**
  
  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_28.png)
  
  chmod 711 abc :更改目录用户权限
  
  给所有人添加 执行权限: 
  
  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_27.png)
  
1. 通配符 （metacharacter)
  
   *:任意字符    ?:表示单字符
  
     ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_23.png)
  
     <span style='background-color:lightorange'>ll -tr \*dw_clsfd.arg_start_endtime_wenbluo\*</span>
  
     等价于：
  
     <span style='background-color:lightorange'>ll -tr | grep \*dw_clsfd.arg_start_endtime_wenbluo\*</span>
  
  2. Pipes and Filters
  
     符号 是 ：  **|**
  
     ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_38.png)
  
  3. cp： copy 复制文件
  
     ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_30.png)
  
     因为 test_dir目录下有 test.sql 文件，所以仅仅是copy 目录到 xiaoxhuang目录下不行；

   那可以调用 **-r指令，将test_dir下 所有文件**都复制到 xiaoxhuang路径下；

     - cp ~/etl_home/sql/dw_clsfd.step2_clsfd_daily_sc_mkt_mtrc.sql ~/etl_home/sql_test/wuxia 
      
       会自动在wuxia目录下生成同样名称的文件 ；dw_clsfd.step2_clsfd_daily_sc_mkt_mtrc.sql
      
     - <span style='background-color:lightblue'>cp 会完全复制文件属性</span>  :X R W ;
      
     - cp 文件夹：
      
       ```shell
        mkdir cp_test
       [wenbluo@phxdpeetl019 sql_test]$ cp -r wbluo cp_test
       [wenbluo@phxdpeetl019 sql_test]$ cd cp_test


​        
​       ```

  - ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_137.png)
  
    <span style='background-color:lightblue'>**当文件已经存在时候，并不在cp ,并不会overwrite , 或者跟window 一样，生成back or override** </span>
  
     - 拓展远程scp  (secure copy )
    
       <p style='color:darkblue'>我在工作中经常要将一些文件传输到另外一个服务器上，而且都是Linux的命令行环境，那么对于我来讲scp就是最直接有效的方法了，其他诸如FTP、SMB以及Winscp这些有界面的文件传输工具到反而有些多余了。</p>
```shell
       scp ./while_test wenbluo@phxdpeetl019.phx.ebay.com:/home/wenbluo/etl_home/sql_test/test/while_test
       wenbluo@phxdpeetl019.phx.ebay.com's password:                                                                       
       
       scp -r .ssh wenbluo@lvsdpeetl001.lvs.ebay.com:/home/wenbluo/
       ###将.ssh 目录赋值到 从phx -->lvs  目录下
       ###其中 :/home/wenbluo/中的 ：不可以少！
```


​       
- SFTP 
  
- wget
  
  Linux wget 是一个下载文件的工具！ --> 类似 windows迅雷
  
  
  
- 
  
   
  
4. **<span style='color:red'>覆盖文件内容！</span>** ————》![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_43.png)
  
5. sudo 指令
  
6. 通配符 ：* and ?
  
   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_32.png)
  
   ##### QA
  
   1. 如何排序
     2. 
  
7. 以文件形式，保存输出结果,<span style='background-color:orange'>输出重定向</span>
  
   **符号 >**
  
   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_33.png)
  
   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_39.png)
  
8. 拓展 ：>>
  
   **在已有的文件下，新增内容！**
  
   



#### Grep

(global regular expression)

<a href=http://man.linuxde.net/grep>unix 常见命令大全</a>

- **查找所有txt文件中，含有wenbluo字样的文件**![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_46.png)

- 删除指令 grep -v ：只读不含有outgoing 的行信息，然后存放到临时文件夹，    ( invert )

  并替换原始文件

- ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_47.png)

- ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_54.png)

  ll -tr | grep wbluo *   实质就是 grep wbluo  *  查看所有包含wbluo 的文件

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_55.png)

  grep -n ：告知在文件第几行出现；

  ```shell
  [wenbluo@phxdpeetl019 test]$ grep -in robert jobs_2 jobs_1
  jobs_2:1:echo  'hello robert ,nice to see U'
  ###在jobs_2:第一行出现
  jobs_1:1:echo hello world ,hello robert;
  ###在jobs_1:第一行出现
  
  ###作用可以结合 vi 中的 rownumber + SHIFT+ G 快速定位
  ```

  grep -il CLSFD_TRFC_SUM_RPT_MKT|CLSFD_DAILY_MKT_SC|CLSFD_WEEKLY_MKT_SC  * ：查找含有xxx忽略大小写的文件：

  --> ll   l:list 简单列表出来

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_56.png)

- ```shell
  $ grep -i robert jobs_1 jobs_2
  jobs_1:echo hello world ,hello robert;
  jobs_2:echo  'hello robert ,nice to see U'
  
  ####grep 格式 ：partern file1  file2 
  ####可以在多个文件中查找
           ### : partern  *
  [wenbluo@phxdpeetl019 test]$ grep -il robert jobs_2 jobs_1
  jobs_2
  jobs_1
  ```

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_163.png)

  当有空格时候，采用引号 ，进行模糊查找 
  
  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_164.png)
  
  
  
- <span style='background-color:lightblue'>**grep -e :匹配多个模型**</span>

  ```shell
   grep -ie 'robert' -ie 'wenbluo'  * |wc -l
   
   ###等价于
  
  egrep -i  'robert|wenbluo' * |wc -l
  11
  
  
  ```

- ```shell
  cat dw_clsfd.step2_bsc_reply.sql |grep -i p_clsfd_t.clsfd_reply_mth_sc |egrep -ni "create|delete|insert"
  2:DELETE FROM P_CLSFD_T.CLSFD_REPLY_MTH_SC
  3:INSERT INTO P_CLSFD_T.CLSFD_REPLY_MTH_SC
  
   cat dw_clsfd.step2_bsc_reply.sql |grep -ni p_clsfd_t.clsfd_reply_mth_sc |egrep -i "create|delete|insert"
  1168:DELETE FROM P_CLSFD_T.CLSFD_REPLY_MTH_SC
  1173:INSERT INTO P_CLSFD_T.CLSFD_REPLY_MTH_SC
  
  ##-ni 位置不要弄错了
  ```

- 如何通过grep 覆盖文件？

   <span style='background-color:lightblue'>**删除failed 集，只保留success**</span>

   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_81.png)

   <span style='background-color:lightgreen'>错误做法一</span>

   ```shell
   grep -v failed  weekly_sc >weekly_sc
   grep: input file ‘weekly_sc’ is also the output
   [wenbluo@phxdpeetl019 ~]$ cat weekly_sc
   [wenbluo@phxdpeetl019 ~]$ ps -u
   ###weekly_sc文件消失
   
   ```

   <span style='background-color:lightgreen'>**正确做法一：放到临时文件夹中/tmp**</span>

   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_82.png)

   <span style='background-color:lightgreen'>**最安全做法：**</span>

   ```shel&#39;l
   ]$ grep -v failed weekly_sc_bak >/tmp/weekly_sc_bak$$
   [wenbluo@phxdpeetl019 ~]$ mv /tmp/weekly_sc_bak$$  ./weekly_sc
   [wenbluo@phxdpeetl019 ~]$ cat weekly_sc
   
   ```
   
- grep -r "printf" *
           在当前目录及所有子目录下递归查找调用了printf函数的行，并显示行号。

- 

#### 重定向 

<a href=http://www.cnblogs.com/chengmo/archive/2010/10/20/1855805.html> 输入输出重定向</a>  

##### 输出重定向

<span style='background-color:lightblue'>**结果将不再在终端显示，而是存放到文件中**</span>

| type                                                   | description                                |
| ------------------------------------------------------ | ------------------------------------------ |
| >                                                      | 输出重定向                                 |
| <                                                      | 输入重定向                                 |
| >>                                                     | 输出追加 (insert)                          |
| /dev/null                                              | 特殊文件                                   |
| 0-->       <                                           | stdint（标准输入） :文件夹： /dev/stdin    |
| 1-->    <span style='color:orange'> 1> 等价与 ></span> | stdout (标准输出）:   文件夹 ：/dev/stdout |
| 2--->    2>                                            | stderr（错误输出）  :/dev/stderr           |
| >&（中间不能有空格）                                   | 输出重定向到指定文件描述符中               |
| &>  （与上面等价）                                     | 输出重定向到指定文件描述符中               |
| 文件描述符                                             | 0 1  2                                     |

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_58.png)

  实例：

- ```shell
  [wenbluo@phxdpeetl019 ~]$ls 2.txt 33
  ls: cannot access 33: No such file or directory 
  2.txt
  [wenbluo@phxdpeetl019 ~]$ ls 2.txt 33 >4.txt 2>&1
  ##终端不显示任何提示信息，不管任何信息（错误输出or标准输出）都放到4.txt文件中
  [wenbluo@phxdpeetl019 ~]$ cat 4.txt
  ls: cannot access 33: No such file or directory
  2.txt
  
  ```

- ```shell
  [wenbluo@phxdpeetl019 test]$ ll
  total 0
  -rw-rw-r-- 1 wenbluo wenbluo 14 Apr  2 01:37 F_4.txt
  -rw-rw-r-- 1 wenbluo wenbluo 48 Apr  2 01:36 w_3.txt
  -rw-rw-r-- 1 wenbluo wenbluo 56 Apr  2 21:55 wbluo_1.txt
  -rw-rw-r-- 1 wenbluo wenbluo 48 Apr  2 01:32 wbluo_2.txt
  [wenbluo@phxdpeetl019 test]$ ll wbluo_1.txt f_2.txt 2>f_6.txt
  -rw-rw-r-- 1 wenbluo wenbluo 56 Apr  2 21:55 wbluo_1.txt
  ###2>仅仅是错误输出重定向，正缺输出还是在终端显示
  [wenbluo@phxdpeetl019 test]$ cat f_6.txt
  ls: cannot access f_2.txt: No such file or directory
  
  ```

  

- /dev/null 2>&1   --> /dev/null  表示特殊文件，（黑洞），并不会往终端输出结果

  ```shell
  [wenbluo@phxdpeetl019 ~]$ ls 2.txt 33 >/dev/null
  ls: cannot access 33: No such file or directory
  ###错误信息依旧显示在终端
  [wenbluo@phxdpeetl019 ~]$ ls 2.txt 33 >/dev/null 2&>1
  ###stdout/stderr都重定向到/dev/null文件中，终端不显示任何信息
  
  ```

  <span style ='background-color:lightblue'>拓展可以清空数据</span>

  cat /dev/null > data.log  ##清空data.log 文件

  -->2019/09/04: 清空文件的新做法,  **>data.log**    

  <span style ='background-color:lightblue'>拓展与IF 的结合操作:</span>

  ```shell
  $ who  | grep -n  wenbluo
  23:wenbluo  pts/28       2019-04-18 23:39 (phxbastion100.phx.ebay.com)
  29:wenbluo  pts/35       2019-04-18 23:17 (phxbastion100.phx.ebay.com)
  31:wenbluo  pts/38       2019-04-19 00:06 (phxbastion100.phx.ebay.com)
  [wenbluo@phxdpeetl019 test]$ who  | grep -n  wenbluo  >/dev/null
  [wenbluo@phxdpeetl019 test]$ echo $?
  0
  [wenbluo@phxdpeetl019 test]$ if who  | grep -n  wenbluo  >/dev/null
  > then
  > echo 'wenbluo  is  log in '
  > else
  > echo 'user is not log in'
  > fi
  wenbluo  is  log in
  
  ```

  

- ```shell
  [wenbluo@phxdpeetl019 ~]$ ls 2.txt 33 2>3.txt
  2.txt
  ###错误信息重定向到3.txt 文件
  [wenbluo@phxdpeetl019 ~]$ ll
  total 76
  -rw-rw-r-- 1 wenbluo wenbluo  3144 Apr  9 18:58 2.txt
  -rw-rw-r-- 1 wenbluo wenbluo    48 Apr  9 20:04 3.txt
  
  ```

- 2019/04/30

  ```shell
  ll vartest4 vartest6 > /dev/null 1>&2
  ls: cannot access vartest6: No such file or directory
  -rwxrwxr-x 1 wenbluo wenbluo 31 Apr 30 00:55 vartest4
  [wenbluo@phxdpeetl019 test]$ ll vartest4 vartest6 > /dev/null 2>&1
  
  ##为什么 1/2位置不能互换？  文件描述符号？
  ```

  2>&1  :表示标准错误输出重定向到标准输出，而标准输出是重定向到 /dev/null文件，

  所以所有输出都重定向到 /dev/null中

  <span style ='background-color:lightgreen'> **1/2位置不可以换**</span>



##### 输入重定向

- ```shell
  cat > reorient.txt <while_test
  ###cat 从while_test 获取数据并输出到reorient.txt 文件中
  cat | wc -l <reorient.txt
  ###cat reorient.txt 文件并 统计行数
  17
  ```

  

## 引用quote

  1. 单引号和双引号的区别:

     ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_50.png)

  2. 

  

## Smart key

- ctrl+c ：退出指令   --暂时不用
- clear :清屏操作
- <a href=https://blog.csdn.net/u012528654/article/details/39085467>smart key</a>
- 

## Concepts

#### 变量

1. 采用echo 输出变量

   两种输出方式

   echo $your_name/ ${your_name}

   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_4.png)

   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_12.png)

   **不要用单引号  ‘**

2. **转义符"\\"、**

3. read命令行 ：接收键盘输入

   ```shell
   
   [wenbluo@lvsdpeetl001 ~]$ read name
   23   
   ###直接read +参数名字
   [wenbluo@lvsdpeetl001 ~]$ echo $name
   23
   [wenbluo@lvsdpeetl001 ~]$ read -p "how old are you ?" age_value
   how old are you ?2222
   ###增强一种人机对话
   [wenbluo@lvsdpeetl001 ~]$ echo $age_value
   2222
   
   ```

   | option  |                                                              |                      |
   | :------ | ------------------------------------------------------------ | :------------------- |
   | read -p | #延迟五秒，没有输入将自动退出 read -p "Input a number:" -t 5 pwd    #read [-p "提示信息"] 变量名 | echo $pwd            |
   | read -n | read -p "Input a word:" -n 5 Word #限制输入长度为5           | echo $Word           |
   | read -t | wait times                                                   |                      |
   | read -d | read -dp -p "Input some words end with q:" word              | 直到输入q,将自动退出 |
   |         |                                                              |                      |
   |         |                                                              |                      |
   |         |                                                              |                      |

4. <span style='background-color:lightgreen'>**系统变量**</span>:

   - $HOME  $PATH  $who   $CPATH

     ```shell
     echo $HOME
     /home/chewu
     
     ```

     <a href='https://medium.com/@jalendport/what-exactly-is-your-shell-path-2f076f02deb4'>whereis my Path ?</a>

     1. Your computer is comprise of files ;of which two types <span style='background-color:orange'> . data files and  executable files</span>

        when you use command i.e ls  cd , are just ececutable files .

     2. PATH is a global variabel that contains <span style='background-color:orange'>a string of different paths separated by a **:** </span>

        your computer then uses this variable to understand what directoried it shoud look in to find the executable you're requesting .

     3. if you want to add new directory into path 

        - export PATH='/my/directory/bin:$PATH'
        - export PATH='$PATH:/my/directory/bin'
        - --><span style=background-color:lightblue>**elimeter is  "   :   "**</span>

     4. if we want the changes to our PATH to persist,so we could edit file : <span style='background-color:orange'> **.bashrc and .bash_profile**</span>

        summary :

        跟windows系统一样，添加applicaition 到环境变量中:

        

        

   - ```shell
     $(hostname)
     
     echo $(hostname)
     phxdpeetl019.phx.ebay.com
     
     [wenbluo@phxdpeetl019 ~]$ echo `hostname`
     phxdpeetl019.phx.ebay.com
     
     ```

     

   - whereis bash

     ##查看bash 位置

     ```shell
     [ARES]/etc/hadoop > whereis bash
     bash: /bin/bash /usr/share/man/man1/bash.1.gz
     ```

     whereis 是一个命令符

   - alias 

   - date  ###当前时间

     --> 拓展：<span style='background-color:lightgreen'>**date + ‘format’ 以特定格式输出**</span>

     date +'%Y%m%d-%H%M%S'

     20190415-215810

     ```shell
     [wenbluo@phxdpeetl019 ~]$ date +'%Y%m%d-%h%M%S'
     20190415-Apr1924
     [wenbluo@phxdpeetl019 ~]$ date '+%Y%m%d-%h%M%S'
     20190415-Apr1935
     ###两者等价，一般后者最好 这样git 上也能使用； 
     ```

     

   - hostname ##服务器名称

     ```shell
     $ hostname
     phxdpeetl019.phx.ebay.com
     
     ```

   - $SHELL    

     ```shell
     $ echo $SHELL
     /bin/bash
     ###返回shell 哪个版本？
     
     ```

   - $(whoami)

     ```shell
     echo $(whoami)
     wenbluo
     [wenbluo@phxdpeetl019 ~]$ echo `whoami`
     wenbluo
     
     ```

   - who am i

     ```shell
     who am i
     wenbluo  pts/27       2019-05-04 23:38 (phxbastion100.phx.ebay.com)
     
     ```

     

   - whereis 

     ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_99.png)

   - which hive  which hadoop

     [ARES]/ > which hadoop
     /apache/hadoop/bin/hadoop

   - 

     

     

     

     

     

     

5. 

6. 

#### 运算符

1. ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_8.png)

   

   ```shell
   a=10
   ((a=a+3))  #a加3
   ```

   ```shell
   [wenbluo@phxdpeetl019 test]$ expr 2 * 2
   expr: syntax error
   [wenbluo@phxdpeetl019 test]$ expr 2 + 2
   4
   [wenbluo@phxdpeetl019 test]$ expr 2 ^ 2
   expr: syntax error
   
   ```

   

2. expr

   background :<span style='background-color:lightgreen'>**Expr   与 let 一样，并且还可以进行字符串运算**</span>

   ```sh
   [chewu@phxdpeetl019.phx.ebay.com:/home/chewu]expr  1 + 1
   2
   [chewu@phxdpeetl019.phx.ebay.com:/home/chewu]expr 1.1 + 1
   expr: non-numeric argument
   
   ##expr只能输出不含有空格的字符串
   
   [chewu@phxdpeetl019.phx.ebay.com:/home/chewu]str2='iamrobert'
   [chewu@phxdpeetl019.phx.ebay.com:/home/chewu]expr length $str2
   9
   [chewu@phxdpeetl019.phx.ebay.com:/home/chewu]str3='i am robert'
   [chewu@phxdpeetl019.phx.ebay.com:/home/chewu]expr length $str3
   expr: syntax error
   
   ####通过${#str3}  
   [chewu@phxdpeetl019.phx.ebay.com:/home/chewu]echo ${#str3}
   11
   
   ###截取字段
   [chewu@phxdpeetl019.phx.ebay.com:/home/chewu]expr substr $str2 1 5
   iamro
   [chewu@phxdpeetl019.phx.ebay.com:/home/chewu]expr substr $str3 1 5
   expr: syntax error
   
   ###
   echo ${str3:1:5}
   am r
   
   
   
   
   ```

   Expr expression1 操作符 expression2  --format   <span style = 'color:red'>**注意间隔**</span>

   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_9.png)

   -->拓展

   - let 指令 

     background : <span style='background-color:lightgreen'>**Let命令让BASH shell执行算数运算的操作**</span>

   - let 变量名 = 变量1 运算符 变量2   --format 

     ```shell
     num1=106
     num2=23
     [chewu@phxdpeetl019.phx.ebay.com:/home/chewu]let add=$num1+$num2
     [chewu@phxdpeetl019.phx.ebay.com:/home/chewu]echo $add
     129
     [chewu@phxdpeetl019.phx.ebay.com:/home/chewu]let div=$num1/$num2
     [chewu@phxdpeetl019.phx.ebay.com:/home/chewu]echo $div
     4
     [chewu@phxdpeetl019.phx.ebay.com:/home/chewu]let mod=$num1%$num2
     [chewu@phxdpeetl019.phx.ebay.com:/home/chewu]echo $mod
     14
     ###let 不适用与小数运算
     let 1+1
     [chewu@phxdpeetl019.phx.ebay.com:/home/chewu]let 1.1+1
     -bash: let: 1.1+1: syntax error: invalid arithmetic operator (error token is ".1+1")
     
     
     
     
     ```

     

3. ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_10.png)

4. 关系运算符![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_11.png)

   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_15.png)

   ```shell
   a=10
   b=13
   [ $a -gt $b ]
   echo $? 
   1
   ## 0 表示TRUE  1表示Flase
   ```

   -->拓展：  不支持文本操作？

   ```shell
   name='wbluo'
   [wenbluo@phxdpeetl019 test]$ [ $name -eq wbluo ]
   bash: [: wbluo: integer expression expected
   
   ```

   

5. 布尔运算符

   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_13.png)

6. 逻辑运算符

   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_14.png)



## Dot 命令  

```shell

[wenbluo@phxdpeetl019 test]$ .  parent.sh  ###与parent.sh文件之间要有空格！
[wenbluo@phxdpeetl019 test]$ echo $a
wbluo
[wenbluo@phxdpeetl019 test]$ echo $b
robert
[wenbluo@phxdpeetl019 test]$

###等价于在父shell 上输入一遍 parent.sh command
##less parent.sh
a='wbluo'
b='robert'
export a ;
```

-->bash  与 dot 命令的区别

## Summary :

该命令可以<span style='background-color:orange'>当前shell </span>中执行file 的内容。也就是说，file中命令就像是你直接输入的一样，<span  style='background-color:orange'>由当前shell 执行，不是在子shell 中</span>。  **(父Shell  和 子Shell 概念 )**

 

```shell


[wenbluo@phxdpeetl019 test]$ bash chid_2.sh
wbluo

[wenbluo@phxdpeetl019 test]$ . chid_2.sh
wbluo
robert
wenbluo

#less chid_2.sh
echo $a
echo $b
echo $c

```

```shell

[wenbluo@phxdpeetl019 test]$ bash chid_2.sh
wbluo


[wenbluo@phxdpeetl019 test]$ . parent.sh
[wenbluo@phxdpeetl019 test]$ bash chid_2.sh
wbluo

wenbluo
#less parent.sh
a='wbluo'
b='robert'
c='wenbluo'
export a
export c

```

#### Bash 

background:

如果文件第一行的前两个字符是 #！ ，<span style='background-color:lightblue'>**那么余下的部分就指定了该文件的解析器**</span>，因此

```shell
#！/bin/ksh 
#！/bin/bash  
```

<span style='background-color:red'> **文件不一定是sh 文件：**</span>

- <span style='background-color:lightblue'>**whereis bash**</span> ：查看你bash 所在的位置：

  ```shell
  [wenbluo@phxdpeetl019 test]$ whereis bash
  bash: /bin/bash /etc/bash.bashrc /usr/share/man/man1/bash.1.gz
  
  [wenbluo@phxdpeetl019 test]$ whereis sh
  sh: /bin/sh /usr/share/man/man1/sh.1.gz /usr/share/man/man1p/sh.1p.gz
  ```

- 如何运行程序：

  有两种方法：一种是显式制定 BASH 去执行：

  **bash hello 或 sh hello （这里 sh 是指向 bash 的一个链接，“lrwxrwxrwx 1 root root 4 Aug 20 05:41 /bin/sh -> bash”）**

  或者可以先将 hello 文件改为<span style='background-color:orange'>可以执行的文件</span>，然后直接运行它

  ```shell
  
  [wenbluo@phxdpeetl019 bin]$ bash chid_2.sh
  hello wenbluo
  
  #!/bin/bash
  echo $a
  echo $b
  echo $c
  
  ###在/bin/bash 目录下直接运行
  
  [wenbluo@phxdpeetl019 bin]$ cd $HOME/etl_home/sql_test/test/
  [wenbluo@phxdpeetl019 test]$ chid_2.sh
  -bash: chid_2.sh: command not found
  ### 由于不是在/bin目录下 或者说在 $PATH 路径下没有找到该文件
  [wenbluo@phxdpeetl019 test]$ ./chid_2.sh
  hello wenbluo
  ### 通过将当前目录. 运行该文件
  
  [wenbluo@phxdpeetl019 test]$ parent.sh
  bash: parent.sh: command not found
  [wenbluo@phxdpeetl019 test]$ bash parent.sh
  ##第二种方法显示 Bash调度文件
  wbluo
  
  
  ```

- 

- 

- 

- 

- **#!/bin/bash** 

  <span style='background-color:orange'>#! 是说明 hello 这个文件的类型</span>的，有点类似于 Windows 系统下用不同文件后缀来表示不同文件类型的意思（但不相同）。"/bin/bash" 就表明该文件是一个 BASH 程序,<span style='background-color:lightblue'>需要由 /bin 目录下的 bash 程序来解释执行</span>

- 

- ```shell
  . ./parent.sh
  [wenbluo@phxdpeetl019 test]$ echo $c
  wenbluo
  
  #less parent.sh
  a='wbluo'
  b='robert'
  c='wenbluo'
  export c
  
  ##因为parent.sh是子shell 
  ##当运行  .  ./parent.sh 就好比 在父shell 环境下再次输入同Parent.sh code 
  
  ```



## ``命令

<span style='background-color:lightblue'>**反引号**</span>

```shell

[wenbluo@phxdpeetl019 ~]$ echo the data and time is : $date
the data and time is :
[wenbluo@phxdpeetl019 ~]$ echo the data and time is : date
the data and time is : date
[wenbluo@phxdpeetl019 ~]$ echo the data and time is : $(date)
the data and time is : Thu May 16 03:32:15 -07 2019
[wenbluo@phxdpeetl019 ~]$ echo the data and time is : `date`
the data and time is : Thu May 16 03:32:22 -07 2019
[wenbluo@phxdpeetl019 ~]$
```

当shell 扫描命令行时，它识别出了反引号，于是期望接下来的是一个命令。date 是一个返回当地时间的一个命令 。在老的版本中依旧采用<span  style='color:red'>``</span>形式，新形式采用 <span style ='color:orange'>$()</span>



- ```shell
  [chewu@phxdpeetl019.phx.ebay.com:/home/chewu]gap=expr $first_stamp - $end_time
  -bash: 1557104401: command not found
  [chewu@phxdpeetl019.phx.ebay.com:/home/chewu]gap=`expr $first_stamp - $end_time`
  [chewu@phxdpeetl019.phx.ebay.com:/home/chewu]echo $gap
  -12202
  
  ```

- ```shell
  start_time=date -d '2019-05-06 22:30:03' +%s
  -bash: -d: command not found
  [chewu@phxdpeetl019.phx.ebay.com:/home/chewu/etl_home/cronlog]start_time=`date -d '2019-05-06 22:30:03' +%s`
  [chewu@phxdpeetl019.phx.ebay.com:/home/chewu/etl_home/cronlog]echo $start_time
  1557207003
  
  ```

- ```shell
  time1=date +'%Y%m%d-%H%M%S'
  -bash: +%Y%m%d-%H%M%S: command not found
  [wenbluo@phxdpeetl019 ~]$ date +'%Y%m%d-%H%M%S'
  20190515-203314
  [wenbluo@phxdpeetl019 ~]$ time1=`date +'%Y%m%d-%H%M%S'`
  [wenbluo@phxdpeetl019 ~]$ echo $time1
  20190515-203334
  ```

- 

```shell

```

## Sudo

- sudo :<span style='background-color:lightblue'>superuser do :允许已经验证的用户用其他用户运行指令；</span>
- 

## 空指令

- ：空占位符 ，不执行操作

- ```shell
  echo hi robert ,wenbluo ,welcome to the shell world!
  echo how old are u ?
  read age
  if [ $age -gt 20 ]
  then
   : ##echo 'You are old enough'  
  else
   echo 'You are novice'
  fi
  export age
  bash 
  
  [wenbluo@phxdpeetl019 test]$ bash w_3.sh
  hi robert ,wenbluo ,welcome to the shell world!
  how old are u ?
  22   ##当大于等于 20的时候 ，不操作
  [wenbluo@phxdpeetl019 test]$
  
  
  ```

- 

## Jobid

background :目前是交互式的命令行，<span style='background-color:lightblue'>**我们可以将一些job放到后台运行**</span>，<span style='background-color:orange'>**前台照样可以操作其他事情**</span>



<span style='background-color:red'>**特殊字符：&**</span>



1. 通过jobs 查看运行的job   [只能在当前session 有效，而且是系统变量，无需要指定那个文件夹]

   也可以通过 **ps -aux |grep xxx  or  ps -ef |grep xx :显示所有进程id**

   ```shell
   jobs
   [1]+  Stopped                 ./jobs_2
   jobs -l
   [1]- 48028 Stopped (tty input)     ./jobs_2
   [2]+ 21491 Stopped (tty input)     ./parent_2.sh
   ## 48028 ：表示进程id ,  ./jobs_2 :表示命令行
   ## [1],[2]:表示任务编号
   ## +:表示当运行的job  ,  -:表示下一个job(当前作业之后的一个作业)
   ## jobs的状态可以是running， stopped， Terminated 
   ```
   
   ![](C:\Users\wenbluo\Desktop\wbluo\shell\161.png)
   
   --》
   
      **如何让10135 相对16328 后运行**
   
   - +- :并不是先后意思，而是+：当前运行的job ，-：表示是当前作业之后的一个作业  
   
     <span style='background-color:lightblue'>**并不是指先运行 +,后再 运行- ...**</span>
   
   - ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_162.png)
   
   - 

- bg

  将任务放在后台运行

  ```shell
  [wenbluo@phxdpeetl019 bin]$ ps -aux
  dw_adm   20870  0.0  0.0   6256   584 ?        S    May07   0:00 /ebay/uc4/agent-bin/Linux_x86_64_12.0.3_B1634/bin/./ucx
  dw_adm   20887  0.0  0.0 107024  1640 ?        S    20:10   0:00 /bin/ksh /ebay/uc4/agent-tmp/JDQPGQLB.TXT
  ^Z  ###ctrl+z
  [1]+  Stopped                 ps -aux
  [wenbluo@phxdpeetl019 bin]$ fg
  ps -aux
  
  ```

  

- fg : foreground

  将任务放在前台运行

  当有多个任务时候：

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_767png.png)

  特点：有三个jobs , 执行顺序：6028 >5897 >5775

  <span style='background-color:lightgreen'>**-->困惑： 难道不是并行运算？**</span> NO

     

  fg %1 :表示将任务1 调前台运行；<span style='background-color:red'>**##并不是pid** </span>

  ```shell
  [wenbluo@phxdpeetl019 bin]$ jobs
  [1]-  Running                 nohup ksh refresh_weekly_sc_core_tmp.ksh 2019-04-28 2019-05-04 > 000001.log 2>&1 &
  [2]+  Stopped                 ps -aux
  
  ###可以看到有两个任务；
  
  
  [wenbluo@phxdpeetl019 bin]$ fg %2  ##也可以fg 2  ,但是一般推荐前者
  ps -aux
  ###将任务 2 放到前台运行；
  
  Signal 18 (CONT) caught by ps (procps version 3.2.8).
  [wenbluo@phxdpeetl019 bin]$ jobs
  [1]+  Running                 nohup ksh refresh_weekly_sc_core_tmp.ksh 2019-04-28 2019-05-04 > 000001.log 2>&1 &
  ### 这个时候只有一个任务在后台运行
  [wenbluo@phxdpeetl019 bin]$
  
  
  ```

- kill 

  ```shell
  ]$ jobs
  [1]   Stopped                 ./jobs_2
  [2]-  Stopped                 ./parent_2.sh
  [3]+  Stopped                 ./explicit.sh
  [wenbluo@phxdpeetl019 test]$ kill %%
  ###终止当前运行的指令 （./explicit.sh)
  [wenbluo@phxdpeetl019 test]$ jobs
  [1]-  Stopped                 ./jobs_2
  [2]+  Stopped                 ./parent_2.sh
  [3]   Terminated              ./explicit.sh
  [wenbluo@phxdpeetl019 test]$ kill %2
  ####终止任务id 为2的jobs
  
  ```

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_78png.png)

  <span style='background-color:lightgreen'>**注意打%** </span>

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_79png.png)

  <span style='background-color:lightgreen'>**or 直接kill pid**</span>

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_80png.png)

  

  

-->拓展

1. 对于正在前台执行的job,我们可以<span style='background-color:lightblue'>**ctrl+Z ，暂停(挂起）**</span>，然后  bg number  防止后台运行  --><span style='background-color:lightgreen'>**前提是没有 &  时候**</span>

   ctrl+c 表示终止；

2. 

- nohup refresh_weekly_sc_core.ksh 2019-04-14 2019-04-20 &   ### &表示后台指令 运行  nohup 表示防止挂起,或者会<span style='background-color:lightblue'>**话意外关闭（断开链接）**</span>

-->instance :

```shell
ksh target_table_load_handler.ksh dw_clsfd.clsfd_daily_mkt_sc_wbluo_2019_04_29 td2 dw_clsfd.clsfd_daily_mkt_sc_wbluo.sql start_date=2019-01-01 end_date=2019-01-02 >test_log_2019_0420.log 2>&1 &
[1] 31619

```



2：

```shell 
### while_test 文件
COUNTER=0
while [ $COUNTER -ge 0 ]
do
    ((COUNTER=$COUNTER+1))
    sleep 5
    echo $COUNTER
done

bash while_test &
[1] 25435  ###先返回pid 
[wenbluo@phxdpeetl019 test]$ 1 ###后面运行成功后，回向前端吐数据  ，如何写了个输出重定向的话，就不会影响前端；
2
3
^C   ####其次我就算终止这个pid, 其实并没有终止掉；
[wenbluo@phxdpeetl019 test]$ 4
5

##只有通过kill 25435 方能跳出死循环 or 关闭session 

```

3: **nohup +&的区别**

<a href='https://www.cnblogs.com/laoyeye/p/9346330.html'> nohup</a>/

## Ps 

background:<span style='background-color:lightblue'>**Linux中的ps命令是Process Status的缩写**</span>

类似<span style='background-color:lightblue'>window下的资源管理器</span>，但是<span style='background-color:lightblue'>unxi下的只是显示瞬间行程状态</span>

### 常见指令

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_73.png)

- PID:cmd 的id 

- TTY:命令所运行的位置

- TIME:运行该命令所占用CPU处理时间

- CMD:该进程的运行命令

- %CPU:进程CPU占用率

- %MEM:进程内存占用率

- STAT :状态码  

  1. R:RUNNING

  2. S:SLEEPING   <span style='background-color:lightgreen'>**休眠中, 受阻, 在等待某个条件的形成或接受到信号** </span>

  3. T:TERMINATE

  4. s:进程的领导者（在他下面有子进程）

  5. +:位于后台的进程组

     

a: 表示该hostname下的终端机所有程序

u:表示该whoami 下（用户为主）的格式显示程序状况  --><span style='background-color:lightgreen'>**只展示用户调度的任务**</span>

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_141.png)

x:表示显示所有程序

```shell
 ps -u wenbluo  ## ps -u 亦可以
 ####查看特定用户的进程
  PID TTY          TIME CMD
 1423 pts/29   00:00:00 ps
 5636 ?        00:00:00 sshd
 5886 pts/29   00:00:00 bash
11991 ?        00:00:00 sshd
12333 pts/46   00:00:00 bash
```

ps -aux | grep weekly_sc 

ps -ef  查看进程 :execute 

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_69.png)



ps -auf | grep 16187    ## <span style ='background-color:lightgreen'>**16187 是pid : 进程id** </span>

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_75.png)

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_76.png)



## Set 命令

### -XV

background :代码调试

- set -xv 查看所有传参后命令行内容，有助于分析实际执行的是什么命令

  --refer to :  xv_test.sh

  <span style='background-color:lightblue'>**++: 表示输出结果**</span>

  

- 

## Eval

- 告知shell取出eval的参数，重新运算求出参数的内容

- ```shell
  a='ls |more'
  [wenbluo@phxdpeetl019 cfg]$ echo $a
  ls |more
  [wenbluo@phxdpeetl019 cfg]$ $a
  ls: cannot access |more: No such file or directory
  [wenbluo@phxdpeetl019 cfg]$ eval $a
  1
  break_point.seq
  date_range.cfg
  
  ```


## paste

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_165.png)

虽然可以指定 以四列作为展示 ，但是是否能更加美丽的展示呢？



## Email

 mailx -s "JOB_ID: ${JOB_ID} RUN SUCCESSFULLY!" DL-eBay-MPT-IMD-CLSFD-SAE@ebay.com;

## Diff 

## Read & Write

- cat date_range.cfg | tr '#' ' ' |  read START_DT END_DT

  -->读文件：并将参数传给 START_DT END_DT

  ```shell
  cat date_range.cfg |tr '#' ' '|read date_1 date_2
  [wenbluo@phxdpeetl019 cfg]$ echo $date_1
  
  [wenbluo@phxdpeetl019 cfg]$ echo $date_2
  
  
  ###困惑怎么没有数据？date_1 /date_2?
  
  ###因为bash  不支持，强制转化为ksh 就能执行
  $ /bin/ksh
  ###先进入ksh 环境
  $  cat date_range.cfg|tr '#' ' ' |read date_1 date_2
  $  echo $date_1
  20190407
  $ echo $date_2
  20190504
  $ exit 
  ###返回到 /bin/bash 环境
  
  
  ```

  

- ```shell
  $ read name
  wenbluo   ###等待从键盘输入
  [wenbluo@phxdpeetl019 test]$ echo $name
  wenbluo
  
  $ read -p 'enter your name：' name_1 name_2 name_3
  ### -p ：parameter :定义参数
  enter your name： 1 2 3
  [wenbluo@phxdpeetl019 test]$ echo $name_1
  1
  [wenbluo@phxdpeetl019 test]$ echo $name_2
  2
  [wenbluo@phxdpeetl019 test]$ echo $name_3
  3
  [wenbluo@phxdpeetl019 test]$ echo $name_1 $name_2 $name_3
  1 2 3
  
  $ read -t5 -p 'enter you name ,you have 5 seconds:' name_5
  enter you name ,you have 5 seconds:[wenbluo@phxdpeetl019 test]$ echo $name_5
  ### read -t :提供了时间限制，计时输入
  [wenbluo@phxdpeetl019 test]$ read -t5 -p 'enter you name ,you have 5 seconds:' name_5
  enter you name ,you have 5 seconds:hi world
  [wenbluo@phxdpeetl019 test]$ echo $name_5
  hi world
  
  
  
  ```

  

## Export

 **Background:**

1.  Set export attribute for shell variables.父shell 与 子shell,继承父shell 所有参数以及属性；

2. 一个shell 中的系统变量会被复制到子shell 中（用export定义的变量）

3. 一个shell中的系统环境变量只对该shell或者它的子shell有效，该<span style='background-color:lightblue'>**shell结束时变量消失**</span>
   （并不能返回到父shell中）<span style='background-color:lightblue'>**脚本执行完后该子shell自动退出**</span>;

   ```shell
   vi explicit.sh
   a='i am apple'
   b='i am banana'
   c='i am cherry'
   export a
   ##export b
   export c
   bash
   
   
   
    a='robert'
   [wenbluo@phxdpeetl019 test]$ bash  explicit.sh
   [wenbluo@phxdpeetl019 test]$ echo $a
   i am apple
   [wenbluo@phxdpeetl019 test]$ exit
   exit   ###强制退出explict.sh 
   [wenbluo@phxdpeetl019 test]$ echo $a
   robert
   
   
   
   [wenbluo@phxdpeetl019 test]$ bash explicit.sh
   [wenbluo@phxdpeetl019 test]$ echo $a
   i am apple
   [wenbluo@phxdpeetl019 test]$ echo $c
   i am cherry
   [wenbluo@phxdpeetl019 test]$ echo $b
   
   ####说明shell 结束时候，变量消失！ ，除非显示export ！
   
   ```

   

4. exit命令 显示退出当前shell ;



-->类似css 继承

- shell 仅仅是一个程序，类似windows 一样是一个应用程序，(excel),运行多个命令就好比打开多个excel文档

- <span style='background-color:lightblue'>每一个shell 建立的变量都是局部变量</span>

  ```shell
  [wenbluo@phxdpeetl019 test]$ bash parent.sh
  [wenbluo@phxdpeetl019 test]$ bash child.sh
  hello wenbluo
  
  #less parent.sh 
  a='wbluo'  ### 也可以写成 export a ='wbluo'
  b='robert'
  export a
  
  #less child.sh 
  echo $a
  echo $b
  
  ```

- 子shell 不能更改父shell 参数

  ```shell
  [wenbluo@phxdpeetl019 test]$ export myglobal=10
  [wenbluo@phxdpeetl019 test]$ bash var4.sh
  myglobal=33
  [wenbluo@phxdpeetl019 test]$ echo $myglobal
  10
  
  #less var4.sh 
  !/bin/bash
  myglobal=33
  echo myglobal=$mglobal
  
  ```

- 如何传递局部变量呢？因为最终会返回到开机shell程序...

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_66.png)

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_67.png)

  为了能让局部变量能够传递下去，需要在子shell 中再开启一个子shell ，那么会子shell2 退出，则是返回到shell 1 而不是 父shell

```shell

less parent.sh
a='wbluo'
b='robert'
c='wenbluo'
echo $a
export c
export a
####没有开启新的shell 

parent_2.sh                 
#! /bin/bash
a='wbluo'
b='robert'
c='wenbluo'
echo $a
export a
export c
echo $c
bash   ##---开启新的shell (子shell)

less chid_2.sh
#!/bin/bash
echo $a
echo $b
echo $c


[wenbluo@phxdpeetl019 test]$ bash parent.sh
wbluo
[wenbluo@phxdpeetl019 test]$ bash chid_2.sh



[wenbluo@phxdpeetl019 test]$ bash parent_2.sh
wbluo
wenbluo
[wenbluo@phxdpeetl019 test]$ bash chid_2.sh
wbluo

wenbluo

```



- ksh  与  sh 的区别



1. 00 20 * * * /home/chewu/etl_home/bin/crontab.wrapper.ksh /export/home/chewu/etl_home/bin/dw_clsfd.job_runner_v2.ksh wenbluo_test > /dev/null 2>&1

## WGET



## SFTP/SCP/WSCP

- 跟ssh  用法一样，先connet 远程机子![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_131.png)

  通过 help 指令，可以查看有哪些cmd 

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_133.png)

- <span style='background-color:lightblue'>**通过get 指令**</span>，将远程的file cp 到local  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_132.png)

- <span style='background-color:lightblue'>**通过put指令**</span>，将local file upload  remote server

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_134.png)

- scp

  ```shell
         scp ./while_test wenbluo@phxdpeetl019.phx.ebay.com:/home/wenbluo/etl_home/sql_test/test/while_test
         wenbluo@phxdpeetl019.phx.ebay.com's password:      
  ```

- df -h 查看空间容量

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_135.png)

## awk

<a href='https://www.cnblogs.com/ggjucheng/archive/2013/01/13/2858470.html'> tutorial </a>

# Sudo

# Mount

<a href='https://blog.csdn.net/qq_39521554/article/details/79501714'>tutorial</a>

background 

shell /unix 系统<span style='background-color:lightblue'>**只有一个根目录  /**  </span>

<span style='background-color:lightblue'>**并不像window 系统，有C:D:E:F:G etc 分区硬盘**</span>

因此，如果要读取根目录之外的文件，这个时候需要挂载，目录即为“**挂载点**”。

总而言之，mount挂载的作用，就是<span style='background-color:lightblue'>**将一个设备（通常是存储设备）挂接到一个已存在的目录上。** </span>



- 如何查看挂在了那些设备？

  cat /etc/mtab

- ```shell
  sudo mount /dev/vdc /mnt/shared
  sudo mount /dev/vdd /opt/data
  sudo mount /dev/vde /export/home
  ```

- 

# Define Function

<a href='C:\Users\wenbluo\Desktop\wbluo\shell\add_date.sh'>date_add</a>

# Crontab

<a href=https://www.cnblogs.com/intval/p/5763929.html>Crontab</a>

1. Linux下的任务调度分为两类：系统任务调度和用户任务调度

2. ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_35.png)

3. ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_51.png)

   拓展：”%“

   - 特殊字符，相当于回车
   - 

4. ![](C:\Users\wenbluo\Desktop\wbluo\others\work_pict\pict_16.png)

   ```shell
   00 01 10 * * /home/chewu/etl_home/bin/crontab.wrapper.ksh /export/home/chewu/etl_home/bin/dw_clsfd.job_runner_v2.ksh motor_sc_monthly > /dev/null 2>&1
   
   ###每月10号运行
   ```

   

5. ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_37.png)

6. 执行文件:

   ```shell
   crontab -l grep bsc_weekly_table_archive.sql
   
   15 0 * * 0 /home/chewu/etl_home/bin/target_table_load_handler.ksh dw_clsfd.x td2 bsc_weekly_table_archive.sql
   
   ###单独调度运行archieve.sql文件  ，该文件就是delete old_table ,insert table ;等dml操作；
   ```

   ```shell
   00 20 * * * /home/chewu/etl_home/bin/crontab.wrapper.ksh /export/home/chewu/etl_home/bin/dw_clsfd.job_runner_v2.ksh wenbluo_test > /dev/null 2>&1
   
   #####运行批处理文件，调用crontab.wrapper.ksh 
   ```

   question :

   - wenbluo_test 在哪个目录下？

     /dw/etl/home/prod/land/dw_clsfd/job_runner ,是一个document 

     ```shell
     [wenbluo@phxdpeetl019 job_runner]$ cd wenbluo_test
     [wenbluo@phxdpeetl019 wenbluo_test]$ ll
     total 12
     drwxrwxr-x 2 wenbluo wenbluo 4096 Mar 28 06:14 cfg
     drwxrwxr-x 2 wenbluo wenbluo 4096 Mar 28 01:23 dat
     -rw-rw-r-- 1 wenbluo wenbluo  142 Mar 28 01:49 job_steps.tmp
     
     ```

     

   - <span style='background-color:lightgreen'>**crontab 有两种调度方式： 一种是tagert_table_load_handler.ksh  一种是crontab.wrapper.ksh and job_runner_v2.ksh**</span>

7. 注释 

   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_83.png)

   如何全部注释掉

   - 通过<span style='background-color:lightblue'>**添加 #**</span>
   - %s /^/# /g ; 

   

8. 

# Special Argument

| 变量 | 含义                                                         |
| ---- | ------------------------------------------------------------ |
| $0   | **当前脚本的文件名/or 开机启动运行的哪个版本shell**          |
| $n   | 传递给脚本或函数的参数。n 是一个数字，表示第几个参数。例如，第一个参数是$1，第二个参数是$2。 |
| $#   | 传递给脚本或函数的参数个数。                                 |
| $*   | 传递给脚本或函数的所有参数。                                 |
| $@   | 传递给脚本或函数的所有参数。被双引号(" ")包含时，与 $* 稍有不同，下面将会讲到。 |
| $?   | 上个命令的退出状态，或函数的返回值。                         |
| $$   | 当前Shell进程ID。对于 Shell 脚本，就是这些脚本所在的进程ID。 |
| $_   | 前一个命令最后一个参数![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_143.png)f![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_161.png) |
| !!   | ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_136.png)      |

- $$的运用

  表示进程id 号，其实shell 也是一个进程，用来控制底层 unix 环境or  shell 环境 的程序

  <span style='background-color:lightblue'>**如何将dir_26 目录下的文件直接搬到dir_25 目录下，并且删除dir_26 目录？**</span>

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_139.png)

  1. 首先将dir_26 目录下的文件，全部搬运到 /tmp /tmp$$ 目录下
  2. 最后再将 /tmp/tmp$$/* 所有文件 搬运到 dir_25 下

  /tmp 目录特点就是，当前会话关闭后，就会自动删除所有文件。

  watch out ! 

  <span style='background-color:red'>**不能通过 mv /home/wenbluo/etl_home/sql_test/test/dir_25/dir_26   /home/wenbluo/etl_home/sql_test/test/dir_25/  命名错误方式**</span>

- $?

  返回最近一次操作的返回码 ：0  表示corret  非0值表示error

- ```shell
   cat arg.sh
  #! /bin/bash
  echo "filename:$0"
  echo  "First parameter :$1"
  echo  "seconde parameter :$2"
  echo "quoted values :$*"
  echo "quoted values:$@"
  echo "total parameter number:$#"
  echo 'hello boy'
  
  $ ./arg.sh wenbluo robert
  ###直接怎么传参！
  filename:./arg.sh
  First parameter :wenbluo
  seconde parameter :robert
  quoted values :wenbluo robert
  quoted values:wenbluo robert
  total parameter number:2
  hello boy
  
  
  #### how to use $@
  
  #! /bin/bash
  echo "number od arguments passed is $#"
  for arg in "$@"
  do
  echo $arg
  done
  
  [wenbluo@phxdpeetl019 test]$ ./arg_2.sh 'a b' c
  number o arguments passed is 2
  a b
  c
  
  
  ```

- 



# Git/Github

**windows 运行shell脚本-->git**

<a href='https://blog.csdn.net/weixin_41714277/article/details/79399270'> git bash </a>&<a href=http://www.ituring.com.cn/article/264697>github</a>&<a href=https://blog.csdn.net/wangrenbao123/article/details/55511461>git-bash 详细教程</a>

git:目前世界上最先进的分布式[版本控制](http://lib.csdn.net/base/git)系统.

![](C:\Users\wenbluo\Desktop\wbluo\shell\other_1.png)

**将二进制文件放置到path环境参数中**

![](C:\Users\wenbluo\Desktop\wbluo\shell\other_2.png)

- 打开本地repository (仓库//或者叫目录)的文件夹  -->这样所有文件都可以被git管理起来

  每个文件的**，每个文件的修改，删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻还可以将文件”还原”。**以便任何时刻都可以追踪历史，或者在将来某个时刻还可以**将文件”还原”**。



## Concept

1. pull/push

   同步更新repository里的文件信息

2. fork

   完全赋值别人的repository 到自己repository中

3. star

   收藏

4. clone

   ![](C:\Users\wenbluo\Desktop\wbluo\git\git_5.png)

   将云端的repository 同步到本地

## QA

1. 在每次创建新的 repository 时候，为什么总是要勾上 initialize this repository with a readme ?

   ![](C:\Users\wenbluo\Desktop\wbluo\git\git_1.png)

2. Github desktop 

   监控作用：当本地repository（目录）下的文件有变动的时候，会及时反馈在desktop界面，然后查看变更记录，<span style='color:orange'>仓库里的文件变动都会被github记录下来</span>

   ![](C:\Users\wenbluo\Desktop\wbluo\git\git_2.png)

   **对于变动不合理的，你可以点击右侧 - 符号，回滚到未变更之前的状态；**

   **其次也可以先同步到本地，本机上编辑文件，然后sync到github网站上；**

   ![](C:\Users\wenbluo\Desktop\wbluo\git\git_3.png)

   当同意该变更时候，我们commit 提交这次变更，那么仓库就会少了pict_3文件

   ![](C:\Users\wenbluo\Desktop\wbluo\git\git_4.png)

   最后点击push ,那么云端就会同步本地repository,删掉pict_3文件；

3. 

# Blog

<a href=http://wiki.jikexueyuan.com/project/shell-learning/shell-learning-first-day.html>shell 学习的第一天</a>

# Practice

<a href=https://blog.csdn.net/l_215851356/article/details/70219330> 10 mins to learn shell</a>

1. 

# Printf  & Echo

# Tips

1. Ctrl+u : 清空整行
2. Tab:自动补充



# Function

- 

# Programming

## []  & [[]]  & (())



## 分号

<a herf='https://www.cnblogs.com/EasonJim/p/8315896.html'>reference</a>

<span style='background-color:lightblue'>**隔断每个语法关键字或命令**</span>

- 如果写成单行，则用 ；区分代码块

- 如果写成多行，则用\n （换行符）区分代码块

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_142.png)

  

## Character 

- concatenate 

  ```shell
   value1='wenbluo'
  [wenbluo@phxdpeetl019 bin]$ echo $value1
  wenbluo
  [wenbluo@phxdpeetl019 bin]$ echo ${value1}"-"
  wenbluo-
  [wenbluo@phxdpeetl019 bin]$ echo ${value1}'1'
  wenbluo1
  [wenbluo@phxdpeetl019 bin]$ echo ${value1}'2344'
  wenbluo2344
  [wenbluo@phxdpeetl019 bin]$ valu2='robert'
  [wenbluo@phxdpeetl019 bin]$ echo ${value1}${valu2}
  wenbluorobert
  ```

- processing skills

  1. ```shell
     $ echo $name
     dw_clsfd.clsfd_daily_onsiterev.sql
     $ echo ${name%.*}
     dw_clsfd.clsfd_daily_onsiterev
     $ echo ${name%%.*}
     dw_clsfd
     
     ##%表示从右往左匹配  
     ##.表示分割符
     ##  %%表示第几个分割符
     ##  * 表示被删除的字符串
     ```

  2. ```shell
     $ echo ${name#*.}
     clsfd_daily_onsiterev.sql
     $ echo ${name##*.}
     sql
     ## #表示从左往右匹配
     ```

  3. ```shell
     echo ${#name}
     34
     ###求字符串长度
     ```

     

## Date

通过 date --h ；可以查看帮助手册

1. 设置时间格式

   ```shell
   date '+%Y%m%d-%H%M%S'
   20190507-001840
   
   ###注意大小写
   
   ```

2. ```shell
   #获取昨天时间
   date -d yesterday
   Mon May  6 00:24:45 -07 2019
   #获取前两天时间  : 当前时间减2d
   date -d -2day
   Mon May  6 00:28:08 -07 2019
   #获取明天时间
   date -d tomorrow
   Wed May  8 00:28:23 -07 2019
   #获取后天时间  :当前时间加2d
   date -d 2day +'%Y%m%d'
   20190509
   
   
   ```

3. 时间运算--本质将时间转化为时间戳形式

   ```shell
   ###此时的date 不再是指系统变量date ，此时是一种函数 
   ###通过man date 可以查看字典
   ###date -d  : 表示display time described by string ,not 'now'
   ###
   
   date -d "$date" +%s
   1557212400
   date -d "2019-01-01" +%s
   1546326000
   
   
   ###将时间戳转化为Y-M-D
   date -d @1546326000  +'%Y%m%d'
   20190101
   
   ###也可以转化为天数  ---通过man date 查看具体参数 是j or  d  or D etc
   date -d "$date" +%j
   127
   
   
   
   
   ```

   

4. 

## Wrapper

- 

  ```shell
  cat refresh_weekly_sc_core.ksh
  #!/usr/bin/ksh
  
  start_date=$1
  end_date=$2
  
  ksh /home/$(whoami)/etl_home/bin/target_table_load_handler.ksh dw_clsfd.step2_clsfd_daily_sc_core_mtrc td2 dw_clsfd.step2_clsfd_daily_sc_core_mtrc.sql start_date=$start_date end_date=$end_date
  ksh /home/$(whoami)/etl_home/bin/target_table_load_handler.ksh dw_clsfd.step3_clsfd_daily_sc_cluster_target td2 dw_clsfd.step3_clsfd_daily_sc_cluster_target.sql
  
  ksh /home/$(whoami)/etl_home/bin/target_table_load_handler.ksh dw_clsfd.step2_clsfd_weekly_sc_core_mtrc td2 dw_clsfd.step2_clsfd_weekly_sc_core_mtrc.sql start_date=$start_date end_date=$end_date
  ksh /home/$(whoami)/etl_home/bin/target_table_load_handler.ksh dw_clsfd.step3_clsfd_weekly_sc_cluster_target td2 dw_clsfd.step3_clsfd_weekly_sc_cluster_target.sql
  ```

- ```shell
  refresh_weekly_sc_core.ksh 2019-01-01 2019-02-28
  ```


## 流程控制

- if..then..fi 

- if..then..else..fi 

  如果为真 0 ，执行then 语句 ，并忽略esle代码块，

  如果为假(非0)，执行else代码块；

- if..then..elif..then..else..fi

  ```shell
  a=10 
  b=20
  if [ $a -gt $b ]
  then 
  echo "$a -ge $b : a大于b"
  elif [ $a -lt $b ]
  then 
  echo  "$a -lt $b : a小于b"
  else 
  echo "$a -eq $b: a等于b"
  fi
  ```

  <span style='color:red'>**困惑一定要打分号？**</span>![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_102.png)
  
  ; :必不可少， 表示语句块的作用
  
  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_145.png)


- for 循环：

  <span style='background-color:lightblue'>**格式 ：for xx  ;do  xxxx done ;**</span>

   In  方法：

  ```shell
  $ for loop in 1 12 3
  > do echo "the value is :$loop"
  > done
  the value is :1
  the value is :12
  the value is :3
  
  ## for do done 
  ## 系统会先创建 创建一个变量 loop  然后赋值给loop;
  
  
  ```

  C 方法：

  ```shell
  #! /bin/bash
  a=0
  b=0
  for (( $a; $a<=20; ))
  do
  let b=$b+$a
  let a=$a+1
  echo "a value is :$a , the cumulate value is :$b"
  sleep 2
  done
  
  
  ```

  

- while 循环：

  <span style='background-color:lightblue'>**格式： while  condition  do done** </span>

  ```shell]
  #! /bin/bash
  read -p "enter your num and name:" num name   ###不支持ksh指令
  while [ $num  -lt 10 ]
  do
     #print($num)
     echo  $num
     #$num+=1
     #$num=$num+1
     let num=$num+1
     name=`expr $name + 1`
    #((name+1))
     sleep 2
     # sleep(2) Error
     echo $name
  done
  
  ###while do  done
  
  
  ```

  1. 写无限循环：  true  or  : 占位符

     ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_146.png)

  2. 

- Case 

```shell
case &cntry  in
'za') clsfd_site_list=201;;
'uk') clsfd_site_list=1011;;
'sg') clsfd_site_list=198;;
*) echo "cannnot decide a proper clsfd_site_id list for input value cntry=" 
esac


```

1. 除了*)模式，各个分支中<span style='background-color:yellow'>**;;**</span>是必须的，;;相当于其他语言中的break
2. | 分割多个模式，相当于or

<span style='background-color:lightblue'>**-->  拓展case ...esac** </span>

- 



# Git

<a href='https://git-scm.com/book/zh/v1/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E7%AE%A1%E7%90%86'>![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_1489.png)</a>



background :分布式版本控制系统

<a href='https://www.cnblogs.com/nanchen/p/9528778.html'>Git命令笔记</a>

<a  href='https://www.liaoxuefeng.com/wiki/896043488029600/897271968352576'>廖雪峰git</a>



## 集中式vs分布式

<a  href='https://www.liaoxuefeng.com/wiki/896043488029600/896202780297248'>分布式的优势</a>

- 集中式：所有人都可以连接服务器，读取并更新文件。劣势：当出现<span style='background-color:lightgreen'>**单点故障**</span>时候，谁都无法操作。如果数据丢失，没有良好的完善机制。

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_148.png)

- 分布式：<span style='background-color:lightgreen'>**每一台电脑都有一份完整的版本库**</span>--git pull 

  分布式控制系统，通常也有一台中央服务器的电脑，

  但是这个服务器的作用仅仅是方便<span style='background-color:lightgreen'>**“交换”**</span>大家的修改，没有它大家也可以干活，只是交换修改不方便而已 --git add /commit /branch /merge 

  ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_149.png)

## Config

git config --global  user.name "wenbluo"

git config --global  user.email  "wenbluo@ebay.com"

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_154.png)

<span style='background-color:lightblue'>**ps：因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址**</span>

git config -l : 查看所有的配置信息  

git config --help

## Repository

版本库又叫仓库，简单来说就是一个目录，然后目录下的文件（增删改），都可以被git监控

1. 初始化本地仓库

   git  init 

   ```shell
   $ git init
   Initialized empty Git repository in C:/Users/wenbluo/Desktop/wbluo/git/learngit/.git/
   ```

   然后就在目录learngit ,生成一个.git 文件  -->请勿改动

   -->

      如何结束inti 初始化？   undo init .

     直接删除掉 .git 目录就行 

     <span style='background-color:lightblue'>**rm -rf  .git   ###r remove -r .git** </span>

2. 添加文件

   git add readme.txt

3. 提交文件

   git commit -m "i wrote a txt "

   ```shell
   $ git commit -m "wrote a readme file"
   [master (root-commit) ffbec42] wrote a readme file
    1 file changed, 3 insertions(+)
    create mode 100644 readme.txt
    
    ### 1 file changed ,3insertions :表示一个文件上传，并且总行数是3行
    ### -m:  comment  备注一定要，这样可以知道每次操作是什么，便于追溯历史文件
   ```

   <span style='background-color:lightblue'>**ps  : add  与 commit 区别**</span>

   - 文件的三种状态  

     对于任何一个文件，在git 内都只有三种状态： committed,  staged ,modified 

   - 三种工作区域： worlking directory ,staging directory ,repository 

     分别存放三种状态的文件： modified , staged, committed

     

     

     

   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_147.png)

   - commit 可以一次提交多个文件

     第一步是用`git add`把文件添加进去，实际上就是把<span style='background-color:lightblue'>**文件修改添加到暂存区**；</span>

     ![](C:\Users\wenbluo\Desktop\wbluo\git\git_8.png)

   - 像这样，你不断对文件进行修改，然后不断提交修改到版本库里，就好比玩RPG游戏时，每通过一关就会自动把游戏状态存盘，如果某一关没过去，你还可以选择读取前一关的状态。有些时候，在打Boss之前，你会手动存盘，以便万一打Boss失败了，可以从最近的地方重新开始。Git也是一样，每当你觉得文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在Git中被称为`commit`

     第二步是用`git commit`提交更改，实际上就是把<span style='background-color:lightblue'>**暂存区的所有内容提交到当前分支**</span>。

     ![](C:\Users\wenbluo\Desktop\wbluo\git\git_9.png)
     
     ```bash
     第一次修改 -> `git add` -> 第二次修改 -> `git commit`
     
     你看，我们前面讲了，Git管理的是修改，当你用`git add`命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，`git commit`只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。
     ```
     
     

4. git  status

   <span style='background-color:lightblue'>**掌握仓库当前的状态**</span>

   ```shell
   $ git status
   On branch master
   No commits yet
   Changes to be committed:
     (use "git rm --cached <file>..." to unstage)
   
           new file:   readme.txt
   ###告知有一个新文件添加到仓库中，待提交
   
   $ git status
   On branch master
   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git checkout -- <file>..." to discard changes in working directory)
   
           modified:   readme.txt
   
   no changes added to commit (use "git add" and/or "git commit -a")
   ###告知有一个文件被修改，但还没被提交
   
   ###当想知道到底变动了什么？可以通过git diff readme.txt
   
   $ git diff readme.txt
   diff --git a/readme.txt b/readme.txt
   index 8797631..4d53f0e 100644
   --- a/readme.txt
   +++ b/readme.txt
   @@ -1,3 +1,2 @@
   -git is a version control system.  ##删除
   +git is a distributed version control system.
    git is free software.  ##新增
   
   ###舍弃变动
   git checkout -- readme.txt  ###返回上一个版本
   
   ```

5. git log  

   ```shell
   ![git_6](C:\Users\wenbluo\Desktop\wbluo\git\git_6.png)#查看操作轨迹
   
   git log --pretty=oneline;
   ##简洁的形式  自上而下 以时间顺序 desc 
   f238418b2efa5acc47489087fd1d2e38d707fef2 (HEAD -> master) append GPL 
   e9eef556941ce1a31385dbcefe2d9efc24d8d34b add distribute
   ffbec428f968e71bf5f4ec1dfe9bd203134a8190 wrote a readme file
   
   ##如果想回到add distribute 版本
   ##采用 git reset --hard head^  or  git reset --hard e9eef55 
   
   ##当回滚到第二版本时候，发现append GPL 没有了！
   $ git log --pretty=oneline
   e9eef556941ce1a31385dbcefe2d9efc24d8d34b (HEAD -> master) add distribute
   ffbec428f968e71bf5f4ec1dfe9bd203134a8190 wrote a readme file
   
   ###如果还想回到最后修改的版本 append GPL 怎么办
     1：当会话没有关闭时候，可以查看commit id 
        git reset --hard f23841
     2:如果会话关闭了，不知道commit id 怎么办?
        $ git reflog  
   e9eef55 (HEAD -> master) HEAD@{0}: reset: moving to e9eef55
   f238418 HEAD@{1}: reset: moving to f238418b
   e9eef55 (HEAD -> master) HEAD@{2}: reset: moving to e9eef55
   f238418 HEAD@{3}: reset: moving to f2384
   e9eef55 (HEAD -> master) HEAD@{4}: reset: moving to head^
   f238418 HEAD@{5}: commit: append GPL  ##查找到了append GPL commit id 
   e9eef55 (HEAD -> master) HEAD@{6}: commit: add distribute
   ffbec42 HEAD@{7}: commit (initial): wrote a readme file
     #简介一点的
      git reflog  | grep -i "append Gpl"
   f238418 HEAD@{5}: commit: append GPL
   
   ```

   

## Working directory  and Repository

1. working directory :工作区

   ![](C:\Users\wenbluo\Desktop\wbluo\git\git_7.png)

2. stage(index):暂缓区

   ![](C:\Users\wenbluo\Desktop\wbluo\git\git_6.png)

   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_150.png)

3. 删除文件

   git checkout -- test.out 

   其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

4. 

## Pull

git checkout master

git pull  :**更新当前分支所有信息到本地**

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_117.png)

git pull origin master  : 更新 orign 中 master 下最新文件信息

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_117.png)

git pull typora typora : <span style='background-color:lightblue'>**表示更新typora connect 下 typora 分支**</span>

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_118.png)

## Push

git remote set-url --push origin git@github.scm.corp.ebay.com:apdrm/DINT-CLSFD.git

git push origin master  ##将 master  branch 上的信息 推送到 origin 上



### QA

1. 如何将本地repository push 到 github上？
   - step one :在github 上创建repository 
   - step two : git remote add typora https://github.com/privatewbluo/typora.git
   - step three  : git  pull typora master  :本地同步 github上typora master 分支信息
   - step four : git add  & git commit 
   - step five : git push typora master 将本地文件push到github上 
   
2. pull request 

   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_166.png)
   
   有什么作用？
   
   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_168.png)
   
    已经告知，将0904 分支 合并到master 主分支上。
   
    git checkout master 
   
    git merge 0904 
   
   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_169.png)

## Merge

如何file 还在working direcory , 那么不管在哪个branch 都能看到or 同步

1. git checkout  master 

   git merge 0904 #meger branch 0904 to master 

2. git branch -d  0904 :删除分支

3. ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_167.png)

4. website 删除分支

   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_170.png)

   zxcxzc

## Status

1. git status 可以查看变化

   -->那么如果查看具体变化呢？

   <span style='background-color:lightblue'>**通过git  diff  shell.md** </span>

2. 

## Fork

clone repository 到个人账户

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_116.png)

## Clone

<a href='https://blog.csdn.net/sdta25196/article/details/80203152'>如何ping github.com 成功？</a>

<span style='background-color:lightblue'>**从远程库 (origin)  clone 到本地环境**</span>

![](C:\Users\wenbluo\Desktop\wbluo\git\git_10.png)

为了避免每次远程clone  repository 到本地 都要输入 账号与密码：

<a href='https://help.github.com/en/articles/connecting-to-github-with-ssh'>可以采用远程ssh </a>



1. ![](C:\Users\wenbluo\Desktop\wbluo\git\git_12.png)

   点击个人账户，添加pub key 

   - <span style='background-color:lightblue'>**ssh-keygen**</span> :生成public  key 

   - Paste the **content** of your public key here, usually from your **~/.ssh/id_rsa.pub**.

     <span style='background-color:lightgreen'>clip < /c/Users/wenbluo/.ssh/id_rsa.pub  :**将密码放到剪贴板**</span>

2. clone的ssh 地址![](C:\Users\wenbluo\Desktop\wbluo\git\git_11.png)

3. git clone git@github.corp.ebay.com:clsfd/CLSFD_DWH.git  

   clone 一个项目

   ---ecg 固定relese repository :git@github.corp.ebay.com:APD/DINT-CLSFD.git

   https://github.corp.ebay.com/APD/DINT-CLSFD/commit/DW_PDP_DWRM32943


### QA

- 可以直接clone 不需要配置or identify?
- 

## Fetch

## Remote

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_151.png)

<span style='background-color:lightgreen'>**只有clone 之后才能添加remote?  目的是能更改github 上的repository** </span>

<span style='background-color:lightblue'>**如何远程通信github 上的repository？**</span>

你已经在本地创建了一个git仓库后，又想在Github创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库协作，一举多得 。

1. git remove set -url  fetch 

   `git remote ``set``-url origin git@github.scm.corp.ebay.com:apdrm/<project>.git`

   ```shell
    git remote set-url git@github.scm.corp.ebay.com:APD/DINT-CLSFD.git
    
   ```

   <span style='background-color:lightblue'>**注意是 git 开头**</span>![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_155.png)

   ##### QA

   - 如何将本地repository push 到githup上？

     1. step_1: git init 

     2. step_2: git add .

     3. step_3:git commit -m 'xxxx'

     4. step_4:git remote add origin xxx

        ```shell
        wenbluo@L-SHC-16505239 MINGW64 ~/Desktop/typora (master)
        $ git remote add orgin  https://github.corp.ebay.com/wenbluo/typora.git
        
        wenbluo@L-SHC-16505239 MINGW64 ~/Desktop/typora (master)
        $ git remote -v
        orgin   https://github.corp.ebay.com/wenbluo/typora.git (fetch)
        orgin   https://github.corp.ebay.com/wenbluo/typora.git (push)
        ##与github上的repository创建连接
        ```

     5. step_5:git push -u origin master

2. git remote set-url  push

   `git remote ``set``-url --push origin git@github.scm.corp.ebay.com:APD/<project>.git`

   ```shell
   git remote set-url --push origin git@github.scm.corp.ebay.com:APD/DINT-CLSFD.git
   ```

   -   git remote add origin  git@github.scm.corp.ebay.com:APD/DINT-CLSFD.git

       -->本地关联远程库：DINT-CLSFD

       -->Origin:远程库，git默认的叫法

   - git  push  -u origin master 

       -->把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

   - git  push origin  master 

3. git remote  -v

   $ git remote -v
   origin  git@github.scm.corp.ebay.com:APD/DINT-CLSFD.git (fetch)
   origin  git@github.scm.corp.ebay.com:APD/DINT-CLSFD.git (push)
   
   #这样就可以对project or  repository DINT-CLSFD 进行**fork or push** 
   
4. ```shell
   $ git remote add typora git@github.corp.ebay.com:wenbluo/typora.git
   ###添加远程链接 typora 
   ###说明远程链接库，并不一定非是 origin
   wenbluo@L-SHC-16505239 MINGW64 ~/Desktop/typora/.git (GIT_DIR!)
   $ git remote -v
   origin  https://github.corp.ebay.com/APD/DINT-CLSFD.git (fetch)
   origin  https://github.corp.ebay.com/APD/DINT-CLSFD.git (push)
   typora  git@github.corp.ebay.com:wenbluo/typora.git (fetch)
   typora  git@github.corp.ebay.com:wenbluo/typora.git (push)
   
   
   git config 文件：
   
   ```
   
5. 如何去掉 remote ?

6. 

git config --list :查看所有配置信息

   ![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_114.png)

   ```
cat .git 目录下 config 文件可以看到 typora remote 还没有push 功能。
   
   ```shell
   $ git remote set-url --push  typora git@github.corp.ebay.com:wenbluo/typora.git
   
   ### --push typora : 对typora link 添加push 功能
   ```

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_119.png)

## Master & Branch

background:  两个指针 ： head  &  branch 

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_120.png)



当你创建branch dev ： git checkout -b dev  

此时 head 指针指向 dev  分支  ，然后每次commit 是在dev  分支 上；



![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_121.png)



如果我们在dev 分支上的工作完成了，就可以将dev 分支合并master 上。git 最简单的办法，就是直接将master 指向dev 的当前提交。



![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_122.png)



```bash
# go back to project root directory and get latest master
cd <project-root> # check with `pwd`
git checkout master  ##切换到主分支
git pull # if this fails, run `git branch --set-upstream master origin/master`  :  更新 master 下 所有文件信息；
git checkout -b ad_cube master  ###查看branch 
##fatal: A branch named 'ad_cube' already exists

```

![](C:\Users\wenbluo\Desktop\wbluo\git\git_13.png)

```shell
# go back to our branch and merge things from latest master
git checkout <branch>
git merge master


```

git checkout -b dev :

-->表示创建分支 dev  ,并且切换到dev 该分支下；  

```shell
###相当于下面两步操作
git branch dev  
git checkout dev  ###切换到dev branch

###查看分支
$ git branch
* dev     ###前面*表示当前分支
  master
###查看各个分支 最后提交的最后comment 
git branch -v 

###查看已经合并的分支
git branch --merge   --no-merge   :没有合并的分支

###删除分支 
git branch -d xxxxx    |  -D  xxxx ：强制删除分支，忽略是否有file ,没有提交到该分支上
  
###查看文档： 
git branch --help
  
```

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_106.png)

<span style='background-color:lightblue'>**git checkout master : 切换 master 主分支**</span>的时候，发现是我们在ad_cube 修改的readme.txt <span style='background-color:lightgreen'>**文件 并没有影响  master 上的文件 ！**</span>

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_107.png)

```shell
  
  git merge ad_cube  
 ### 将ad_cube 合并到当前分支 ：
 ### 将ad_cube 分支 与mster 合并。
 
  
  ##此时 master  readme.txt文件就会变更
  
  git branch -d  ad_cube ##删除分支 ad_cube 
```

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_108.png)



<span style='background-color:orange'>**git checkout -b dev origin/dev  : copy  远程repostiory 的一个branch dev**</span>

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_123.png)

**Tips**&**QA**

1. 如何安全branch  切换到其他们branch /master ?

   留心 staging areas / 缓冲区，那些没有提交的修改,他会和你即将检出的分支产生冲突，从而阻止你change branch .

2. 

## Tag

<a href='https://wiki.vip.corp.ebay.com/display/DW/KnowledgeBase+for+ECG+DW+Developers'>Reference tag </a>

git tag  : 可以查看已经有那些标签

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_103.png)

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_115.png)

通过githup ,可以通过tag <span style='background-color:lightblue'>**DW_PDP_DWRM32943_1**</span>

1. ​    总共有35个变化，其中17个新增 ( <span style='color:green'>**绿色部分**</span> )，18个delete  ( <span style='color:red'>**红色部分**</span>)

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_104.png)

### QA :difference Branch & Tag

## Release 

<a href='https://wiki.vip.corp.ebay.com/display/HWDI/GitHub+essentials+for+APD+ETL+development'>git to release </a>

1. 



## checkout 

1. git checkout -b ad_cube 

   ```shell
   $ git checkout -b ad_cube
   fatal: A branch named 'ad_cube' already exists.
   
   ```

2. git checkout -- readme.txt  

   <span style='background-color:lightblue'>**恢复最近一次readme.txt 原始文件**</span>

   -   注意：  <span style='background-color:lightblue'>**--  两个横杠一个都不能少！**</span>

![](C:\Users\wenbluo\Desktop\wbluo\shell\shell_105.png)

1. 

## Stash

临时性的将当场工作“隐藏”起来，等以后恢复工作现场后继续工作. <span style='background-color:lightblue'>**然后就可以切换git checkout master ,等其他branch 工作**</span>;

git stash  ##隐藏当前工作区

git stash list ##查看有多少stash

git stash apply  stash@{o}  : 通过apply id 来恢复

## Roll Back

## 

