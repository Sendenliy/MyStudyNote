# 简介

![image-20240623130147039](https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240623130147837-706078052.png)

MyBatisPlus可以节省我们大量工作时间，所有的CRUD代码它都可以自动化完成！

- JPA 、 tk-mapper、MyBatisPlus偷懒的！
- MyBatis 本来就是简化 JDBC 操作的！简化 MyBatis ！
- 官网：https://mp.baomidou.com/

![image-20240623125631733](https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240623125632051-1674440627.png)

特性

- 无侵入：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
- 损耗小：启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作， BaseMapper
- **强大的 CRUD 操作：内置通用 Mapper、通用 Service**，仅仅通过少量配置即可实现单表大部分CRUD 操作，更有强大的条件构造器，满足各类使用需求, 以后简单的CRUD操作，它不用自己编写了！
- 支持 Lambda 形式调用：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
- **支持主键自动生成**：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
- 支持 ActiveRecord 模式：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
- 支持自定义全局通用操作：支持全局通用方法注入（ Write once, use anywhere ）
- **内置代码生成器**：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用（自动帮你生成代码）
- **内置分页插件**：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询
- 分页插件支持多种数据库：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 等多种数据库
- **内置性能分析插件**：可输出 Sql 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
- **内置全局拦截插件**：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作

![image-20240623130315442](https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240623130315897-1997576945.png)

# 快速开始

使用第三方组件常规步骤：

1. 导入对应的依赖
2. 研究依赖如何配置
3. 代码如何编写
4. 提高扩展技术能力！

## 步骤

1. 创建数据库 mybatis_plus

2. 创建user表

   - 真实开发中:  version（乐观锁）、deleted（逻辑删除）、gmt_create、gmt_modified

   ```sql
   create database mybatis_plus;
   use mybatis_plus;
   
   DROP TABLE IF EXISTS `user`;
   
   CREATE TABLE `user`
   (
       id BIGINT NOT NULL COMMENT '主键ID',
       name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
       age INT NULL DEFAULT NULL COMMENT '年龄',
       email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
       PRIMARY KEY (id)
   );
   INSERT INTO `user` (id, name, age, email) VALUES
   (1, 'Jone', 18, 'test1@baomidou.com'),
   (2, 'Jack', 20, 'test2@baomidou.com'),
   (3, 'Tom', 28, 'test3@baomidou.com'),
   (4, 'Sandy', 21, 'test4@baomidou.com'),
   (5, 'Billie', 24, 'test5@baomidou.com');
   ```

3. 编写项目，初始化项目！使用SpringBoot初始化！

4. 导入依赖

   说明：我们使用 mybatis-plus 可以节省我们大量的代码，尽量不要同时导入 mybatis 和 mybatisplus！版本的差异！

   ```xml
   <dependencies>
       <!--mybatis plus-->
       <dependency>
           <groupId>com.baomidou</groupId>
           <artifactId>mybatis-plus-spring-boot3-starter</artifactId>
           <version>3.5.7</version>
       </dependency>
       <!--lombok-->
       <dependency>
           <groupId>org.projectlombok</groupId>
           <artifactId>lombok</artifactId>
           <version>1.18.32</version>
       </dependency>
       <!--mysql-->
       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
           <version>5.1.47</version>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
   
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-test</artifactId>
           <scope>test</scope>
       </dependency>
   </dependencies>
   ```

5. 连接数据库！这一步和 mybatis 相同！

   - MySQL5和MySQL8的驱动不同，MySQL8需要增加时区的配置

     ```properties
     spring.application.name=MybatisPlus
     spring.datasource.username=root
     spring.datasource.password=root
     spring.datasource.url=jdbc:mysql://localhost:3306/mybatis_plus?useSSL=false&useUnicode=true&characterEncoding=utf-8
     #serverTimezone=GMT%2B8   mysql8
     spring.datasource.driver-class-name=com.mysql.jdbc.Driver
     #spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver   mysql8
     ```

