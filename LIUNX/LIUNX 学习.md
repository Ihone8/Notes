### LIUNX 学习

####  处理目录的常用命令


* #####  ls: 列出目录

  1. ls -a ：全部的文件，连同隐藏档( 开头为 . 的文件) 一起列出来(常用)
  2. ls  -d ：仅列出目录本身，而不是列出目录内的文件数据（常用）
  3. ls  -l  ：长数据列出，包含文件的属性和权限等等数据
  4.  ls -al ~   将该目录下所有文件列出来（包含属性和隐藏档）

* ##### cd：切换目录  [Change Directory]

  1. cd  [ 相对路径  / 绝对路径]
     1.  使用 mkdir 命令创建一个文件夹 MyDirectory <br/>使用绝对路径切换到 MyDirectory   cd  /root/ MyDirectory <br/>使用相对路径切换到 MyDirectory   cd ./MyDirectory
     2.  cd ~ 表示回到家的目录 也就是  /root 这个目录
     3. cd ..   表示返回上一级的目录

* ##### pwd：显示目前的目录 [ Print Working Directory ]

  1. pwd [ -P  ]（注意大小写）  显示出确实的路径，而非使用连接 【Link】的路径

* ##### mkdir：创建一个新的目录 [make directory]

  1. mkdir [ -m ] ： 配置文件的权限！，直接配置，不需要看默认权限（umask）的脸色
  2. mkdir [ -p ] ： 创建多级树结构目录 如：mkdir -p tets/test1/test2  不使用 -p 则会创建失败

* ##### rmdir：删除一个空的目录

   1. rmdir [ -p ] 目录名称 ：连同上一级 [ 空的 ] 目录页一起删除


* ##### cp: 复制文件或目录

  1. cp ~/test1    /tem 将该目录下 test1 文件/ 目录 复制到   tem 文件夹中
  2. cp -i ~/test1    /tem 当 tem 目录下存在 test1 文件 或目录时 <br/>cp: overwrite ‘/tem’? n   <==n不覆盖，y为覆盖

* ##### rm: 移除文件或目录

  1. rm [ -f ] 就是 force 的意思，忽略不存在的文件，不会出现警告信息；
  2. rm [ -i ]   删除时会先询问 是否 删除  n / y 
  3. rm  [ -r]  递归删除啊！最常用在目录的删除了！这是非常危险的选项！！！

* ##### mv (移动文件与目录，或修改名称)

  1.  mkdir mydemo   创建一个文件夹
  2. mkdir t.txt 创建一个 txt 文件
  3. mv t.txt  mydemo  把 t.txt 文件 移动到 mydemo 文件夹
  4. cd mydemo  进入 mydemo 文件夹  mv t.txt 1.txt 将 t.txt 名称改为 1.txt





#### 用户和用户组管理

* ##### 添加新的用户账号 使用 useradd  注: 刚添加的账号是被锁定 无法使用

  * useradd  [ 选项]  用户名称

    1. -c comment 指定一段注释性描述。
    2. -d 目录 指定用户主目录，如果此目录不存在，则同时使用-m选项，可以创建主目录
    3. -g 用户组 指定用户所属的用户组。
    4. -G 用户组，用户组 指定用户所属的附加组
    5. -s Shell文件 指定用户的登录Shell。
    6. -u 用户号 指定用户的用户号，如果同时有-o选项，则可以重复使用其他用户的标识号。

  * ```
    useradd –d /usr/sam -m sam
    添加一个用户 sam  -d  和 -m  为 用户 sam 产生一个主目录/usr/sam（/usr为默认的用户主目录所在的父目录）。
    ```

  * ```
    添加用户组
    groupadd group , groupadd gem 添加 group 组 和 adm 组
    
    useradd -s /bin/sh -g group –G adm,root gem
    创建一个用户 adm 该用户登入shell 是 bin/sh 属于 group 组
    同时属于 adm  和 root 组 ，group 是 主组
    ```

* ##### 删除账号

  * ```
    userdel [ -r ] 用户名
    ```

  * 

