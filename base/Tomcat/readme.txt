Tomcat服务器
目录结构：
  1.下载安装
  2.运行配置
    1)修改默认端口。
      将${tomcat-dir}/conf/server.xml中默认端口号8080修改为其他端口，这里修改为8001
    2)配置虚拟目录
      新建WEB-INF目录，在目录下新建web.xml文件，内容为：
      在${tomcat-dir}/conf/server.xml的<HOST></HOST>直接加入<Context path='v1' docBase="${WEB-INF-dir}"/>
      浏览器中输入127.0.0.1:8001/v1/
    3)配置首页
      在WEB-INF目录下放入index.html
  3.安全配置

经验：
1.使用IntelliJ IDEA配置Tomcat图文教程
    https://www.cnblogs.com/Knowledge-has-no-limit/p/7240585.html


配置Tomcat使用https协议（配置SSL）
参考图文教程：http://ln-ydc.iteye.com/blog/1330674

环境：
1.java 1.6
2.tomcat 6_0_26
---------------------------------------------------------------------------------------------------------------------------
生成安全证书：
1.java环境：因为SUN公司提供了制作证书的工具keytool。
      在JDK 1.4以后的版本中都包含了这一工具，它的位置为<JAVA_HOME>\bin\keytool.exe。
2.创建证书的命令：
    keytool -genkeypair -alias "tomcat" -keyalg "RSA" -keystore "f:\tomcat.keystore"  
参数的意思如下：
这里密码我输的是tomcat,名字与姓氏为域名，其它的根据具体情况输入

以上命令将生产一对非对称密钥和自我签名的证书f:\tomcat.keystore.
将证书保存到你要存放的地方，我的保存在D:\Tools\Web\ssl\tomcat.keystore
注意：“名字与姓氏”应该是域名，输成了姓名，和真正运行的时候域名不符，会出问题
---------------------------------------------------------------------------------------------------------------------------
配置tomcat:
定位到tomcat的安装目录，找到conf下的server.xml文件
找到如下已经被注释的代码：
Xml代码  收藏代码

    <!--  
        <Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true"  
                   maxThreads="150" scheme="https" secure="true"  
                   clientAuth="false" sslProtocol="TLS" />  
        -->  
去掉注释，修改为：
Xml代码  收藏代码
    <Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true"  
                  maxThreads="150" scheme="https" secure="true"  
                  clientAuth="false" sslProtocol="TLS"   
           keystoreFile="D:\Tools\Web\ssl\tomcat.keystore"  
           keystorePass="tomcat"  
           ciphers="tomcat"/>  

这里，密码和证书的位置根据个人的具体环境而设置，属性参数如下所述：
属性 	描述
clientAuth 	如果设为true，表示Tomcat要求所有的SSL客户出示安全证书，对SSL客户进行身份验证
keystoreFile 	指定keystore文件的存放位置，可以指定绝对路径，也可以指定相对于<CATALINA_HOME>（Tomcat安装目录）环境变量 的相对路径。如果此项没有设定，默认情况下，Tomcat将从当前操作系统用户的用户目录下读取名为“.keystore”的文件。
keystorePass 	指定keystore的密码，如果此项没有设定，在默认情况下，Tomcat将使用“changeit”作为默认密码。
sslProtocol 	指定套接字（Socket）使用的加密/解密协议，默认值为TLS，用户不应该修改这个默认值。
ciphers 	指定套接字可用的用于加密的密码清单，多个密码间以逗号（,）分隔。如果此项没有设定，在默认情况下，套接字可以使用任意一个可用的密码。

访问支持ssl的web站点：
启动本场tomcat,在浏览器中输入:https://localhost:8443/ ，这里用ie访问
显示不安全访问
选择继续浏览此网站






keytool用法总结：
图文教程：http://ln-ydc.iteye.com/blog/1335213
内容概览：

keytool的几个常用的命令。
1.创建证书
2.查看证书库
3.导出证书文件
4.导入证书的信息
5.查看证书信息
6.删除密钥库中的条目
7.修改证书条目的口令
--------------------------------------------------------------------------------------
预备知识：
keytool的概念
SUN公司提供了制作证书的工具keytool。在JDK 1.4以后的版本中都包含了这一工具，它的位置为<JAVA_HOME>\bin\keytool.exe。
--------------------------------------------------------------------------------------
主要内容:
1.创建证书
    keytool -genkeypair -alias "test1" -keyalg "RSA" -keystore "test.keystore"  
说明：
密钥库密码为testtest
证书条目密码为testtest1,若别名为test2则密码为testtest2
功能：
创建一个别名为test1的证书条目，该条目存放在名为test.keystore的密钥库中，若test.keystore密钥库不存在则创建。
参数说明：
-genkeypair：生成一对非对称密钥;
-alias：指定密钥对的别名，该别名是公开的;
-keyalg：指定加密算法，本例中的采用通用的RAS加密算法;
-keystore:密钥库的路径及名称，不指定的话，默认在操作系统的用户目录下生成一个".keystore"的文件
注意：
1.“名字与姓氏”应该是域名，若输成了姓名，和真正运行的时候域名不符，会出问题;
2.再次输入密码，第一次输入的是密钥库(keystore)的密码，第二次输入的是证书条目的密码
3.这里所说的证书库和密钥库是等同的(个人观点)
为了测试需要，这里再创建两个别名为test2和test3的证书条目在test.keystore密钥库中，代码如下：
    keytool -genkeypair -alias "test2" -keyalg "RSA" -keystore "test.keystore"  
    keytool -genkeypair -alias "test3" -keyalg "RSA" -keystore "test.keystore"  
2.查看证书库
    keytool -list -keystore test.keystore  
功能：
查看名为test.keystore的证书库中的证书条目
3.导出到证书文件
    keytool -export -alias test1 -file test.crt -keystore test.keystore  
功能：
将名为test.keystore的证书库中别名为test1的证书条目导出到证书文件test.crt中
4.导入证书的信息
    keytool -import -keystore test_cacerts -file test.crt   
功能：
将证书文件test.crt导入到名为test_cacerts的证书库中，
5.查看证书信息
    keytool -printcert -file "test.crt"   
功能：
查看证书文件test.crt的信息
6.删除密钥库中的条目
删除前查看密钥库test.keysote中的证书条目
    keytool -list -keystore test.keystore  
删除密钥库test.keystore中别名为test2的证书条目
    keytool -delete -keystore test.keystore -alias test2  
删除后查看密钥库test.keystore中的证书条目
    keytool -list -keystore test.keystore  
7.修改证书条目的口令
交互的方式
    keytool -keypasswd -alias test1 -keystore test.keystore  
功能：
将密钥库test.keystore中别名为test1的证书条目的密码修改为testtesttest1
非交互方式
    keytool -keypasswd -alias test1 -keypass testtesttest1 -new testtest1 -storepass testtest -keystore test.keystore  
功能：
将密钥库test.keystore中别名为test1的证书条目的密码修改为testtest1
