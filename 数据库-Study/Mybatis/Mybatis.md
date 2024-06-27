<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240429111949062-605101945.png" alt="image-20240429111957699" width="300" />

# 简介

## 什么是MyBatis

1. MyBatis 是一款优秀的`持久层框架`
2. 它支持自定义 SQL、存储过程以及高级映射
3. MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集的过程
4. MyBatis 可以使用简单的 XML 或注解来配置和映射原生类型，将接口和 Java POJO 【Plain Old Java Objects,普通老式 Java对象】映射成数据库中的记录。
5. MyBatis 本是apache的一个开源项目ibatis, 2010年这个项目由apache 迁移到了google code，并且改名为MyBatis 。
6. 2013年11月迁移到Github .

如何获得

1. Mybatis官方文档 : https://mybatis.org/mybatis-3/zh_CN/index.html

2. GitHub : https://github.com/mybatis/mybatis-3

3. Maven仓库

   ```xml
   <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
   <dependency>
       <groupId>org.mybatis</groupId>
       <artifactId>mybatis</artifactId>
       <version>3.5.16</version>
   </dependency>
   ```

## 持久化

持久化是将程序数据在`持久状态`和`瞬时状态`间转换的机制。

1. 即把数据（如内存中的对象）保存到可永久保存的存储设备中（如磁盘）。
2. 持久化的主要应用是将内存中的对象存储在数据库中，或者存储在磁盘文件中、XML数据文件中等等。
3. JDBC就是一种持久化机制。文件IO也是一种持久化机制。

为什么需要持久化服务呢？

1. 那是由于内存本身的缺陷引起的`内存断电后数据会丢失`，但有一些对象是无论如何都不能丢失的，比如银行账号等，遗憾的是，人们还无法保证内存永不掉电。
2. 内存过于昂贵，与硬盘、光盘等外存相比，内存的价格要高2~3个数量级，而且维持成本也高，至少需要一直供电吧。所以即使对象不需要永久保存，也会因为内存的容量限制不能一直呆在内存中，需要持久化来缓存到外存。

## 持久层

什么是持久层？

完成持久化工作的代码块  ----> dao层 【DAO (Data Access Object) 数据访问对象】

1. 大多数情况下特别是企业级应用，数据持久化往往也就意味着将内存中的数据保存到磁盘上加以固化，而持久化的实现过程则大多通过各种关系数据库来完成。

2. 不过这里有一个字需要特别强调，也就是所谓的“层”。对于应用系统而言，数据持久功能大多是必不可少的组成部分。

   之所以要独立出一个“持久层”的概念,而不是“持久模块”，“持久单元”，也就意味着，我们的系统架构中，应该有一个相对独立的逻辑层面，专著于数据持久化逻辑的实现.

3. 与系统其他部分相对而言，这个层面应该具有一个较为清晰和严格的逻辑边界。 【说白了就是用来操作数据库存在的！】

## 为什么需要Mybatis

1. Mybatis就是帮助程序猿将数据存入数据库中 , 和从数据库中取数据 

2. 传统的jdbc操作 , 有很多重复代码块。

   - 数据取出时的封装
   - 数据库的建立连接等等

   通过框架可以减少重复代码,提高开发效率

3. MyBatis 是一个半自动化的ORM框架 

   (Object Relationship Mapping) -->对象关系映射

4. 所有的事情，不用Mybatis依旧可以做到，只是用了它，所有实现会更加简单！

5. MyBatis的优点

   - 简单易学：本身就很小且简单。

     没有任何第三方依赖，最简单安装只要两个jar文件+配置几个sql映射文件就可以了。

     易于学习，易于使用，通过文档和源代码，可以比较完全的掌握它的设计思路和实现。

   - 灵活：mybatis不会对应用程序或者数据库的现有设计强加任何影响。 

     `sql写在xml里`，便于统一管理和优化。通过sql语句可以满足操作数据库的所有需求。

   - 解除sql与程序代码的耦合：通过提供DAO层，将`业务逻辑`和`数据访问逻辑`分离，使系统的设计更清晰，更易维护，更易单元测试。sql和代码的分离，提高了可维护性。

   - 提供映射标签，支持对象与数据库的orm字段关系映射

   - 提供对象关系映射标签，支持对象关系组建维护

   - 提供xml标签，支持编写动态sql。

   最重要的一点，使用的人多！公司需要！

# 第一个程序

思路流程：搭建环境-->导入Mybatis--->编写代码--->测试 

## 创建流程

1. 搭建实验数据库并连接

   ```mysql
   CREATE DATABASE `mybatis`;
   USE `mybatis`;
   DROP TABLE IF EXISTS `user`;
   
   CREATE TABLE `user` (
   	`id` INT ( 20 ) NOT NULL,
   	`name` VARCHAR ( 30 ) DEFAULT NULL,
   	`pwd` VARCHAR ( 30 ) DEFAULT NULL,
   	PRIMARY KEY ( `id` ) 
   ) ENGINE = INNODB DEFAULT CHARSET = utf8;
   
   INSERT INTO `user` ( `id`, `name`, `pwd` )
   VALUES
   	( 1, '狂神', '123456' ),
   	( 2, '张三', 'abcdef' ),
   	( 3, '李四', '987654' );
   ```

2. 新建一个普通的Maven项目，删除src目录

3. 导入MyBatis相关 jar 包  记得配置文件过滤Build

   ```xml
   <dependencies>
       <!--MySQL驱动-->
       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
           <version>5.1.47</version>
       </dependency>
       <!--Mybatis-->
       <dependency>
           <groupId>org.mybatis</groupId>
           <artifactId>mybatis</artifactId>
           <version>3.5.16</version>
       </dependency>
       <!--junit-->
       <dependency>
           <groupId>junit</groupId>
           <artifactId>junit</artifactId>
           <version>4.12</version>
       </dependency>
   </dependencies>
   ```

4. 添加Maven子模块

5. 编写MyBatis核心配置文件  

   在resource下新建mybatis-config.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "https://mybatis.org/dtd/mybatis-3-config.dtd">
   <!--核心配置文件-->
   <configuration>
       <!--可以配置多套环境  default默认环境-->
       <environments default="development">
           <!--连接数据库-->
           <environment id="development">
               <!--事务管理-->
               <transactionManager type="JDBC"/>
               <dataSource type="POOLED">
                   <property name="driver" value="com.mysql.jdbc.Driver"/>
                   <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
                   <property name="username" value="root"/>
                   <property name="password" value="root"/>
               </dataSource>
           </environment>
       </environments>
       <!--每一个Mapper.xml都需要在Mybatis核心配置文件中注册-->
       <mappers>
           <mapper resource="com/sun/dao/UserMapper.xml"/>
       </mappers>
   </configuration>
   ```

6. 编写MyBatis工具类  

   ```java
   //sqlSessionFactory--> 用来构建sqlSession
   public class MybatisUtils {
       private static SqlSessionFactory sqlSessionFactory;
       static{
           try {
               //1.获取sqlSessionFactory对象
               String resource = "mybatis-config.xml";
               InputStream inputStream = Resources.getResourceAsStream(resource);
               sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
           } catch (IOException e) {
               e.printStackTrace();
           }
       }
       
       //2.从SqlSessionFactory 中获得 SqlSession 的实例。
       //SqlSession 提供了在数据库执行 SQL 命令所需的所有方法。
       public static SqlSession getSqlSession(){
           return  sqlSessionFactory.openSession();
       }
   }
   ```

7. 创建实体类  

   ```java
   public class User {
       private int id;
       private String name;
       private String pwd;
       
       public User() {}
       public User(int id, String name, String pwd) {
           this.id = id;
           this.name = name;
           this.pwd = pwd;
       }
   
       public int getId() {
           return id;
       }
   
       public void setId(int id) {
           this.id = id;
       }
   
       public String getName() {
           return name;
       }
   
       public void setName(String name) {
           this.name = name;
       }
   
       public String getPwd() {
           return pwd;
       }
   
       public void setPwd(String pwd) {
           this.pwd = pwd;
       }
   
       @Override
       public String toString() {
           return "User{" +
                   "id=" + id +
                   ", name='" + name + '\'' +
                   ", pwd='" + pwd + '\'' +
                   '}';
       }
   }
   ```

8. 编写Mapper接口类  

   ```java
   public interface UserDao {
       List<User> getUserList();
   }
   ```

9. 编写Mapper.xml配置文件- - 接口实现类

   由原来的UserDaoImpl转变为一个Mapper配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
   
   <!-- namespace=绑定一个对应的Dao/Mapper接口 -->
   <mapper namespace="com.sun.dao.UserDao">
       <!--Select查询语句-->
       <!--id为：重写UserDao的方法名-->
       <!--resultType为：结果类型，集合的泛型类型-->
       <select id="getUserList" resultType="com.sun.pojo.User">
           select * from mybatis.user
       </select>
   </mapper>
   ```

10. 编写测试类  

    尽量写在test文件夹中与在main文件夹中对应的位置

    ```java
    public class UserDaoTest {
        @Test
        public void test() {
            //1.获得sqlSession对象
            SqlSession sqlSession = MybatisUtils.getSqlSession();
    
            //2.执行SQL
    
            //方式一：getMapper【推荐】
            UserDao userDao = sqlSession.getMapper(UserDao.class);
            List<User> userList = userDao.getUserList();
    
            //方式二：【不推荐】
            List<User> objects = sqlSession.selectList("com.sun.dao.UserDao.getUserList");
            
            for (User user : userList) {
                System.out.println(user);
            }
    
            //3.关闭sqlSession
            sqlSession.close();
        }
    }
    ```

## 精简步骤

1. Maven导入Mybatis依赖
2. 编写工具类
3. 编写核心配置文件
4. 编写实体类
5. 编写Mapper接口
6. 实现Mapper接口
7. 编写测试类

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240429195520989-1530012961.png" alt="image-20240429195519680" width="250" />

## 问题说明

### 配置文件没有注册

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240429184001767-609176432.png" alt="image-20240429184000548" width="600" />

解决：每一个Mapper.xml都需要在Mybatis核心配置文件中注册

### Maven静态资源过滤问题  

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240429190242619-1569747235.png" alt="image-20240429190241171" width="600" />

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240429190331791-804524640.png" alt="image-20240429190330574" width="600" />

解决：自己写的配置文件没有被导出或不生效

```xml
<!--在build中配置resources，来防止我们资源导出失败的问题-->
<build>
    <resources>
        <resource>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>true</filtering>
        </resource>
        <resource>
            <!--使得src/main/java可以导出文件**/*.properties和**/*.xml-->
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <!--设置不去过滤-->
            <filtering>true</filtering>
        </resource>
    </resources>
</build>
```

## namespace

`配置文件中namespace中的名称为对应Mapper接口/Dao接口的完整包名,必须一致！  `

1. 将上面案例中的UserMapper接口改名为 UserDao
2. 将UserMapper.xml中的namespace改为为UserDao的路径
3. 再次测试

# CRUD操作

## select

1. select标签是mybatis中最常用的标签之一

