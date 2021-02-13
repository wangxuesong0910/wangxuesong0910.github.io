# JDBC

## 获取数据库连接方式

```java
 @Test
    public  void testConnection5() throws Exception {
       //1.加载配置文件
       InputStream is = ConnectionTest.class.getClassLoader().getResourceAsStream("jdbc.properties");
       Properties pros = new Properties();
       pros.load(is);

       //2.读取配置信息
       String user = pros.getProperty("user");
       String password = pros.getProperty("password");
       String url = pros.getProperty("url");
       String driverClass = pros.getProperty("driverClass");

       //3.加载驱动
       Class.forName(driverClass);

       //4.获取连接
       Connection conn = DriverManager.getConnection(url,user,password);
       System.out.println(conn);

   }
```

## 使用PrepareStatement实现数增删改

### 插入

```java
   @Test
    public void CnnTest1() throws Exception {

        //此处将获取数据库连接与关闭资源封装到了JDBCUtils类中
        Connection connection = JDBCUtils.getConnection();

        //想要执行的sql语句
        String sql = "INSERT INTO user_table VALUES(?,?,?)";

//PreparedStatement prepareStatement​(String sql, int[] columnIndexes)
// 创建一个默认的 PreparedStatement对象，能够返回由给定数组指定的自动生成的键。
        PreparedStatement ps = connection.prepareStatement(sql);
        //设置值
        ps.setString(1, "zzz");
        ps.setString(2, "123456");
        ps.setInt(3, 20000);

//boolean execute​() 执行此 PreparedStatement对象中的SQL语句，可能是任何类型的SQL语句。  
        ps.execute();

        JDBCUtils.CloseResource(connection, ps);


    }
```

### 增加

```java
@Test
    public void ConTest2() throws Exception {
        //1.获取数据库连接
        InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("jdbc.properties");
        Properties ps = new Properties();
        ps.load(is);
        String user = ps.getProperty("user");
        String password = ps.getProperty("password");
        String url = ps.getProperty("url");
        String driverClass = ps.getProperty("driverClass");

        Class.forName(driverClass);

        Connection connection = DriverManager.getConnection(url, user, password);

        //2.预编译sql语句
        String sql = "UPDATE customers SET name=? WHERE id=?";
        PreparedStatement pps = connection.prepareStatement(sql);
        //3.填充占位符
        pps.setString(1, "wxs");
        pps.setInt(2, 13);
        //4.执行sql语句
        pps.execute();
        //5.关闭连接
        pps.close();
        connection.close();


    }
```

### 删除

```java
@Test
    public void ConTest3() throws Exception {
        //1.获取数据库连接
        InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("jdbc.properties");

        Properties properties = new Properties();
        properties.load(is);

        String user = properties.getProperty("user");
        String password = properties.getProperty("password");
        String url = properties.getProperty("url");
        String driverClass = properties.getProperty("driverClass");

        Class.forName(driverClass);

        Connection connection = DriverManager.getConnection(url, user, password);

        //2.预编译sql语句（获取prepareStstement实例）
        String sql = "DELETE FROM user_table WHERE user=?";
        PreparedStatement ps = connection.prepareStatement(sql);


        //3.填充占位符
        ps.setString(1, "zzz");


        //执行sql 语句
        ps.execute();

        //5.关闭资源

        connection.close();
        ps.close();
    }

```

## 使用 PrepareStatement实现查询操作

