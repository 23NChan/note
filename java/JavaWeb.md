##  Part01_基础加强

### Junit

junit使用：白盒测试

步骤：

1. 定义一个测试类(测试用例)
   + 建议：
     + 测试类名：被测试的类名Test
     + 包名：xxx.xxx.xx.test
2. 定义测试方法：可以独立运行
   + 建议：
   + 方法名：test测试的方法名
   + 返回值：void
   + 参数列表：空参
3. 给方法加@Test

判定结果：

+ 红色：失败
+ 绿色：成功
+ 一般会使用断言操作来处理结果
  + Assert.assertEquals(期望的结果, 运算的结果);

```java
public class CalculatorTest {
    @Test
    public void testadd(){
        //创建计算器对象
        Calculator calculator = new Calculator();
        //调用add方法
        int result = calculator.add(10, 20);
        //断言
        Assert.assertEquals(30,result);
    }
}
```

补充：

+ @Before
  + 修饰的方法会在测试方法之前被自动执行
+ @After
  + 修饰的方法会在测试方法执行之后被自动执行

### 反射

**框架设计的灵魂**

框架：半成品软件，可以在框架的基础上进行软件开发，简化编码

反射：将类的各个组成部分封装为其他对象，这就是反射机制

+ 好处

  + 可以在程序运行过程中，操作这些对象
  + 可以解耦，提高程序的可扩展性

+ 获取Class对象的方式

  + Class.forName("全类名")：将字节码文件加载到内存，返回Class对象
  + 类名.class：通过类名的属性class获取
  + 对象.getClass：通过对象的方法获取

  ```java
  Class cls1 = Class.forName("cn.reflect.Person");
  System.out.println(cls1);
  Class cls2 = Person.class;
  System.out.println(cls2);
  Person p = new Person();
  Class cls3 = p.getClass();
  System.out.println(cls3);
  System.out.println(cls2 == cls1);//ture
  ```

+ 结论

  + 同一个字节码文件(*.class)在一次程序运行过程中，之后被加载一次，不论通过哪一种方式获取的Class对象都是同一个

+ 使用Class对象

  + 获取功能

    + 获取成员变量

      + Field[] getFields()：返回所有公共成员变量的Field对象数组

        Field[] getDeclaredFields()：返回所有成员变量的Field对象数组

        Field getField(String name)：返回一个公共成员变量的Field对象

        Field getDeclaredField(String name)：返回一个成员变量的Field对象

    + 获取构造方法

      + Constructor<?>[] getConstructors()：返回所有公共构造方法对象的数组

        Constructor<?>[] getDeclaredConsructors()：返回所有构造方法对象的数组

        Constructor<T> getConstructor(class<?> ...parameterTypes)：返回单个公共构造方法对象

        Constructor<T> getDeclaredConstructor(class<?> ...parameterTypes)：返回单个构造方法对象

    + 获取成员方法

      + Method[] getMethods()：返回所有公共方法对象的数组，包括继承的 

        Method[] getDeclaredMethods()：返回所有方法对象的数组 不包括继承的

        Method getMethod(String name,Class<?>...parameterTypes) 返回一个公共方法对象

        Method getDeclaredMethod(String name,Class<?>...parameterTypes) 返回一个方法对象

    + 获取类名

      + String getName()

  + Field：成员变量

    + 操作：
      1. 设置值
         + void set(Object obj,Object value)
      2. 获取值
         + get(Object obj)
      3. 忽略访问权限修饰符的安全检查
         + setAccessible(true)：暴力反射

  + Method：对象方法

    + 执行方法
      + Object invoke(Object obje, Object... args)

  案例：

  ​		需求：写一个"框架"，可以帮我们创建任意类的对象，并且执行其中任意方法

  + 实现：
    1. 配置文件
    2. 反射
  + 步骤：
    1. 将需要创建的对象全类名和需求执行的方法定义在配置文件中
    2. 在程序中加载读取配置文件
    3. 使用反射计数来加载类文件进内存
    4. 创建对象
    5. 执行方法

```java
public static void main(String[] args) throws Exception {
    //可以执行或创建任意类的对象，可以执行任意方法

    /**
     * 不能改变该类的任何代码。可以创建任意类的对象，可以执行任意方法
     */

    //1.加载配置文件
    //1.1创建Properties对象
    Properties pro = new Properties();
    Properties pro1 = new Properties();
    //1.2加载配置文件转换为集合;
    //1.2.1获取class目录下的配置文件
    ClassLoader classLoader = ReflectTest.class.getClassLoader();
    //InputStream is = classLoader.getResourceAsStream("pro.properties");
    //pro1.load(is);
    pro.load(new FileReader("Part01_FoundationStrengthening\\src\\files\\pro.properties"));
    //获取配置文件中定义的数据
    String className = pro.getProperty("className");
    String methodName = pro.getProperty("methodName");

    //3.加载该类进内存
    Class cls = Class.forName(className);
    Object obj = cls.newInstance();
    Method method = cls.getMethod(methodName);
    method.invoke(obj);

}
```

### 注解

+ 概念：说明程序，给计算机看
+ 注释：用文字描述程序，给人看
+ 概念描述：
  + JDK1.5之后的新特性
  + 说明程序的
  + 使用注解：@注解名称
+ **作用分类**
  
  1. 编写文档：通过代码里标识的注解生成文档
  2. 代码分析：通过代码里标识的注解对代码进行分析
  3. 编译检查：通过代码里标识的注解让编译器能够实现基本的编译检查
+ JDK中预定义的一些注解
  + @Override：检测被该注解标注的方法是否是继承自父类(接口)
  + @Deprecated：将该注解标注的内容已过时
  + @SuppressWarnings：压制警告
    + 一般穿参数all  @SuppressWarnings("all")