2. select语句有很多属性可以详细配置每一条SQL语句

   - id：就是对应的namespace中的方法名

     命名空间中唯一的标识符

     接口中的方法名与映射文件中的SQL语句ID 一一对应

   - parameterType

     传入SQL语句的参数类型 。【万能的Map，可以多尝试使用】

   - resultType

     SQL语句执行的返回值类型。【完整的类名或者别名】

### 根据id查询用户

1. 在UserMapper.java接口中添加对应方法

   ```java
   public interface UserMapper {
       //根据ID查询用户
       User getUserById(int id);
   }
   ```

2. 在UserMapper.xml接口实现类中添加Select语句

   ```xml
   <mapper namespace="com.sun.dao.UserMapper">
       <select id="getUserById" parameterType="int" resultType="com.sun.pojo.User">
           select * from mybatis.user where id=#{id}
       </select>
   </mapper>
   ```

3. 在UserDaoTest.java测试类中测试

   ```java
   public class UserMapperTest {
       @Test
       public void getUserById(){
           //1.获得sqlSession对象
           SqlSession sqlSession = MybatisUtils.getSqlSession();
   
           //2.执行SQL
           UserMapper mapper = sqlSession.getMapper(UserMapper.class);
           User userById = mapper.getUserById(1);
           System.out.println(userById);
   
           //3.关闭sqlSession
           sqlSession.close();
       }
   }
   ```

### 根据密码和名字查询用户

思路一：直接在方法中传递参数  

1. 在接口方法的参数前加 @Param属性，引用对象不需要写@Param

2. Sql语句编写的时候，直接取@Param中设置的值即可，不需要单独设置参数类型

   ```java
   //通过密码和名字查询用户
   @Select("select * from mybatis.user where name=#{username} pwd=#{pwd}")
   User selectUserByNP(@Param("username") String username,@Param("pwd") String pwd);
   /*
       <select id="selectUserByNP" resultType="com.kuang.pojo.User">
       	select * from user where name = #{username} and pwd = #{pwd}
   	</select>
   */
   ```

思路二：使用万能的Map  

1. 在接口方法中，参数直接传递Map

   ```java
   User selectUserByNP2(Map<String,Object> map);
   ```

2. 编写sql语句的时候，需要传递参数类型，参数类型为map

   ```xml
   <select id="selectUserByNP2" parameterType="map" resultType="com.sun.pojo.User">
   	select * from user where name = #{username} and pwd = #{pwd}
   </select>
   ```

3. 在使用方法的时候，Map的 key 为 sql中取的值即可，没有顺序要求！

   ```java
   Map<String, Object> map = new HashMap<String, Object>();
   map.put("username","小明");
   map.put("pwd","123456");
   
   User user = mapper.selectUserByNP2(map);
   ```

总结：Map和对象

1. 关于参数数量
   - 如果参数过多，我们可以考虑直接使用Map实现或者注解
   - 如果参数比较少，直接传递参数即可
2. 关于参数传递方式
   - Map传递参数，直接在SQL中取出key【parameterType="map"】
   - 对象传递参数，直接在SQL中取出对象的属性【parameterType="Object"】【parameterType="com.sun.pojo.User"】
   - 参数只有一个基本类型的情况，可以直接在SQL中取到

## insert

我们一般使用insert标签进行插入操作，它的配置和select标签差不多！ 

`注意点：增、删、改操作需要提交事务！  `

给数据库增加一个用户  

1. 在UserMapper接口中添加对应的方法

   ```java
   //插入一个用户
   int addUser(User user);
   ```

2. 在UserMapper.xml中添加insert语句

   ```xml
   <!--对象中的属性可以直接取出来-->
   <insert id="addUser" parameterType="com.sun.pojo.User">
       insert into mybatis.user (id,name,pwd) values (#{id},#{name},#{pwd})
   </insert>
   ```

3. 测试  

   ```java
   //增删改需要提交事务
   @Test
   public void addUser(){
       //1.获得sqlSession对象
       SqlSession sqlSession = MybatisUtils.getSqlSession();
   
       //2.执行SQL
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
       int user = mapper.addUser(new User(5, "小红", "123"));
       if(user > 0){
           System.out.println("插入成功");
       }
       //3.提交事务
       sqlSession.commit();
       //4.关闭sqlSession
       sqlSession.close();
   }
   ```

## update

我们一般使用update标签进行更新操作，它的配置和select标签差不多！

修改用户的信息

1. 同理，编写接口方法

   ```java
   //修改用户
   int updateUser(User user);
   ```

2. 编写对应的配置文件SQL

   ```xml
   <update id="updateUser" parameterType="com.sun.pojo.User">
       update mybatis.user set name=#{name},pwd=#{pwd} where id=#{id};
   </update>
   ```

3. 测试

   ```java
   @Test
   public void updateUser(){
       //1.获得sqlSession对象
       SqlSession sqlSession = MybatisUtils.getSqlSession();
   
       //2.执行SQL
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
       mapper.updateUser(new User(4,"修改","456"));
   
       //3.提交事务
       sqlSession.commit();
       //4.关闭sqlSession
       sqlSession.close();
   }
   ```

## delete

我们一般使用delete标签进行删除操作，它的配置和select标签差不多！  

根据id删除一个用户

1. 同理，编写接口方法

   ```java
   //删除用户
   int deleteUser(int id);
   ```

2. 编写对应的配置文件SQL

   ```xml
   <delete id="deleteUser" parameterType="int">
       delete from mybatis.user where id=#{id};
   </delete>
   ```

3. 测试

   ```java
   @Test
   public void deleteUser(){
       //1.获得sqlSession对象
       SqlSession sqlSession = MybatisUtils.getSqlSession();
   
       //2.执行SQL
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
       mapper.deleteUser(4);
   
       //3.提交事务
       sqlSession.commit();
       //4.关闭sqlSession
       sqlSession.close();
   }
   ```

## 思考题

模糊查询like语句该怎么写?

1. 第1种：在Java代码中添加sql通配符%

   ```java
   List<User> getUserLike(String value);
   ```

   ```xml
   <mapper namespace="com.sun.dao.UserMapper">
       <select id="getUserLike" resultType="com.sun.pojo.User">
           select *
           from mybatis.user
           where name like #{value};
       </select>
   ```

   ```java
   @Test
   public void getUserLike() {
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   
       List<User> users = mapper.getUserLike("%李%");
       for (User user : users) {
           System.out.println(user);
       }
   
       sqlSession.commit();
       sqlSession.close();
   }
   ```

2. 第2种：在sql语句中拼接通配符，会引起sql注入

   ```xml
   <select id="getUserLike" resultType="com.sun.pojo.User">
       select *
       from mybatis.user
       where name like #{value}"%"
   </select>
   ```

   ```java
   List<User> users = mapper.getUserLike("李");
   ```

## 小结

1. 所有的`增删改`操作都需要`提交事务`！
2. 接口所有的普通参数，尽量都写上@Param参数，尤其是多个参数时，必须写上！
3. 有时候根据业务的需求，可以考虑使用map传递参数！
4. 为了规范操作，在SQL的配置文件中，我们尽量将Parameter参数和resultType都写上！

# 配置解析

## 核心配置文件

1. mybatis-config.xml 系统核心配置文件

2. MyBatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息。

3. 能配置的内容如下：

   ```xml
   configuration（配置）
       properties（属性）
       settings（设置）
       typeAliases（类型别名）
       typeHandlers（类型处理器）不重要
       objectFactory（对象工厂）不重要
       plugins（插件）
       environments（环境配置）
           environment（环境变量）
               transactionManager（事务管理器）
               dataSource（数据源）
       databaseIdProvider（数据库厂商标识）
       mappers（映射器）
   <!-- 注意元素节点的顺序！顺序不对会报错 -->
   ```

4. 主要看：typeAliases（类型别名）、typeAliases（类型别名）外部引入、mappers（映射器）

## 属性-Properties

可以通过Properties属性来实现引用配置。

```xml
<dataSource type="POOLED">
  <property name="driver" value="${driver}"/>
  <property name="url" value="${url}"/>
  <property name="username" value="${username}"/>
  <property name="password" value="${password}"/>
</dataSource>

<properties resource="org/mybatis/example/config.properties">
  <property name="username" value="dev_user"/>
  <property name="password" value="F2Fa3!33TYyg"/>
</properties>
```

这些属性可以在外部进行配置，并可以进行动态替换。你既可以在典型的 Java 属性文件中配置这些属性，也可以在 properties 元素的子元素中设置

我们来优化我们的配置文件

第一步 ：在资源目录下新建一个db.properties

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?useSSL=true&useUnicode=true&characterEncoding=UTF-8
username=root
password=root
```

第二步 : 在核心配置文件中引入该文件

```xml
<!--1.引入外部配置文件-->
<!--db.properties在resource目录下可以不用写路径-->
<properties resource="db.properties"/>	<!--无内容，自闭合-->

<!--可在内部添加属性配置-->
<properties resource="db.properties">
    <property name="password" value="root"/>
</properties>

<!--同一字段，优先使用外部配置文件的，内部添加的被覆盖-->

<!--2.设置好的属性可以在整个配置文件中用来替换需要动态配置的属性值-->
<environments default="test">
    <environment id="test">
        <transactionManager type="JDBC"/>
        <dataSource type="POOLED">
            <!--新特性：使用占位符-->
            <property name="driver" value="${driver}"/>
            <property name="url"  value="${url}"/>
            <property name="username" value="${username}"/>
            <property name="password" value="${password}"/>
        </dataSource>
    </environment>
</environments>
```

## 设置-settings

一个配置完整的 settings 元素的示例如下：

- 缓存开启

  ```xml
  <!--显示的开启全局缓存 默认开启-->
  <setting name="cacheEnabled" value="true"/>
  ```

- 输出日志

  ```xml
  <setting name="logImpl" value="LOG4J | STDOUT_LOGGING"/>
  ```

- 开启自动驼峰命名规则映射

  ```xml
  <setting name="mapUnderscoreToCamelCase" value="true"/>
  ```

```xml
<settings>
    <!--缓存开启关闭-->
    <setting name="cacheEnabled" value="true"/>
    <!--懒加载-->
    <setting name="lazyLoadingEnabled" value="true"/>
    <setting name="multipleResultSetsEnabled" value="true"/>
    <setting name="useColumnLabel" value="true"/>
    <setting name="useGeneratedKeys" value="false"/>
    <setting name="autoMappingBehavior" value="PARTIAL"/>
    <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
    <setting name="defaultExecutorType" value="SIMPLE"/>
    <setting name="defaultStatementTimeout" value="25"/>
    <setting name="defaultFetchSize" value="100"/>
    <setting name="safeRowBoundsEnabled" value="false"/>
    <!--是否开启自动驼峰命名规则映射 last_name lastName-->
    <setting name="mapUnderscoreToCamelCase" value="false"/>
    <setting name="localCacheScope" value="SESSION"/>
    <setting name="jdbcTypeForNull" value="OTHER"/>
    <setting name="lazyLoadTriggerMethods"
    value="equals,clone,hashCode,toString"/>
    <setting name="defaultScriptingLanguage" value="org.apache.ibatis.scripting.xmltags.XMLLanguageDriver"/>
  	<setting name="defaultEnumTypeHandler" value="org.apache.ibatis.type.EnumTypeHandler"/>
      <setting name="callSettersOnNulls" value="false"/>
      <setting name="returnInstanceForEmptyRow" value="false"/>
      <setting name="logPrefix" value="exampleLogPreFix_"/>
      <!--指定Mybatis所用日志的具体实现，未指定时自动查找-->
      <setting name="logImpl" value="SLF4J | LOG4J | LOG4J2 | JDK_LOGGING | COMMONS_LOGGING | STDOUT_LOGGING | NO_LOGGING"/>
      <setting name="proxyFactory" value="CGLIB | JAVASSIST"/>
      <setting name="vfsImpl" value="org.mybatis.example.YourselfVfsImpl"/>
      <setting name="useActualParamName" value="true"/>
      <setting name="configurationFactory" value="org.mybatis.example.ConfigurationFactory"/>