```java
 //使用PrepareStatement实现查询操作
    @Test
    public void ConTest4() throws Exception {
        //1.获取数据库连接
        InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("jdbc.properties");
        Properties properties = new Properties();
        properties.load(is);

        String user = properties.getProperty("user");
        String password = properties.getProperty("password");
        String url = properties.getProperty("url");
        String driverClass = properties.getProperty("driverClass");

        Class.forName(driverClass);

        Connection connection = DriverManager.getConnection(url, user, password);

        //2.预编译sql，获取PrepareStatement实例
        String sql = "SELECT `user`,`password`,`balance` FROM user_table;";
        PreparedStatement ps = connection.prepareStatement(sql);

        //执行，并返回结果集
        //ResultSet executeQuery​() 执行此 PreparedStatement对象中的SQL查询，并返回查询生成的 ResultSet对象。
        ResultSet resultSet = ps.executeQuery();

        //处理结果集
        //boolean next​() 将光标从当前位置向前移动一行
        while (resultSet.next()){
            String user1 = resultSet.getString(1);
            String password1 = resultSet.getString(2);
            int balance = resultSet.getInt(3);

            //将数据库中的数据封装
            /**
             * ORM思想(object relational mapping)
             * - 一个数据表对应一个java类
             * - 表中的一条记录对应java类的一个对象
             * - 表中的一个字段对应java类的一个属性
             */
            User_table user_table = new User_table(user1, password1, balance);
            System.out.println(user_table);
        }


    }
```

### 针对某一张表实现通用查询

```java
 //针对表user的通用查询操作
    public User userQuery(String sql,Object...args) {
        Connection connection = null;
        PreparedStatement ps = null;
        ResultSet resultSet = null;
        try {
            //获取数据库连接
            //获取数据库连接
            InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("jdbc.properties");
            Properties properties = new Properties();
            properties.load(is);

            String user = properties.getProperty("user");
            String password = properties.getProperty("password");
            String url = properties.getProperty("url");
            String driverClass = properties.getProperty("driverClass");

            Class.forName(driverClass);
             connection = DriverManager.getConnection(url, user, password);


            //形参传入sql语句（预编译）
            ps = connection.prepareStatement(sql);
            //填充占位符
            for (int i = 0; i < args.length; i++) {
                ps.setObject(i+1,args[i]);
            }

            //执行查询并获取结果集
             resultSet= ps.executeQuery();
            //获取结果集的元数据 ：ResultSetMetaData getMetaData​()
            ResultSetMetaData metaData = resultSet.getMetaData();
            //通过ResultSetMetaData，得到列的个数
            //int getColumnCount​() 返回此 ResultSet对象中的列数。
            int columnCount = metaData.getColumnCount();

            //遍历表中数据
            while (resultSet.next()) {
                User user1 = new User();
                for (int i = 0; i < columnCount; i++) {

                    //获取列值：
                    // Object getObject​(int columnIndex)
                    // 获取此 ResultSet对象的当前行中指定列的值为Java编程语言中的 Object 。
                    Object columnValue = resultSet.getObject(i + 1);

                    //获取列的别名：使用类的属性名充当(ResultSetMetaData)
                    //String getColumnLabel​(int column) 获取指定列的建议标题用于打印输出和显示。  (as后显示的)
                    //String getColumnName​(int column) 获取指定列的名称。
                    String columnLabel = metaData.getColumnLabel(i + 1);

                    //使用反射
                    Field field = User.class.getDeclaredField(columnLabel);
                    field.setAccessible(true);
                    //void set​(Object obj, Object value) 将指定的对象参数中由此 Field对象表示的字段设置为指定的新值。
                    field.set(user1,columnValue);
                }
            return user1;
            }
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        } catch (NoSuchFieldException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } finally {
            try {
                if (connection!=null)
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
            try {
                if (ps!=null)
                ps.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
            try {
                if (resultSet!=null)
                resultSet.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }

        }

        return null;
    }
    @Test
    public void ConnTest8(){
        String sql = "SELECT `id`,`name`,`password`,`address`,`phone` FROM `user` WHERE `id`=?";
        User user = userQuery(sql,3);
        System.out.println(user);
    }
}
```

### 针对表的字段名与类的属性名不同的情况

1. 必须声明sql时，使用类的属性名来命名字段的别名
2. 使用ResultSetMetaData时，需要使用getColumnLabel()来替换getColumnName()
   * 获取类的别名：
   * 说明：如果sql中没有给字段起别名，getColumnLabel()获取的就是列名

## 针对所有表的通用查询