+ **自己定义注解**

  + 格式：
    + 元注解
    
    + public @interface 注解名称{
    
      ​		属性列表;
    
      }
    
  + 本质：注解本质上就是一个接口，该接口默认继承Annotation接口
    + public interface MyAnno extends java.lang.annotation.Annotation {}
    
  + 属性：接口中可以定义的成员方法

    + 要求：
      1. 属性的返回值类型有下列取值
         + 基本数据类型
         + String
         + 枚举
         + 注解
         + 以上类型的数组
      2. 定义了属性在使用时需要给属性赋值
         1. 如果定义属性时，使用default关键字给属性默认初始化值，则使用注解时，可以不进行属性的赋值
         2. 如果只有一个属性需要赋值，并且属性的名称是value，则value可以省略，直接定义值即可
         3. 数组赋值时，值使用{}包裹。如果数组中只有一个值，则{}省略

  + 元注解：用于描述注解的注解

    + @Target：描述注解能够作用的位置
      + ElementType取值：
        + TYPE：可以作用于类上
        + METHOD：可以作用于方法上
        + FIELD：可以作用于成员变量上
    + @Retention：描述注解被保留的阶段
      + @Retention(RententionPolicy.RUNTIME)：当前被描述的注解会保留到class字节码文件中，并被JVM读取到
    + @Documented：描述注解是否被抽取到api文档中
      + 
    + @Inherited：描述注解是否被子类继承

+ 在程序中使用注解：获取注解中定义的属性值

  1. 获取注解定义位置的对象 (Class，Method，Field)
  2. 获取指定的注解
  3. 调用注解中的抽象方法获取配置的属性

  ```java
  @Pro(className = "cn.annotation.Demo1", methodName = "show")
  public class ReflectTest {
      public static void main(String[] args) throws Exception {
          //解析注解
          //获取该类的字节码文件对象
          Class<ReflectTest> reflectTestClass = ReflectTest.class;
          //获取上边的注解对象
          //其实就是在内存中生成了一个该注解接口的子类实现对象
          /*
          public class ProImpl implements Pro{
              public String className(){
                  return "cn.annotation.Demo1";
              }
              public String methodName(){
                  return "show";
              }
          }
           */
          Pro annotation = reflectTestClass.getAnnotation(Pro.class);
          //调用注解对象中定义的抽象方法获取返回值
          String className = annotation.className();
          String methodName = annotation.methodName();
  
          Class cls = Class.forName(className);
          Object obj = cls.newInstance();
          Method method = cls.getMethod(methodName);
          method.invoke(obj);
      }
  }
  ```

```java
public static void main(String[] args) throws IOException {
    //创建j计算器对象
    Calculator calculator = new Calculator();
    //获取字节码对象
    Class aClass = calculator.getClass();
    //获取所有方法
    Method[] methods = aClass.getMethods();
    int number = 0;//出现异常的次数
    BufferedWriter bw = new BufferedWriter(new FileWriter("bug.txt"));
    for (Method method:methods){
        //判断方法上是否有Check注解
        if (method.isAnnotationPresent(Check.class)){
            //有,执行
            try {
                method.invoke(calculator);
            } catch (Exception e) {
                //捕获异常

                //记录到文件中
                number++;
                bw.write(method.getName()+ "方法出异常了");
                bw.newLine();
                bw.write("异常的名称:" +e.getCause().getClass().getSimpleName());
                bw.newLine();
                bw.write("异常的原因:"+e.getCause().getMessage());
                bw.newLine();
                bw.write("-----------------------");
                bw.newLine();

            }
        }
    }
    bw.write("本次一共出现"+number+"此异常");
    bw.flush();
    bw.close();
}
```

## Part02_JDBC

### 概念：java数据库连接

JDBC本质：官方定义的一套操作所有关系型数据库的规则(接口)，各个数据库厂商实现这套接口，提供数据库驱动jar包，我们可以使用这套接口(JDBC)编程，真正执行的代码是驱动jar包中的实现类

快速入门：

+ 步骤
  1. 导入驱动jar包
  2. 注册驱动
  3. 获取数据库连接对象 Connection
  4. 定义sql
  5. 获取执行sql语句的对象 Statement
  6. 执行sql，接受返回结果
  7. 处理结果
  8. 释放资源
  
+ ```java
  //1.导入驱动jar包
  //2.注册驱动
  Class.forName("com.mysql.jdbc.Driver");
  //3.获取数据库连接对象
  Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/a_sqltest", "root", "root");
  //4.定义sql语句
  String sql = "update account set money = money+1000 where id =1";
  //5.获取执行sql的对象 Statement
  Statement statement = connection.createStatement();
  //6.执行sql
  int count = statement.executeUpdate(sql);
  //7.处理结果
  System.out.println(count);
  //8.释放资源
  statement.close();
  connection.close();
  ```

### 详解各个对象

#### DriverManager：驱动管理对象

+ 功能：

  1. 注册驱动：告诉程序该使用哪一个数据库驱动jar

     ​	注意：mysql5之后的驱动jar包可以省略注册驱动的步骤

  2. 获取数据库连接：

     + 方法：static Connection getConnection(String url, String user, String password)
     + url ：指定连接的路径
       + 语法：jdbc:mysql://ip地址(域名):端口号/数据库名称

#### Connection：数据库连接对象

+ 获取执行sql 的对象
  + Statement createStatement()
  + PreparedStatement prepareStatement(String sql)
+ 管理事务：
  + 开启事务：setAutoCommit(boolean autoCommit)：调用该方法设置参数为false，即开启事务
  + 提交事务：commit()
  + 回滚事务：rollback()

#### Statement：执行sql的对象

1. 执行slq
   1. bolean execute(String sql)：可以执行任意的sql    (了解)
   2. int executeUpdate(String sql)：执行DML(insert、update、delete) 语句、DDL(create、alter、drop)语句
      + 返回值：影响的行数，可以通过这个影响的行数判断DML语句是否执行成功 返回值>0的则执行成功，反之，则失败。
   3. ResultSet executeQuery(String sql)：执行DQL(select)语句

+ ``` java
  public static void main(String[] args) {
          Statement statement = null;
          Connection conn = null;
          //注册驱动
          try {
              Class.forName("com.mysql.jdbc.Driver");
              //定义sql
              String sql = "insert into account values (3,'王五',1000)";
              //获取Connection对象
              conn = DriverManager.getConnection("jdbc:mysql:///a_sqltest", "root", "root");
              //获取执行sql的对象
              statement = conn.createStatement();
              //执行sql
              int count = statement.executeUpdate(sql);//影响行数
              //处理结果
              System.out.println(count);
              if (count > 0) {
                  System.out.println("执行成功");
              } else {
                  System.out.println("执行失败");
              }
          } catch (ClassNotFoundException e) {
              throw new RuntimeException(e);
          } catch (SQLException e) {
              throw new RuntimeException(e);
          }finally {
              //避免空指针异常
              if (statement !=null){
                  try {
                      statement.close();
                  } catch (SQLException e) {
                      throw new RuntimeException(e);
                  }
              }
              if (conn !=null){
                  try {
                      conn.close();
                  } catch (SQLException e) {
                      throw new RuntimeException(e);
                  }
              }
          }
      }
  ```

