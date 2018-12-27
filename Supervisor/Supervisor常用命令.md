##  Supervisor常用命令

* ```
  Linux的命令执行 supervisorctl 进入supervisor的命令行界面
  
  > status    # 查看程序状态
  > stop program_name   # 关闭 program_name 程序
  > start program_name  # 启动 program_name 程序
  > restart program_name    # 重启 program_name 程序
  > reread    ＃ 读取有更新（增加）的配置文件，不会启动新添加的程序，也不会重启任何程序
  > reload    #  载入最新的配置文件，停止原有的进程并按照新的配置启动
  > update    ＃ 重启配置文件修改过的程序，配置没有改动的进程不会收到影响而重启
  ```