```java
 //针对查询的通用操作
    public <T> T userQuery1(Class<T> clazz, String sql, Object... args) {
        Connection connection = null;
        PreparedStatement ps = null;
        ResultSet rs = null;


        try {
            //获取数据库连接
            connection = JDBCUtils.getConnection();
            //预编译sql
            ps = connection.prepareStatement(sql);

            //填充占位符
            for (int i = 0; i < args.length; i++) {
                ps.setObject(i + 1, args[i]);

            }

            //获取结果集
            rs = ps.executeQuery();
            //获取元数据
            ResultSetMetaData metaData = ps.getMetaData();
            //获取列数
            int columnCount = metaData.getColumnCount();

            //取数据
            while (rs.next()) {
                T t = clazz.newInstance();
                for (int i = 0; i < columnCount; i++) {
                    //获取列值
                    Object object = rs.getObject(i + 1);

                    //获取列名
                    String columnLabel = metaData.getColumnLabel(i + 1);


                    Field filed = clazz.getDeclaredField(columnLabel);
                    filed.setAccessible(true);
                    filed.set(t, object);

                }
                return t;
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } catch (InstantiationException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (NoSuchFieldException e) {
            e.printStackTrace();
        } finally {
            JDBCUtils.CloseResource(connection, ps, rs);
        }

        return null;
    }
    
    
    @Test
    public void ConnTest8() {
        String sql = "SELECT `id`,`name`,`password`,`address`,`phone` FROM `user` WHERE `id`=?";
        User user = userQuery1(User.class, sql, 3);
        System.out.println(user);
    }

```

## 数据批处理

```java
public class ConnTest4 {

    /**
     * Connection connection = JDBCUtils.getConnection();
     * 工具类或其他方法发涉及到异常，主调用类应该使用try-catch处理异常，其他类或工具类应用throws抛出异常
     *
     * @throws Exception
     */
    @Test
    //批量插入数据操作一：
    public void ConnTest41() throws Exception {
        long start = System.currentTimeMillis();

        Connection connection = JDBCUtils.getConnection();

        String sql = "insert into goods(name)values(?)";

        PreparedStatement ps = connection.prepareStatement(sql);

        for (int i = 1; i < 10000; i++) {
            ps.setString(1, "name" + i);
            ps.executeUpdate();
        }

        long end = System.currentTimeMillis();

        System.out.println("执行花费时间为：" + (end - start));//448267

        connection.close();
        ps.close();
    }

    @Test
    //批量插入数据操作二：
    /**
     * 相对于操作一的修改：
     *  1. 使用addBatch()、executeBatch()、clearBatch()
     *  2.mysql服务器默认是关闭处理的，我们需要通过一个参数，让mysql开启批处理的支持
     *      ?rewriteBatchedStatements=true 写在配置文件的url后面
     *  3.使用更新的mysql驱动：mysql-connector-java-5.1.37-bin.jar
     */
    public void ConnTest42() throws Exception {
        long start = System.currentTimeMillis();

        Connection connection = JDBCUtils.getConnection();

        String sql = "insert into goods(name)values(?)";

        PreparedStatement ps = connection.prepareStatement(sql);

        for (int i = 1; i < 20000; i++) {
            ps.setString(1, "name" + i);
//            ps.executeUpdate();

            //1.“攒”sql
            ps.addBatch();
            if (i % 500 == 0) {
                //2.执行
                ps.executeBatch();
                //3.清空
                ps.clearBatch();
            }
        }

        long end = System.currentTimeMillis();

        System.out.println("执行花费时间为：" + (end - start));//10000条：448267
                                                                //20000条：4055

        connection.close();
        ps.close();
    }

    @Test
    //批处理数据操作三
    //在层次二基础上操作
    //使用Connection 的 setAutoCommit(false)  /  commit()
    public void ConnTest43() throws Exception{
        long start = System.currentTimeMillis();

        Connection connection = JDBCUtils.getConnection();

        //1.设置为不自动提交数据
        connection.setAutoCommit(false);

        String sql = "insert into goods(name)values(?)";

        PreparedStatement ps = connection.prepareStatement(sql);

        for (int i = 1; i < 10000; i++) {
            ps.setString(1, "name" + i);
//            ps.executeUpdate();

            //1.“攒”sql
            ps.addBatch();
            if (i % 500 == 0) {
                //2.执行
                ps.executeBatch();
                //3.清空
                ps.clearBatch();
            }
        }
        connection.commit();

        long end = System.currentTimeMillis();

        System.out.println("执行花费时间为：" + (end - start));//10000条：448267
                                                              //10000条：4055
                                                              //10000条：2099

        connection.close();
        ps.close();
    }
}

```