6. 传统方式：`pojo-dao（连接mybatis，配置mapper.xml文件）-service-controller`

   使用了mybatis-plus 之后

   - pojo

     ```java
     @Data
     @AllArgsConstructor
     @NoArgsConstructor
     public class User {
         //对应数据库中的主键（UUID，自增id，雪花算法，redis，zookeeper）
         pprivate Long id;
         private String name;
         private Integer age;
         private String email;
     }
     ```

   - mapper接口

     ```java
     //在对应的Mapper上面实现基本的接口BaseMapper
     @Repository //代表持久层
     public interface UserMapper extends BaseMapper<User> {
         
         //所有的CRUD操作已经编写完成
     }
     ```

   - 主启动类：需要扫描我们的mapper包下的所有接口@MapperScan("XXX")【注意点】

     ```java
     //扫描Mapper文件夹
     @MapperScan("com.sunm.mapper")
     @SpringBootApplication
     public class MybatisPlusApplication {
     
         public static void main(String[] args) {
             SpringApplication.run(MybatisPlusApplication.class, args);
         }
     
     }
     ```

   - 测试类：测试

     ```java
     @SpringBootTest
     class MybatisPlusApplicationTests {
         //继承了BaseMapper，所有的方法都来自父类；也可以编写自己的方法
         @Autowired
         private UserMapper userMapper;
     
         @Test
         void contextLoads() {
             //查询所有的用户
             //参数是一个Wrapper，条件构造器，这里不用，null
             List<User> users = userMapper.selectList(null);
             users.forEach(System.out::println);
         }
     }
     ```

7. 结果

   

## 思考

1. SQL谁帮我们写的 ? MyBatis-Plus 都写好了
2. 方法哪里来的？ MyBatis-Plus 都写好了

## 配置日志

我们所有的sql现在是不可见的，我们希望知道它是怎么执行的，所以我们必须要看日志！

配置完毕日志之后，后面的学习就需要注意这个自动生成的SQL。

```properties
spring.application.name=MybatisPlus
#数据库连接配置
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis_plus?useSSL=false&useUnicode=true&characterEncoding=utf-8
spring.datasource.driver-class-name=com.mysql.jdbc.Driver

#配置日志
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```

![image-20240623143051171](https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240623143052012-450837202.png)

# 基本功能-CRUD

## Insert插入操作

```
数据库插入的id的默认值为：全局的唯一id  
@Test
public void testInsert() {
    User user = new User();
    user.setName("测试");
    user.setAge(18);
    user.setEmail("123456789@qq.com");

    int insert = userMapper.insert(user);
    System.out.println(insert);//受影响的行数
    System.out.println(user);//id自动回填
}
```

结果：

![image-20240623143644449](https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240623143645116-1927693079.png)

## Update更新操作

```java
@Test
public void testUpdate() {
    User user = new User();
    user.setId(5L);
    user.setName("我是第五位");

    //int updateById(@Param("et") T entity);
    int i = userMapper.updateById(user);
    System.out.println(i);
}
```

结果：

![image-20240623150530659](https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240623150531467-659181779.png)

当修改年龄时，会配置动态SQL，所有的sql都是自动帮你动态配置的！

![image-20240623150812126](https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240623150812523-760921612.png)

## Select查询操作

1. 查询单个：.selectById

   ```java
   //查询
   @Test
   public void testSelect() {
       User user = userMapper.selectById(1L);
       
       System.out.println(user);
   }
   ```

   

2. 批量查询：.selectBatchIds

   ```java
   public void testSelect() {
       List<User> users = userMapper.selectBatchIds(Arrays.asList(1, 2, 3));
       users.forEach(System.out::println);
   }
   ```

   

3. 条件查询：.selectByMap

   ```java
   public void testSelect() {
       HashMap<String, Object> map = new HashMap<>();
       map.put("name", "测试");
       map.put("age", 18);
   
       List<User> users = userMapper.selectByMap(map);
       users.forEach(System.out::println);
   }
   ```

   

## Delete删除操作

1. 单个删除：

   ```java
   public void testDelete() {
       userMapper.deleteById(1804765105839177730L);
   }
   ```

2. 批量删除：

   ```java
   public void testDelete() {
       userMapper.deleteByIds(Arrays.asList(1804765105839177731L, 1804765105839177732L));
   }
   ```

3. 条件删除：

   ```java
   public void testDelete() {
       HashMap<String, Object> map = new HashMap<>();
       map.put("id", 1804765105839177733L);
   
       userMapper.deleteByMap(map);
   }
   ```

# 核心功能

## 主键生成策略@TableId

### 源码分析

分布式系统唯一id生成：https://www.cnblogs.com/haoxinyue/p/5208136.html