* ##### 用户组管理

  * ```
    添加用户组
    groupadd 选项 用户组名称
     选项：
     	 -g GID 指定新用户组的组标识号（GID）
     	 -o 一般和 -g 选项同时使用，表示新用户组的 GID 可以和系统已存在用户组的 GID 相同
     	
     示例：
     	groupadd group1  添加一个名为 group1 的用户组
     	groupadd -g 101 group2 添加一个 group2 用户组 ，标识号为 101
    ```

  * ```
    删除用户组
    groupdel 用户组
    	示例：
    		groupdel group1 删除组名为 group1 的用户组
    ```

  * ```
    修改用户组的属性
    groupmod 选项 用户组
    	选项：
    		g GID 为用户组指定新的组标识号。
    		-o 与-g选项同时使用，用户组的新GID可以与系统已有用户组的GID相同。
    		-n新用户组 将用户组的名字改为新名字
    	示例：
    		groupmod -g 102 group2 修改 group2 用户组的标识号为 102
    		groupmod –g 10000 -n group3 group2 修改group2 标识号为 10000 名称为 group3
    
    ```

  *  如果一个用户同时属于多个用户组，那么用户可以在用户组之间切换，以便具有其他用户组的权限。            new  root    将当前用切换到 root 组

* ##### 与用户账号有关系的系统文件

  * ```
     /etc/passwd  文件 是用户管理工作涉及最重要的一个文件 记录每一个用户的属性 对每个用户都是可读的
     cat /etc/passwd  使用 该命令读取该文件内容
     如下：
     	root:x:0:0:root:/root:/bin/bash
       	bin:x:1:1:bin:/bin:/sbin/nologin
       	daemon:x:2:2:daemon:/sbin:/sbin/nologin
       	lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
       	sync:x:5:0:sync:/sbin:/bin/sync
       	shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
       	。。。。。 
       	分别用 : 分割开 对应下面的意思
     ```

    	用户名:口令:用户标识号:组标识号:注释性描述:主目录:登录Shell

     ```
  
     ```


* ##### 拥有账户文件

  * 由于/etc/passwd文件是所有用户都可读的，如果用户的密码太简单或规律比较明显的话，一台普通的计算机就能够很容易地将它破解，因此对安全性要求较高的Linux系统都把加密后的口令字分离出来，单独存放在一个文件中，这个文件是/etc/shadow文件。 有超级用户才拥有该文件读权限，这就保证了用户密码的安全性。
    /etc/shadow中的记录行与/etc/passwd中的一一对应，它由pwconv命令根据/etc/passwd中的数据自动产生，它的文件格式与/etc/passwd类似，由若干个字段组成，字段之间用":"隔开。这些字段是：

    登录名:加密口令:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:标志

    1. "登录名"是与/etc/passwd文件中的登录名相一致的用户账号
    2. "口令"字段存放的是加密后的用户口令字，长度为13个字符。如果为空，则对应用户没有口令，登录时不	   需要口令；如果含有不属于集合 { ./0-9A-Za-z }中的字符，则对应的用户不能登录。
    3. "最后一次修改时间"表示的是从某个时刻起，到用户最后一次修改口令时的天数。时间起点对不同的系统	  可能不一样。例如在SCO Linux 中，这个时间起点是1970年1月1日。
    4. "最小时间间隔"指的是两次修改口令之间所需的最小天数。
    5. "最大时间间隔"指的是口令保持有效的最大天数。
    6. "警告时间"字段表示的是从系统开始警告用户到用户密码正式失效之间的天数。
    7. "不活动时间"表示的是用户没有登录活动但账号仍能保持有效的最大天数。
    8. "失效时间"字段给出的是一个绝对的天数，如果使用了这个字段，那么就给出相应账号的生存期。期满	后，该账号就不再是一个合法的账号，也就不能再用来登录了。

  ```c
  /etc/shadow 示例：
  	bin:*:17110:0:99999:7:::
  	daemon:*:17110:0:99999:7:::
  	lp:*:17110:0:99999:7:::
  	sync:*:17110:0:99999:7:::
  	shutdown:*:17110:0:99999:7:::
  	halt:*:17110:0:99999:7:::
  	。。。。。。
  
  ```

* ##### 用户组的所有信息都存放在/etc/group文件中。

  * 将用户分组是Linux 系统中对用户进行管理及控制访问权限的一种手段。

    每个用户都属于某个用户组；一个组中可以有多个用户，一个用户也可以属于不同的组。

    当一个用户同时是多个组中的成员时，在/etc/passwd文件中记录的是用户所属的主组，也就是登录时所属的默认组，而其他组称为附加组。

    用户要访问属于附加组的文件时，必须首先使用newgrp命令使自己成为所要访问的组中的成员。

    用户组的所有信息都存放在/etc/group文件中。此文件的格式也类似于/etc/passwd文件，由冒号(:)隔开若干个字段，这些字段有

    ```
    组名:口令:组标识号:组内用户列表
    ```

    1. "组名"是用户组的名称，由字母或数字构成。与/etc/passwd中的登录名一样，组名不应重复。

    2. "口令"字段存放的是用户组加密后的口令字。一般Linux 系统的用户组都没有口令，即这个字段一般为空，或者是*。

    3. "组标识号"与用户标识号类似，也是一个整数，被系统内部用来标识组。

    4. "组内用户列表"是属于这个组的所有用户的列表/b]，不同用户之间用逗号(,)分隔。这个用户组可能是用户的主组，也可能是附加组。

       ````
       cat /tec/group  示例：
           root:x:0:LGQ
           bin:x:1:
           daemon:x:2:
           sys:x:3:
           tty:x:5:
           disk:x:6:
           lp:x:7:
           mem:x:8:
           kmem:x:9:
           wheel:x:10:
           。。。。
       
       ````