## 事务处理

### 6.1 数据库事务介绍

- **事务：一组逻辑操作单元,使数据从一种状态变换到另一种状态。**
- **事务处理（事务操作）：**保证所有事务都作为一个工作单元来执行，即使出现了故障，都不能改变这种执行方式。当在一个事务中执行多个操作时，要么所有的事务都**被提交(commit)**，那么这些修改就永久地保存下来；要么数据库管理系统将放弃所作的所有修改，整个事务**回滚(rollback)**到最初状态。
- 为确保数据库中数据的**一致性**，数据的操纵应当是离散的成组的逻辑单元：当它全部完成时，数据的一致性可以保持，而当这个单元中的一部分操作失败，整个事务应全部视为错误，所有从起始点以后的操作应全部回退到开始状态。 

### 6.2 JDBC事务处理

- 数据一旦提交，就不可回滚。

- 数据什么时候意味着提交？

  - **当一个连接对象被创建时，默认情况下是自动提交事务**：每次执行一个 SQL 语句时，如果执行成功，就会向数据库自动提交，而不能回滚。
  - **关闭数据库连接，数据就会自动的提交。**如果多个操作，每个操作使用的是自己单独的连接，则无法保证事务。即同一个事务的多个操作必须在同一个连接下。

- **JDBC程序中为了让多个 SQL 语句作为一个事务执行：**

  - 调用 Connection 对象的 **setAutoCommit(false);** 以取消自动提交事务
  - 在所有的 SQL 语句都成功执行后，调用 **commit();** 方法提交事务
  - 在出现异常时，调用 **rollback();** 方法回滚事务

  > 若此时 Connection 没有被关闭，还可能被重复使用，则需要恢复其自动提交状态 setAutoCommit(true)。尤其是在使用数据库连接池技术时，执行close()方法前，建议恢复自动提交状态。

### 案例

【案例：用户AA向用户BB转账100】

```java
public void testJDBCTransaction() {
	Connection conn = null;
	try {
		// 1.获取数据库连接
		conn = JDBCUtils.getConnection();
		// 2.开启事务
		conn.setAutoCommit(false);
		// 3.进行数据库操作
		String sql1 = "update user_table set balance = balance - 100 where user = ?";
		update(conn, sql1, "AA");

		// 模拟网络异常
		//System.out.println(10 / 0);

		String sql2 = "update user_table set balance = balance + 100 where user = ?";
		update(conn, sql2, "BB");
		// 4.若没有异常，则提交事务
		conn.commit();
	} catch (Exception e) {
		e.printStackTrace();
		// 5.若有异常，则回滚事务
		try {
			conn.rollback();
		} catch (SQLException e1) {
			e1.printStackTrace();
		}
    } finally {
        try {
			//6.恢复每次DML操作的自动提交功能
			conn.setAutoCommit(true);
		} catch (SQLException e) {
			e.printStackTrace();
		}
        //7.关闭连接
		JDBCUtils.closeResource(conn, null, null); 
    }  
}

```

其中，对数据库操作的方法为：