```java
public enum IdType {
    
    //1.数据库`ID自增`。该类型请确保`数据库设置了 ID自增` 否则无效
    AUTO(0),
    
    //2.该类型为`未设置主键类型`(注解里等于跟随全局,全局里约等于 INPUT)
    NONE(1),
    
    //3.`用户输入ID`。该类型可以通过自己注册自动填充插件进行填充</p>
    INPUT(2),

    /* 以下2种类型、只有当插入对象ID 为空，才自动填充。 */
    
    /*4.分配ID (主键类型为number或string）,(雪花算法)
     * 默认实现类 com.baomidou.mybatisplus.core.incrementer.DefaultIdentifierGenerator
     * @since 3.3.0
     */
    ASSIGN_ID(3),
    
    /**
     *5.分配UUID (主键类型为 string) (UUID.replace("-",""))
     * 默认实现类 com.baomidou.mybatisplus.core.incrementer.DefaultIdentifierGenerator
     */
    ASSIGN_UUID(4);

    private final int key;

    IdType(int key) {
        this.key = key;
    }
}
```

### 策略实现

1. 【默认：ASSIGN_ID(3)】雪花算法：

   snowflake是Twitter开源的分布式ID生成算法，结果是：**一个long型的ID**。

   其核心思想是：使用41bit作为毫秒数，10bit作为机器的ID（5个bit是数据中心，5个bit的机器ID），12bit作为毫秒内的流水号（意味着每个节点在每毫秒可以产生 4096 个 ID），最后还有一个符号位，永远是0。可以保证几乎全球唯一！

2. 主键自增：AUTO(0)

   1. 实体类字段上 @TableId(type = IdType.AUTO)

      ```java
      @Data
      @AllArgsConstructor
      @NoArgsConstructor
      public class User {
          @TableId(type = IdType.AUTO)
          private Long id;
          
          private String name;
          private Integer age;
          private String email;
      }
      ```

   2. 数据库字段一定要是自增！

      

   3. 再次测试插入即可！

      

3. 手动输入：@TableId(type = IdType.INPUT)

   

## 自动填充@TableField

创建时间、修改时间！这些个操作一遍都是自动化完成的，我们不希望手动更新！

阿里巴巴开发手册：

- 所有的数据库表：gmt_create、gmt_modified几乎所有的表都要配置上！
- 而且需要自动化！

### 方式一：数据库级别

（工作中不允许你修改数据库）

1. 在表中新增字段 create_time, update_time，设置默认为当前时间戳。【MySQL8+】

   

2. 我们需要先把实体类同步！

   ```java
   private Date createTime;
   private Date updateTime;
   ```

3. 再次更新查看结果即可

### 方式二：代码级别

官网：https://baomidou.com/guides/auto-fill-field/

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.FIELD, ElementType.ANNOTATION_TYPE})
public @interface TableField {
    /**
     * 字段 update set 部分注入, 该注解优于 el 注解使用
     * 
     * 例1：@TableField(.. , update="%s+1") 其中 %s 会填充为字段
     * 输出 SQL 为：update 表 set 字段=字段+1 where ...
     * 
     * 例2：@TableField(.. , update="now()") 使用数据库时间
     * 输出 SQL 为：update 表 set 字段=now() where ...
     */
    String update() default "";
    
    /**
     * 字段自动填充策略
     * 
     * 在对应模式下将会忽略 insertStrategy 或 updateStrategy 的配置,等于断言该字段必有值
     */
    FieldFill fill() default FieldFill.DEFAULT;
}

