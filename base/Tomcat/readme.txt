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

