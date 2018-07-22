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
      
   
