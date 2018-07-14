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