</settings>
```

### 日志工厂- logImpl

在测试SQL的时候，要是在控制台输出 SQL 的话，能够有更快的排错效率

对于以往的开发过程，我们会经常使用到debug模式来调节，跟踪我们的代码执行过程。但是现在使用Mybatis是基于接口，配置文件的源代码执行过程。因此，我们必须选择日志工具来作为我们开发，调节程序的工具。

Mybatis内置的日志工厂logImpl 提供日志功能，指定 MyBatis 所用日志的具体实现，未指定时将自动查找，会使用最先找到的（按上文列举的顺序查找）。如果一个都未找到，日志功能就会被禁用。

具体的日志实现有以下几种工具：

- SLF4J
- LOG4J（3.5.9 起废弃）【掌握】
- Log4j 2
- JDK logging
- Apache Commons Logging
- STDOUT_LOGGING【掌握】控制台输出，标准的日志工厂实现
- NO_LOGGING 没有日志输出

#### STDOUT_LOGGING

```xml
<settings>
	<setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240430173901857-768556983.png" alt="image-20240430173900937" width="500" />

#### Log4j

1. Log4j是Apache的一个开源项目。通过使用Log4j，我们可以控制日志信息输送的目的地是控制台、文件、GUI组件....
2. 我们也可以控制每一条日志的输出格式；
3. 通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程。
4. 这些可以通过一个配置文件来灵活地进行配置，而不需要修改应用的代码。

使用步骤：

1. 导入log4j的包

   ```xml
   <!-- https://mvnrepository.com/artifact/log4j/log4j -->
   <dependency>
       <groupId>log4j</groupId>
       <artifactId>log4j</artifactId>
       <version>1.2.17</version>
   </dependency>
   ```

2. 配置文件编写

   ```properties
   #将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码
   log4j.rootLogger=DEBUG,console,file
   
   #控制台输出的相关设置
   log4j.appender.console = org.apache.log4j.ConsoleAppender
   log4j.appender.console.Target = System.out
   log4j.appender.console.Threshold=DEBUG
   log4j.appender.console.layout = org.apache.log4j.PatternLayout
   log4j.appender.console.layout.ConversionPattern=[%c]-%m%n
   
   #文件输出的相关设置
   log4j.appender.file = org.apache.log4j.RollingFileAppender
   log4j.appender.file.File=./log/sun.log
   log4j.appender.file.MaxFileSize=10mb
   log4j.appender.file.Threshold=DEBUG
   log4j.appender.file.layout=org.apache.log4j.PatternLayout
   log4j.appender.file.layout.ConversionPattern=[%p][%d{yy-MM-dd}][%c]%m%n
   
   #日志输出级别
   log4j.logger.org.mybatis=DEBUG
   log4j.logger.java.sql=DEBUG
   log4j.logger.java.sql.Statement=DEBUG
   log4j.logger.java.sql.ResultSet=DEBUG
   log4j.logger.java.sql.PreparedStatement=DEBUG
   ```

3. setting设置日志实现

   ```xml
   <settings>
       <setting name="logImpl" value="LOG4J"/>
   </settings>
   ```

4. 在程序中使用Log4j进行输出！

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240430180927109-1040095706.png" alt="image-20240430180926200" width="700" />

5. 简单使用

   - 在要使用Log4j的类中，导入包

     ```java
      import org.apache.log4j.Logger;
     ```

   - 定义属性，参数为当前类的class

     ```java
     static Logger logger = Logger.getLogger(UserMapperTest.class);
     ```

   - 在相应的方法中使用logger

     ```java
     public class UserMapperTest {
         static Logger logger = Logger.getLogger(UserMapperTest.class);
         @Test
         public void testLog4j() {
             logger.info("info:进入了log4j方法");
             logger.debug("debug:进入了log4j方法");
             logger.error("error:进入了log4j方法");
         }
     }
     ```

   - 控制台输出

     <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240430182449924-1529438129.png" alt="image-20240430182448940" width="300" />

   - 也可在生成的文件中查看

     <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240430182408898-1607080401.png" alt="image-20240430182407765" width="600" />

## 类型别名-typeAliases

类型别名可为 Java 类型设置一个缩写名字。 它仅用于 XML 配置，意在降低冗余的全限定类名书写。

方式一：typeAilas

当这样配置时， User 可以用在任何使用 com.kuang.pojo.User 的地方。

```xml
<!--可以给实体类起别名-->
<typeAliases>
    <typeAlias type="com.sun.pojo.User" alias="User"/>
</typeAliases>
```

方式二：package

也可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean，比如:

```xml
<typeAliases>
    <package name="com.sun.pojo"/>
</typeAliases>
```

- 每一个在包 com.kuang.pojo 中的 Java Bean，在没有注解的情况下，会使用 Bean 的首字母小写的非限定类名来作为它的别名。例如上述com.sun.pojo包下的User实体类的别名会变成user


- 若有注解，则别名为其注解值。例如下面就会使用author作为别名

  ```java
  import org.apache.ibatis.type.Alias;
  @Alias("author")
  public class Author {
      ...
  }
  ```

两种方式的区别：

1. 实体类较少的时候，使用第一种方式；实体类十分多，建议使用第二种方式。
2. 第一种可以自定义别名；第二种则不行。

常见的 Java 类型内建的类型别名

它们都是不区分大小写的。注意，为了应对原始类型的命名重复，采取了特殊的命名风格。

| _byte                     | byte         |
| ------------------------- | ------------ |
| _char (since 3.5.10)      | char         |
| _character (since 3.5.10) | char         |
| _long                     | long         |
| _short                    | short        |
| _int                      | int          |
| _integer                  | int          |
| _double                   | double       |
| _float                    | float        |
| _boolean                  | boolean      |
| string                    | String       |
| byte                      | Byte         |
| char (since 3.5.10)       | Character    |
| character (since 3.5.10)  | Character    |
| long                      | Long         |
| short                     | Short        |
| int                       | Integer      |
| integer                   | Integer      |
| double                    | Double       |
| float                     | Float        |
| boolean                   | Boolean      |
| date                      | Date         |
| decimal                   | BigDecimal   |
| bigdecimal                | BigDecimal   |
| biginteger                | BigInteger   |
| object                    | Object       |
| date[]                    | Date[]       |
| decimal[]                 | BigDecimal[] |
| bigdecimal[]              | BigDecimal[] |
| biginteger[]              | BigInteger[] |
| object[]                  | Object[]     |
| map                       | Map          |
| hashmap                   | HashMap      |
| list                      | List         |
| arraylist                 | ArrayList    |
| collection                | Collection   |
| iterator                  | Iterator     |

## 类型处理器-typeHandlers

1. 无论是 MyBatis 在预处理语句（PreparedStatement）中设置一个参数时，还是从结果集中取出一个值时， 都会用类型处理器将获取的值以合适的方式转换成 Java 类型。
2. 你可以重写类型处理器或创建你自己的类型处理器来处理不支持的或非标准的类型。【了解即可】

## 对象工厂-ObjectFactory

1. MyBatis 每次创建结果对象的新实例时，它都会使用一个对象工厂（ObjectFactory）实例来完成。
2. 默认的对象工厂需要做的仅仅是实例化目标类，要么通过默认构造方法，要么在参数映射存在的时候通过有参构造方法来实例化。
3. 如果想覆盖对象工厂的默认行为，则可以通过创建自己的对象工厂来实现。【了解即可】

## 插件-plugins

- mybatis-plus-boot-starter - - -自动识别数据库生成Mybatis代码，会有问题
- mybatis-plus - - -和Mybatis互补，提高效率的
- 通用Mapper

## 环境配置-environments

```xml
<environments default="development">
  <environment id="development">
    <transactionManager type="JDBC">
      <property name="..." value="..."/>
    </transactionManager>
    <dataSource type="POOLED">
      <property name="driver" value="${driver}"/>
      <property name="url" value="${url}"/>
      <property name="username" value="${username}"/>
      <property name="password" value="${password}"/>
    </dataSource>
  </environment>
</environments>
```

- MyBatis 可以配置成适应多种环境
- **尽管可以配置多个环境，但每个 SqlSessionFactory实例只能选择一种环境。**

配置MyBatis的多套运行环境，将SQL映射到多个不同的数据库上，必须指定其中一个为默认运行环境（通过default指定）

子元素节点：environment

具体的一套环境，通过设置id进行区别，id保证唯一！

1. 子元素节点：transactionManager - [ 事务管理器 ]

   ```xml
   <!-- 语法 -->
   <transactionManager type="[ JDBC | MANAGED ]"/>
   ```

   - JDBC – 这个配置直接使用了 JDBC 的提交和回滚功能，它依赖从数据源获得的连接来管理事务作用域。默认情况下它在关闭连接时启用自动提交

   - MANAGED – 这个配置几乎没做什么。它从不提交或回滚一个连接，而是让容器来管理事务的整个生命周期。默认情况下它会关闭连接

   - 这两种事务管理器类型都不需要设置任何属性。

2. 子元素节点：数据源（dataSource）

   - dataSource 元素使用标准的 JDBC 数据源接口来配置 JDBC 连接对象的资源。

   - 数据源是必须配置的。连接数据库：dbcp、c3p0、druid

   - 有三种内建的数据源类型

     ```java
     type="[UNPOOLED|POOLED|JNDI]"）
     ```

   - unpooled： 这个数据源的实现只是每次被请求时打开和关闭连接。

   - pooled【默认】： 这种数据源的实现利用`“池”`的概念将 JDBC 连接对象组织起来 , 这是一种`使得并发 Web 应用快速响应请求`的流行处理方式。

   - jndi：这个数据源的实现是为了能在如 Spring 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置一个 JNDI 上下文的引用。

   数据源也有很多第三方的实现，比如dbcp，c3p0，druid等等....

## 映射器-mappers

### mappers

1. 映射器 : 定义映射SQL语句文件
2. 既然 MyBatis 的行为其他元素已经配置完了，我们现在就要定义 SQL 映射语句了。但是首先我们需要告诉 MyBatis 到哪里去找到这些语句。 Java 在自动查找这方面没有提供一个很好的方法，所以最佳的方式是告诉 MyBatis 到哪里去找映射文件。你可以使用相对于类路径的资源引用， 或完全限定资源定位符（包括 file:/// 的 URL），或类名和包名等。
3. 映射器是MyBatis中最核心的组件之一，在MyBatis 3之前，只支持xml映射器，即：所有的SQL语句都必须在xml文件中配置。而从MyBatis 3开始，还支持接口映射器，这种映射器方式允许以Java代码的方式注解定义SQL语句，非常简洁。