#### ResultSet：结果集对象，封装查询结果

+ boolean next()：游标向下移动一行,并判断是否有数据

+ getXxx(参数)：获取数据
  + Xxx：代表数据类型
  + 参数：
    + int：代表列的编号，**从1开始**
    + String ：代表列名称
  
+ ``` java
  public static void main(String[] args) {
          Connection conn = null;
          Statement statement = null;
          ResultSet resultSet = null;
          try {
              Class.forName("com.mysql.jdbc.Driver");
              conn = DriverManager.getConnection("jdbc:mysql:///a_sqltest", "root", "root");
              statement = conn.createStatement();
              String sql = "select * from emp";
              resultSet = statement.executeQuery(sql);
              //处理结果
              while (resultSet.next()){
                  int id = resultSet.getInt(1);
                  String name = resultSet.getString("name");
                  System.out.println(id +" " +name);
              }
          } catch (ClassNotFoundException e) {
              throw new RuntimeException(e);
          } catch (SQLException e) {
              throw new RuntimeException(e);
          } finally {
              if (resultSet != null){
                  try {
                      resultSet.close();
                  } catch (SQLException e) {
                      throw new RuntimeException(e);
                  }
              }
              if (statement != null) {
                  try {
                      statement.close();
                  } catch (SQLException e) {
                      throw new RuntimeException(e);
                  }
              }
              if (conn != null) {
                  try {
                      conn.close();
                  } catch (SQLException e) {
                      throw new RuntimeException(e);
                  }
              }
          }
      }
  ```

#### PreparedStatement：执行sql的对象

+ SQL注入问题：在拼接sql时，有一些sql的特殊关键字参与字符串的拼接。会造成安全问题
+ 解决sql注入问题：使用PreparedStatement对象来解决
+ 预编译的sql：参数使用 (?) 作为占位符
+ 步骤
  1. 导包
  2. 注册驱动
  3. 获取数据库连接对象Connection
  4. 定义sql
     + 注意：sql的参数使用?作为占位符。
  5. 获取执行sql语句的对象PreparedStatement    Connection.prepareStatement(String sql)
  6. 给?赋值：
     + 方法：setXxx(参数1,参数2)
       + 参数1：? 的位置编号 从1开始
       + 参数2：? 的值
  7. 执行sql，接收返回结果，不需要传递sql语句
  8. 处理结果
  9. 释放资源

### 抽取JDBC工具类 

+ 目的简化书写

+ 分析：

  1. 注册驱动也抽取

  2. 抽取一个方法获取连接对象

     + 需求：不想传参数（麻烦），还得保证工具类的通用性

     + 解决：配置文件

       jdbc.properties

  3. 抽取一个方法释放资源

```java
/**
 * JDBC工具类
 */
public class JDBCUtils {
    private static String url;
    private static String user;
    private static String password;
    private static String driver;
    /**
     * 文件的读取，只需要读取一次即可拿到这些值。使用静态代码块
     */
    static{
        //读取资源文件，获取值
        try {
            //1. 创建Properties集合类
            Properties prop =new Properties();
            //2. 加载文件
            //获取src路径下的文件的方式--->ClassLoader 类加载器
            ClassLoader classLoader = JDBCUtils.class.getClassLoader();
            URL res = classLoader.getResource("JDBC.properties");
            prop.load(new FileReader(res.getPath()));
            //3. 获取数据，赋值
            url= prop.getProperty("url");
            user = prop.getProperty("user");
            password = prop.getProperty("password");
            driver = prop.getProperty("driver");
            //注册驱动
            Class.forName(driver);
        } catch (IOException e) {
            throw new RuntimeException(e);
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        }
    }

    //获取连接对象
    public static Connection getConnection() {
        try {
            return DriverManager.getConnection(url,user,password);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    //释放资源方法
    public static void close(Statement stat, Connection conn) {
        if (stat != null) {
            try {
                stat.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
        if (conn != null) {
            try {
                conn.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
    }
    public static void close(ResultSet resultSet, Statement stat, Connection conn) {
        if (resultSet!=null){
            try {
                resultSet.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
        if (stat != null) {
            try {
                stat.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
        if (conn != null) {
            try {
                conn.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

```java
/**
 * 演示JDBC工具类
 */
public class JDBCDemo6 {
    public static void main(String[] args) {
        List<Emp> list = new JDBCTest().findAll();
        list.stream().forEach(e -> System.out.println(e.toString()));
    }

    /**
     * 查询所有emp对象
     * @return List<Emp>
     */
    @SuppressWarnings("all")
    public List<Emp> findAll(){
        List<Emp> emps = null;
        Connection conn = null;
        Statement statement = null;
        ResultSet resultSet = null;
        try {
            conn = JDBCUtils.getConnection();
            String sql = "select * from emp";
            statement = conn.createStatement();
            resultSet = statement.executeQuery(sql);
            //遍历结果集，封装对象，装载集合
            Emp emp =null;
            emps  = new ArrayList<>();
            while (resultSet.next()){
                //获取数据
                int id = resultSet.getInt("id");
                String workno = resultSet.getString("workno");
                String name = resultSet.getString("name");
                String gender = resultSet.getString("gender");
                int age = resultSet.getInt("age");
                String idcard = resultSet.getString("idcard");
                Date entrydate = resultSet.getDate("entrydate");
                String country_id = resultSet.getString("country_id");
                //封装对象
                emp = new Emp();
                emp.setId(id);
                emp.setWorkno(workno);
                emp.setName(name);
                emp.setGenderr(gender);
                emp.setAge(age);
                emp.setIdcard(idcard);
                emp.setEntrydate(entrydate);
                emp.setCountry_id(country_id);
                //装载集合
                emps.add(emp);
            }
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            JDBCUtils.close(resultSet,statement,conn);
        }
        return emps;
    }
}
```

### 登录练习

```java
/**
 * 登录案例
 */