//填充策略
public enum FieldFill {
    DEFAULT,       // 默认不处理
    INSERT,        // 插入填充字段
    UPDATE,        // 更新填充字段
    INSERT_UPDATE  // 插入和更新填充字段
}
```

1. 删除数据库的默认值、更新操作！

2. 实体类字段属性上需要增加注解

   ```java
   @TableField(fill = FieldFill.INSERT)	//插入时填充
   private Date createTime;
   @TableField(fill = FieldFill.INSERT_UPDATE)	//插入和更新时填充
   private Date updateTime;
   ```

3. 编写处理器MyMetaObjectHandler 类来处理这个注解

   实现 `MetaObjectHandler` 接口，并重写 `insertFill` 和 `updateFill` 方法。

   ```java
   @Component
   @Slf4j
   public class MyMetaObjectHandler implements MetaObjectHandler {
       @Override
       public void insertFill(MetaObject metaObject) {//插入时
           log.info("Start insert fill");
           
           //参数：字段名，插入的字段值，要处理的数据
           //MetaObjectHandler setFieldValByName(String fieldName, Object fieldVal, MetaObject metaObject)
           this.setFieldValByName("createTime", new Date(), metaObject);
           this.setFieldValByName("updateTime", new Date(), metaObject);
       }
   
       @Override
       public void updateFill(MetaObject metaObject) {//更新时
           log.info("Start update fill");
           this.setFieldValByName("updateTime", new Date(), metaObject);
       }
   }
   ```

4. 配置自动填充处理器

   确保你的 MyMetaObjectHandler 类被 Spring 管理，可以通过 `@Component 或 @Bean `注解来实现。

5. 测试插入

   ![image-20240623154249701](https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240623154250332-2019202431.png)

6. 测试更新

   ![image-20240623154348532](https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240623154348748-476699820.png)

## 逻辑删除@TableLogic

### 简介

1. 区别：管理员可以查看被删除的记录！防止数据的丢失，类似于回收站！

   - 物理删除 ：从数据库中直接移除
   - 逻辑删除 ：数据库中没有被移除，而是在数据库中标记记录为“已删除”来让他失效！保留数据的历史痕迹，同时确保查询结果的整洁性。 deleted = 0 => deleted =

2. MyBatis-Plus 的逻辑删除功能：会在执行数据库操作时自动处理逻辑删除字段

   以下是它的工作方式：

   - **插入**：逻辑删除字段的值不受限制。
   - **查找**：自动添加条件，过滤掉标记为已删除的记录。
   - **更新**：防止更新已删除的记录。
   - **删除**：将删除操作转换为更新操作，标记记录为已删除。

   例如：

   - **删除**：update user set deleted=1 where id = 1 and deleted=0
   - **查找**：select id,name,deleted from user where deleted=0

3. 支持的数据类型

   逻辑删除字段支持所有数据类型，但推荐使用 Integer、Boolean 或 LocalDateTime。

   如果使用 datetime 类型，可以配置逻辑未删除值为 null，已删除值可以使用函数如 now() 来获取当前时间。

### MP实现

1. 在数据表中增加一个 deleted 字段

   

2. 实体类中增加属性

   ```java
   @TableLogic //逻辑删除
   private Integer deleted;
   ```

3. 配置全局逻辑删除属性

   ```properties
   # 全局逻辑删除字段名
   mybatis-plus.global-config.db-config.logic-delete-field=deleted
   # 逻辑已删除值
   mybatis-plus.global-config.db-config.logic-delete-value=1
   # 逻辑未删除值
   mybatis-plus.global-config.db-config.logic-not-delete-value=0
   ```

4. 测试一下删除！

   记录依旧在数据库，但是deleted值已经变化了！

   

此时执行查询操作：

![image-20240623171452583](https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240623171453198-1991924196.png)

## SQL分析与打印p6spy

### 简介

1. MyBatis-Plus提供了SQL分析与打印的功能，通过集成p6spy组件，可以方便地**输出SQL语句及其执行时长**。本功能适用于MyBatis-Plus 3.1.0及以上版本。

2. `p6spy`不仅限于记录SQL日志，它还提供了一些高级功能，如：

   - **慢SQL检测**：通过配置outagedetection和outagedetectioninterval，p6spy可以记录执行时间超过设定阈值的SQL语句。
   - **自定义日志格式**：通过logMessageFormat，你可以自定义SQL日志的输出格式，包括时间戳、执行时间、SQL语句等。
   - **日志输出控制**：appender配置项允许你选择日志输出到控制台、文件或日志系统。

   `p6spy`是一个强大的工具，它为MyBatis-Plus用户提供了便捷的SQL分析与打印功能。通过合理配置，你可以在开发和测试阶段有效地监控和优化SQL语句。然而，由于性能损耗，建议在生产环境中谨慎使用。

3. 注意事项：

   - `driver-class-name`应配置为`p6spy`提供的驱动类。
   - `url`前缀应为`jdbc:p6spy`，后跟实际的数据库连接地址。
   - 如果打印的SQL为`null`，请在`excludecategories`中增加`commit`。
   - 如果批量操作不打印SQL，请去除`excludecategories`中的`batch`。
   - 对于批量操作打印重复的问题，请使用`MybatisPlusLogFactory`（3.2.1新增）。
   - 请注意，该插件可能会带来性能损耗，不建议在生产环境中使用。

   通过以上步骤，你就可以在开发过程中方便地分析和打印SQL语句了。记得根据实际需要调整配置，以达到最佳的使用效果。

### 使用步骤

1. 引入依赖

   ```xml
   <dependency>
       <groupId>com.github.gavlyukovskiy</groupId>
       <artifactId>p6spy-spring-boot-starter</artifactId>
       <version>1.9.1</version>
   </dependency>
   ```

2. 配置application.properties

   ```properties
   #修改以下两处
   spring.datasource.url=jdbc:p6spy:mysql://localhost:3306/mybatis_plus?useSSL=false&useUnicode=true&characterEncoding=utf-8
   spring.datasource.driver-class-name=com.p6spy.engine.spy.P6SpyDriver
   
   #decorator配置
   #Register P6LogFactory to log JDBC events
   decorator.datasource.p6spy.enable-logging=true
   # Use logging for default listeners [slf4j, sysout, file, custom]
   decorator.datasource.p6spy.logging=slf4j
   decorator.datasource.flexy-pool.acquiring-strategy.increment-pool.timeout-millis=500
   ```

3. 配置spy.properties

   ```properties
   modulelist=com.baomidou.mybatisplus.extension.p6spy.MybatisPlusLogFactory,com.p6spy.engine.outage.P6OutageFactory
   # 自定义日志打印
   logMessageFormat=com.baomidou.mybatisplus.extension.p6spy.P6SpyLogger
   #日志输出到控制台
   appender=com.baomidou.mybatisplus.extension.p6spy.StdoutLogger
   # 使用日志系统记录 sql
   #appender=com.p6spy.engine.spy.appender.Slf4JLogger
   # 设置 p6spy driver 代理
   deregisterdrivers=true
   # 取消JDBC URL前缀
   useprefix=true
   # 配置记录 Log 例外,可去掉的结果集有error,info,batch,debug,statement,commit,rollback,result,resultset.
   excludecategories=info,debug,result,commit,resultset
   # 日期格式
   dateformat=yyyy-MM-dd HH:mm:ss
   # 实际驱动可多个
   #driverlist=org.h2.Driver
   # 是否开启慢SQL记录
   outagedetection=true
   # 慢SQL记录标准 2 秒
   outagedetectioninterval=2
   ```

4. 测试查询

   

## 条件构造器Wrapper

### 简介

1. 条件构造器（Wrapper）：用于构建复杂的数据库查询和更新条件。

   - Wrapper 类允许开发者以链式调用的方式构造查询条件，无需编写繁琐的 SQL 语句，从而提高开发效率并减少 SQL 注入的风险。
   - 我们写一些复杂的sql就可以使用它来替代！

2. 主要的 Wrapper 类及其功能：

   - **AbstractWrapper**：这是一个抽象基类，提供了所有 Wrapper   类共有的方法和属性。它定义了条件构造的基本逻辑，包括字段（column）、值（value）、操作符（condition）等。所有的   QueryWrapper、UpdateWrapper、LambdaQueryWrapper 和 LambdaUpdateWrapper 都继承自 AbstractWrapper。
   - **QueryWrapper**：专门用于构造查询条件，支持基本的等于、不等于、大于、小于等各种常见操作。它允许你以链式调用的方式添加多个查询条件，并且可以组合使用 `and` 和 `or` 逻辑。
   - **UpdateWrapper**：用于构造更新条件，可以在更新数据时指定条件。与 QueryWrapper 类似，它也支持链式调用和逻辑组合。使用 UpdateWrapper 可以在不创建实体对象的情况下，直接设置更新字段和条件。
   - **LambdaQueryWrapper**：这是一个基于 Lambda 表达式的查询条件构造器，它通过 Lambda 表达式来引用实体类的属性，从而避免了硬编码字段名。这种方式提高了代码的可读性和可维护性，尤其是在字段名可能发生变化的情况下。
   - **LambdaUpdateWrapper**：类似于 LambdaQueryWrapper，LambdaUpdateWrapper 是基于 Lambda 表达式的更新条件构造器。它允许你使用 Lambda 表达式来指定更新字段和条件，同样避免了硬编码字段名的问题。

3. 操作：官网https://baomidou.com/guides/wrapper/

   

### queryWrapper测试

1. 条件：`.isNotNull、.ge`   查询列表：`.selectList`

   ```java
   public void test() {
       //查询name和email不为空，age>=20的用户，
       QueryWrapper<User> queryWrapper = new QueryWrapper<>();
       queryWrapper.isNotNull("name")
               .isNotNull("email")
               .ge("age", 20);
   
       userMapper.selectList(queryWrapper).forEach(System.out::println);
   }
   ```

   

2. 条件：`.eq、模糊查询.like`     查询单个：`.selectOne`

   ```java
   public void test2() {
       //查询name为“测试”的用户，
       QueryWrapper<User> queryWrapper = new QueryWrapper<>();
       queryWrapper.eq("name", "Tom");
       //queryWrapper.like("name", "Tom");
   
       User user = userMapper.selectOne(queryWrapper);
       System.out.println(user);
   }
   ```

   

3. 条件：`.between`     查询单个：`.selectCount`

   ```java
   public void test3() {
       //查询age在20-30的用户
       QueryWrapper<User> queryWrapper = new QueryWrapper<>();
       queryWrapper.between("age", 20, 30);
   
       Long l = userMapper.selectCount(queryWrapper);
       System.out.println(l);
   }
   ```

   

4. 条件：`.notLike、.likeLeft`     条件查询：`.selectMaps`

   ```java
   public void test4() {
           QueryWrapper<User> queryWrapper = new QueryWrapper<>();
   //        queryWrapper.like("name", "Tom");
           queryWrapper.notLike("name", "a")	//.notLike(col,val): where col not like %val%
                   .likeLeft("email", "qq.com");    //.likeLeft(col,val): where col like %val
   
           userMapper.selectMaps(queryWrapper).forEach(System.out::println);
       }
   ```

   

5. 条件：`.inSql`     条件查询：`.selectObjs`

   ```java
   public void test5() {
       QueryWrapper<User> queryWrapper = new QueryWrapper<>();
       //id在子查询中查询出来
       queryWrapper.inSql("id", "select id from user where id > 2");
   
       List<Object> objects = userMapper.selectObjs(queryWrapper);
       objects.forEach(System.out::println);
   }
   ```

   

6. 条件：`.orderByDesc`     条件查询：`.selectList`

   ```java
   public void test6() {
       QueryWrapper<User> queryWrapper = new QueryWrapper<>();
       //通过age降序排序
       queryWrapper.orderByDesc("age");
   
       List<User> users = userMapper.selectList(queryWrapper);
       users.forEach(System.out::println);
   }
   ```

![image-20240623183334871](https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240623183335511-1478653724.png)

# 插件

## 乐观锁插件@Version

### 简介

官网：https://baomidou.com/plugins/optimistic-locker/

1. 区别：
   - 乐观锁 : 故名思意十分乐观，它总是认为不会出现问题，无论干什么不去上锁！如果出现了问题，再次更新值测试
   - 悲观锁：故名思意十分悲观，它总是认为总是出现问题，无论干什么都会上锁！再去操作！
2. 乐观锁的实现通常包括以下步骤：
   1. 读取记录时，获取当前的版本号（version）。
   2. 在更新记录时，将这个版本号一同传递。
   3. 执行更新时， `set version = newVersion `where `version = oldVersion`
   4. 如果版本号不匹配，则更新失败
3. 【注意事项】：
   - 支持的数据类型包括：int, Integer, long, Long, Date, Timestamp, LocalDateTime。
   - 对于整数类型，newVersion 是 oldVersion + 1。
   - `newVersion` 会自动回写到实体对象中。
   - 仅支持 `updateById(id)` 和 `update(entity, wrapper)` 方法。
   - 在 `update(entity, wrapper)` 方法中，`wrapper` 不能复用。
4. 示例：

```sql
#乐观锁：
#1、读取记录时，获取当前的版本号。 where version = 1
#2.在更新记录时，将这个版本号一同传递。 set version = version + 1
-- A
update user set name = "sunm", version = version + 1
where id = 2 and version = 1