### 引入资源方式

1. 使用相对于类路径的资源引用 

   ```xml
   <mappers>
       <mapper resource="com/sun/dao/UserMapper.xml"/>
       <!--通配注册 spring支持-->
       <mapper resource="com/sun/dao/*.xml"/>
   </mappers>
   ```

2. 使用映射器接口实现类的完全限定类名

   - 需要配置文件名称和接口名称一致
   - 并且位于同一目录下

   ```xml
   <mappers>
   	<mapper class="com.sun.dao.UserMapper"/>
   </mappers>
   ```

3. 将包内的映射器接口实现全部注册为映射器

   - 需要配置文件名称和接口名称一致
   - 并且位于同一目录下

   ```xml
   <mappers>
   	<package name="com.sun.dao"/>
   </mappers>
   ```

4. 使用完全限定资源定位符（URL）【不看】

   ```xml
   <mappers>
   	<mapper url="file:///var/mappers/AuthorMapper.xml"/>
   </mappers>
   ```

### Mapper配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.kuang.mapper.UserMapper">
</mapper>
```

namespace中文意思：命名空间，作用如下：

1. namespace和子元素的id联合保证唯一 , 区别不同的mapper
2. 绑定DAO接口namespace的命名必须跟某个接口同名接口中的方法与映射文件中sql语句id应该一一对应
3. namespace命名规则 : 包名+类名

MyBatis 的真正强大在于它的映射语句，这是它的魔力所在。

- 由于它的异常强大，映射器的 XML 文件就显得相对简单。
- 如果拿它跟具有相同功能的 JDBC 代码进行对比，你会立即发现省掉了将近 95% 的代码。
- MyBatis 为聚焦于 SQL 而构建，以尽可能地为你减少麻烦。

## 生命周期和作用域

作用域和生命周期类是至关重要的，因为错误的使用会导致非常严重的`并发问题`

### Mybatis的执行过程

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240430161529123-386397311.png" alt="image-20240430161524421" width="400" />

### 作用域

SqlSessionFactoryBuilder 

```java
//获得SqlSessionFactory对象
sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
```

1. 这个类可以被实例化、使用和丢弃。SqlSessionFactoryBuilder 的作用在于创建 SqlSessionFactory，创建成功后，SqlSessionFactoryBuilder 就失去了作用，所以它只能存在于创建 SqlSessionFactory 的方法中，而不要让其长期存在。
2. 因此 SqlSessionFactoryBuilder 实例的最佳作用域是`方法作用域（也就是局部方法变量）`。`一旦创建了就不再需要`它了。
3. 你可以重用  SqlSessionFactoryBuilder 来创建多个 SqlSessionFactory 实例，但最好还是不要一直保留着它，以保证所有的 XML 解析资源可以被释放给更重要的事情。

SqlSessionFactory

```java
//获得 SqlSession 的实例。
sqlSessionFactory.openSession();
```

1. SqlSessionFactory `一旦被创建就应该在应用的运行期间一直存在`，没有任何理由丢弃它或重新创建另一个实例。 使用  SqlSessionFactory 的最佳实践是在应用运行期间不要重复创建多次，多次重建 SqlSessionFactory  被视为一种代码“坏味道”。
2. 因此 SqlSessionFactory 的最佳作用域是`应用作用域`。有很多方法可以做到，最简单的就是使用单例模式或者静态单例模式。
3. SqlSessionFactory 可以被认为是一个`数据库连接池`，它的`作用是创建 SqlSession 接口对象`。因为MyBatis 的本质就是 Java 对数据库的操作，所以 SqlSessionFactory 的生命周期存在于整个MyBatis 的应用之中，所以一旦创建了 SqlSessionFactory，就要长期保存它，直至不再使用MyBatis 应用，所以可以认为 SqlSessionFactory 的生命周期就等同于 MyBatis 的应用周期。
4. 由于 SqlSessionFactory 是一个对数据库的连接池，所以它占据着数据库的连接资源。如果创建多个 SqlSessionFactory，那么就存在多个数据库连接池，这样不利于对数据库资源的控制，也会导致数据库连接资源被消耗光，出现系统宕机等情况，所以尽量避免发生这样的情况。
5. 因此在一般的应用中我们往往希望 SqlSessionFactory 作为一个单例，让它在应用中被共享。所以说 SqlSessionFactory 的最佳作用域是应用作用域。
6. 如果说 SqlSessionFactory 相当于数据库连接池，那么 SqlSession 就相当于一个数据库连接（Connection 对象），你可以在一个事务里面执行多条 SQL，然后通过它的 commit、rollback等方法，提交或者回滚事务。所以它应该存活在一个业务请求中，处理完整个请求后，应该关闭这条连接，让它归还给 SqlSessionFactory，否则数据库资源就很快被耗费精光，系统就会瘫痪，所以用 try...catch...finally... 语句来保证其正确关闭。

SqlSession

```java
//执行SQL
UserDao userDao = sqlSession.getMapper(UserDao.class);
```

1. 每个线程都应该有它自己的 SqlSession 实例。SqlSession  的实例不是线程安全的，因此是不能被共享的，所以它的最佳的作用域是`请求或方法作用域`。 连接到连接池的一个请求，用完之后需要赶紧关闭，否则资源被占用！
2. 换句话说，每次收到 HTTP 请求，就可以打开一个 SqlSession，返回一个响应后，就关闭它。  这个关闭操作很重要，为了确保每次都能执行关闭操作，你应该把这个关闭操作放到 finally 块中。 

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240430163510475-1470002987.png" alt="image-20240430163503936" width="500" />

# ResultMap

`要解决的问题：属性名和字段名不一致  `

## 查询为null问题

1. 查看之前的数据库的字段名

   <img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240627192447775-903572823.png" alt="image-20240430164033920" width="300" />

2. Java中的实体类设计，属性设置不一样的字段名

   ```java
   public class User {
       private int id;
       private String name;
       private String password;
   }
   ```

3. 接口

   ```java
   public interface UserMapper {
       //根据ID查询用户
       User getUserById(int id);
   }
   ```

4. mapper映射文件

   ```xml
   <mapper namespace="com.sun.dao.UserMapper">
       <select id="getUserById" parameterType="int" resultType="com.sun.pojo.User">
           select *
           from mybatis.user
           where id = #{id};
       </select>
   </mapper>
   ```

5. 测试

   ```java
   public class UserMapperTest {
       @Test
       public void getUserById() {
           //1.获得sqlSession对象
           SqlSession sqlSession = MybatisUtils.getSqlSession();
   
           UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
           User user = userMapper.getUserById(1);
           System.out.println(user);
   
           sqlSession.close();
       }
   }
   ```

结果:<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240430164714725-1630621341.png" alt="image-20240430164713986" width="300"/>

分析：

```sql
select * from mybatis.user where id = #{id};
#等价于
select id,name,pwd from user where id = #{id}
```

- mybatis会根据这些查询的列名(会将列名转化为小写,数据库不区分大小写)
- 去对应的实体类中查找相应列名的set方法设值
- 由于找不到setPwd() , 所以password返回null ; 【自动映射】

## 解决方案

方案一：为列名指定别名 , 别名和java实体类的属性名一致 

```sql
<select id="getUserById" parameterType="int" resultType="com.sun.pojo.User">
    select id, name, pwd as password
    from mybatis.user
    where id = #{id};
</select>
```

方案二：使用结果集映射->ResultMap 【推荐】

```xml
<mapper namespace="com.sun.dao.UserMapper">
    <!--resultMap id为select中resultMap的名字-->
    <resultMap id="UserMap" type="com.sun.pojo.User">
        <!--column：数据库中的字段  property：实体类中的属性-->
        <result column="id" property="id"/>
        <result column="name" property="name"/>
        <result column="pwd" property="password"/>
    </resultMap>
    <select id="getUserById" parameterType="int" resultMap="UserMap">
        select *
        from mybatis.user
        where id = #{id};
    </select>
</mapper>
```

## ResuleMap

结果集映射

### 自动映射

1. resultMap 元素是 MyBatis 中最重要最强大的元素。它可以让你从 90% 的 JDBCResultSets 数据提取代码中解放出来。

2. 实际上，在为一些比较复杂的连接语句编写映射代码的时候，一份 resultMap 能够代替实现同等功能的长达数千行的代码。

3. ResultMap 的设计思想是：对于简单的语句根本不需要配置显式的结果映射，而对于复杂一点的语句只需要描述它们的关系就行了。你已经见过简单映射语句的示例了，但并没有显式指定 resultMap 。比如：

   ```xml
   <select id="selectUsers" resultType="map">
     select id, username, hashedPassword
     from some_table
     where id = #{id}
   </select>
   ```

上述语句只是简单地将所有的`列映射到 HashMap 的键上`，这由 resultType 属性指定。虽然在大部分情况下都够用，但是 HashMap 不是一个很好的模型。你的程序更可能会使用 JavaBean 或POJO（Plain Old Java Objects，普通老式 Java 对象）作为模型。

### 手动映射

1. 返回值类型为resultMap

   ```xml
   <select id="getUserById" parameterType="int" resultMap="UserMap">
       select *
       from mybatis.user
       where id = #{id};
   </select>
   ```

2. 编写resultMap，实现手动映射！  

   ```xml
   <resultMap id="UserMap" type="com.sun.pojo.User">
       <!--column：数据库中的字段  property：实体类中的属性-->
       <result column="pwd" property="password"/>
   </resultMap>
   ```

如果世界总是这么简单就好了，但是肯定不是的。

数据库中，存在一对多，多对一的情况，我们之后会使用到一些高级的结果集映射，association，collection这些

# 分页的实现

## limit实现分页

思考：为什么需要分页？

在学习mybatis等持久层框架的时候，会经常对数据进行增删改查操作，使用最多的是对数据库进行查询操作，如果查询大量数据的时候，我们往往使用分页进行查询，也就是每次处理小部分数据，这样对数据库压力就在可控范围内。

使用Limit实现分页

```properties
#语法
SELECT * FROM table LIMIT stratIndex，pageSize

# 检索记录行 6-15
SELECT * FROM table LIMIT 5,10; 

