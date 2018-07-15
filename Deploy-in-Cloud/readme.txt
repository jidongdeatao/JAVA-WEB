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
              查看
              sudo yum -y install mysql-server
           6)安装MySQL有没有
           6)安装MySQL
              
          
  4、自动化发布
    使用Shell脚本自动化发布
  5、线上验证
    前台：
    后台：
    图片服务器
    前端服务器：