-- B 线程抢先完成，这个时候 version = 2，会导致 A 修改失败！
update user set name = "sunm", version = version + 1
where id = 2 and version = 1
```

### MP实现-乐观锁插件

乐观锁是一种并发控制机制，用于确保在更新记录时，该记录未被其他事务修改。MyBatis-Plus 提供了 `OptimisticLockerInnerInterceptor` 插件，使得在应用中实现乐观锁变得简单。

1. 给数据库增加version字段

   

2. 实体类添加对应的字段：@Version

   ```java
   @Version //乐观锁Version注解
   private Integer version;
   ```

3. 注册配置插件：@Configuration

   ```java
   @Configuration  //配置类
   @EnableTransactionManagement    //自动管理事务
   @MapperScan("com.sunm.mapper")  //扫描Mapper文件夹
   public class MyBatisPlusConfig {
       //注册乐观锁插件
       @Bean
       public MybatisPlusInterceptor mybatisPlusInterceptor() {
           MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
           interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
           return interceptor;
       }
   }
   ```

4. 测试乐观锁

   1. 单线程下：

      ```java
      //乐观锁-成功
      @Test
      public void testOptimisticLockSuccess() {
          //1.查询用户
          User user = userMapper.selectById(1L);
          //2.修改用户
          user.setName("新的" + user.getName());
          user.setEmail("11111111@qq.com");
          //3.执行更新操作
          userMapper.updateById(user);
      }
      
      // 测试乐观锁失败！多线程下
      @Test
      public void testOptimisticLocker2(){
          // 线程 1
          User user = userMapper.selectById(1L);
          user.setName("kuangshen111");
          user.setEmail("24736743@qq.com");
          
          // 模拟另外一个线程执行了插队操作
          User user2 = userMapper.selectById(1L);
          user2.setName("kuangshen222");
          user2.setEmail("24736743@qq.com");
          userMapper.updateById(user2);
          
          // 自旋锁来多次尝试提交！
          userMapper.updateById(user); // 如果没有乐观锁就会覆盖插队线程的值！
      }
      ```

      ![image-20240623160810439](https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240623160811066-540677914.png)

   2. 多线程下：

      ```java
      //乐观锁-失败
      @Test
      public void testOptimisticLockFailure() {//线程2插队线程1
          //1.1查询用户
          User user1 = userMapper.selectById(1L);
          //1.2修改用户
          user1.setName("线程1新的" + user1.getName());
          user1.setEmail("11111111@qq.com");
      
          //2.1查询用户
          User user2 = userMapper.selectById(1L);
          //2.2修改用户
          user2.setName("线程2新的" + user2.getName());
          user2.setEmail("11111111@qq.com");
          //2.3执行更新操作
          userMapper.updateById(user2);
      
          //1.3执行更新操作
          userMapper.updateById(user1);   //如果没有乐观锁，就会覆盖插队线程的值
      }
      ```

      ![image-20240623161359063](https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240623161359727-1178646605.png)

      ![image-20240623161642107](https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240623161642732-1671688620.png)

## 分页插件

### 简介

1. 常用的分页手段：

   1. 原始的 limit 进行分页
   2. pageHelper 第三方插件
   3. MP 其实也内置了分页插件！

2. MP分页官网：https://baomidou.com/plugins/pagination/

3. MyBatis-Plus 的分页插件 `PaginationInnerInterceptor` 提供了强大的分页功能，支持多种数据库，使得分页查询变得简单高效。

4. PaginationInnerInterceptor属性：[建议单一数据库类型的均设置 dbType]

   |  属性名  |   类型   | 默认值 |           描述           |
   | :------: | :------: | :----: | :----------------------: |
   | overflow | boolean  | false  | 溢出总页数后是否进行处理 |
   | maxLimit |   Long   |        |     单页分页条数限制     |
   |  dbType  |  DbType  |        |        数据库类型        |
   | dialect  | IDialect |        |        方言实现类        |

5. 注意事项：

   - 生成 countSql 时，如果 left join 的表不参与 where 条件，会将其优化掉。建议在任何带有 left join 的 SQL 中，都给表和字段加上别名。
   - 在使用多个插件时，请将分页插件放到插件执行链的最后面，以避免 COUNT SQL 执行不准确的问题。

### 使用步骤

1. 配置拦截器组件

   ```java
   //分页插件
   @Bean
   public MybatisPlusInterceptor paginationInterceptor() {
       MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
       interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL)); // 如果配置多个插件, 切记分页最后添加
       // 如果有多数据源可以不配具体类型, 否则都建议配上具体的 DbType
       return interceptor;
   }
   ```

2. 直接使用Page对象

   ```java
   public void testSelect() {
       //参数：IPage的实现类，高级条件查询
       //default <P extends IPage<T>> P selectPage(P page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper)
       Page<User> page = new Page<>(1, 5);  //public Page(long current, long size)
   
       userMapper.selectPage(page, null);
       page.getRecords().forEach(System.out::println);
   }
   ```

   

## 代码自动生成器AutoGenerator

- 新版：https://baomidou.com/guides/new-code-generator/

AutoGenerator 是 MyBatis-Plus 的代码生成器，通过 AutoGenerator 可以快速生成 Entity、Mapper、Mapper XML、Service、Controller 等各个模块的代码，极大的提升了开发效率。

测试：

```java
// 演示例子，执行 main 方法控制台输入模块表名回车自动生成对应项目目录中
public class CodeGenerator {