public class JDBCDemo7 {
    public static void main(String[] args) {
        //键盘录入接收用户名和密码
        Scanner sc = new Scanner(System.in);
        System.out.print("请输入用户名：");
        String username = sc.nextLine();
        System.out.print("请输入密码：");
        String password = sc.nextLine();
        //调用方法
        boolean flag = new JDBCDemo7().login(username,password);
        //判断结果
        if (flag){
            System.out.println("登陆成功");
        }else {
            System.out.println("登录失败");
        }
    }

    /**
     * 登录方法
     */
    public boolean login(String username, String password) {
        if (username == null || password == null) {
            return false;
        }
        Connection conn = null;
        Statement statement = null;
        ResultSet resultSet = null;
        try {
            //连接数据库
            conn = JDBCUtils.getConnection();
            //定义sql
            String sql = "select * from user where username = '" + username + "' and password ='" + password + "'";
            //获取执行sql对象
            statement = conn.createStatement();
            //执行查询
            resultSet = statement.executeQuery(sql);
            return resultSet.next();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            JDBCUtils.close(resultSet, statement, conn);
        }

    }
    
    /**
    * 登录方法，使用PreparedStatement实现
    */
	public boolean login2(String username, String password) {
        if (username == null || password == null) {
            return false;
        }
        Connection conn = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;
        try {
            //连接数据库
            conn = JDBCUtils.getConnection();
            //定义sql
            String sql = "select * from user where username = ? and password = ?";
            //获取执行sql对象
            preparedStatement = conn.prepareStatement(sql);
            //给?赋值
            preparedStatement.setString(1,username);
            preparedStatement.setString(2,password);
            //执行查询
            resultSet = preparedStatement.executeQuery();
            return resultSet.next();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            JDBCUtils.close(resultSet, preparedStatement, conn);
        }

    }
}
```

### JDBC控制事务

1. 事务：一个包含多个步骤的业务操作。如果这个业务操作被事务管理，则这多个步骤要么同时成功，要么同时失败
2. 操作：
   1. 开启事务
   2. 提交事务
   3. 回滚事务
3. 使用Connection对象来管理事务
   + 开启事务：setAutoCommit(boolean autoCommit) 调用该方法设置参数为false，即开启事务
   + 提交事务：commit()
   + 回滚事务：rollback()

## Part03_数据库连接池

概念：其实就是一个容器(集合)，存放数据库连接的容器

​			当系统初始化好后，容器被创建，容器中会申请一些连接对象，当用户来访问数据库时，从容其中获取连接对象，			用户访问完之后，会将连接对象归还给容器。

实现：

+ 标准接口：DataSource      javax.sql包下
  + 方法：
    + 获取连接：getConnection()
    + 归还连接：Connection.close()。如果连接对象Connection是从连接池中获取的，那么调用Connection.close()方法，则不会再关闭连接，而是归还连接
  + 一般我们不去实现它，有数据库厂商来实现
    1. C3P0：数据库连接池技术
    2. Druid：数据库连接池实现技术，由阿里巴巴提供

### C3P0

+ 步骤：

  1. 导入jar包(两个) c3p0-0.9.5.2.jar  mchange-commons-java-0.2.12.jar

  2. 定义配置文件

     + 名称：c3p0.properties 或者 c3p0.config.xml
     + 路径：直接将文件放在src目录下即可

  3. 创建核心对象 数据库连接池对象 ComboPooledDataSource

  4. 获取连接：getConnection

     ``` sql
     //创建数据库连接池对象
     DataSource ds = new ComboPooledDataSource();
     //获取连接对象
     Connection connection = ds.getConnection();
     ```

### Druid

+ 步骤：
  1. 导入jar包  https://repo1.maven.org/maven2/com/alibaba/druid/
  2. 定义配置文件
     + properties形式
     + 可以叫任意名称，可以放在任意目录下
  3. 获取数据库连接池对象：通过工厂类来获取    DruidDataSourceFactory
  4. 获取连接：getConnection
+ 定义工具类
  1. 定义一个类 JDBCUtils
  2. 提供静态代码块加载配置文件，初始化连接池对象
  3. 提供方法
     1. 获取连接方法：通过数据库连接池获取链接
     2. 释放资源
     3. 获取连接池的方法

```java
public class JDBCUtils {

    //定义成员变量 DataSource
    private static DataSource ds;