#### 磁盘管理

* df：列出文件系统的整体磁盘使用量

   ```
    df [选项] 目录名称
   ```



  >- -a ：列出所有的文件系统，包括系统特有的 /proc 等文件系统；
  >- -k ：以 KBytes 的容量显示各文件系统；
  >- -m ：以 MBytes 的容量显示各文件系统；
  >- -h ：以人们较易阅读的 GBytes, MBytes, KBytes 等格式自行显示；
  >- -H ：以 M=1000K 取代 M=1024K 的进位方式；
  >- -T ：显示文件系统类型, 连同该 partition 的 filesystem 名称 (例如 ext3) 也列出；
  >- -i ：不用硬盘容量，而以 inode 的数量来显示
  >
  >

  * > ```
    >  df 后面不加任何选择 默认是不将所有文件显示出来
    > 示例：
    > 	df 
    >         Filesystem     1K-blocks    Used Available Use% Mounted on
    >         /dev/vda1       41151808 1808428  37229948   5% /
    >         devtmpfs          931516       0    931516   0% /dev
    >         tmpfs             941860       0    941860   0% /dev/shm
    >         tmpfs             941860    8488    933372   1% /run
    > 
    >  
    >  	df -a 
    >  		Filesystem     1K-blocks    Used Available Use% Mounted on
    >         rootfs                 -       -         -    - /
    >         sysfs                  0       0         0    - /sys
    >         proc                   0       0         0    - /proc
    >         devtmpfs          931516       0    931516   0% /dev
    >         securityfs             0       0         0    - /sys/kernel/security
    >         tmpfs             941860       0    941860   0% /dev/shm
    >         devpts                 0       0         0    - /dev/pts
    >         tmpfs             941860    8488    933372   1% /run
    >         tmpfs             941860       0    941860   0% /sys/fs/cgroup
    >         。。。。。。。。
    > 
    > ```
    >
    >

