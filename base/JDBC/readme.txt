JDBC应用教程：

   使用详解：
    1）JDBC常用接口
    2）JDBC编程步骤
    3）执行SQL语句
    4）管理结果集
    5）事务管理
  高级应用：
    1）分页
    2）高级查询
    3）高级函数使用
    
    JDBC编程步骤：
    1）加载驱动程序：Class.forName(driverClass)
      加载Mysql驱动：Class.forName（“com.mysql.jdbc.Driver”）；
      加载Oracle驱动:Class.forName("oracle.jdbc.driver.OracleDriver")
    2）获得数据库连接
      DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/imooc","root","rootpassword");
    3）创建Statement对象:
      conn.createStatement();
      
    简单的测试JDBC程序参见simple.txt：
    
    详细设计：采用MVC三层架构-从下往上进行设计
      View（视图层）->显示：该层作用用于把用户行为通知Controller，Controller反过来也会更新Views
      Control（控制层）->协调控制
      Model（模型层）->数据：该层作用用于通知Controller，Controller反过来也会更新Model
      DB（数据库）
      
      第一步：先搭建模型层
      
      第二步：搭建控制层
      
      第三步：搭建视图层
      
   
  JDBC详细讲解：参考慕课网视频:https://www.imooc.com/video/8125
   1）JDBC存储过程的调用
      分为调用无参存储过程、
      带输入参数的存储过程、
      带输出参数的存储过程、
      带输入、输出参数的存储过程
   2）JDBC的事务处理
   3）JDBC数据库连接池
      目前有两种开源配置：
         1）dbcp连接
         2）c3p0连接
   4）JDBC升级之替代产品
      目前有三种更好的应用：
         1）Commons-dbutils
         2) Hiberate
         3) myBatis
        