```java
//使用事务以后的通用的增删改操作（version 2.0）
public void update(Connection conn ,String sql, Object... args) {
	PreparedStatement ps = null;
	try {
		// 1.获取PreparedStatement的实例 (或：预编译sql语句)
		ps = conn.prepareStatement(sql);
		// 2.填充占位符
		for (int i = 0; i < args.length; i++) {
			ps.setObject(i + 1, args[i]);
		}
		// 3.执行sql语句
		ps.execute();
	} catch (Exception e) {
		e.printStackTrace();
	} finally {
		// 4.关闭资源
		JDBCUtils.closeResource(null, ps);

	}
}
```

### 6.3 事务的ACID属性    

1. **原子性（Atomicity）**
   原子性是指事务是一个不可分割的工作单位，事务中的操作要么都发生，要么都不发生。 
2. **一致性（Consistency）**
   事务必须使数据库从一个一致性状态变换到另外一个一致性状态。
3. **隔离性（Isolation）**
   事务的隔离性是指一个事务的执行不能被其他事务干扰，即一个事务内部的操作及使用的数据对并发的其他事务是隔离的，并发执行的各个事务之间不能互相干扰。
4. **持久性（Durability）**
   持久性是指一个事务一旦被提交，它对数据库中数据的改变就是永久性的，接下来的其他操作和数据库故障不应该对其有任何影响。

#### 6.3.1 数据库的并发问题

- 对于同时运行的多个事务, 当这些事务访问数据库中相同的数据时, 如果没有采取必要的隔离机制, 就会导致各种并发问题:
  - **脏读**: 对于两个事务 T1, T2, T1 读取了已经被 T2 更新但还**没有被提交**的字段。之后, 若 T2 回滚, T1读取的内容就是临时且无效的。
  - **不可重复读**: 对于两个事务T1, T2, T1 读取了一个字段, 然后 T2 **更新**了该字段。之后, T1再次读取同一个字段, 值就不同了。
  - **幻读**: 对于两个事务T1, T2, T1 从一个表中读取了一个字段, 然后 T2 在该表中**插入**了一些新的行。之后, 如果 T1 再次读取同一个表, 就会多出几行。
- **数据库事务的隔离性**: 数据库系统必须具有隔离并发运行各个事务的能力, 使它们不会相互影响, 避免各种并发问题。
- 一个事务与其他事务隔离的程度称为隔离级别。数据库规定了多种事务隔离级别, 不同隔离级别对应不同的干扰程度, **隔离级别越高, 数据一致性就越好, 但并发性越弱。**

#### 6.3.2 四种隔离级别

- 数据库提供的4种事务隔离级别：

  ![](.\事务隔离级别.png)

- Oracle 支持的 2 种事务隔离级别：**READ COMMITED**, SERIALIZABLE。 Oracle 默认的事务隔离级别为: **READ COMMITED** 。

- Mysql 支持 4 种事务隔离级别。Mysql 6默认的事务隔离级别为: **REPEATABLE READ。**

#### 6.3.3 在MySql中设置隔离级别

- 每启动一个 mysql 程序, 就会获得一个单独的数据库连接. 每个数据库连接都有一个全局变量 @@tx_isolation, 表示当前的事务隔离级别。

- 查看当前的隔离级别: 

  ```mysql
  SELECT @@tx_isolation;
  ```

- 设置当前 mySQL 连接的隔离级别:  

  ```mysql
  set  transaction isolation level read committed;
  ```

- 设置数据库系统的全局的隔离级别:

  ```mysql
  set global transaction isolation level read committed;
  ```

- 补充操作：

  - 创建mysql数据库用户：

    ```mysql
    create user tom identified by 'abc123';
    ```

  - 授予权限

    ```mysql
    #授予通过网络方式登录的tom用户，对所有库所有表的全部权限，密码设为abc123.
    grant all privileges on *.* to tom@'%'  identified by 'abc123'; 
    
     #给tom用户使用本地命令行方式，授予atguigudb这个库下的所有表的插删改查的权限。
    grant select,insert,delete,update on atguigudb.* to tom@localhost identified by 'abc123'; 
    
    ```

    ## Dao的相关类的实现

     