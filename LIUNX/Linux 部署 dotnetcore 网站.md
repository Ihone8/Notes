# Linux 部署 dotnetcore 网站

* 要先在 linux 系统安装 dotnetcore 环境，安装方式如下

  ```
  -- 安装地址如下， 可根据自己 Liunx 系统不同安装对应的版本。
  https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial#install
  以下是 Centos 安装方法
  sudo 是权限不足时加的。
  sudo rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm
  sudo yum update
  sudo yum install dotnet-sdk-2.2
  ```

* 配置 Nginx 

  ```
  curl -o  nginx.rpm http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm

  rpm -ivh nginx.rpm
  yum install nginx

  安装成功！

  输入：systemctl start nginx 来启动nginx。

  输入：systemctl enable nginx 来设置nginx的开机启动（linux宕机、重启会自动运行nginx不需要连上去输入命令）。

  配置防火墙
  systemctl status firewalld 		-- 查看防火墙状态 
  systemctl stop firewalld 		--关闭防火墙
  命令：systemctl start firewalld  -- 打开防火墙
  命令：firewall-cmd --zone=public --add-port=80/tcp --permanent（开放80端口）
  命令：systemctl restart firewalld（重启防火墙以使配置即时生效）

  测试nginx是否可以访问。 直接访问你的 IP 地址
  ```

*  配置nginx对ASP.NET Core应用的转发

   * 修改 /etc/nginx/conf.d/default.conf 文件。替换为下面内容

     ```
     server {
         listen       80;
         location / {
     	proxy_pass http://localhost:5000;
     	proxy_http_version 1.1;
     	proxy_set_header Upgrade $http_upgrade;
     	proxy_set_header Connection keep-alive;
     	proxy_set_header Host $host;
     	proxy_cache_bypass $http_upgrade;
         }
     }
     ```

   * 上传至CentOS进行覆盖。执行： nginx -s reload 使其即时生效

   * 运行ASP.NET Core应用程序  进入到你的 发布文件目录。使用 dotnet  项目.dll 命令。

   * 然后访问你的 IP 地址。 看看是否能正常访问。如果不能正常访问的话。显示 502，这个问题是由于SELinux保护机制所导致，我们需要将nginx添加至SELinux的白名单。

     接下来我们通过一些命令解决这个问题。。

     ```
     yum install policycoreutils-python

     sudo cat /var/log/audit/audit.log | grep nginx | grep denied | audit2allow -M mynginx

     sudo semodule -i mynginx.pp
     ```

   * 然后再次访问应用程序 看是否能正常访问。

   * 现在已经差不多完成部署了。但是每次都要我们手动去启动该网站。显然这是个很不人性化的操作。所以就有了我们接下来的操作。

*  配置守护服务（Supervisor）

   * 目前存在三个问题

     问题1：ASP.NET Core应用程序运行在shell之中，如果关闭shell则会发现ASP.NET Core应用被关闭，从而导致应用无法访问，这种情况当然是我们不想遇到的，而且生产环境对这种情况是零容忍的。

     问题2：如果ASP.NET Core进程意外终止那么需要人为连进shell进行再次启动，往往这种操作都不够及时。

     问题3：如果服务器宕机或需要重启我们则还是需要连入shell进行启动。

     为了解决这个问题，我们需要有一个程序来监听ASP.NET Core 应用程序的状况。在应用程序停止运行的时候立即重新启动。这边我们用到了Supervisor这个工具，Supervisor使用Python开发的。

   * 安装 Supervisor

     ```
     yum install python-setuptools

     easy_install supervisor
     ```

   * 配置  Supervisor

     ```
     mkdir /etc/supervisor

     echo_supervisord_conf > /etc/supervisor/supervisord.conf

     修改 supervisord.conf 内容 最底部的俩句

     ;[include]
     ;files = relative/directory/*.ini
     改为
     [include]
     files = conf.d/*.conf

     ps:如果服务已启动，修改配置文件可用“supervisorctl reload”命令来使其生效
     ```

*  配置对ASP.NET Core应用的守护

   * 创建一个 WebApplication1.conf 文件 。内容大致如下。

     ```
     [program:WebApplication1]
     command=dotnet WebApplication1.dll ; 运行程序的命令
     directory=/home/wwwroot/WebApplication1/ ; 命令执行的目录
     autorestart=true ; 程序意外退出是否自动重启
     stderr_logfile=/var/log/WebApplication1.err.log ; 错误日志文件
     stdout_logfile=/var/log/WebApplication1.out.log ; 输出日志文件
     environment=ASPNETCORE_ENVIRONMENT=Production ; 进程环境变量
     user=root ; 进程执行的用户身份
     stopsignal=INT
     ```

     * 将文件拷贝至：“/etc/supervisor/conf.d/WebApplication1.conf”下

     * 运行supervisord，查看是否生效

       ```
       supervisord -c /etc/supervisor/supervisord.conf

       ps -ef | grep WebApplication1

       如果存在dotnet WebApplication1.dll 进程则代表运行成功，这时候在使用浏览器进行访问
       ```

       ​

