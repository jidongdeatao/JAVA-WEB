项目部署云服务器

  1、云服务器购买与域名（申请，备案，配置）
    选取的是阿里云ECS，操作系统镜像CentOS，域名在阿里云万网购买,备案也会比较快
    安全组起到与防火墙相同作用，为了安全一定要配置
    如果网络不通，先检查iptables，再检查安全组
    域名解析 使用A字段就可以了，比较简单，不再赘述
    
  2、源配置
    使用了阿里云源配置官网（http://mirrors.aliyun.com/)
    CentOS源配置说明：http://mirrors.aliyun.com/help/centos
    源配置步骤：
      1）备份
        命令：sudo mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
        对源进行重命名
      2）下载CentOS-Base.repo到/etc/yum.repos.d
        wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
      3）源配置步骤
        运行yum makecache生成缓存
        
  3、配置线上环境
    新建用户:useradd -d /usr/wang -m wang
           cd /usr/wang
           passwd wang #重置密码
           配置用户的sudo权限:使用root账号：sudo vim /etc/sudoers  在其中添加:wang ALL=(ALL) ALL
    安装应用软件：
        JDK、Tomcat、Maven、Vsftpd、Nginx、文件服务器、MySQL、Git、iptables
        1)安装JDK
          使用命令:yum -qa | grep jdk 如果有默认安装的软件，使用命令删除：sudo yum remove ..
            cd /
            sudo mkdir developer
            cd developer
            sudo mkdir setup
            cd setup
            sudo wget jdk的源
            sudo chmod 777 jdk
            sudo rpm -ivh jdk
          查看是否安装成功,cd /usr/java/jdk..
          配置jdk的环境变量：sudo vim /etc/profile
                          在文件最下边进行添加：（注意路径要和实际环境保持一致）
                          export JAVA_HOME=/usr/java/jdk1.7.0_80
                          export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
                          export MAVEN_HOME=/developer/apache-maven-3.0.5
                          export NODE_HOME=/usr/local/node-v4.4.7-linux-x64
                          export RUBY_HOME=/usr/local/ruby
                          export CATALINA_HOME=/developer/apache-tomcat-7.0.73

                          export PATH=$PATH:$JAVA_HOME/bin:$CATALINA_HOME/bin:$MAVEN_HOME/bin:$NODE_HOME/bin:/usr/local/bin:$RUBY_HOME/bin


                          export LC_ALL=en_US.UTF-8
          使用命令：source /etc/profile 使配置生效
          2）安装Tomcat
                cd /developer
                sudo wget tomcat压缩包地址
                解压:sudo tar -zxvf tomcat压缩包
            配置Tomcat的字符集：
              sudo vim tomcat/conf/server.xml
              在语句中加入URIEncoding="UTF-8"，保存退出
              <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443"  URIEncoding="UTF-8"/>
           启动Tomcat：
              cd tomcat/bin
              sudo ./startup.sh
           现在可以访问到服务器的Tomcat（记住：安全组与iptables可以允许访问,CentOS 7默认情况下是开启了防火墙的，如果不知道如何配置iptables规则，这里可以先使用service iptables stop命令暂时关闭防火墙）
              浏览器中输入公网IP+8080端口
           3）安装Maven
                cd /developer
                sudo wget maven压缩包地址
                sudo tar -zxvf maven压缩包
                把之前下载的压缩包移到setup/目录下
                查看安装是否成功：mvn -version
           4）安装vsftpd
              sudo yum -y install vsftpd 
              单独创建ftp用户
              sudo useradd ftpuser -d /product/ftpfile/ -s /sbin/nologin
              sudo chown -R ftpuser.ftpuser /product/ftpfile/
              重置ftpuser密码：
              sudo passwd ftpuser
              接着，
              cd /etc/vsftpd/
              sudo vim chroot_list，里面添加“ftpuser"就可以了
              检查文件安全策略：sudo vim /etc/selinux/config，看下是不是“disabled”,  如果不是“disabled”可以通过输入命令：sudo setsebool -P ftp_home_dir 1 来修改
              将原有的配置文件弄成备份：sudo mv vsftpd.conf vsftpd.conf.bak
              然后重新重新创建一个配置文件：sudo wget vsftpd.conf的下载地址
              vsftp 需要开启iptables策略，这里放在最后再讲，这里先略过
           5）安装Nginx
              cd /developer/setup
              sudo wget Nginx压缩包地址
              解压：sudo tar -zxvf nginx压缩包
              Nginx需要编译，所以先安装下依赖环境：
              sudo yum -y install gcc zlib zlib-devel pcre-devel openssl openssl-devel
              然后，cd 到Nginx解压到目录
              sudo ./configure
              sudo make
              sudo make install,就编译完成了
              然后开始进行nginx的配置
              通过whereis nginx 可以查到nginx目录,默认情况下是/usr/local/nginx
              cd /usr/local/nginx
              cd conf
              sudo vim nginx.conf,在其中# HTTPS server 上面增加一条命令：include vhost/*.conf;
              在/usr/local/nginx/conf目录下，sudo mkdir vhost
              cd vhost
              sudo wget 域名解析配置文件地址（这里参考）
              然后，
              cd /usr/local/nginx/sbin
              sudo ./nginx，启动nginx
              
              同样，Nginx需要配置防火墙，最后讲，先略过
           6)安装MySQL
              查看有没有默认安装mysql:sudo rpm -qa | grep mysql-server
              sudo yum -y install mysql-server
              （如果是CentOS7会出现yum install mysql-server没有可用包：通过以下办法解决
                  下载mysql的repo源
                    # wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm 
                  安装mysql-community-release-el7-5.noarch.rpm包
                    # rpm -ivh mysql-community-release-el7-5.noarch.rpm
                    # ls -1 /etc/yum.repos.d/mysql-community*
                      会获得两个mysql的yum repo源：/etc/yum.repos.d/mysql-community.repo，/etc/yum.repos.d/mysql-community-source.repo。
                  # yum install mysql-server
                  # yum install mysql-devel）
                  
               重置密码:重置密码前，首先要登录
                $ mysql -u root
                  登录时有可能报这样的错：ERROR 2002 (HY000): Can‘t connect to local MySQL server through socket ‘/var/lib/mysql/mysql.sock‘ (2)，原因是/var/lib/mysql的访问权限问题。下面的命令把/var/lib/mysql的拥有者改为当前用户：
                $ sudo chown -R wang:wang /var/lib/mysql
                然后，重启服务：$ service mysqld restart
                
              编辑MySQL的字符集：
                sudo vim /etc/my.cnf
                加入命令：[client]
                         default-character-set=utf8
                         [mysqld]
                         character-set-server=utf8
                         collation-server=utf8_general_ci
                重启：systemctl restart mysql.service
                查看是否生效：
                mysql> show variables like 'char%';
                        
              配置MySQL的自启动：sudo chkconfig mysqld on
              查看自启动状态：sudo chkconfig --list mysqld
                其中2、3、4、5都是on说明自启动已打开
              重启服务：$ sudo service mysqld restart
              登录数据库：mysql -u root
              mysql > select user,host,password from mysql.user;
                修改密码：
              mysql > set password for root@localhost = password('rootpassword');
              mysql > set password for root@127.0.0.1 = password('rootpassword');
              mysql > set password for root@主机名 = password('rootpassword');
                退出，重新登录：mysql -u root -p
                干掉无用用户：
              mysql > delete from mysql.user where user='';
              mysql > flush privileges;
                新建用户：
              mysql > GRANT USAGE ON *.* TO 'mmall'@'localhost' IDENTIFIED BY 'mmallpassword' WITH GRANT OPTION;
              (注意，这里使用insert into mysql.user(host,user,password) values ("localhost","mail",password("mailpassword"));会报错。
              mysql > flush privileges;
                创建数据库并付给新用户访问权限；
              mysql > create database `mmail` default character set utf8 COLLATE utf8_general_ci;
              mysql > grant all privileges on mmail.* to mmall@localhost identified by 'mmallpassword';
              mysql > flush privileges;
              
              将sql文件导入到数据库中：
              首先下载sql文件:cd /developer 
                            sudo wget 地址
              登录数据库，导入
                  mysql > use mmall;
                  mysql > source /developer/mmall.sql
              可以查看其中的表了:
                  mysql > select * from mmail_user;
                  
         7)安装Git
         sudo wget git地址
         解压:sudo tar -zxvf git压缩包
         Git安装依赖：
         sudo yum -y install zlib-devel openssl-devel cpio expat-devel gettext-devel curl-devel perl-ExtUtils-CBuilder perl-ExtUtils-MakeMaker
         cd git解压缩包目录内
         sudo make prefix=/usr/local/git all
         sudo make prefix=/usr/local/git install
       
         编译出现错误：
         参考
         http://www.mamicode.com/info-detail-1420741.html
         https://blog.csdn.net/tanga842428/article/details/76861389
         https://blog.csdn.net/yuan1125/article/details/51050102
         https://www.cnblogs.com/jinjiezhilu/p/7866035.html
         
        8）最后配置防火墙iptables
        cd /etc/sysconfig
        如果有没有iptables文件，随便输入个命令：
        sudo iptables -P OUTPUT ACCEPT
        sudo service iptables save
        将这个iptables备份，使用
        这个规则：
        
              # Generated by iptables-save v1.4.7 on Fri Jan  6 16:53:09 2017
              #*filter
              #:INPUT ACCEPT [174:12442]
              #:FORWARD ACCEPT [0:0]
              #:OUTPUT ACCEPT [96:10704]
              #-A INPUT -p tcp -m tcp --dport 3306 -j ACCEPT 
              #-A INPUT -p tcp -m state --state NEW -m tcp --dport 21 -j ACCEPT
              #-A INPUT -p tcp -m tcp --dport 8080 -j ACCEPT


              #COMMIT
              # Completed on Fri Jan  6 16:53:09 2017


              #------------------------------------

              # Firewall configuration written by system-config-firewall
              # Manual customization of this file is not recommended.
              *filter
              :INPUT ACCEPT [0:0]
              :FORWARD ACCEPT [0:0]
              :OUTPUT ACCEPT [0:0]
              -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT 
              -A INPUT -p icmp -j ACCEPT
              -A INPUT -i lo -j ACCEPT

              #ssh port 
              -A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT

              #vsftpd
              -A INPUT -p TCP --dport 61001:62000 -j ACCEPT
              -A OUTPUT -p TCP --sport 61001:62000 -j ACCEPT

              -A INPUT -p TCP --dport 20 -j ACCEPT
              -A OUTPUT -p TCP --sport 20 -j ACCEPT
              -A INPUT -p TCP --dport 21 -j ACCEPT
              -A OUTPUT -p TCP --sport 21 -j ACCEPT


              #mysql port
              #-A INPUT -p tcp -m tcp --dport 3306 -j ACCEPT

              #tomcat remote debug port
              #-A INPUT -p tcp -m tcp --dport 5005 -j ACCEPT

              #-A INPUT -p tcp -m tcp --dport 8080 -j ACCEPT

              #nginx
              -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT

              -A INPUT -j REJECT --reject-with icmp-host-prohibited
              -A FORWARD -j REJECT --reject-with icmp-host-prohibited
              COMMIT
        
        
        对于CentOS 7不怎么支持chkconfig 与 service。使用下面命令启动或关闭：systemctl 是管制服务的主要工具
        $ sudo systemctl start iptables
            systemctl 是管制服务的主要工具， 它整合了chkconfig 与 service功能于一体。
            systemctl is-enabled iptables.service
            systemctl is-enabled servicename.service #查询服务是否开机启动
            systemctl enable *.service #开机运行服务
            systemctl disable *.service #取消开机运行
            systemctl start *.service #启动服务
            systemctl stop *.service #停止服务
            systemctl restart *.service #重启服务
            systemctl reload *.service #重新加载服务配置文件
            systemctl status *.service #查询服务运行状态
            systemctl --failed #显示启动失败的服务
            注：*代表某个服务的名字，如http的服务名为httpd
                         
                         
                         
                         
          
  4、自动化发布
    使用Shell脚本自动化发布
    参见deploy文件夹中的deploy.sh
    
    
  5、线上验证
    前台：
    后台：
    图片服务器
    前端服务器：