* du：检查磁盘空间使用量  ，Linux du命令也是查看使用空间的，但是与df命令不同的是Linux du命令是对文件和目录磁盘使用的空间的查看，还是和df命令有一些区别的

    ``` du
    du  [ 选项 ]  文件或目录名称 
    ```

  > * -a ：列出所有的文件与目录容量，因为默认仅统计目录底下的文件量而已。
  > * -h ：以人们较易读的容量格式 (G/M) 显示；
  > * -s ：列出总量而已，而不列出每个各别的目录占用容量；
  > * -S ：不包括子目录下的总计，与 -s 有点差别。
  > * -k ：以 KBytes 列出容量显示；
  > * -m ：以 MBytes 列出容量显示；

  > ```
  > > > > 示例： 
  > > > > 	1. du   列出目前目录下的所有文件容量  直接输入 du 没加任何选项时，则 du 会分析当前目录			文件与目录所占的容量
  > > > > 		8	./.oracle_jre_usage
  > > > >         8	./.pip
  > > > >         4	./.ssh
  > > > >         4	./runoob
  > > > >         4	./tem/test1/test2/test3
  > > > >         8	./tem/test1/test2
  > > > >         12	./tem/test1
  > > > >         。。。。。
  > > > >     2. du -a 将文件容量也列出来
  > > > >     	4	./.bashrc
  > > > >         4	./.bash_profile
  > > > >         4	./mydemo/1.txt
  > > > >         8	./mydemo
  > > > >         4	./.pydistutils.cfg
  > > > >         4	./.bash_history
  > > > >         。。。。。。
  > > > >     3.  du -sm /* 检查根目录底下每个目录所占用的容量 ，通配符 * 来代表每个目录。
  > > > >     	0	/bin
  > > > >         140	/boot
  > > > >         0	/dev
  > > > >         32	/etc
  > > > >         1	/home
  > > > >         0	/lib
  > > > >         0	/lib64
  > > > >         。。。。。。
  > > > > ```
  > > > ```
  > > ```
  > ```

* fdisk：用于磁盘分区 ，fdisk 是 Linux 的磁盘分区表操作工具。

   ```
    fdisk [ 选项 ] 装置名称
   ```

  > * -l ：输出后面接的装置所有的分区内容。若仅有 fdisk -l 时， 则系统将会把整个系统内能够搜寻到的装置的分区均列出来。

  > ```
  > > > > 示例：
  > > > > 	1. fdisk -l  列出所有分区信息
  > > > > 		Disk /dev/vda: 42.9 GB, 42949672960 bytes, 83886080 sectors
  > > > >         Units = sectors of 1 * 512 = 512 bytes
  > > > >         Sector size (logical/physical): 512 bytes / 512 bytes
  > > > >         I/O size (minimum/optimal): 512 bytes / 512 bytes
  > > > >         Disk label type: dos
  > > > >         Disk identifier: 0x0008de3e
  > > > > 
  > > > >            Device Boot      Start         End      Blocks   Id  System
  > > > >         /dev/vda1   *        2048    83884031    41940992   83  Linux
  > > > > 	2. df /            <==注意：重点在找出磁盘文件名而已
  > > > > 		Filesystem     1K-blocks    Used Available Use% Mounted on
  > > > > 		/dev/vda1       41151808 1808820  37229556   5% /
  > > > > 		
  > > > > 	输入fdisk /dev/vda1
  > > > >         Welcome to fdisk (util-linux 2.23.2).
  > > > > 
  > > > >         Changes will remain in memory only, until you decide to write them.
  > > > >         Be careful before using the write command.
  > > > > 
  > > > >         Device does not contain a recognized partition table
  > > > >         Building a new DOS disklabel with disk identifier 0x4c214682.
  > > > > 
  > > > >         Command (m for help):   =>> 等待你的输入
  > > > >         
  > > > >         输入 m 后，就会看到底下这些命令介绍
  > > > >         
  > > > >         	Command action
  > > > >                a   toggle a bootable flag
  > > > >                b   edit bsd disklabel
  > > > >                c   toggle the dos compatibility flag
  > > > >                d   delete a partition		 <==删除一个partition
  > > > >                g   create a new empty GPT partition table
  > > > >                G   create an IRIX (SGI) partition table
  > > > >                l   list known partition types
  > > > >                m   print this menu
  > > > >                n   add a new partition		<==新增一个partition
  > > > >                o   create a new empty DOS partition table
  > > > >                p   print the partition table	 <==在屏幕上显示分割表
  > > > >                q   quit without saving changes		<==不储存离开fdisk程序
  > > > >                s   create a new empty Sun disklabel
  > > > >                t   change a partition's system id
  > > > >                u   change display/entry units
  > > > >                v   verify the partition table
  > > > >                w   write table to disk and exit		 <==将刚刚的动作写入分割表
  > > > >                x   extra functionality (experts only)
  > > > > 		输入 p 	<== 这里可以输出目前磁盘的状态
  > > > > 		 这个磁盘的文件名与容量
  > > > > 		Disk /dev/vda1: 42.9 GB, 42947575808 bytes, 83881984 sectors 
  > > > >         Units = sectors of 1 * 512 = 512 bytes	
  > > > >         Sector size (logical/physical): 512 bytes / 512 bytes 
  > > > >         I/O size (minimum/optimal): 512 bytes / 512 bytes
  > > > >         Disk label type: dos
  > > > >         Disk identifier: 0x4c214682
  > > > > 
  > > > >      	Device Boot      Start         End      Blocks   Id  System
  > > > >      	
  > > > >      	
  > > > >      	想要不储存离开吗？按下 q 就对了！不要随便按 w 啊！
  > > > > 
  > > > > 		使用 p 可以列出目前这颗磁盘的分割表信息，这个信息的上半部在显示整体磁盘的状态。
  > > > > 	
  > > > > ```
  > > > >
  > > > >
  > > > ```
  > > ```
  > ```

* 磁盘格式化

  * 磁盘分割完毕后自然就是要进行文件系统的格式化，格式化的命令非常的简单，使用 `mkfs`（make filesystem） 命令。

    语法：

     ```
    mkfs [-t 文件系统格式] 装置文件名
    	-t ：可以接文件系统格式，例如 ext3, ext2, vfat 等(系统有支持才会生效)
     ```

    > ```
    > 示例：
    > 	1. mkfs [tab] [tab] 查看 mkfs 支持的格式  [tab] 是按 Tab 建，不是输入[tab]
    >     
    > mkfs         mkfs.btrfs   mkfs.cramfs  mkfs.ext2    mkfs.ext3    mkfs.ext4    		mkfs.minix   mkfs.xfs
    > ```
    >
    >