    /**
     * <p>
     * 读取控制台内容
     * </p>
     */
    public static String scanner(String tip) {
        Scanner scanner = new Scanner(System.in);
        StringBuilder help = new StringBuilder();
        help.append("请输入" + tip + "：");
        System.out.println(help.toString());
        if (scanner.hasNext()) {
            String ipt = scanner.next();
            if (StringUtils.isNotBlank(ipt)) {
                return ipt;
            }
        }
        throw new MybatisPlusException("请输入正确的" + tip + "！");
    }

    public static void main(String[] args) {
        // 代码生成器
        AutoGenerator mpg = new AutoGenerator();

        //1. 全局配置
        GlobalConfig gc = new GlobalConfig();
        String projectPath = System.getProperty("user.dir");
        gc.setOutputDir(projectPath + "/src/main/java");	//代码生成位置
        gc.setAuthor("jobob");	//作者注释
        gc.setOpen(false);
        gc.setSwagger2(true); //实体属性 Swagger2 注解
        
        mpg.setGlobalConfig(gc);

        //2. 数据源配置
        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setUrl("jdbc:mysql://localhost:3306/ant?useUnicode=true&useSSL=false&characterEncoding=utf8");
        // dsc.setSchemaName("public");
        dsc.setDriverName("com.mysql.jdbc.Driver");
        dsc.setUsername("root");
        dsc.setPassword("密码");
        
        mpg.setDataSource(dsc);

        //3. 包配置
        PackageConfig pc = new PackageConfig();
        pc.setModuleName(scanner("模块名"));
        pc.setParent("com.baomidou.ant");
        
        mpg.setPackageInfo(pc);

        //4. 自定义配置
        InjectionConfig cfg = new InjectionConfig() {
            @Override
            public void initMap() {
                // to do nothing
            }
        };

        // 如果模板引擎是 freemarker
        String templatePath = "/templates/mapper.xml.ftl";
        // 如果模板引擎是 velocity
        // String templatePath = "/templates/mapper.xml.vm";

        // 自定义输出配置
        List<FileOutConfig> focList = new ArrayList<>();
        // 自定义配置会被优先输出
        focList.add(new FileOutConfig(templatePath) {
            @Override
            public String outputFile(TableInfo tableInfo) {
                // 自定义输出文件名 ， 如果你 Entity 设置了前后缀、此处注意 xml 的名称会跟着发生变化！！
                return projectPath + "/src/main/resources/mapper/" + pc.getModuleName()
                        + "/" + tableInfo.getEntityName() + "Mapper" + StringPool.DOT_XML;
            }
        });
        /*
        cfg.setFileCreate(new IFileCreate() {
            @Override
            public boolean isCreate(ConfigBuilder configBuilder, FileType fileType, String filePath) {
                // 判断自定义文件夹是否需要创建
                checkDir("调用默认方法创建的目录，自定义目录用");
                if (fileType == FileType.MAPPER) {
                    // 已经生成 mapper 文件判断存在，不想重新生成返回 false
                    return !new File(filePath).exists();
                }
                // 允许生成模板文件
                return true;
            }
        });
        */
        cfg.setFileOutConfigList(focList);
        mpg.setCfg(cfg);

        //5. 配置模板
        TemplateConfig templateConfig = new TemplateConfig();

        // 配置自定义输出模板
        //指定自定义模板路径，注意不要带上.ftl/.vm, 会根据使用的模板引擎自动识别
        // templateConfig.setEntity("templates/entity2.java");
        // templateConfig.setService();
        // templateConfig.setController();

        templateConfig.setXml(null);
        mpg.setTemplate(templateConfig);

        //6. 策略配置
        StrategyConfig strategy = new StrategyConfig();
        
        strategy.setNaming(NamingStrategy.underline_to_camel);
        strategy.setColumnNaming(NamingStrategy.underline_to_camel);
        strategy.setSuperEntityClass("你自己的父类实体,没有就不用设置!");
        strategy.setEntityLombokModel(true);
        strategy.setRestControllerStyle(true);
        // 公共父类
        strategy.setSuperControllerClass("你自己的父类控制器,没有就不用设置!");
        // 写于父类中的公共字段
        strategy.setSuperEntityColumns("id");
        strategy.setInclude(scanner("表名，多个英文逗号分割").split(","));
        strategy.setControllerMappingHyphenStyle(true);
        strategy.setTablePrefix(pc.getModuleName() + "_");
        
        mpg.setStrategy(strategy);
        mpg.setTemplateEngine(new FreemarkerTemplateEngine());
        
        //执行
        mpg.execute();
    }

}
```