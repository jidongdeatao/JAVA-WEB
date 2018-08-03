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


keytool制作证书的用法总结：
图文教程：http://ln-ydc.iteye.com/blog/1335213



Tomcat安全配置规范
    1.1 背景介绍
    Tomcat是一个免费的开源的Servlet容器，它是Apache基金会的Jakarta项目中的一个核心项目，由Apache，Sun和其它一些公司及个人共同开发而成。由于有了Sun的参与和支持，最新的Servlet和Jsp规范总能在Tomcat中得到体现。
    尽管Tomcat的Web服务器功能较少，但是，Tomcat容易架设、容易移植、与其他服务器兼容性极好，相对于其它以C、C＋＋所编写的Web服务器，Tomcat比较能忍受远程缓冲区溢出的攻击。因为Tomcat的Java虚拟机是位于网络及操作系统之间，它可以防止几乎所有类型的缓冲区溢出攻击。此外，Tomcat的运行速度也比较快，足以运行今日大多数的企业网站。因此，Tomcat服务器的应用也是极为广泛的。
    在使用Tomcat服务器时，我们要避免错误的配置引起的安全问题，同时，也使用更合理的配置或插件来加强Tomcat的安全防护。
    1.2	使用对象
    本规范的读者及使用对象主要为Tomcat相关的需求分析人员、设计人员、开发人员、测试人员等。
    1.3	适用范围
    本规范的制定考虑了Tomcat 6.X、7.X和8.X各版本特性和共性，适合于公司绝大部分使用Tomcat开发的Web产品，要求使用Tomcat开发的Web产品必须遵循。
    早期版本请升级到6.X、7.X和8.X，然后参考本配置规范。
    1.4	用词约定
    	规则：强制必须遵守的约定
    	建议：需要加以考虑的约定
    	说明：对此规则或建议进行相应的解释
    	实施指导：对此规则或建议的实施进行相应的指导
    	注意事项：规则引起的注意事项、不适用该规则的地方，简写为注。
