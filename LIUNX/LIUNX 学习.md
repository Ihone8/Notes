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