#### linux yum 命令

* yum（ Yellow dog Updater, Modified）是一个在Fedora和RedHat以及SUSE中的Shell前端软件包管理器。

  基於RPM包管理，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软体包，无须繁琐地一次次下载、安装。

  yum提供了查找、安装、删除某一个、一组甚至全部软件包的命令，而且命令简洁而又好记。

  ```
  yum [options] [command] [package ...]
  ```

  > * **options：**可选，选项包括-h（帮助），-y（当安装过程提示选择全部为"yes"），-q（不显示安装的过程）等等。
  > * **command：**要进行的操作。
  > * **package**操作的对象。

  > ```
  > yum常用命令
  > 1.列出所有可更新的软件清单命令：yum check-update
  > 2.更新所有软件命令：yum update
  > 3.仅安装指定的软件命令：yum install <package_name>
  > 4.仅更新指定的软件命令：yum update <package_name>
  > 5.列出所有可安裝的软件清单命令：yum list
  > 6.删除软件包命令：yum remove <package_name>
  > 7.查找软件包 命令：yum search <keyword>
  > 8.清除缓存命令:
  >     yum clean packages: 清除缓存目录下的软件包
  >     yum clean headers: 清除缓存目录下的 headers
  >     yum clean oldheaders: 清除缓存目录下旧的 headers
  >     yum clean, yum clean all (= yum clean packages; yum clean oldheaders) :清除	 缓存目录下的软件包及旧的headers
  > ```
  >
  >

### dotNetCore 网站 部署到 LIUNX 

* 第一步  首先 你得有 LIUNX 服务器  

* 第二步  使用工具连接你的 LIUNX 服务器 ，我使用的是 XShell6 ( 连接LIUNX 服务器) ，和  WinSCP (  LIUNX 服务器传输文件 )

* 好啦， 以上我们都软件都有了之后就开始我们的 dotnetcore 部署啦

  1. 打开 VS 创建一个 dotnetcore 网站应用，本人使用的 VS2017 已经安装 dotnetcore SDK

     ![](F:\Person\Notes\Images\dotnetcorewebCreate.png)

     ![](F:\Person\Notes\Images\dotnetcoreversion.png)

   ![](F:\Person\Notes\Images\Home.png)

好啦， 我们的 dotnetcore 网站应用就创建完成，是不是感觉和 ASP.NET MVC 一样的目录结构

![](F:\Person\Notes\Images\run.png)

我们把 Home 文件夹下面  index 页面 ，稍微 更改下，然后运行下看下效果

![](F:\Person\Notes\Images\webrun.png)

好啦， 我们的 dotnetcore 网站已经成功创建， 并且运行起来了， 

2. 开始发布我们的 dotnetcore 网站 ， 和平时 .net 网站发布一样，没什么区别，右键项目 ，点击发布

![](F:\Person\Notes\Images\fb.png)

点击发布按钮之后，可以看到下面的页面 ，我们选择文件夹发布，浏览选择我们要发布的文件的文件夹

![](F:\Person\Notes\Images\fbfile.png)

选择完成之后，点击右下角的 发布按钮，静静的等待。发布完成之后，打开我们刚刚浏览选择的发布文件夹，你可以看到如下图，这些就是发布之后的文件

![](F:\Person\Notes\Images\fbzhwjj.png)

3. 好啦， 我们的 dotnetcore 网站已经成功发布好了在我们本地，我们该如何运行起来了，

    在我们发布的文件夹中 打开 cmd 窗口，在下图红框中输入 cmd 命令，会出现下图

   ![](F:\Person\Notes\Images\QQ截图20181206183527.png)

   ![](F:\Person\Notes\Images\QQ截图20181206183603.png)

4. 在  cmd  窗口 输入   dotnet  加上  你的项目程序集名称 ，如下图

    

![](F:\Person\Notes\Images\QQ截图20181206183929.png)



 按回车之后，我们发布的dotnetcore 网站就已经部署在我们本地 localhost:5000 端口，可直接复制到浏览器查看，为了防止 VS冲突，我们先关闭我们  VS 环境，dotnetcore 默认发布地址是本地 5000 端口

![](F:\Person\Notes\Images\QQ截图20181206184006.png)

![](F:\Person\Notes\Images\dotnetcoreweb.png)

 复制 https://localhost:5000 到浏览查看，看到以上页面，说明我们的 网站已经成功发布在我们 本地了

4. 以上步骤都完成了之后，就开始我们上我们的 LIUNX 服务器瞎搞啦。

    