#如果只给定一个参数，它表示返回最大的记录行数目：检索前 5 个记录行
SELECT * FROM table LIMIT 5;
#换句话说，LIMIT n 等价于 LIMIT 0,n。
```

## 使用Mybatis实现分页

使用万能的Map，步骤：

1. 修改Mapper接口

   ```java
   //分页
   List<User> getUserByLimit(Map<String, Integer> map);
   ```

2. Mapper接口实现，参数为map

   ```xml
   <select id="getUserByLimit" parameterType="map" resultType="com.sun.pojo.User">
       select *
       from mybatis.user
       limit #{startIndex},#{pageSize}
   </select>
   ```

3. 在测试类中传入参数测试推断：起始位置 = （当前页面 - 1 ） * 页面大小

   ```java
   @Test
   public void getUserByLimit() {
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   
       Map<String, Integer> map = new HashMap<>();
       map.put("startIndex", 3);
       map.put("pageSize", 2);
   
       List<User> userByLimit = mapper.getUserByLimit(map);
       System.out.println(userByLimit);
   
       sqlSession.close();
   }
   ```

## RowBounds分页

我们除了使用Limit在SQL层面实现分页，也可以使用RowBounds在Java代码层面实现分页，当然此种方式作为了解即可。我们来看下如何实现的！

步骤：

1. mapper接口

   ```java
   List<User> getUserByRowBound();
   ```

2. mapper接口实现

   ```xml
   <select id="getUserByRowBound" resultMap="UserMap">
       select *
       from mybatis.user
   </select>
   ```

3. 测试类在这里，我们需要使用RowBounds类

   ```java
   @Test
   public void getUserByRowBound() {
       SqlSession sqlSession = MybatisUtils.getSqlSession();
   
       //RowBounds实现
       RowBounds rowBounds = new RowBounds(1, 2);
   
       List<User> userList = sqlSession.selectList("com.sun.dao.UserMapper.getUserByRowBound", null, rowBounds);
   
       for (User user : userList) {
           System.out.println(user);
       }
   
       sqlSession.close();
   }
   ```

## PageHelper

分页插件

官方文档：https://pagehelper.github.io/  

# 使用注解开发

## 面向接口编程

1. 大家之前都学过面向对象编程，也学习过接口，但在真正的开发中，很多时候我们会选择面向接口编程
2. 根本原因 : `解耦` , 可拓展 , 提高复用 , 分层开发中 , 上层不用管具体的实现 , 大家都遵守共同的标准, 使得开发变得容易 , 规范性更好
3. 在一个面向对象的系统中，系统的各种功能是由许许多多的不同对象协作完成的。在这种情况下，各个对象内部是如何实现自己的,对系统设计人员来讲就不那么重要了；
4. 而各个对象之间的协作关系则成为系统设计的关键。小到不同类之间的通信，大到各模块之间的交互，在系统设计之初都是要着重考虑的，这也是系统设计的主要工作内容。面向接口编程就是指按照这种思想来编程。

关于接口的理解

1. 接口从更深层次的理解，应是`定义`（规范，约束）与`实现`（名实分离的原则）的分离。
2. 接口的本身反映了系统设计人员对系统的抽象理解。
3. 接口应有两类：
   - 第一类是：对一个个体的抽象，它可对应为一个抽象体(abstract class)；
   - 第二类是：对一个个体某一方面的抽象，即形成一个抽象面（interface）；
4. 一个体有可能有多个抽象面。抽象体与抽象面是有区别的。

三个面向区别

1. 面向对象是指，我们考虑问题时，以对象为单位，考虑它的属性及方法 .
2. 面向过程是指，我们考虑问题时，以一个具体的流程（事务过程）为单位，考虑它的实现 .
3. 接口设计与非接口设计是针对复用技术而言的，与面向对象（过程）不是一个问题。更多的体现就是对系统整体的架构

## 利用注解开发

mybatis最初配置信息是基于 XML ,映射语句(SQL)也是定义在 XML 中的。而到MyBatis 3提供了新的基于注解的配置。不幸的是，Java 注解的的表达力和灵活性十分有限。最强大的 MyBatis 映射并不能用注解来构建

sql 类型主要分成 :

- @select ()
- @update ()
- @Insert ()
- @delete ()

【注意】利用注解开发就不需要mapper.xml映射文件了

1. 我们在我们的接口中添加注解

   ```java
   public interface UserMapper {
       @Select("select * from mybatis.user")
       List<User> getUsers();
   }
   ```

2. 在mybatis的核心配置文件中注入

   ```xml
   <!--绑定接口-->
   <mappers>
       <mapper class="com.sun.dao.UserMapper"/>
   </mappers>
   ```

3. 我们去进行测试

   ```java
   public class UserMapperTest {
       @Test
       public void getUsers() {
           SqlSession sqlSession = MybatisUtils.getSqlSession();
           
           //底层主要应用反射
           UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   
           List<User> users = mapper.getUsers();
           for (User user : users) {
               System.out.println(user);
           }
           sqlSession.close();
       }
   }
   ```

4. 结果-注解只能完成简单的操作

   ```java
   User{id=1, name='狂神', password='null'}
   User{id=2, name='张三', password='null'}
   User{id=3, name='李四', password='null'}
   User{id=4, name='李武', password='null'}
   ```

5. 利用Debug查看本质

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240501110940177-1071898378.png" alt="image-20240501110939729" width="500" />

6. 本质上利用了jvm的动态代理机制

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240501111415157-1474957633.png" alt="image-20240501111414954" width="400" />

7. Mybatis详细的执行流程

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240501113832598-1983244667.jpg" alt="未命名文件" width="500" />

## 注解CRUD

改造MybatisUtils工具类的getSession( ) 方法，重载实现

【增删改时不需要再写SqlSession.commit()】 【记得绑定Mapper接口到核心配置类中】

```java
//2.获得 SqlSession 的实例。 使得事务可以自动提交。
public static SqlSession getSqlSession() {
    return sqlSessionFactory.openSession(true);
}
```

### 查询

【注意点：增删改一定记得对事务的处理】

1. 编写接口方法注解

   ```java
   @Select("select * from mybatis.user where id=#{id}")
   User getUserById(@Param("id") int id);
   ```

2. 测试

   ```java
   @Test
   public void getUserById() {
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   
       User user = mapper.getUserById(1);
       System.out.println(user);
   
       sqlSession.close();
   }
   ```

### 新增

1. 编写接口方法注解

   ```java
   @Insert("insert into mybatis.user(id, name, pwd) VALUES (#{id},#{name},#{password})")
   int addUser(User user);
   ```

2. 测试

   ```java
   @Test
   public void addUser() {
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   
       User user = new User(5, "hello0", "1111");
       mapper.addUser(user);
   
       sqlSession.close();
   }
   ```

### 修改

1. 编写接口方法注解

   ```java
   @Update("update mybatis.user set name=#{name},pwd=#{password} where id=#{id}")
   int updateUser(User user);
   ```

2. 测试

   ```java
   @Test
   public void updateUser() {
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   
       mapper.updateUser(new User(5, "hello1", "2222"));
   
       sqlSession.close();
   }
   ```

### 删除

1. 编写接口方法注解

   ```java
   @Delete("delete from mybatis.user where id=#{uid}")
   int deleteUserById(@Param("uid") int id);
   ```

2. 测试

   ```java
   @Test
   public void deleteUser() {
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   
       mapper.deleteUserById(5);
   
       sqlSession.close();
   }
   ```

## 关于@Param()注解

@Param注解用于给方法参数起一个名字。

以下是总结的使用原则：

1. 在方法只接受一个基本类型参数的情况下，可以不使用@Param。
2. 在方法接受多个参数的情况下，建议一定要使用@Param注解给参数命名。
3. 基本类型的参数或者String类型，需要加上；引用类型，不需要加。
4. 在SQL中引用的就是@Param(“uid”)中设定的属性名
5. 如果参数是 JavaBean ， 则不能使用@Param。
6. 不使用@Param注解时，参数只能有一个，并且是Javabean。

## #与 $ 的区别

1. \#{} 的作用主要是替换预编译语句(PrepareStatement)中的占位符? 【推荐使用】

   能够在很大程度上防止SQL注入

   ```java
   INSERT INTO user (name) VALUES (#{name});
   INSERT INTO user (name) VALUES (?);
   ```

2. ${} 的作用是直接进行字符串替换

   传入的数据会直接显示在生成的SQL中，无法防止SQL注入

   ```java
   INSERT INTO user (name) VALUES ('${name}');
   INSERT INTO user (name) VALUES ('kuangshen');
   ```

# Lombok

## 简介

- Project Lombok is a java library - - -Java库

- that automatically plugs into your editor and build tools, spicing up your java. - - -编辑和构建工具的自动化插件

- Never write another getter or equals method again, with one  annotation your class has a fully featured builder

  不需要再写getter/setter或equals方法，而是用一个注释 

- Automate your  logging variables, and much more. 	

## Lombok的使用

1. IDEA安装Lombok插件

   A plugin that adds first-class support for Project Lombok Features

   - @Getter and @Setter	 `get、set`
   - @FieldNameConstants
   - @ToString
   - @EqualsAndHashCode
   - @AllArgsConstructor,`有参构造` @RequiredArgsConstructor and @NoArgsConstructor `无参构造`
   - @Data   `无参构造、get、set、equals、hashcode、toString`
   - @Accessors

   @Log, @Log4j, @Log4j2, @Slf4j, @XSlf4j, @CommonsLog, @JBossLog, @Flogger, @CustomLog

   @Builder，@SuperBuilder，@Singular,@Jacksonized，@Delegate，@Value@Tolerate

   @Wither@With@SneakyThrows@StandardException@val@var@experimental @var@UtilityClass

   

2. 引入Maven依赖

   ```xml
   <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
   <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
       <version>1.18.32</version>
       <scope>provided</scope>
   </dependency>
   ```

3. 在实体类代码中增加注解

   ```java
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class User {
       @Getter and @Setter
       private int id;
       private String name;
       private String password;
   }
   ```

# 多对一的处理

## 数据库设计

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240501133955068-922136424.png" alt="image-20240501133954505" width="100" />

```sql
CREATE TABLE `teacher` (
    `id` INT(10) NOT NULL,
    `name` VARCHAR(30) DEFAULT NULL,
    PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8;

INSERT INTO teacher(`id`, `name`) VALUES (1, '秦老师');

CREATE TABLE `student` (
    `id` INT(10) NOT NULL,
    `name` VARCHAR(30) DEFAULT NULL,
    `tid` INT(10) DEFAULT NULL,
    PRIMARY KEY (`id`),
    KEY `fktid` (`tid`),
    CONSTRAINT `fktid` FOREIGN KEY (`tid`) REFERENCES `teacher` (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8;

INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('1', '小明', '1');
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('2', '小红', '1');
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('3', '小张', '1');
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('4', '小李', '1');
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('5', '小王', '1');
```

## 项目搭建

1. 编写实体类

   ```java
   @Data
   public class Student {
       private int id;
       private String name;
       //学生需要关联一个老师
       private Teacher teacher;
   }
   
   @Data
   public class Teacher {
       private int id;
       private String name;
   }
   ```

2. 编写实体类对应的Mapper接口 【两个】

   无论有没有需求，都应该写上，以备后来之需！

   ```java
   public interface StudentMapper {
       //查询所有的学生信息，以及对应的老师信息
       public List<Student> getStudent();
   }
   
   public interface TeacherMapper {
       @Select("select * from mybatis.teacher where id=#{tid}")
       Teacher getTeacher(@Param("tid") int id);
   }
   ```

3. 编写Mapper接口对应的 mapper.xml配置文件 【两个】

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <!--核心配置文件-->
   <mapper namespace="com.sun.dao.StudentMapper">
       <select id="getStudent" resultType="student">
           select * from mybatis.student s,mybatis.teacher t
           where s.tid = t.id;
       </select>
   </mapper>
   
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <!--核心配置文件-->
   <mapper namespace="com.sun.dao.TeacherMapper">
   </mapper>
   ```

4. 在核心配置文件中绑定注册我们的Mapper接口或者文件

   ```xml
   <mappers>
       <mapper class="com.sun.dao.TeacherMapper"/>
       <mapper class="com.sun.dao.StudentMapper"/>
   </mappers>
   ```

5. 测试

   ```java
   public class myTest {
       @Test
       public void getTeacher() {
           SqlSession sqlSession = MybatisUtils.getSqlSession();
           TeacherMapper mapper = sqlSession.getMapper(TeacherMapper.class);
   
           Teacher teacher = mapper.getTeacher(1);
           System.out.println(teacher);
   
           sqlSession.close();
       }
       @Test
       public void getStudent() {
           SqlSession sqlSession = MybatisUtils.getSqlSession();
           StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
   
           List<Student> student = mapper.getStudent();
           for (Student stu : student) {
               System.out.println(stu);
           }
           //Student(id=1, name=小明, teacher=null)
           sqlSession.close();
       }
   }
   ```

## 按查询嵌套处理

要求：查询所有的学生信息，以及对应的老师信息

1. 编写对应的StudentMapper.xml文件  

   ```xml
   <mapper namespace="com.sun.dao.StudentMapper">
       <!--1.查询所有的学生信息-->
       <select id="getStudent" resultMap="studentTeacher">
           select * from mybatis.student
       </select>
   
       <!--2.根据查询出来学生的tid，寻找对应的老师.   子查询方式-->
       <resultMap id="studentTeacher" type="student">
           <result property="id" column="id"/>
           <result property="name" column="name"/>
   
           <association property="teacher" column="tid" javaType="teacher" select="getTeacher"/>
       </resultMap>
   
       <select id="getTeacher" resultType="teacher">
           select * from mybatis.teacher where id = #{tid}
       </select>
   </mapper>
   ```

2. 测试结果

   ```java
   Student(id=1, name=小明, teacher=Teacher(id=1, name=秦老师))
   Student(id=2, name=小红, teacher=Teacher(id=1, name=秦老师))
   Student(id=3, name=小张, teacher=Teacher(id=1, name=秦老师))
   Student(id=4, name=小李, teacher=Teacher(id=1, name=秦老师))
   Student(id=5, name=小王, teacher=Teacher(id=1, name=秦老师))
   ```

3. 注意点说明：

   resultMap中复杂的属性关键字

   - association：对象
     1. property="Student实体类中的名字" 
     2. column="数据库中的名字" 
     3. javaType="对象类型名"，指定的属性的类型名
   - collection：集合
     1. property="Teacher实体类中的名字" 
     2. column="数据库中的名字" 
     3. ofType="泛型的对象类型名"，集合中的泛型信息

## 按结果嵌套处理

我们还可以按照结果进行嵌套处理

1. 编写对应的mapper.xml文件

   ```xml
   <mapper>
   	<select id="getStudent2" resultMap="studentTeacher2">
           select s.id sid, s.name sname, t.name tname
           from mybatis.student s,mybatis.teacher t
           where s.tid = t.id;
       </select>
       <resultMap id="studentTeacher2" type="student">
           <result property="id" column="sid"/>
           <result property="name" column="sname"/>
           
           <association property="teacher" javaType="teacher">
               <result property="name" column="tname"/>
           </association>
       </resultMap>
   </mapper>
   ```

2. 测试结果

   ```java
   Student(id=1, name=小明, teacher=Teacher(id=0, name=秦老师))
   Student(id=2, name=小红, teacher=Teacher(id=0, name=秦老师))
   Student(id=3, name=小张, teacher=Teacher(id=0, name=秦老师))
   Student(id=4, name=小李, teacher=Teacher(id=0, name=秦老师))
   Student(id=5, name=小王, teacher=Teacher(id=0, name=秦老师))
   ```

## 小结

1. 按照查询进行嵌套处理就像SQL中的子查询
2. 按照结果进行嵌套处理就像SQL中的联表查询

# 一对多的处理

## 实体类编写

```java
@Data
public class Student {
    private int id;
    private String name;
    private int tid;
}

@Data
public class Teacher {
    private int id;
    private String name;

    //一个老师拥有多个学生
    private List<Student> students;
}
```

## 按结果嵌套处理

要求：查询所有的老师信息，以及对应的学生信息

1. TeacherMapper接口编写方法

   ```java
   //获取指定老师的所有信息及其学生
   Teacher getTeacher(@Param("tid") int id);
   ```

2. 编写接口对应的Mapper配置文件

   ```xml
   <mapper namespace="com.sun.dao.TeacherMapper">
       <select id="getTeachers" resultType="teacher">
           select *
           from mybatis.teacher;
       </select>
   
       <select id="getTeacher" resultMap="teacherStudent">
           select s.id sid, s.name sname, t.name tname, t.id tid
           from mybatis.teacher t,mybatis.student s
           where s.tid = t.id and t.id = #{tid}
       </select>
       <resultMap id="teacherStudent" type="teacher">
           <result property="id" column="tid"/>
           <result property="name" column="tname"/>
   
           <collection property="students" ofType="student">
               <result property="id" column="sid"/>
               <result property="name" column="sname"/>
               <result property="tid" column="tid"/>
           </collection>
       </resultMap>
   </mapper>
   ```

3. 测试结果

   ```java
   Teacher(id=1, name=秦老师, 
           students=[Student(id=1, name=小明, tid=1), 
                     Student(id=2, name=小红, tid=1), 
                     Student(id=3, name=小张, tid=1), 
                     Student(id=4, name=小李, tid=1), 
                     Student(id=5, name=小王, tid=1)
                    ]
          )
   ```

4. 注意点

   collection：集合

   1. property="Teacher实体类中的名字" 

   2. column="数据库中的名字" 

   3. ofType="泛型的对象类型名"，集合中的泛型信息

      直接取出对象，可以不写javaType

## 按查询嵌套处理

1. 编写接口对应的Mapper配置文件

   ```xml
   <mapper>    
   	<resultMap id="teacherStudent2" type="teacher">
           <result property="id" column="id"/>
           <result property="name" column="name"/>
   
           <collection property="students" column="id" javaType="ArrayList" ofType="student"
                       select="getStudentByTeacherID"/>
       </resultMap>
       <select id="getStudentByTeacherID" resultType="student">
           select *
           from mybatis.student
           where tid = #{tid};
       </select>
   </mapper>
   ```

2. 测试结果同上

3. 注意点

   子查询中需要teacher的ID，可以通过resultMap中的column来传递

## 小结

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240501153428072-1699568420.png" alt="image-20240501153427972" width="300" />

1. 关联-association
2. 集合-collection
3. 所以association是用于一对一和多对一，而collection是用于一对多的关系
4. JavaType和ofType都是用来指定对象类型的
   - JavaType是用来指定pojo中属性的类型
   - ofType指定的是映射到list集合属性中pojo的类型。

注意说明：

1. 保证SQL的可读性，尽量通俗易懂
2. 注意一对多和多对一 中：字段和属性对应的问题
3. 通过日志来查看自己的错误，建议使用Log4j

# 动态SQL

## 简介

什么是动态SQL：`动态SQL指的是根据不同的查询条件 , 生成不同的Sql语句`  

`所谓动态SQL，本质还是SQL语句，只是我们可以在SQL层面，去执行一个逻辑代码`

`动态SQL就是在拼接SQL语句，我们只要保证SQL的正确性，按照SQL的格式，去排列组合就可以了`

```java
官网描述：
    MyBatis 的强大特性之一便是它的动态 SQL。如果你有使用 JDBC 或其它类似框架的经验，你就能体会到根据不同条件拼接 SQL 语句的痛苦。例如拼接时要确保不能忘记添加必要的空格，还要注意去掉列表最后一个列名的逗号。利用动态 SQL 这一特性可以彻底摆脱这种痛苦。
    虽然在以前使用动态 SQL 并非一件易事，但正是 MyBatis 提供了可以被用在任意 SQL 映射语句中的强大的动态 SQL 语言得以改进这种情形。
    动态 SQL 元素和 JSTL 或基于类似 XML 的文本处理器相似。在 MyBatis 之前的版本中，有很多元素需要花时间了解。MyBatis 3 大大精简了元素种类，现在只需学习原来一半的元素便可。
    MyBatis 采用功能强大的基于 OGNL 的表达式来淘汰其它大部分元素。
-------------------------------
    - if
    - choose (when, otherwise)
    - trim (where, set)
    - foreach
```

## 搭建环境

新建一个数据库表：blog

字段：id，title，author，create_time，views

```mysql
CREATE TABLE `blog` (
    `id` varchar(50) NOT NULL COMMENT '博客id',
    `title` varchar(100) NOT NULL COMMENT '博客标题',
    `author` varchar(30) NOT NULL COMMENT '博客作者',
    `create_time` datetime NOT NULL COMMENT '创建时间',
    `views` int(30) NOT NULL COMMENT '浏览量'
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```

1. 创建Mybatis基础工程  

   - 导包
   - 编写配置文件
   - 编写实体类
   - 编写实体类对应的Mapper接口和Mapper.xml文件

2. 实体类编写 【注意set方法作用】

   ```java
   @Data
   public class Blog {
       private String id;
       private String title;
       private String author;
       private Date createTime;	//java.util.Date
       private int views;
   }
   ```

3. 编写Mapper接口及xml文件

4. IDutil工具类-生成随机ID

   ```java
   @SuppressWarnings("all")
   public class IDUtils {
       public static String getId() {
           return UUID.randomUUID().toString().replaceAll("-", "");
       }
   
   //    @Test
   //    public void test() {
   //        System.out.println(IDUtils.getId());
   //    }
   }
   ```

5. mybatis核心配置文件，下划线驼峰自动转换

   ```xml
   <settings>
       <setting name="mapUnderscoreToCamelCase" value="true"/>
   </settings>
   ```

6. 插入初始数据

   编写接口

   ```java
   public interface BlogMapper {
       //插入数据
       int addBlog(Blog blog);
   }
   ```

   sql配置文件

   ```xml
   <mapper namespace="com.sun.dao.BlogMapper">
       <insert id="addBlog" parameterType="blog">
           insert into mybatis.blog(id, title, author, create_time, views)
           values (#{id}, #{title}, #{author}, #{createTime}, #{views});
       </insert>
   </mapper>
   ```

   初始化博客方法

   ```java
   public class myTest {
       @Test
       public void addInitBlog() {
           SqlSession session = MybatisUtils.getSqlSession();
           BlogMapper mapper = session.getMapper(BlogMapper.class);
   
           Blog blog = new Blog();
           blog.setId(IDUtils.getId());
           blog.setTitle("标题一");
           blog.setAuthor("狂神说");
           blog.setCreateTime(new Date());
           blog.setViews(9999);
           mapper.addBlog(blog);
   
           blog.setId(IDUtils.getId());
           blog.setTitle("标题二");
           mapper.addBlog(blog);
   
           blog.setId(IDUtils.getId());
           blog.setTitle("标题三");
           mapper.addBlog(blog);
   
           blog.setId(IDUtils.getId());
           blog.setTitle("标题四");
           mapper.addBlog(blog);
   
           session.close();
       }
   }
   ```

   初始化数据结果

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240501161219775-1077215248.png" alt="image-20240501161219171" width="600" />

## if

需求：根据作者名字和博客名字来查询博客！如果作者名字为空，那么只根据博客名字查询，反之，则根据作者名来查询

1. 编写接口类

   ```java
   //查询博客
   List<Blog> queryBlogIF(Map map);
   ```

2. 编写SQL语句

   ```xml
   <mapper namespace="com.sun.dao.BlogMapper">
   	<select id="queryBlogIF" parameterType="map" resultType="blog">
           <!--where state = ‘ACTIVE’-->
           select * from mybatis.blog where 1=1
           <if test="title!=null">
               and title=#{title}
           </if>
           <if test="autor!=null">
               and author=#{author}
           </if>
       </select>
   </mapper>
   ```

3. 测试

   ```java
   @Test
   public void queryBlogIF() {
       SqlSession session = MybatisUtils.getSqlSession();
       BlogMapper mapper = session.getMapper(BlogMapper.class);
   
       HashMap map = new HashMap<>();
       map.put("title", "标题二");
       map.put("author", "狂神说");
   
       List<Blog> blogs = mapper.queryBlogIF(map);
       for (Blog blog : blogs) {
           System.out.println(blog);
       }
       
       session.close();
   }
   ```

4. 测试结果

   - 如果 title不等于 null，那么查询语句为 select * from user where title=#{title}
   - 如果author不等于 null，那么查询语句为 select * from user where and author=#{author}

## choose(when,otherwise)

有时候，我们不想用到所有的查询条件，只想选择其中的一个，查询条件有一个满足即可。针对这种情况，Mybatis提供了choose元素，它有点像java中的Switch语句。

1. sql配置文件

   ```xml
   <select id="queryBlogChoose" parameterType="map" resultType="blog">
       select *
       from mybatis.blog
       <where>
           <choose>
               <when test="title!=null">
                   title=#{title}
               </when>
               <when test="author!=null">
                   and author=#{author}
               </when>
               <otherwise>
                   and views=#{views}
               </otherwise>
           </choose>
       </where>
   </select>
   ```

2. 测试类

   ```java
   @Test
   public void queryBlogChoose() {
       SqlSession session = MybatisUtils.getSqlSession();
       BlogMapper mapper = session.getMapper(BlogMapper.class);
   
       HashMap map = new HashMap<>();
       map.put("title", "标题一");
       //map.put("author", "狂神说");
       map.put("views", "9999");
   
       List<Blog> blogs = mapper.queryBlogChoose(map);
       for (Blog blog : blogs) {
           System.out.println(blog);
       }
       session.close();
   }
   ```

## trim(where,set)

### where

*where* 元素只会在子元素返回任何内容的情况下才插入 “WHERE” 子句。

而且，若子句的开头为 “AND” 或 “OR”，*where* 元素也会将它们去除

```xml
<!--修改if例子中的SQL语句-->
<select id="queryBlogIF" parameterType="map" resultType="blog">
    select * from mybatis.blog
    <where>
        <if test="title!=null">
            title=#{title}
        </if>
        <if test="autor!=null">
            and author=#{author}
        </if>
    </where>
</select>
```

### Set

用于动态更新语句的类似解决方案叫做 *set*。*set* 元素可以用于动态包含需要更新的列，忽略其它不更新的列。

*set* 元素会动态地在行首插入 SET 关键字，并会删掉额外的逗号（这些逗号是在使用条件语句给列赋值时引入的）

1. 编写接口方法

   ```java
   int updateBlog(Map map);
   ```

2. sql配置文件

   ```xml
   <mapper>
   	<update id="updateBlog" parameterType="map">
           update mybatis.blog
           <set>
               <if test="title!=null">
                   title=#{title},
               </if>
               <if test="author!=null">
                   author=#{author}
               </if>
           </set>
           where id=#{id}
       </update>
   </mapper>
   ```

### trim

```xml
<!--trim-->
<trim prefix="" prefixOverrides="" suffix="" suffixOverrides="">
    
</trim>
```

 trim可以自定义的覆盖了后缀值设置，并且自定义了前缀值。

```xml
<!--自定义where-->
<trim prefix="WHERE" prefixOverrides="AND |OR ">
  ...
</trim>
```

- *prefixOverrides* 属性会忽略通过管道符分隔的文本序列（注意此例中的空格是必要的）。
- 上述例子会移除所有 *prefixOverrides* 属性中指定的内容，并且插入 *prefix* 属性中指定的内容。

```xml
<!--自定义set-->
<trim prefix="SET" suffixOverrides=",">
  ...
</trim>
```

## Foreach

```xml
<select id="selectPostIn" resultType="domain.blog.Post">
  SELECT * FROM POST P WHERE ID in
  <foreach item="item" index="index" collection="list" open="(" separator="," close=")">
        #{item}
  </foreach>
</select>
```

*foreach* 元素的功能非常强大

- 它允许你指定一个集合list，
- 声明可以在元素体内使用的集合项（item）
- 和索引（index）变量。
- 它也允许你指定开头open与结尾close的字符串
- 以及集合项迭代之间的分隔符separator。这个元素也不会错误地添加多余的分隔符

**提示** 

- 你可以将任何可迭代对象（如 List、Set 等）、Map 对象或者数组对象作为集合参数传递给 *foreach*。
- 当使用可迭代对象或者数组时，index 是当前迭代的序号，item 的值是本次迭代获取到的元素。
- 当使用 Map 对象（或者 Map.Entry 对象的集合）时，index 是键，item 是值。

需求：我们需要查询 blog 表中 id 分别为1,2,3的博客信息

1. 编写接口

   ```java
   List<Blog> queryBlogForeach(Map map);
   ```

2. 编写SQL语句

   ```xml
   <!--传递一个万能的Map 这个map中可以存在一个集合-->
   <select id="queryBlogForeach" parameterType="map" resultType="blog">
       select * from mybatis.blog
       <where>
           <foreach collection="ids" item="id" open="and (" separator="or" close=")">
               id=#{id}
           </foreach>
       </where>
   </select>
   ```

3. 测试

   ```java
   @Test
   public void queryBlogForeach() {
       SqlSession session = MybatisUtils.getSqlSession();
       BlogMapper mapper = session.getMapper(BlogMapper.class);
   
       HashMap map = new HashMap();
   
       ArrayList<Integer> ids = new ArrayList<>();
       ids.add(1);
       ids.add(2);
       ids.add(3);
       map.put("ids", ids);
   
       List<Blog> blogs = mapper.queryBlogForeach(map);
       for (Blog blog : blogs) {
           System.out.println(blog);
       }
       session.close();
   }
   ```

## SQL片段

有时候可能某个 sql 语句我们用的特别多，为了增加代码的重用性，简化代码，我们需要将这些代码抽取出来，然后使用时直接调用。

1. 提取SQL片段：sql

   ```xml
   <sql id="if-title-author">
       <if test="title!=null">
           title=#{title},
       </if>
       <if test="author!=null">
           author=#{author}
       </if>
   </sql>
   ```

2. 引用SQL片段：include

   ```xml
   <update id="updateBlog" parameterType="map">
       update mybatis.blog
       <set>
           <include refid="if-title-author"></include>
       </set>
       where id=#{id}
   </update>
   ```

注意：

- 最好基于 单表来定义 sql 片段，提高片段的可重用性
- 在 sql 片段中不要包括 where

## 小结

其实动态 sql 语句的编写往往就是一个拼接的问题，为了保证拼接准确，我们最好首先要写原生的 sql 语句出来，然后在通过 mybatis 动态sql 对照着改，防止出错。多在实践中使用才是熟练掌握它的技巧  

# 缓存[了解]

## 简介

```
查询：连接数据库，耗资源
	一次查询的结果，暂存在一个可以直接取出来的地址值！
	内存：缓存
MySQL缓存目前使用：Redis! K-V
```

1. 什么是缓存 [ Cache ]？

   - 存在内存中的临时数据。
   - 将用户经常查询的数据放在缓存（内存）中，用户去查询数据就不用从磁盘上(关系型数据库数据文件)查询，从缓存中查询，从而提高查询效率，解决了高并发系统的性能问题。
   - 三高问题：高并发、高可用、高性能

2. 为什么使用缓存？

   减少和数据库的交互次数，减少系统开销，提高系统效率。

3. 什么样的数据能使用缓存？

   `经常查询并且不经常改变的数据`

## Mybatis缓存

1. MyBatis包含一个非常强大的查询缓存特性，它可以非常方便地定制和配置缓存。缓存可以极大的提升查询效率。
2. MyBatis系统中默认定义了两级缓存：一级缓存和二级缓存
   - 默认情况下，只有一级缓存开启。（`SqlSession`级别的缓存，也称为本地缓存）它仅仅对一个会话中的数据进行缓存
   - 二级缓存需要手动开启和配置，他是基于namespace级别的缓存。
   - 为了提高扩展性，MyBatis定义了缓存接口Cache。我们可以通过实现Cache接口来自定义二级缓存

## 一级缓存

一级缓存就是一个map  ，一级缓存也叫本地缓存：SqlSession

1. 与数据库同一次会话期间查询到的数据会放在本地缓存中，从获取SqlSession到SqlSession.close()期间。
2. 以后如果需要获取相同的数据，直接从缓存中拿，没必须再去查询数据库；
3. 一级缓存是SqlSession级别的缓存，是一直开启的，我们关闭不了它；

### 体验测试

1. 在mybatis中加入日志，方便测试结果

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "https://mybatis.org/dtd/mybatis-3-config.dtd">
   <!--核心配置文件-->
   <configuration>
       <!--引入外部配置文件-->
       <properties resource="db.properties">
           <property name="password" value="root"/>
       </properties>
   
       <settings>
           <!--标准的日志工厂实现-->
           <setting name="logImpl" value="STDOUT_LOGGING"/>
       </settings>
   
       <!--可以给实体类起别名-->
       <typeAliases>
           <package name="com.sun.pojo"/>
       </typeAliases>
   
       <!--default默认环境-->
       <environments default="development">
           <environment id="development">
               <!--事务管理-->
               <transactionManager type="JDBC"/>
               <dataSource type="POOLED">
                   <property name="driver" value="${driver}"/>
                   <property name="url"
                             value="${url}"/>
                   <property name="username" value="${username}"/>
                   <property name="password" value="${password}"/>
               </dataSource>
           </environment>
       </environments>
   	<!--绑定Mapper-->
       <mappers>
           <mapper class="com.sun.dao.UserMapper"/>
       </mappers>
   </configuration>
   ```

2. 编写接口方法

   ```java
   public interface UserMapper {
       User queryUserById(@Param("id") int id);
   }
   ```

3. 接口对应的Mapper文件

   ```xml
   <mapper namespace="com.sun.dao.UserMapper">
       <select id="queryUserById" parameterType="_int" resultType="user">
           select *
           from mybatis.user
           where id = #{id};
       </select>
   </mapper>
   ```

4. 测试

   ```java
   @Test
   public void test() {
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   
       User user = mapper.queryUserById(1);
       System.out.println(user);
       System.out.println("===========================");
       //在一个SqlSession中查询两次相同的记录
       User user2 = mapper.queryUserById(1);
       System.out.println(user2);
       System.out.println(user == user2);
   
       sqlSession.close();
   }
   ```

5. 日志结果分析

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240501194030098-1705274829.png" alt="image-20240501194029595" width="400" />

### 一级缓存失效的四种情况

一级缓存失效情况：没有使用到当前的一级缓存。

效果就是：还需要再向数据库中发起一次查询请求！

- 映射语句文件中的所有 select 语句的结果将会被缓存。
- 映射语句文件中的所有 insert、update 和 delete 语句会`刷新缓存`。
- 缓存会使用最近最少使用算法（LRU, Least Recently Used）算法来清除不需要的缓存。
- 缓存`不会定时进行刷新`（也就是说，没有刷新间隔）。
- 缓存会保存列表或对象（无论查询方法返回哪种）的 1024 个引用。
- 缓存会被视为读/写缓存，这意味着获取到的对象并不是共享的，可以安全地被调用者修改，而不干扰其他调用者或线程所做的潜在修改。

1. sqlSession不同

   ```java
   @Test
   public void test() {
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       SqlSession sqlSession2 = MybatisUtils.getSqlSession();
   
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
       UserMapper mapper2 = sqlSession2.getMapper(UserMapper.class);
   
       User user = mapper.queryUserById(1);
       System.out.println(user);
       System.out.println("===========================");
       User user2 = mapper2.queryUserById(1);
       System.out.println(user2);
       System.out.println(user == user2);
   
       sqlSession.close();
       sqlSession2.close();
   }
   ```

   ​                         <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240501201821284-1322961624.png" alt="image-20240501201820368" width="400" /> 

   ​         观察结果：发现发送了两条SQL语句！

   ​         结论：每个sqlSession中的缓存相互独立

2. sqlSession相同：查询条件不同

   ```sql
   @Test
   public void test() {
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   
       User user = mapper.queryUserById(1);
       System.out.println(user);
       System.out.println("===========================");
       User user2 = mapper.queryUserById(2);
       System.out.println(user2);
       System.out.println(user == user2);
   
       sqlSession.close();
   }
   ```

   									<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240501195124208-1105671540.png" alt="image-20240501195123937" width="400" />
   	
   						观察结果：发现发送了两条SQL语句！很正常的理解
   	
   						结论：当前缓存中，不存在这个数据

3. sqlSession相同，两次查询之间执行了增删改操作！

   - 增加方法

     ```java
     int updateUser(User user);
     ```

   - 编写SQL

     ```xml
     <update id="updateUser" parameterType="user">
         update mybatis.user
         set name =#{name},
             pwd=#{pwd}
         where id = #{id};
     </update>
     ```

   - 测试

     ```java
     @Test
     public void test() {
         SqlSession sqlSession = MybatisUtils.getSqlSession();
         UserMapper mapper = sqlSession.getMapper(UserMapper.class);
     
         User user = mapper.queryUserById(1);
         System.out.println(user);
     
         int aaa = mapper.updateUser(new User(2, "aaa", "456"));
         System.out.println("===========================");
         User user2 = mapper.queryUserById(1);
         System.out.println(user2);
         System.out.println(user == user2);
     
         sqlSession.close();
     }
     ```

     <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240501194938398-889543354.png" alt="image-20240501194937869" width="400" />

     ​	观察结果：查询在中间执行了增删改操作后，重新执行了

     ​	结论：因为增删改操作可能会对当前数据产生影响  

4. sqlSession相同，手动清除一级缓存  

   ```java
   sqlSession.clearCache();
   ```

## 二级缓存

1. 二级缓存也叫全局缓存，一级缓存作用域太低了，所以诞生了二级缓存
2. 基于namespace级别的缓存，一个名称空间，对应一个二级缓存；
3. 要启用全局的二级缓存，只需要在你的 SQL 映射文件中添加一行：cache/>
4. 二级缓存是事务性的。这意味着，当 SqlSession 完成并提交时，或是完成并回滚，但没有执行 flushCache=true 的 insert/delete/update 语句时，缓存会获得更新。
5. 工作机制
   - 一个会话查询一条数据，这个数据就会被放在当前会话的一级缓存中；
   - 如果当前会话关闭了，这个会话对应的一级缓存就没了；但是我们想要的是，会话关闭了，一级缓存中的数据被保存到二级缓存中；
   - 新的会话查询信息，就可以从二级缓存中获取内容；
   - 不同的mapper查出的数据会放在自己对应的缓存（map）中

### 使用步骤

1. 开启全局缓存 【mybatis-config.xml】

   ```xml
   <settings>
       <!--显示的开启全局缓存-->
       <setting name="cacheEnabled" value="true"/>
   </settings>
   ```

2. 去每个mapper.xml中配置使用二级缓存，这个配置非常简单；【xxxMapper.xml】

   ```xml
   <!--namespace级别-->
   <!--不加参数，需要所有的实体类先实现序列化接口implements Serializable-->
   <cache/>
   
   <!--也可以自定义参数
   	1.这个更高级的配置创建了一个 FIFO 缓存，
   	2.每隔 60 秒刷新，
   	3.最多可以存储结果对象或列表的 512 个引用，
   	4.而且返回的对象被认为是只读的，因此对它们进行修改可能会在不同线程中的调用者产生冲突。
   -->
   <cache
     eviction="FIFO"
     flushInterval="60000"
     size="512"
     readOnly="true"/>
   ```

3. 代码测试

   ```java
   @Test
   public void test() {
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       SqlSession sqlSession2 = MybatisUtils.getSqlSession();
   
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
       User user = mapper.queryUserById(1);
       System.out.println(user);
       sqlSession.close();
       System.out.println("===========================");
   	//会话关闭了，一级缓存中的数据被保存到二级缓存中；
       UserMapper mapper2 = sqlSession2.getMapper(UserMapper.class);
       User user2 = mapper2.queryUserById(1);
       System.out.println(user2);
       System.out.println(user == user2);
       sqlSession2.close();
   }
   ```

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240501202448645-249049643.png" alt="image-20240501202448276" width="400" />

### 结论

1. 只要开启了二级缓存，我们在同一个Mapper中的查询，可以在二级缓存中拿到数据
2. 查出的数据都会被默认先放在一级缓存中
3. 只有会话提交或者关闭以后，一级缓存中的数据才会转到二级缓存中

## 缓存原理

缓存顺序：

1. 先看二级缓存中有没有
2. 再看一级缓存中有没有
3. 查询数据库

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240501205005516-462997605.png" alt="image-20240501205005200" width="500" />

## 自定义缓存-EhCache

除了上述自定义缓存的方式，你也可以通过实现你自己的缓存，或为其他第三方缓存方案创建适配器，来完全覆盖缓存行为。

```xml
<cache type="com.domain.something.MyCustomCache"/>
```

第三方缓存实现--EhCache

1. Ehcache是一种广泛使用的java分布式缓存，主要面向通用缓存；是一个纯Java的进程内缓存框架

2. 要在应用程序中使用Ehcache，需要引入依赖的jar包

   ```xml
   <!-- https://mvnrepository.com/artifact/org.mybatis.caches/mybatis-ehcache -->
   <dependency>
       <groupId>org.mybatis.caches</groupId>
       <artifactId>mybatis-ehcache</artifactId>
       <version>1.2.3</version>
   </dependency>
   ```

3. 在mapper.xml中使用对应的缓存即可

   ```xml
   <cache type="org.mybatis.caches.ehcache.EhBlockingCache"/>
   ```

4. 编写ehcache.xml文件，如果在 加载时 未找到 /ehcache.xml 资源或出现问题，则将使用默认配置。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
            updateCheck="false">
       <!--
       diskStore：为缓存路径，ehcache分为内存和磁盘两级，此属性定义磁盘的缓存位
       置。参数解释如下：
       user.home – 用户主目录
       user.dir – 用户当前工作目录
       java.io.tmpdir – 默认临时文件路径
       -->
       <diskStore path="./tmpdir/Tmp_EhCache"/>
       <defaultCache
               eternal="false"
               maxElementsInMemory="10000"
               overflowToDisk="false"
               diskPersistent="false"
               timeToIdleSeconds="1800"
               timeToLiveSeconds="259200"
               memoryStoreEvictionPolicy="LRU"/>
       <cache
               name="cloud_user"
               eternal="false"
               maxElementsInMemory="5000"
               overflowToDisk="false"
               diskPersistent="false"
               timeToIdleSeconds="1800"
               timeToLiveSeconds="1800"
               memoryStoreEvictionPolicy="LRU"/>
       <!--
       defaultCache：默认缓存策略，当ehcache找不到定义的缓存时，则使用这个缓存策略。只能定义一个。
       -->
       <!--
       name:缓存名称。
       maxElementsInMemory:缓存最大数目
       maxElementsOnDisk：硬盘最大缓存个数。
       eternal:对象是否永久有效，一但设置了，timeout将不起作用。
       overflowToDisk:是否保存到磁盘，当系统宕机时
       timeToIdleSeconds:设置对象在失效前的允许闲置时间（单位：秒）。仅当eternal=false对象不是永久有效时使用，可选属性，默认值是0，也就是可闲置时间无穷大。
       timeToLiveSeconds:设置对象在失效前允许存活时间（单位：秒）。最大时间介于创建
       时间和失效时间之间。仅当eternal=false对象不是永久有效时使用，默认是0.，也就是对象存活时间无穷大。
       diskPersistent：是否缓存虚拟机重启期数据 Whether the disk store
       persists between restarts of the Virtual Machine. The default value isfalse.
       diskSpoolBufferSizeMB：这个参数设置DiskStore（磁盘缓存）的缓存区大小。默认是30MB。每个Cache都应该有自己的一个缓冲区。
       diskExpiryThreadIntervalSeconds：磁盘失效线程运行时间间隔，默认是120秒。
       memoryStoreEvictionPolicy：当达到maxElementsInMemory限制时，Ehcache将
       会根据指定的策略去清理内存。默认策略是LRU（最近最少使用）。你可以设置为FIFO（先进先出）或是LFU（较少使用）。
       clearOnFlush：内存数量最大时是否清除。
       memoryStoreEvictionPolicy:可选策略有：LRU（最近最少使用，默认策略）、
       FIFO（先进先出）、LFU（最少访问次数）。
       FIFO，first in first out，这个是大家最熟的，先进先出。
       LFU， Less Frequently Used，就是上面例子中使用的策略，直白一点就是讲一直以来最少被使用的。如上面所讲，缓存的元素有一个hit属性，hit值最小的将会被清出缓存。
       LRU，Least Recently Used，最近最少使用的，缓存的元素有一个时间戳，当缓存容量满了，而又需要腾出地方来缓存新的元素的时候，那么现有缓存元素中时间戳离当前时间最远的元素将被清出缓存。
       -->
   </ehcache>
   ```

## 引用缓存-cache-ref

回想一下上一节的内容，对某一命名空间的语句，只会使用该命名空间的缓存进行缓存或刷新。 

但你可能会想要在多个命名空间中共享相同的缓存配置和实例。

要实现这种需求，你可以使用 cache-ref 元素来引用另一个缓存。

```xml
<cache-ref namespace="com.someone.application.data.SomeMapper"/>
```