    static {
        try {
            //加载配置文件
            Properties pro = new Properties();
            pro.load(JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties"));
            //获取DataSource
            ds = DruidDataSourceFactory.createDataSource(pro);
        } catch (IOException e) {
            throw new RuntimeException(e);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * 获取连接
     */
    public static Connection getConnection() throws SQLException {
        return ds.getConnection();
    }

    /**
     * 释放资源
     */
    public static void close(Statement statement, Connection connection) {
        close(null,statement,connection);
    }

    public static void close(ResultSet resultSet, Statement statement, Connection connection) {
        if (resultSet != null) {
            try {
                resultSet.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
        if (statement != null) {
            try {
                statement.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
        if (connection != null) {
            try {
                connection.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
    }
    /**
     * 获取连接池
     */
    public static DataSource getDataSource(){
        return ds;
    }
}
```

### Spring JDBC

+ Spring框架对JDBC的简单封装。提供了一个JDBCTemplate对象对象简化开发
+ 步骤：
  1. 导入jar包
  2. 创建JdbcTemplate对象。依赖于数据源DataSource
     + JdbcTemplate template = new JdbcTemplate(ds);
  3. 调用JdbcTemplate的方法来完成CRUD的操作
     + update()：执行DML语句。增、删、改语句
     + queryForMap()：查询结果将结果封装为map集合
       + 这个方法查询的结果集长度只能是1
     + queryForList()：查询结果将结果封装为list集合
       + 每一条记录封装为一个Map集合，再将Map集合装载到List集合中
     + query()：查询结果，将结果封装为JavaBean对象
       + query的参数：RowMapper
         + 一般使用BeanPropertyRowMapper实现类。可以完成数据到JavaBean的自动封装
         + new BeanPropertyRowMapper<类型>(类型.class)
     + queryForObject：查询结果，将结果封装为对象
       + 一般用于聚合函数的查询

## Part04_JavaScript

+ 概念：客户端脚本语言
+ 脚本语言：不需要编译

JavaScript = ECMAScript + JavaScript(BOM+DOM)

### ECMAScript：客户端脚本语言的标准

基本语法：

1. 与html结合方式
   1. 内部js：定义<script>，标签体内容就是js代码
   2. 外部js：定义<script>，通过src属性引入外部的js文件
2. 注释：与java相同
3. 数据类型
   + 原始数据类型（基本数据类型）：
     + number
     + string
     + boolean
     + null
     + undefined：未定义。如果一个变量没有初始化值，则会被默认赋值为undefined
   + 引用数据类型：对象
4. 变量
   + var 变量名 = 初始值;
   + typeof(变量名)：检测数据类型
5. 运算符
6. 流程控制语句

#### 基本对象

##### Function：函数(方法)对象

**创建**

+ function 方法名称(形式参数列表){}
+ var 方法名 = function(形式参数列表){}

**属性**

length：代表形式参数的个数

**特点**

+ 方法定义时，形参的类型不用写，返回值类型不用写
+ 方法是一个对象，如果定义名称相同的方法，会覆盖
+ 在JS中，方法的调用只与方法的名称有关，和参数列表无关
+ 在方法声明中有一个隐层的内置对象(数组)，arguments，封装所有的实际参数

**调用**

方法名称(实际参数列表)

##### Array：数组对象

**创建**

+ var arr = new Array(元素列表);
+ var arr = new Array(默认长度);
+ var arr = [元素列表];

**方法**

+ join(参数)：将数组中的元素按照指定的分隔符拼接为字符串
+ push()：向数组的末尾添加一个或更多元素，并返回新的长度

**属性**

length：数组的长度

**特点**

+ JS中，数组元素的类型可变
+ 数组长度可变

##### Date：日期对象

**创建**

+ var date  = new Date();

**方法**

+ toLoaleString()：返回当前date对象对应的时间本地字符串格式
+ getTime()：获取毫秒值。返回当前如期对象描述的时间到1970年1月1日零点的毫秒值差

##### Math：数学

**创建**

直接使用无须创建

**方法**

与java基本一致

##### RegExp：正则表达式对象

**正则表达式**

```
单字符:[]
	如：[a] [ab] [a-zA-Z0-9_]
	特殊符号代表特殊含义的单个字符
		\d:单个数字字符[0-9]
		\w:单个单词字符[a-zA-Z_]
量词符号
	?:表示出现0次或1次
	*:表示出现0次或多次
	+:出现1次或多次
	{m,n}: m<=数量<=n
```

**正则对象**

**创建**

+ var reg = new RegExp("正则表达式");
+ var reg = /正则表达式/;

**方法**

+ test(参数)：验证指定的字符串是否符合正则定义的规范

##### Global

特点：全局对象，这个Global中封装的方法不需要对象就可以直接调用。方法名();

方法：

+ encodeURI():url编码
+ decodeURI():url解码
+ encodeURIComponent():url编码，编码的字符更多
+ decodeURIComponent():url解码
+ parseInt():将字符串转为数字：注意判断每一个字符是否是数字，直到不是数字为之
+ isNaN：判断一个值是否是NaN
  + NaN六亲不认，所有NaN参与的==比较全部为false
+ eval()：把字符串作为脚本代码执行

### DOM简单学习：为了满足案例要求

**功能**：控制HTML文档的内容

**代码**：获取页面的标签对象

+ document.getElementById("id值")：通过元素的id获取元素

**操作Element对象**

+ 修改属性值：
  + 明确获取的对象是哪一个
  + 查看API文档，找其中有哪些属性可以设置
+ 修改标签体内容
  + 属性：innerHTML

``` html
<a href="http://www.baidu.com" id="dom">DOM演示</a>
<h1 id="h1">aaaaa</h1>
<script>
    var elementById = document.getElementById("dom");
    elementById.href = "http://fanyi.baidu.com";

    var elementById1 = document.getElementById("h1");
    alert("修改");
    elementById1.innerHTML = "bbbbb";
</script>
```

### 事件简单学习

**功能**：某些组件被执行了某些操作后触发某些代码

**事件**：onclick --- 单击事件

**绑定事件**

1. 直接在html标签上，指定事件的属性，属性值就是js代码
2. 通过js获取元素对象，指定事件属性，设置一个函数

```html
<script>
    function fun(){
        alert("单击事件");
    }
    let elementById = document.getElementById("h2");
    elementById.onclick=fun();
</script>
<h1 id="h1" onclick="fun()">单击事件弹窗</h1>
<h1 id="h2">单击事件2</h1>
```



### BOM

**概念**:Browser Object Model 浏览器对象模型

**组成**：

+ Window:窗口对象
+ Navigator：浏览器对象
+ Screen：显示器屏幕对象
+ History：历史记录对象
+ Location：地址栏对象

#### Window：窗口对象

**方法**

+ **与弹出框有关的方法**
  + alert() 显示带有一段信息和一个确认按钮的警告框
  + confirm()  显示带有一段信息以及确认按钮和取消按钮的对话框
    + 如果用户点击确定按钮，则方法返回true
    + 如果用户点机取消按钮，则方法返回false
  + prompt()   显示可提供用户输入的对话框
    + 返回值：获取用户输入的值
+ **与打开关闭有关的方法**
  + close() 关闭浏览器窗口
    + 关闭调用窗口
  + open()  打开一个新的浏览器窗口
    + 返回新的Window对象
+ **定时器**
  + setTimeout()    在指定的毫秒数后调用函数或计算表达式
    + 参数
      1. js代码或者方法对象
      2. 毫秒值
    + 返回值：唯一标识，用于取消懂事起
  + clearTimeout()  取消setTimeout()  方法设置的timeout
  + setInterval()   按照指定的周期(以毫秒计)来调用函数或计算表达式
  + clearInterval()  取消由setInterval() 设置的timeout
+ **属性**
  1. 获取其他BOM
     + history
     + location
     + Navigator
     + Screen
  2. 获取DOM对象
     + document

#### Location：地址栏对象

1. 创建(获取)
   1. window.location
   2. location
2.  方法
   + reload()    刷新
3. 属性
   1. href    设置或返回完整的URL

#### History：历史记录对象

1. 创建(获取)
   1. window.history
   2. history
2. 方法
   + back()     加载历史列表中前一个URL
   + forward() 加载历史列表中下一个URL
   + go(参数)         加载历史列表中某个具体页面  正数表示前进几个历史记录  负数后退
3. 属性
   + length    返回当前窗口历史列表中的url数量

### DOM

概念：Document Object Model 	文档对象模型

W3C DOM 标准被分为3各部分

+ 核心 DOM  针对任何结构化文档的标准模型
  + Document：文档对象
  + Element：元素对象
  + Attribute：属性对象
  + Text：文本对象
  + Comment：注释对象
  + Node：节点对象，其他5个的父对象
+ XML DOM 针对XML文档的标准模型
+ HTML DOM 针对HTML文档的标准模型

#### 核心DOM模型

##### Document：文档对象

1. 创建： 在html dom模型中可以使用window对象来获取
   + window.document
   + document
2. 方法
   1. 获取Element对象
      1. getElementById()：根据id属性值获取元素对象。id属性值一般唯一
      2. getElementsByTagName()：根据元素名称获取元素对象们。返回值是一个数组
      3. getElementsByClassName()：根据Class属性值获取元素对象们。返回值是一个数组
      4. getElementsByName()：根据name属性值获取元素对象们。返回值是一个数组
   2. 创建其他DOM对象
      1. createAttribute(name)
      2. createComment()
      3. createElement()
      4. createTextNode()  
3. 属性

##### Element：元素对象

通过document来获取和创建

方法

+ removeAttribute()：删除属性
+ setAttribute()：设置属性

##### Node：节点对象

所有dom对象都可以被认为是一个节点

方法：CRUD dom树

+ appendChild()：向节点的子节点列表的末尾添加新的子节点
+ removeChild()：删除(并返回)当前节点的指定子节点
+ replaceChild()：用新节点替换一个子节点

#### HTML DOM

1. 属性标签体的设置和获取：innerHTML
2. 使用html元素对象的属性
3. 控制样式

### 事件

**概念**：某些组件被执行了某些操作后，触发某些代码的执行

**常见的事件：**

| 事件        | 说明                       |
| ----------- | -------------------------- |
| onclick     | 单击事件                   |
| ondblclick  | 双击事件                   |
| onblur      | 失去焦点                   |
| onfocus     | 元素获得焦点               |
| onload      | 一张页面或一幅图像完成加载 |
| onmousedown | 鼠标按钮被按下             |
| onmouseup   | 鼠标按钮被松开             |
| onmousemove | 鼠标被移动                 |
| onmouseover | 鼠标移动某元素之上         |
| onmouseout  | 鼠标从某元素一开           |
| onkeydown   | 某个键盘按键被按下         |
| onkeyup     | 某个键盘按键被松开         |
| onkeypress  | 某个键盘按键被按下并松开   |
| onchange    | 域的内容被改变             |
| onselect    | 文本被选中                 |
| onsubmit    | 确认按钮被点击             |
| onreset     | 重置按钮被点击             |

**onmousedown**

+ 定义方法时，定义一个形参，接受event对象
+ event对象的button属性可以获取鼠标哪个键被点击了
+ event.button

**onkeydown**

+ event.keyCode返回按键ASCII码



## Part05_Bootstrap

概念：一个前端开发的框架

**框架**：一半成品软件，开发人员可以在框架基础上，在进行开发，简化编码

**好处**：

+ 定义了很多的css样式和js插件。开发人员直接可以使用这些样式和插件得到丰富的页面效果
+ 响应式布局

**快速入门**

1. 下载Bootstrap
2. 在项目中将这三个文件夹复制
3. 创建html页面，引入必要的资源文件

```html
<!doctype html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>Bootstrap 101 Template</title>
    <!-- Bootstrap -->
    <link rel="stylesheet" href="css/bootstrap.min.css">

</head>
<body>
<h1>你好，世界！</h1>

<!-- jQuery (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
<script src="https://fastly.jsdelivr.net/npm/jquery@1.12.4/dist/jquery.min.js"></script>
<!-- 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
<script src="js/bootstrap.min.js"></script>
</body>
</html>
```

### 响应式布局

实现：依赖于栅格系统，将一行平均分成12个格子，可以指定元素占几个格子

**步骤**：

1. 定义容器
   + 容器分类：
     1. container：
     2. container-fluid：
2. 定义行       样式：row
3. 定义元素 ，指定该元素在不同设备上，所占的格子数目。样式：col-设备代号-格子数目
   + 设备代号：
   + xs：超小屏幕(<768px)：col-xs-12
   + sm：小屏幕(>=768px)
   + md：中等屏幕(>=992px)
   + lg：大屏幕(>=1200px)
4. 注意
   + 一行中如果格子的数目超过12，则超出的部分自动换行
   + 栅格类属性可以向上兼容

### css样式和js插件

#### 全局css样式

**按钮**：class="btn btn-default"

**图片**：

+ class="img-responsive"：图片在任意尺寸都占100%
+ 形状
  + img-rounded
  + img-circle
  + img-thumbnail

**表格**

+ table
+ table-bordered
+ table-hover

**表单**

+ 给表单项添加：class="form-control"

#### 组件

**导航条**

```html
<nav class="navbar navbar-inverse">
    <div class="container-fluid">
        <!-- Brand and toggle get grouped for better mobile display -->
        <div class="navbar-header">
            <!-- 定义汉堡按钮-->
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">首页</a>
        </div>

        <!-- Collect the nav links, forms, and other content for toggling -->
        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
            <ul class="nav navbar-nav">
                <li class="active"><a href="#">链接1 <span class="sr-only">(current)</span></a></li>
                <li><a href="#">连接2</a></li>
                <li class="dropdown">
                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">下拉菜单 <span class="caret"></span></a>
                    <ul class="dropdown-menu">
                        <li><a href="#">Action</a></li>
                        <li><a href="#">Another action</a></li>
                        <li><a href="#">Something else here</a></li>
                        <li role="separator" class="divider"></li>
                        <li><a href="#">Separated link</a></li>
                        <li role="separator" class="divider"></li>
                        <li><a href="#">One more separated link</a></li>
                    </ul>
                </li>
            </ul>
            <form class="navbar-form navbar-left">
                <div class="form-group">
                    <input type="text" class="form-control" placeholder="Search">
                </div>
                <button type="submit" class="btn btn-default">Submit</button>
            </form>
            <ul class="nav navbar-nav navbar-right">
                <li><a href="#">Link</a></li>
                <li class="dropdown">
                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">Dropdown <span class="caret"></span></a>
                    <ul class="dropdown-menu">
                        <li><a href="#">Action</a></li>
                        <li><a href="#">Another action</a></li>
                        <li><a href="#">Something else here</a></li>
                        <li role="separator" class="divider"></li>
                        <li><a href="#">Separated link</a></li>
                    </ul>
                </li>
            </ul>
        </div><!-- /.navbar-collapse -->
    </div><!-- /.container-fluid -->
</nav>
```

**分页条**

```html
<nav aria-label="Page navigation">
    <ul class="pagination">
        <li class="disabled">
            <a href="#" aria-label="Previous">
                <span aria-hidden="true">&laquo;</span>
            </a>
        </li>
        <li class="active"><a href="#">1</a></li>
        <li><a href="#">2</a></li>
        <li><a href="#">3</a></li>
        <li><a href="#">4</a></li>
        <li><a href="#">5</a></li>
        <li>
            <a href="#" aria-label="Next">
                <span aria-hidden="true">&raquo;</span>
            </a>
        </li>
    </ul>
</nav>
```

#### 插件

**轮播图**

```html
<div id="carousel-example-generic" class="carousel slide" data-ride="carousel">
  <!-- Indicators -->
  <ol class="carousel-indicators">
    <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
    <li data-target="#carousel-example-generic" data-slide-to="1"></li>
    <li data-target="#carousel-example-generic" data-slide-to="2"></li>
  </ol>

  <!-- Wrapper for slides -->
  <div class="carousel-inner" role="listbox">
    <div class="item active">
      <img src="..." alt="...">
      <div class="carousel-caption">
        ...
      </div>
    </div>
    <div class="item">
      <img src="..." alt="...">
      <div class="carousel-caption">
        ...
      </div>
    </div>
    ...
  </div>

  <!-- Controls -->
  <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
    <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
    <span class="sr-only">Previous</span>
  </a>
  <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
    <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
    <span class="sr-only">Next</span>
  </a>
</div>
```

## Part06_XML

### **概念**

+ Extensible Markyup Language 可扩展标记语言
+ 可扩展：标签都是自己定义的

**功能**

1. 配置文件
2. 在网络中传输

**xml与html的区别**

+ xml标签都是自定义的，html标签是预定义的
+ xml的语法严格，html语法松散
+ xml是存储数据的，html是展示数据的

### 语法

**基本语法**

1. xml文档的后缀名   .xml
2. xml第一行必须定义为文档声明
3. xml文档中有且仅有一个根标签
4. 属性值必须使用引号(单双都可)
5. 标签必须正确关闭
6. xml标签名称区分大小写 

**组成部分**

1. 文档声明

   1. 格式：<?xml 属性列表 ?>
   2. 属性列表
      + version：版本号，必须的属性
      + encoding：编码方式。告知解析引擎当前文档使用的字符集，默认值：ISO-8859-1
      + standalone：是否独立  yes/no

2. 指令（了解）结合css 

   +  <?xml-stylesheet type="text/css" href="a.css" ?>

3. 属性

   + id属性值唯一

4. CDATA区

   + 该区中的数据会被原样展示

   + ```html
     <![CDATA[ 数据 ]]>
     ```

5. **约束**：规定xml文档的书写规则

   + 作为框架的使用者(程序员)

     1. 能够在xml中引入约束文档
     2. 能够简单的读懂约束文档

   + DTD：简单的约束技术

     + 内部引入

       ```html
       <!DOCTYPE students[
               ]>
       ```

     + 外部引入

       + 本地：<!DOCTYPE 根标签名 SYSTEM "dtd文件位置">
       + 网络：<!DOCTYPE 根标签名 PUBLIC "dtd文件名字" "dtd文件的位置URL">

   + Schema：复杂的约束技术

     + 太麻烦看代码


### 解析

**操作xml文档**

1. 解析(读取)：将文档中的数据读取到内存中
2. 写入：将内存中的数据保存到xml文档中。持久化的存储

**xml解析方式**

1. DOM：将标记语言文档一次性加载进内存，在内存中形成一颗dom树
   + 优点：操作方便，可以对文档进行CRUD的所有操作
   + 缺点：占内存
2. SAX：逐行读取，基于事件驱动
   + 优点：不占内存
   +  缺点：只能读，不能增删改

**xml常见的解析器**

1. JAXP：sun公司提供的解析器，支持dom和sax两种思想 (性能低，代码麻烦没人用)
2. DOM4J：一款非常优秀的解析器
3. jsoup：一款java的html解析器，可以直接解析某个url地址、html文档内容。它提供了一套非常省力的API，可通过DOM，CSS以及类似于JQuery的操作方法来取出和操作数据
4. PULL：Android操作系统内置的解析器，sax方式的

### Jsoup

**快速入门**

1. 导入jar包
2. 获取Document对象
3. 获取对应的标签Element对象
4. 获取数据

```java
//2.获取Document对象  根据xml文档获取
//2.1获取student.xml的path路径
String path = JsoupDemo01.class.getClassLoader().getResource("users.xml").getPath();
//2.2解析xml文档,加载文档进内存，获取dom树 --->Document
Document document = Jsoup.parse(new File(path), "utf-8");
//3.获取元素对象  Element
Elements elements = document.getElementsByTag("name");

System.out.println(elements.size());
//3.1获取第一个name的Element对象
Element element = elements.get(0);
//3.2获取数据
String name = element.text();
System.out.println(name);
```

#### 对象的使用

Jsoup：工具类，可以解析html或xml文档，返回Document对象

+ parse：解析html或xml文档，返回Document
  + parse(File in, String charsetName)：解析xml或html文件的
  + parse(String html)：解析xml或html字符串
  + parse(URL url, int timeoutMillis)：通过网络路径获取指定的html或xml的文档对象

Document：文档对象。代表内存中的dom树

+ 获取Element对象
  + getElementById(String id)：根据id属性值获取唯一的element对象
  + getElementsByTag(String tagName)：根据标签名称获取元素对象集合
  + getElementsByAttribute(String key)：根据属性名称获取元素对象集合
  + getElementsByAttributeValue(String key, String value)：根据属性名和属性值获取元素对象集合

Elements：元素Element对象的集合。可以当做ArrayList<Element>来使用 

Element：元素对象

+ 获取子元素对象
+ 获取属性值
  + String attr(String key)：根据属性名称获取属性值
+ 获取文本内容
  + String text()：获取所有包括子标签的文本内容
  + String html()：获取标签体的所有内容包括子标签的字符串内容

Node：节点对象

+ 是Document和Element的父类

#### 快捷查询方式

1. selector：选择器
   + 使用的方法：Elements    select(String cssQuery)
   + 语法：参考Selector类中定义的语法
2. XPath：XPath即为XML路径语言，它是一种用来确定XML(标准通用标记语言的子集)文档中某部分位置的语言
   + 使用Jsoup的Xpath需要额外导入jar包
   + 查询w3cshool参考手册，使用xpath的语法完成查询

## Part07_Tomcat_Servlet

**web服务器软件**

+ 服务器：安装了服务器软件的计算机  
+ 服务器软件：接受用户的请求，处理请求，做出响应
+ 在web服务器软件中，可以不是web项目，让用户通过浏览器访问这些项目

**常见的java相关web服务器软件**

+ webLogic：oracle公司，大型的JavaEE服务器，支持所有的JavaEE规范，收费
+ webSphere：IBM公司，大型的JavaEE服务器，支持所有的JavaEE规范，收费
+ JBOSS：JBOSS公司，大型的JavaEE服务器，支持所有的JavaEE规范，收费
+ Tomcat：Apache基金组织，中小型JavaEE服务器，仅仅支持少量的JavaEE规范servlet/jsp，开源

### Tomcat：web服务器软件

一般会将tomcat的默认端口号修改为80。80端口号是http协议的默认端口号

#### 部署：

**部署项目方式**

1. 直接将项目放到webapps目录下即可

   + /hello：项目的访问路径-->虚拟目录
   + 简化部署：将项目打成war包，将war包放置到webapps目录下，war包会自动解压缩

2. 配置conf/server.xml文件

   ``` xml
   <!--在<host>标签体中配置-->
   <Context docBase="D:\hello" path="/hehe" />
   ```

   + docBase：项目存放的路径
   + path：虚拟目录

3. 在conf\Catalina\localhost目录下创建任意名称的xml文件。在文件中编写

   ``` xml
   <Context docBase="D:\hello" />
   ```

   + 虚拟目录就是xml文件名称

**静态项目和动态项目**

+ 目录结构
  + java动态项目的目录结构：
    + 项目的根目录
      + WEB-INF目录：
        + web.xml：web项目的核心配置文件
        + classes：放置字节码文件的目录
        + lib目录：放置依赖的jar包

### Servlet

+ servlet就是一个接口，定义了java类被浏览器访问到(tomcat识别)的规则
+ 自定义类，实现servlet接口，复写方法

**快速入门**

1. 创建JavaEE项目

2. 定义类，实现servlet接口

3. 实现接口中的抽象方法

4. 配置Servelet

   在web.xml中

```xml
<servlet>
    <servlet-name>demo1</servlet-name>
    <servlet-class>com.web.servlet.ServletDemo01</servlet-class>
</servlet>

<servlet-mapping>
    <servlet-name>demo1</servlet-name>
    <url-pattern>/demo1</url-pattern>
</servlet-mapping>
```

**执行原理**

1. 当服务器接收到客户端浏览器的请求后，会解析请求URL路径，获取访问的Servlet的资源路径
2. 查找web.xml文件，是否有对应的<url-pattern>标签体内容
3. 如果有，则找到对应的<servlet-class>全类名
4. tomcat会将字节码文件加载进内存，并且创建其对象
5. 调用其方法

**Servlet中的生命周期**

1. 被创建：执行init方法，只执行一次
   + 在<servlet>标签下配置创建时机
     1. 第一次被访问是创建，默认
        + <load-on-startup>的值为负数
     2. 在服务器启动时创建
        + <load-on-startup>的值为0或正整数
   + Servlet的init方法，只执行一次，说明Servlet在内存中只存在一个对象是单例的
     +  多个用户同时访问，可能存在线程安全问题
     +  解决：尽量不要在Servlet中定义成员变量，即使定义了成员变量，也不要修改值 
2. 提供服务：执行servlet方法，执行多次
3. 被销毁：执行destroy方法，只执行一次

 **Servlet注解配置**

Servlet3.0支持注解配置。可以不需要web.xml

```java
@WebServlet("资源路径")
```

**IDEA与tomcat相关配置**

1. IDEA会为每一个tomcat部署的项目单独建立一份配置文件

   + 查看控制台的log:

   + ```java
     Using CATALINA_BASE:   "C:\Users\86130\AppData\Local\JetBrains\IntelliJIdea2022.1\tomcat\78b2eb6a-a077-4661-a9d3-ddae3b94ad3e"
     ```

2. 工作空间项目  和  tomcat部署的web项目

   + tomcat真正访问的是"tomcat部署的web项目"，"tomcat部署的web项目"对应着"工作空间项目" 的web目录下的所有资源
   + WEB-INF目录下的资源不能被浏览器直接访问

**Servlet体系结构**

Servlet -- 接口
        |
GenericServlet -- 抽象类
        |
HttpServlet -- 抽象类

+ GenericServlet：将Servlet接口中除service方法外全部空实现
+ HttpServlet：对http协议的一种封装，简化操作
  1. 定义类继承HttpServlet
  2. 复写doGet/doPost方法

**Servlet相关配置**

urlPatterns：servlet访问路径

1. 一个Servlet可以定义多个访问路径
2. 路径定义规则
   1. /xxx
   2. /xxx/xxx：多层路径，目录结构
   3. *.do

## Part08_Http

