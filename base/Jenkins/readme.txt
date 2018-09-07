Jekins官网
https://jenkins.io

安装：
下载war包

启动：
    切换到war包所在路径：
    使用命令：java -jar jenkins.war
    打开浏览器输入：localhost：8080
    会看到一个提示：
    解锁 Jenkins
        为了确保管理员安全地安装 Jenkins，密码已写入到日志中（不知道在哪里？）该文件在服务器上：
        /Users/XXX/.jenkins/secrets/initialAdminPassword
        请从本地复制密码并粘贴到下面。
    按照提示操作
    选择左侧推荐插件安装，等待一小会
    然后创建账户；
        admin
        password
        admin
        admin@qq.com

插件管理：
  安装几款好用的插件：
    在可选插件列表中选择：
      Rebuilder
      Safe_restart
      
Jekins基础设置：
    配置管理员全局安全属性：
      在系统管理-全局安全配置中设置-添加admin用户，给予admin全部权限
    添加用户:
      系统管理-管理用户-新建用户
        test
        password
        test
        test@qq.com
     赋予这个用户除了administer之外的全部权限   
    
##################################################################################
要部署的Linux服务器准备：
  1.稳定IP 2.用户名、密码 3.SSH服务开启 4.防火墙。。
  5.java 
  这里java一定要使用yum install java安装，不然会导致jenkins无法连接
  6.git  
      配置git的用户名：
        git config --global user.name "testforfun1"
      配置email：
        git config --global user.email "testforfun1@163.com"
      这两个值不要求为真，有就可以
      然后生成git的公钥与私钥证书：
          ssh-keygen -t rsa -C "testforfun1@163.com"
          可以选择默认设置，这样就在～/.ssh/目录下存在两个文件：id_rsa.pub（公钥） id_rsa（私钥）
      将Git证书配置到github上，保证Linux服务器与GitHub之间可以联通
          登陆到自己的GitHub上，在settings - SSH KEY-新建个New Key
          新建这个名字为：LinuxEnv，然后将之前生成的公钥文件id_rsa.pub复制进去
          然后在Linux服务器验证下，命令：ssh git@github.com
  7.Maven
    下载安装完Maven后，要配置Maven的环境变量，在/etc/profile添加:
        export MAVEN_HOME=/developer/apache-maven-3.0.5
        export PATH=$PATH:$MAVEN_HOME/bin
        保存然后再执行：./etc/profile
        验证：mvn --version
  8.Tomcat
    这里我修改了Tomcat的服务端口为8090
    启动运行：
    在tomcat主目录下输入：./bin/startup.sh
 ##################################################################################
 
 开始配置：
 新建节点：
    系统管理-节点管理-新建节点
        输入名称：TestEnv
             配置IP、路径、用户名、密码
             远程构建目录必填项：/root/jeckin
 启动从节点验证Linux服务器已经成功注册到Jenkins上
    回到首页创建一个新任务,名称：TestTask，自由风格，确定
        然后 限制项目的运行节点-选择之前的节点名称TestEnv
        在构建 中 选择执行shell ，输入一个简单的命令ifconfig来验证
        保存然后点击立即构建，
        在构建历史-命令台输出查看结果
        
        
  测试项目：
      Fork一个项目：
      使用git clone：
      使用Intell 导入下载的源码，使用Maven进行检查:打开右侧Maven Project，对于该项目-LifeCycle-使用shift同时选中：clean与install 执行
                出现 BUILD SUCCESS 说明代码是没有问题的
      导入sql语句到mysql中
      修改代码中数据库参数
      打开右侧Maven Project，Plugins按钮-tomcat7：run，执行
      如果成功，打开浏览器输入127.0.0.1:8071就可以看到
      
  自动化部署过程：
  git同步最新代码-》maven打包（之前先要停止Tomcat） -〉部署应用（启动Tomcat） -》验证结果
    
  创建Jenkins部署任务：
  创建Jenkins任务 -》填写Server信息-〉配置git参数 -》填写构建语句，实际部署测试环境
  运行部署任务：
  执行Jenkins部署任务-》确定执行结果成功-〉打开浏览器访问部署应用程序-》确定发布结果的正确性
  
  
               
 
