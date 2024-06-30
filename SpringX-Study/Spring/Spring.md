<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240429134708228-1513611112.png" alt="image-20240429134716858" width="300" />

# Spring概述

## 简介

1. Spring : 春天 --->给软件行业带来了春天
2. 2002年，Rod Jahnson首次推出了Spring框架雏形interface21框架。
3. 2004年3月24日，Spring框架以interface21框架为基础，经过重新设计，发布了1.0正式版。
5. Spring理念 : 使现有技术更加实用 ，本身就是一个大杂烩 , 整合现有的框架技术

官网 : http://spring.io/ 

官方下载地址：https://repo.spring.io/ui/native/libs-milestone-local/org/springframework/spring/ 2024.5.2

GitHub : https://github.com/spring-projects

Maven：

- Spring Framework 6.x - Java 17 (LTS)
- Spring Framework 5.x - Java 8 (LTS) 至 Java 17
- Spring Framework 4.x - Java 6 至 Java 13
- Spring Framework 3.x - Java 5 至 Java 8

```xml
<!-- spring-webmvc版本太高不适配Java1.8也称Java8 -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>6.1.6</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.18</version>
</dependency>

<!--了解-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>6.1.6</version>
</dependency>
```

## 优点

`Spring是一个轻量级的控制反转(IOC)和面向切面(AOP)的容器（框架）`

- Spring是一个开源免费的框架 , 容器
- Spring是一个轻量级的框架 , 非入侵式的
- 控制反转 IOC , 面向切面 Aop
- 对事务的支持 , 对框架整合的支持

## 组成

Spring 框架是一个分层架构，由 7 个定义良好的模块组成。Spring 模块构建在核心容器之上，核心容器定义了创建、配置和管理 bean 的方式 .  

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240429134911155-1876541969.png" alt="image-20240429134920353" width="500" />

组成 Spring 框架的每个模块（或组件）都可以单独存在，或者与其他一个或多个模块联合实现。每个模块的功能如下：

- Spring AOP：通过配置管理特性，Spring AOP 模块直接将面向切面的编程功能 , 集成到了 Spring框架中。所以，可以很容易地使 Spring 框架管理任何支持 AOP的对象。Spring AOP 模块为基于Spring 的应用程序中的对象提供了事务管理服务。通过使用 Spring AOP，不用依赖组件，就可以将声明性事务管理集成到应用程序中。
- Spring ORM：Spring 框架插入了若干个 ORM 框架，从而提供了 ORM 的对象关系工具，其中包括 JDO、Hibernate 和 iBatis SQL Map。所有这些都遵从 Spring 的通用事务和 DAO 异常层次结构。
- Spring Web 模块：Web 上下文模块建立在应用程序上下文模块之上，为基于 Web 的应用程序提供了上下文。所以，Spring 框架支持与 Jakarta Struts 的集成。Web 模块还简化了处理多部分请求以及将请求参数绑定到域对象的工作。
- Spring Web MVC 框架：MVC 框架是一个全功能的构建 Web 应用程序的 MVC 实现。通过策略接口， MVC 框架变成为高度可配置的，MVC 容纳了大量视图技术，其中包括 JSP、Velocity、Tiles、iText和 POI。
- Spring Context上下文：Spring 上下文是一个配置文件，向 Spring 框架提供上下文信息。Spring 上下文包括企业服务，例如 JNDI、EJB、电子邮件、国际化、校验和调度功能。
- Spring DAO：JDBC DAO 抽象层提供了有意义的异常层次结构，可用该结构来管理异常处理和不同数据库供应商抛出的错误消息。异常层次结构简化了错误处理，并且极大地降低了需要编写的异常代码数量（例如打开和关闭连接）。Spring DAO 的面向 JDBC 的异常遵从通用的 DAO 异常层次结构。
- SpringCore核心容器：核心容器提供 Spring 框架的基本功能。核心容器的主要组件是 BeanFactory ，它是工厂模式的实现。 BeanFactory 使用控制反转（IOC） 模式将应用程序的配置和依赖性规范与实际的应用程序代码分开。

## 拓展

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240429135038278-731830493.png" alt="image-20240429135047515" width="500" />

SpringBoot：构建一切。 SpringCloud：协调一切。 SpringCloudDataFlow：连接一切。

Spring Boot

- Spring Boot是 Spring 的一套快速配置开发的脚手架
- 可以基于Spring Boot 快速开发单个微服务;
- 约定大于配置。Spring Boot使用了约束优于配置的理念，很多集成方案已经帮你选择好了，能不配置就不配置 
- Spring Boot专注于快速、方便集成的单个微服务个体
- 学习SpringBoot的前提是需要掌握Spring和SpringMVC

Spring Cloud

- Spring Cloud是基于Spring Boot实现的；
- Spring Cloud关注全局的服务治理框架； 
- Spring Cloud很大的一部分是基于Spring Boot来实现，Spring Boot可以离开Spring Cloud独立使用开发项目，但是Spring Cloud离不开Spring Boot，属于依赖的关系。
- SpringBoot在SpringClound中起到了承上启下的作用，如果你要学习SpringCloud必须要学习SpringBoot。

Spring弊端：

​		“配置地狱”

# IOC基础

## 分析实现

新建一个空白的maven项目，导入spring

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240502124219064-1975720754.png" alt="image-20240502124218967" width="300" />

我们先用我们原来的方式写一段代码

1. 先写一个UserDao接口

   ```java
   public interface UserDao {
       void getUser();
   }
   ```

2. 再去写Dao的实现类

   ```java
   public class UserDaoImpl implements UserDao {
       @Override
       public void getUser() {
           System.out.println("默认获取用户的数据");
       }
   }
   ```

3. 然后去写UserService的接口

   ```java
   public interface UserService {
       void getUser();
   }
   ```

4. 最后写Service的实现类

   ```java
   public class UserServiceImpl implements UserService {
       private UserDao userDao = new UserDaoOracleImpl();
   
       @Override
       public void getUser() {
           userDao.getUser();
       }
   }
   ```

5. 测试一下

   ```java
   public class MyTest {
       public static void main(String[] args) {
           //用户实际调用的是业务层，Dao层不需要他们接触
           UserServiceImpl userService = new UserServiceImpl();
           userService.getUser();
       }
   }
   ```

这是我们原来的方式 , 那我们现在修改一下。

1. 把Userdao的实现类增加一个

   ```java
   public class UserDaoMysqlImpl implements UserDao {
       @Override
       public void getUser() {
           System.out.println("Mysql获取用户数据");
       }
   }
   ```

2. 紧接着我们要去使用MySql的话 , 我们就需要去service实现类里面修改对应的实现 

   ```java
   public class UserServiceImpl implements UserService {
   
       //private UserDao userDao = new UserDaoImpl();
       private UserDao userDao = new UserDaoMysqlImpl();
   
       @Override
       public void getUser() {
           userDao.getUser();
       }
   }
   ```

3. 假设, 我们再增加一个Userdao的实现类 .

   ```java
   public class UserDaoOracleImpl implements UserDao {
       @Override
       public void getUser() {
           System.out.println("Oracle获取数据");
       }
   }
   ```

   那么我们要使用Oracle , 又需要去service实现类里面修改对应的实现 . 

   假设我们的这种需求非常大 , 这种方式就根本不适用了, 每次变动 , 都需要修改大量代码 . 这种设计的耦合性太高

4. 那我们如何去解决呢 ?

   我们可以在需要用到他的地方 , 不去实现它 , 而是留出一个`接口` , 利用set , 我们去代码里修改下 .

   ```java
   public class UserServiceImpl implements UserService {
       //private UserDao userDao = new UserDaoImpl();
       //private UserDao userDao = new UserDaoMysqlImpl();
       //private UserDao userDao = new UserDaoOracleImpl();
       
       private UserDao userDao;
       //利用set进行动态实现值的注入
       public void setUserDao(UserDao userDao){
           this.userDao = userDao;
       }
   
       @Override
       public void getUser() {
           userDao.getUser();
       }
   }
   ```

5. 现在去我们的测试类里 , 进行测试 ;  

   ```java
   public class MyTest {
       public static void main(String[] args) {
           //用户实际调用的是业务层，Dao层不需要他们接触
           UserServiceImpl userService = new UserServiceImpl();
           //用户可以指定实现类
           userService.setUserDao(new UserDaoMysqlImpl());
           userService.getUser();
       }
   }
   ```

- 他们已经发生了根本性的变化 , 很多地方都不一样了 . 

- 以前所有东西都是由程序去主动进行控制创建，控制权在程序手上；而现在是由我们自行控制创建对象 , 把主动权交给了调用者 ，程序变成了被动的接收对象。

- 程序不用去管怎么创建,怎么实现了。 它只负责提供一个接口

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240502134546379-2144861512.png" alt="image-20240502134545363" width="300" />

这种思想 , 从本质上解决了问题 , 我们程序员不再去管理对象的创建了 , 更多的去关注业务的实现 . 耦合性大大降低 . 这也就是`IOC的原型` !

## IOC本质

控制反转IoC(Inversion of Control)，是一种设计思想，DI(依赖注入)是实现IOC的一种方法，也有人认为DI只是IoC的另一种说法。

没有IoC的程序中 , 我们使用面向对象编程 , 对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方。个人认为所谓控制反转就是：获得依赖对象的方式反转了。  

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240429135418046-2035195182.png" alt="image-20240429135427059" width="500" />

IoC是Spring框架的核心内容，使用多种方式完美的实现了IoC，可以使用XML配置，也可以使用注解，新版本的Spring也可以零配置实现IoC。

Spring容器在初始化时先读取配置文件，根据配置文件或元数据创建与组织对象存入容器中，程序使用时再从Ioc容器中取出需要的对象

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240429135426276-1441238491.png" alt="image-20240429135435055" width="300" />

采用XML方式配置Bean的时候，Bean的定义信息是和实现分离的，而采用注解的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。

**控制反转是一种通过描述（XML或注解）并通过第三方去生产或获取特定对象的方式。**

**在Spring中实现控制反转的是IoC容器，其实现方法是依赖注入（Dependency Injection,DI）。**

# HelloSpring

## 导入jar包

注 : spring 需要导入commons-logging进行日志记录 . 我们利用maven , 他会自动下载对应的依赖项 .  

## 编写代码

1. 编写一个Hello实体类

   ```java
   public class Hello {
       private String str;
       public Hello() {
       }
       public Hello(String str) {
           this.str = str;
       }
       public String getStr() {
           return str;
       }
       public void setStr(String str) {
           this.str = str;
       }
       @Override
       public String toString() {
           return "Hello{" +
                   "str='" + str + '\'' +
                   '}';
       }
   }
   ```

2. 编写我们的spring文件 , 这里我们命名为beans.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
   		https://www.springframework.org/schema/beans/spring-beans.xsd">
       
       <!--使用Spring来创建对象，在Spring这些都称为Bean
       类型 变量名 = new 类型()  Hello hello = new Hello()
           bean = 对象  new Hello()
           id = 变量名
           class = new 的对象
           property = 相当于给对象中的属性设置一个值
       
       -->
       <bean id="hello" class="com.sun.pojo.Hello">
           <property name="str" value="Spring"/>
       </bean>
   
   </beans>
   ```

3. 我们可以去进行测试了 .

   ```java
   public class MyText {
       public static void main(String[] args) {
           //1.获取Spring的上下文对象
           //参数：configLocation--配置文件，可以多个
           ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
   
           //2.使用对象   我们现在的对象都在Spring中管理了，要使用，直接去里面取出来就可以了
           Hello hello = (Hello) context.getBean("hello");
           System.out.println(hello.toString());
       }
   }
   ```

## 思考

1. Hello 对象是谁创建的 ? 【 hello 对象是由Spring创建的 】  

2. Hello 对象的属性是怎么设置的 ? 【hello 对象的属性是由Spring容器设置的 】

3. 这个过程就叫控制反转 
   - 控制 : 谁来控制对象的创建 , 传统应用程序的对象是由程序本身控制new创建的；使用Spring后 , 对象是由Spring来bean创建的
   - 反转 : 程序本身不创建对象 , 而变成被动的接收对象 .

4. 依赖注入 : 就是利用set方法来进行注入的。必须要有set方法

5. IOC是一种编程思想，由主动的编程变成被动的接收。`对象由Spring来创建，管理，装配。`

6. 可以通过new  ClassPathXmlApplicationContext去浏览一下底层源码

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240502135414274-1501051533.png" alt="image-20240502135412513" width="300" />

## 修改案例一

我们在案例一中， 新增一个Spring配置文件beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="mysqlImpl" class="com.sun.dao.UserDaoMysqlImpl"/>
    <bean id="OracleImpl" class="com.sun.dao.UserDaoOracleImpl"/>

    <!--
    ref：引用spring容器中创建好的对象
    value：具体的值，基本数据类型
    -->
    <bean id="UserServiceImpl" class="com.sun.service.UserServiceImpl">
        <property name="userDao" ref="mysqlImpl"/>
    </bean>

</beans>
```

测试！

```java
public class MyTest {
    public static void main(String[] args) {
        /*
        UserServiceImpl userService = new UserServiceImpl();
        userService.setUserDao(new UserDaoMysqlImpl());
        userService.getUser();
         */
        //1.获取ApplicationContext：拿到Spring的容器
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");

        //2.需要什么，就直接get
        UserServiceImpl serviceImpl = (UserServiceImpl) context.getBean("UserServiceImpl");
        serviceImpl.getUser();
    }
}
```

- 我们彻底不用再程序中去改动了 , 要实现不同的操作 , 只需要在xml配置文件中进行修改
- 所谓的IoC,一句话搞定 : 对象由Spring 来创建 , 管理 , 装配 !

# IOC创建对象方式

## 无参构造方法创建-默认

1. User.java  

   ```java
   public User() {
       System.out.println("User的无参构造");
   }
   ```

2. beans.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <bean id="user" class="com.sun.pojo.User">
           <property name="name" value="张三"/>
       </bean>
   </beans>
   ```

3. 测试类

   ```java
   public class MyTest {
       public static void main(String[] args) {
           ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
   
           User user = (User) context.getBean("user");
           user.show();
       }
   }
   /*输出结果：
       User的无参构造
       name = 张三
   */
   ```

结果可以发现，在调用show方法之前，User对象已经通过无参构造初始化了！

## 有参构造方法创建

有参构造需要修改xml文件，否则会报错- - - 基于构造函数的依赖注入

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240502143436955-228677078.png" alt="image-20240502143436118" width="400" />

1. User . java  

   ```java
   public User(String name) {
       this.name = name;
   }
   ```

2. beans.xml 有三种方式编写

   通过下标赋值- - 构造函数变元索引

   - 可以使用index属性显式指定构造函数参数的索引
   - 除了解决多个简单值的二义性之外， 解决了一个构造函数有两个相同类型的参数时的歧义
   - 索引从0开始

   ```xml
   <bean id="user" class="com.sun.pojo.User">
       <!--<property name="name" value="张三"/>-->
       <constructor-arg index="0" value="李四"/>
   </bean>
   ```

   通过类型赋值- - 构造函数参数类型匹配

   - 如果多个参数类型一致，无法区别，不建议使用
   - type：普通数据类型可以直接写，引用类型需要写全限定名(包名 + 类型)

   ```xml
   <bean id="user" class="com.sun.pojo.User">
       <constructor-arg type="java.lang.String" value="王五"/>
   </bean>
   ```

   通过参数名设置- - 构造函数参数名

   ```xml
   <bean id="user" class="com.sun.pojo.User">
       <constructor-arg name="name" value="赵六"/>
   </bean>
   ```

3. 测试

   

## 结论

- Spring容器context，在注册bean时就已经实例化了对象，需要用到对象时直接通过ApplicationContext的getBean获取就可以了。而且获取到的都是同一个对象，是同一份实例。
- 在配置文件xml加载的时候。其中管理的对象都已经初始化了！

# Spring配置

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240502145818713-1527818670.png" alt="image-20240502145817120" width="300" />

## 别名

alias 设置别名 , 为bean设置别名 , 可以设置多个别名，也可以通过别名获取到这个对象

```xml
<alias name="user" alias="bieming"/>
```

测试

```java
public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");

        User user = (User) context.getBean("user");
        user.show();
        User bieming = (User) context.getBean("bieming");
        bieming.show();
    }
}
/*测试结果：
UserT被创建了！
name = 赵六
name = 赵六
*/
```

## Bean的配置

- `bean`就是java对象，由Spring创建和管理
- `id` 是bean的标识符，要唯一
  - 如果没有配置id，name就是默认标识符
  - 如果配置id，又配置了name，那么name是别名
- `name`可以设置多个别名。可以用逗号、分号、空格隔开
- 如果不配置id和name，可以根据applicationContext.getBean(.class)获取对象
- `class`是bean的全限定名 = 包名 + 类名

```xml
<bean id="userT" class="com.sun.pojo.UserT" name="userT2,u2 u3">
    <property name="name" value="田七"/>
</bean>
```

## import

一般团队的合作通过import来实现，他可以将多个配置文件，导入合并为一个

```xml
<import resource="{path}/beans.xml"/>
```

# 依赖注入DI

依赖注入（Dependency Injection,DI）：Set注入

- 依赖 : 指Bean对象的创建依赖于容器。 Bean对象的依赖资源 
- 注入 : 指Bean对象中的所有属性由容器来注入。所依赖的资源 , 由容器来设置和装配

什么是依赖注入？

1. 依赖注入 (DI) 是一个过程
2. 对象仅通过构造函数参数、工厂方法的参数或对象实例构造后设置的属性来定义其依赖项（即与它们一起工作的其他对象）。
3. 从工厂方法返回。然后，容器在创建 bean 时注入这些依赖项。
4. 这个过程从根本上来说是 bean 本身的逆过程（因此得名“控制反转”），通过使用类的直接构造或服务定位器模式自行控制其依赖项的实例化或位置。

## 基于构造函数的依赖注入

见上述- - IOC创建对象方式 - - 有参构造方法创建- - beans.xml 有三种方式编写

什么是基于构造函数的依赖注入？

- 基于构造函数的DI是：容器调用一个具有多个参数并且每个参数代表一个依赖项的构造器。
- 用具体的参数调用一个`static`工厂方法 来构造bean几乎是等价的。
- 并且对于处理构造函数和`static`工厂方法的参数是类似的。

## 基于Setter的依赖注入【重点】

### 简介

什么是基于Setter的依赖注入？

- 基于setter的DI是：在调用无参数构造函数[或无参数`static`工厂方法]后， 容器调用setter方法来实例化bean

- 要求被注入的属性 , 必须有set方法 
- set方法的方法名由set + 属性首字母大写 
- 如果属性是boolean类型, 没有set方法 , 是 is

### 环境搭建

1. 复杂类型Address.java

   ```java
   public class Address {
       private String address;
   	//省略了get/set/toString方法 实际需要加上
   }
   ```

2. 真实测试对象Student.java  

   ```java
   public class Student {
       //普通类型
       private String name;
       //引用类型
       private Address address;
       //数组
       private String[] books;
       //集合
       private List<String> hobbys;
       private Map<String, String> card;
       private Set<String> games;
       //配置类
       private Properties info;
       //空指针
       private String wife;
       
       //省略了get/set/toString方法 实际需要加上
   }
   ```

3. beans.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <bean id="student" class="com.sun.pojo.Student">
           <property name="name" value="梨花"/>
       </bean>
   </beans>
   ```

4. 测试类

   ```java
   public class MyTest {
       public static void main(String[] args) {
           ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
           Student student = (Student) context.getBean("student");
           System.out.println(student.getAddress()); //null
       }
   }
   ```

### 注入

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240502162308074-2073171824.png" alt="image-20240502162306909" width="300" />

1. 常量注入

   ```xml
   <!--name-vlue注入-->
   <bean id="student" class="com.sun.pojo.Student">
       <property name="name" value="梨花"/>
   </bean>
   
   //普通类型
   private String name;
   ```

   ```java
   public class MyTest {
       public static void main(String[] args) {
           ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
           Student student = (Student) context.getBean("student");
           System.out.println(student.getName());
       }
   }
   /*
   测试结果：梨花
   */
   ```

2. Bean注入

   注意点：这里的值是一个引用，ref

   ```xml
   <bean id="addressb" class="com.sun.pojo.Address">
       <property name="address" value="in home"/>
   </bean>
   
   <!--对象类型，ref注入-->
   <bean id="student" class="com.sun.pojo.Student">
       <property name="name" value="梨花"/>
       <property name="address" ref="addressb"/>
   </bean>
   
       //引用类型
       private Address address;
   ```

3. 数组注入

   ```xml
   <!--array-value-->
   <property name="books">
       <array>
           <value>红楼梦</value>
           <value>水浒传</value>
           <value>三国演义</value>
           <value>西游记</value>
       </array>
   </property>
   
    	//数组
       private String[] books;
   ```

4. List注入

   ```xml
   <!--list-value-->
   <property name="hobbys">
       <list>
           <value>听歌</value>
           <value>打游戏</value>
           <value>写作</value>
       </list>
   </property>
   
       //集合
       private List<String> hobbys;
   ```

5. Map注入

   ```xml
   <!--map-entry(key-value)-->
   <property name="card">
       <map>
           <entry key="身份证" value="123456789"/>
           <entry key="银行卡" value="987654321"/>
       </map>
   </property>
   
       private Map<String, String> card;
   ```

6. set注入

   ```xml
   <!--set-value-->
   <property name="games">
       <set>
           <value>LOL</value>
           <value>AOA</value>
           <value>BOB</value>
       </set>
   </property>
   
       private Set<String> games;
   ```

7. Null注入

   ```xml
   <!--不存在：null-->
   <!--空值：value=""-->
   <property name="wife">
       <null/>
   </property>
   
       //空指针
       private String wife;
   ```

8. Properties注入

   ```xml
   <!--props-prop(key)
   value值写在尖括号中间
   -->
   <property name="info">
       <props>
           <prop key="学号">2112080214</prop>
           <prop key="性别">女</prop>
           <prop key="姓名">小明</prop>
       </props>
   </property>
   
       //配置类
       private Properties info;
   ```

测试结果

```java
Student{
    name='梨花', 
    address=Address{address='in home'}, 
    books=[红楼梦, 水浒传, 三国演义, 西游记], 
    hobbys=[听歌, 打游戏, 写作], 
    card={身份证=123456789, 银行卡=987654321}, 
    games=[LOL, AOA, BOB], 
    info={学号=2112080214, 性别=女, 姓名=小明}, 
    wife='null'
}
```

## 拓展方式注入

User.java ： 【注意：这里没有有参构造器！】  

```java
public class User {
    private String name;
    private int age;
    //省略了get/set/toString方法，现实需加上
}
```

### P命名空间注入（基于Setter的DI）

- 需要在头文件中加入xml约束文件：xmlns:p="http://www.springframework.org/schema/p"
- 可以直接注入属性的值：property

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--p命名空间注入，可以直接注入属性的值：property-->
    <bean id="user" class="com.sun.pojo.User" p:name="小红" p:age="18"/>
</beans>
```

### c 命名空间注入（基于构造函数的DI）

- 需要在头文件中加入xml约束文件：xmlns:c="http://www.springframework.org/schema/c"
- 需要写有参构造
- 有参构造会覆盖无参构造，还需要手动加上无参构造，否则bean会报错
- 可以通过构造器注入属性的值：construct-args

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--c命名空间注入，可以通过构造器注入属性的值：construct-args-->
    <bean id="user2" class="com.sun.pojo.User" c:age="20" c:name="小兰"/>
</beans>
```

测试

```java
@Test
public void test() {
    ApplicationContext context = new ClassPathXmlApplicationContext("userbeans.xml");
    User user = context.getBean("user2", User.class);
    System.out.println(user);
}
```

## Bean的作用域

### 简介

在Spring中，那些组成应用程序的主体及由Spring IoC容器所管理的对象，被称之为bean。简单地讲， bean就是由IoC容器初始化、装配及管理的对象

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240429140146607-947347128.png" alt="image-20240429140155175" style="zoom:50%;" />

| 范围          | 描述                                                         |
| :------------ | :----------------------------------------------------------- |
| singleton单例 | （默认）将单个 bean 定义范围限定为每个 Spring IoC 容器的单个对象实例。 |
| prototype原型 | 将单个 bean 定义的范围限定为任意数量的对象实例。             |
| request       | 将单个 bean 定义的范围限定为单个 HTTP 请求的生命周期。也就是说，每个 HTTP 请求都有自己的 Bean 实例，该实例是根据单个 Bean 定义创建的。仅在支持 Web 的 Spring 上下文中有效`ApplicationContext`。 |
| session       | 将单个 bean 定义的范围限定为 HTTP 的生命周期`Session`。仅在支持 Web 的 Spring 上下文中有效`ApplicationContext`。 |
| application   | 将单个 bean 定义的范围限定为`ServletContext`.仅在支持 Web 的 Spring 上下文中有效`ApplicationContext`。 |
| websocket     | 将单个 bean 定义的范围限定为`WebSocket`.仅在支持 Web 的 Spring 上下文中有效`ApplicationContext`。 |

- request、session、application和websocket作用域仅在基于web的应用中使用（不必关心你所采用的是什么web应用框架），
- 只能用在基于web的Spring ApplicationContext环境

### Singleton-默认

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240502173356973-1681678890.png" alt="image-20240502173355744" width="500" />

仅管理单例 bean 的`一个共享实例`，并且对具有与该 bean 定义匹配的一个或多个 ID 的 bean 的所有请求都会导致 Spring 容器返回一个特定的 bean 实例。

换句话说，当您定义一个 bean 定义并将其范围限定为单例时，Spring IoC 容器将创建由该 bean 定义的对象的一个实例,不管你是否使用，他都存在了。该单个实例存储在此类单例 bean 的缓存中，并且该命名 bean 的所有后续请求和引用都会返回缓存的对象。

Spring 单例的范围最好描述为`每个容器和每个 bean`。这意味着，如果您在单个 Spring 容器中为特定类定义一个 bean，则 Spring 容器会创建该 bean 定义所定义的类的一个且唯一的一个实例。

注意，Singleton作用域是Spring中的缺省作用域。要在XML中将bean显式的定义成singleton，可以这样配置：

```xml
<bean id="user2" class="com.sun.pojo.User" c:age="20" c:name="小兰" scope="singleton"/>
```

测试：

```java
@Test
public void test() {
    ApplicationContext context = new ClassPathXmlApplicationContext("userbeans.xml");
    User user = context.getBean("user2", User.class);
    User user2 = context.getBean("user2", User.class);
    //getBean的两个User是相等的，即同一个
    System.out.println(user == user2);  
    //true
}
```

### Prototype

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240502173434697-78483707.png" alt="image-20240502173432912" width="500" />



Prototype是原型类型，它在我们创建容器的时候并没有实例化，而是`每次发出对该特定 bean 的请求时创建一个新的 bean 实例`，而且我们`每次获取到的对象都不是同一个对象`。也就是说，该 bean 被注入到另一个 bean 中，或者您通过getBean()容器上的方法调用来请求它。

通常，应该对有状态的bean应该使用prototype作用域，而对无状态的bean则应该使用singleton作用域。（数据访问对象（DAO）通常不配置为原型，因为典型的 DAO 不保存任何会话状态。）

在XML中将bean定义成prototype，可以这样配置：  

```xml
<bean id="user2" class="com.sun.pojo.User" c:age="20" c:name="小兰" scope="prototype"/>
```

```java
@Test
public void test() {
    ApplicationContext context = new ClassPathXmlApplicationContext("userbeans.xml");
    User user = context.getBean("user2", User.class);
    User user2 = context.getBean("user2", User.class);
    //getBean的两个User是相等的，即同一个
    System.out.println(user == user2);  
    //false
}
```

### request、session等

request、session、application和websocket作用域仅在`基于web的应用中使用`（不必关心你所采用的是什么web应用框架）

#### Request

​		当一个bean的作用域为Request，表示在一次HTTP请求中，一个bean定义对应一个实例；即每个HTTP请求都会有各自的bean实例，它们依据某个bean定义创建而成。该作用域仅在基于web的Spring ApplicationContext情形下有效。

​		针对每次HTTP请求，Spring容器会根据loginAction bean的定义创建一个全新的LoginAction bean实例，且该loginAction bean实例仅在当前HTTP request内有效，因此可以根据需要放心的更改所建实例的内部状态，而其他请求中根据loginAction bean定义创建的实例，将不会看到这些特定于某个请求的状态变化。当处理请求结束，request作用域的bean实例将被销毁。

#### Session

​		当一个bean的作用域为Session，表示在一个HTTP Session中，一个bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效。考虑下面bean定义：

​		针对某个HTTP Session，Spring容器会根据userPreferences bean定义创建一个全新的userPreferences bean实例，且该userPreferences bean仅在当前HTTP Session内有效。与request作用域一样，可以根据需要放心的更改所创建实例的内部状态，而别的HTTP Session中根据userPreferences创建的实例，将不会看到这些特定于某个HTTP Session的状态变化。当HTTP Session最终被废弃的时候，在该HTTP Session作用域内的bean也会被废弃掉。

# Bean的自动装配

## 简介

Spring中bean有三种装配机制，分别是：

1. 在xml中显式配置；(手动装配：每一个bean的属性都要手动设置，否则就会为空)
2. 在java中显式配置；(手动)
3. `隐式的bean发现机制和自动装配`【重要】
   - 基于注解的容器配置：https://docs.spring.io/spring-framework/reference/core/beans/annotation-config.html
   - 自动装配-不设置，也可以自动找到
   - 自动装配是使用spring满足bean依赖的一种方法
   - spring会在应用上下文中为某个bean寻找其依赖的bean。

Spring的自动装配需要从两个角度来实现，或者说是两个操作：

1. 组件扫描(component scanning)：spring会自动发现应用上下文中所创建的bean；
2. 自动装配(autowiring)：spring自动满足bean之间的依赖，也就是我们说的IoC/DI；

组件扫描和自动装配组合发挥巨大威力，使的显示的配置降低到最少。推荐不使用自动装配xml配置 , 而使用注解 .

## 测试环境搭建

一个人有两个宠物

1. 新建一个项目

2. 新建两个实体类，Cat Dog 都有一个叫的方法

   ```java
   public class Dog {
       public void shout() {
           System.out.println("汪！");
       }
   }
   public class Cat {
       public void shout() {
           System.out.println("喵~");
       }
   }
   ```

3. 新建一个用户类 User

   ```java
   public class People {
       private Cat cat;
       private Dog dog;
       private String name;
       //没粘贴get/set/toString方法
   }
   ```

4. 编写Spring配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <bean id="cat" class="com.sun.pojo.Cat"/>
       <bean id="dog" class="com.sun.pojo.Dog"/>
       <bean id="people" class="com.sun.pojo.People">
           <property name="name" value="芍药"/>
           <property name="cat" ref="cat"/>
           <property name="dog" ref="dog"/>
       </bean>
   </beans>
   ```

5. 测试

   ```java
   public class MyTest {
       public static void main(String[] args) {
           ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
           People people = context.getBean("people", People.class);
           people.getDog().shout();
           people.getCat().shout();
       }
   }
   /*
   汪！
   喵~
   */
   ```

## byName

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240502180812387-941808760.png" alt="image-20240502180809444" width="400" />

使用autowire byName首先需要保证：

- 所有bean的id唯一，并且这个bean需要和自动注入的属性的set方法后面的名字一致。
- 不一致会报空指针异常java.lang.NullPointerException

测试：

1. 修改bean配置，增加一个属性 autowire="byName"，按名称自动装配

   ```xml
   <bean id="cat" class="com.sun.pojo.Cat"/>
   <bean id="dog" class="com.sun.pojo.Dog"/>
   
   <bean id="people" class="com.sun.pojo.People" autowire="byName">
       <property name="name" value="芍药"/>
   </bean>
   ```

2. 再次测试，结果依旧成功输出！

3. 我们将 cat 的bean id修改为 catXXX

4. 再次测试， 执行时报空指针java.lang.NullPointerException。因为按byName规则找不对应set方法，真正的setCat就没执行，对象就没有初始化，所以调用时就会报空指针错误。

byName原理：

当一个bean节点带有 autowire byName的属性时。

- 将查找其类中所有的set方法名，例如setCat，获得将set去掉并且首字母小写的字符串，即cat。
- 然后去spring容器中寻找是否有此字符串名称id的对象bean。
- 如果有，就取出注入；如果没有，就报空指针异常

## byType

使用autowire byType首先需要保证：

- 所有bean的class类型唯一，并且这个bean需要和自动注入的属性的类型一致。
- 不一致会报异常NoUniqueBeanDefinitionException
- 会自动在容器上下文中查找，和自己对象属性类型相同的bean
- 将id属性修改或者去掉，也不影响结果。

测试：

1. 将user的bean配置修改一下 ： autowire="byType"，按类型自动装配

2. 测试，正常输出

   ```xml
   <bean id="cat" class="com.sun.pojo.Cat"/>
   <bean id="dog111" class="com.sun.pojo.Dog"/>
   <bean id="people" class="com.sun.pojo.People" autowire="byType">
       <property name="name" value="芍药"/>
   </bean>
   ```

3. 再注册一个cat 的bean对象！

4. 测试，报错：NoUniqueBeanDefinitionException

## 使用注解

jdk1.5开始支持注解，spring2.5开始全面支持注解。

### 要使用注解须知

1. 导入约束：xmlns:context="http://www.springframework.org/schema/context"

2. 添加架构位置：http://www.springframework.org/schema/context和https://www.springframework.org/schema/context/spring-context.xsd
3. 配置开启属性注解支持：<context:annotation-config/

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
		https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context
		https://www.springframework.org/schema/context/spring-context.xsd">

	<context:annotation-config/>

</beans>
```

### @Autowired和@Qualifier【常用】

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
//spring特有
```

@Autowired

- 可以应用于属性上，也可以用于set方法上。
- 应用@Autowired，甚至可以去掉set方法。前提是你这个自动装配的属性在IOC(Spring)容器中存在
- @Autowired是优先按类型byType自动装配的，其次按照名字byName。

@Qualifier

- 类型和名字都没有对应的之后，单独使用@Autowired会报错
- 可以利用注解@Qualifier(value = "dog2")指定装配的bean-id。【注意指定的类型要一致】
- @Qualifier不能单独使用。

【科普】

```java
@Autowired(required=false) 
/*说明： false，对象可以为null，但必须有定义；
		true，对象必须存对象，不能为null
*/
public @interface Autowired {
    boolean required() default true;
}

@Nullable
//说明：这个字段可以为null
public People(@Nullable String name) {
    this.name = name;
}
```

测试：

1. 将User类中的set方法去掉，使用@Autowired注解

   ```java
   public class People {
       @Autowired
       private Cat cat;
       @Autowired
       @Qualifier(value = "dog2")
       private Dog dog;
       private String name;
   
       @Override
       public String toString() {
           return "People{" +
                   "cat=" + cat +
                   ", dog=" + dog +
                   ", name='" + name + '\'' +
                   '}';
       }
       public Cat getCat() {
           return cat;
       }
   
       public Dog getDog() {
           return dog;
       }
       
       public String getName() {
           return name;
       }
       public void setName(String name) {
           this.name = name;
       }
   }
   ```

2. 此时配置文件内容

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          http://www.springframework.org/schema/context/spring-context.xsd">
   
       <!--开启注解的支持-->
       <context:annotation-config/>
   
       <bean id="cat" class="com.sun.pojo.Cat"/>
       <bean id="dog1" class="com.sun.pojo.Dog"/>
       <bean id="dog2" class="com.sun.pojo.Dog"/>
       <bean id="people" class="com.sun.pojo.People"/>
   </beans>
   ```

3. 测试，成功输出结果！

### @Resource

```java
import javax.annotation.Resource;
//java原生

@Resource(name = "dog1")
private Dog dog;
```

- @Resource如有指定的name属性，先按该属性进行byName方式查找装配；
- 其次再进行默认的byName方式进行装配
- 如果以上都不成功，则按byType的方式自动装配。
- 都不成功，则报异常。

### Autowired与Resource异同

相同点：

- @Autowired与@Resource都可以用来装配bean。
- 都可以写在字段上，或写在setter方法上。
- 作用相同都是用注解方式注入对象

不同点：

1. @Autowired（属于spring规范）
   - 默认按类型byType装配
   - 默认情况下必须要求依赖对象必须存在，如果要允许null 值，可以设置它的required属性为false，如：@Autowired(required=false) 
   - 如果我们想使用指定名称装配可以结合@Qualifier注解进行使用，如：@Qualifier( value =“ ”)
2. @Resource（属于J2EE复返）
   - 默认按照名称byName进行装配
   - 名称可以通过name属性进行指定。如：@Resource(name = "dog1")
   - 如果name属性一旦指定，就只会按照名称进行装配。
   - 如果没有指定name属性，当注解写在字段上时，默认取字段名进行按照名称查找，如果注解写在setter方法上默认取属性名进行装配。 
   - 当找不到与名称匹配的bean时才按照类型进行装配。
3. @Autowired先byType，@Resource先byName。

# 使用注解开发

## 说明

### 导aop包

在spring4之后，想`要使用注解形式`，必须要`引入aop的包`。spring-webmvc会帮助我们自动导入。

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240502194934168-592865987.png" alt="image-20240502194931925" width="300" />

### 要使用注解须知

1. 导入约束：xmlns:context="http://www.springframework.org/schema/context"
2. 添加架构位置：http://www.springframework.org/schema/context和http://www.springframework.org/schema/context/spring-context.xsd
3. 配置开启属性注解支持：<context:annotation-config/

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">
    
    <!--指定要扫描的包，这个包下的注释就会生效-->
    <context:component-scan base-package="com.sun.pojo"/>
    <context:annotation-config/>
</beans>
```

## Bean的实现-@Component

@Component

- 组件
- 放在类上
- 说明这个类被Spring管理了，就是bean
- 默认id为类名的小写

我们之前都是使用 bean 的标签进行bean注入，但是实际开发中，我们一般都会使用注解！

1. 配置扫描哪些包下的注解

   ```xml
   <!--指定要扫描的包，这个包下的注释就会生效-->
   <context:component-scan base-package="com.sun.pojo"/>
   ```

2. 在指定包下编写类，增加注解

   ```java
   //等价于 <bean id="user" class="com.sun.pojo.User"/>
   //Component：组件	默认id为类名的小写字母
   @Component
   public class User {
       public String name = "小樱";
   }
   ```

3. 测试

   ```java
   public class MyTest {
       public static void main(String[] args) {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           User user = context.getBean("user", User.class);
           System.out.println(user.name);
           //小樱
       }
   }
   ```

## 属性注入-@Value

使用注解注入属性

1. 可以不用提供set方法，直接在属性名上添加@value("值")

   ```java
   @Component
   public class User {
       /*相当于
       <bean id="user" class="com.sun.pojo.User">
       	<property name="name" value="小英"/>
       </bean>
       */
       @Value("小英")
       public String name;
   }
   ```

2. 如果提供了set方法，在set方法上添加@value("值");

   ```java
   @Component
   public class User {
       public String name;
   	//相当于<property name="name" value="小英"/>
       @Value("小英")
       public void setName(String name) {
           this.name = name;
       }
   }
   ```

## @Component衍生注解

@Component三个衍生注解，功能和@Component一样，都是代表将某个类注册到Spring容器中，装配Bean

为了更好的在Web开发中按照mvc三层架构分层，Spring可以使用其它三个注解

- dao层：@Repository 仓库
- service层：@Service
- controller层：@Controller

写上这些注解，就相当于将这个类交给Spring管理装配了！

## 自动装配注解

见上述- - Bean的自动装配 - - 使用注解

- @Autowired：优先byType。
- @Qualifier：和@Autowired组合使用，@Autowired(value = “”)指定装配的bean-id名字
- @Nullable：说明标记的自动可以为Null
- @Resource：优先byName。@Resource(name =“”)

## 作用域-@scope

@scope

- @Scope("singleton")：默认的，Spring会采用单例模式创建这个对象。关闭工厂 ，所有的对象都会销毁。
- prototype：多例模式。关闭工厂 ，所有的对象不会销毁。内部的垃圾回收机制会回收

## 小结-XML与注解

### XML与注解比较

1. XML可以适用任何场景 ，结构清晰，维护方便
2. 注解不是自己提供的类使用不了，开发简单方便，维护相对复杂，修改麻烦

### xml与注解整合开发 

【推荐】

1. xml用来管理Bean
2. 注解只负责完成属性注入

### 注解

1. 在使用过程中，唯一需要注意的问题是`必须让注解生效，需要开启注解的支持`。
2. 可以不用扫描，扫描是为了类上的注解
3. 该语句的作用

   - 进行注解驱动注册，从而使注解生效
   - 用于激活那些已经在spring容器里注册过的bean上面的注解，也就是显示的向Spring注册
   - 如果不扫描包，就需要手动配置bean
   - 如果不加注解驱动，则注入的值为null！

```xml
<!--指定要扫描的包，这个包下的注释就会生效-->
<context:component-scan base-package="com.sun.pojo"/>
<context:annotation-config/>
```

## 基于Java的容器配置

https://docs.spring.io/spring-framework/reference/core/beans/java.html

应用上下文ApplicationContext具有多个实现类，除了通过XML还可以通过注解配置。

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240502205221378-1203377272.png" alt="image-20240502205201319" width="400" />

- 现在可以完全不使用Spring的xml配置，全权交给Java来做
- JavaConfig 原来是 Spring 的一个子项目，它通过 Java 类的方式提供 Bean 的定义信息
- 在 Spring4 之后， JavaConfig 已正式成为 Spring的核心功能 。

### 使用

1. 编写实体类User.java

   - 类名上加注解`@Component`，说明这个类被Spring接管，注册到了容器中
   - 属性名上加注解`@Value("用户")`，给属性注入值

2. 编写配置类MyConfig.java

   - 类名上加注解`@Configuration`，说明这是一个配置类，和我们之前的applicationContext.xml配置文件一样

   - @Configuration本质上也是@Component，和User一样会被Spring接管，也会被注册到容器中

     ```java
     @Component
     public @interface Configuration {
         @AliasFor(
             annotation = Component.class
         )
         String value() default "";
     
         boolean proxyBeanMethods() default true;
     }
     ```

   - 类名上加`@ComponentScan("com.sun.pojo")`，说明指定要扫描的包，这个包下的注释就会生效。相当于<context:component-scan base-package="com.sun.pojo"/>
   - 类名上加`@Import(MyConfig2.class)`，说明融合了MyConfig2.class的配置
   - 方法名上加`@Bean`，相当于我们之前写的一个bean标签
     1. 方法的名，相当于bean标签中的id属性
     2. 方法中的返回值，相当于bean标签中的class属性
     3. return后的对象，就是返回要注入到bean的对象

3. 编写测试类MyTest.java

   - 如果完全使用了配置类的方式去做，我们就只能通过new AnnotationConfigApplicationContext()来获取容器
   - 获取容器的参数为配置类的class对象new AnnotationConfigApplicationContext(MyConfig.class)

### 测试

1. 编写一个实体类

   ```java
   @Component
   public class User {
       @Value("用户")
       private String name;
   
       @Override
       public String toString() {
           return "User{" +
                   "name='" + name + '\'' +
                   '}';
       }
   
       public String getName() {
           return name;
       }
   
       public void setName(String name) {
           this.name = name;
       }
   }
   ```

2. 新建一个config配置包，编写一个MyConfig配置类

   ```java
   @Configuration
   public class MyConfig {
       @Bean
       public User getUser() {
           return new User();
       }
   }
   ```

3. 测试

   ```java
   public class MyTest {
       public static void main(String[] args) {
           ApplicationContext context = new AnnotationConfigApplicationContext(MyConfig.class);
   
           User getUser = (User) context.getBean("getUser");
           System.out.println(getUser.getName());  //用户
       }
   }   
   ```

关于这种Java类的配置方式，我们在之后的SpringBoot 和 SpringCloud中还会大量看到，我们需要知道这些注解的作用即可！

# 代理模式

为什么要学习代理模式，因为AOP的底层机制就是动态代理！

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240503130703082-2112623131.png" alt="image-20240503130704030" width="400" />

代理模式：

- 静态代理
- 动态代理

## 静态代理

### 静态代理角色分析

1. 抽象角色 : 一般使用`接口或者抽象类`来实现
2. 真实角色 : 被代理的角色
3. 代理角色 : 代理真实角色 ; 代理真实角色后 , 一般会做一些附属的操作
4. 客户 : 使用代理角色来进行一些操作 .

### 代码实现

1. Rent . java 即抽象角色

   ```java
   //租房的接口
   public interface Rent {
       public void rent();
   }
   ```

2. Host . java 即真实角色

   ```java
   //房东
   public class Host implements Rent {
   
       @Override
       public void rent() {
           System.out.println("房东要出租房子");
       }
   }
   ```

3. Proxy . java 即代理角色

   ```java
   public class Proxy implements Rent {
       private Host host;
   
       public Proxy() {}
   
       public Proxy(Host host) {
           this.host = host;
       }
   
       @Override
       public void rent() {
           seeHouse();
           host.rent();
           hetong();
           fare();
       }
       public void seeHouse() {
           System.out.println("中介带你看房");
       }
       public void fare() {
           System.out.println("收中介费");
       }
       public void hetong() {
           System.out.println("签合同");
       }
   }
   ```

4. Client . java 即客户

   ```java
   //房客
   public class Client {
       public static void main(String[] args) {
           //房东租房子
           Host host = new Host();
           //代理 帮房东租房子，但是会有附属操作
           Proxy proxy = new Proxy(host);
           //不用面对房东 直接找中介
           proxy.rent();
       }
   }
   ```

分析： 在这个过程中，你直接接触的就是中介，你看不到房东，但是你依旧租到了房东的房子通过代理，这就是所谓的代理模式

## 静态代理的好处

1. 可以使得我们的真实角色更加纯粹！不再去关注一些公共的事情
2. 公共的业务由代理来完成 ！实现了业务的分工 
3. 公共业务发生扩展时，变得更加集中和方便

缺点 :

- 一个真实角色就会产生一个代理角色。类多了 , 工作量变大了， 开发效率降低 .

我们想要静态代理的好处，又不想要静态代理的缺点，所以 , 就有了动态代理 !

## 静态代理理解

1. 创建一个抽象角色，比如咋们平时做的用户业务，抽象起来就是增删改查！  

   ```java
   public interface UserService {
       public void add();
       public void delete();
       public void update();
       public void query();
   }
   ```

2. 我们需要一个真实对象来完成这些增删改查操作

   ```java
   public class UserServiceImpl implements UserService {
       @Override
       public void add() {
           System.out.println("增加了一个用户");
       }
   
       @Override
       public void delete() {
           System.out.println("删除了一个用户");
       }
   
       @Override
       public void update() {
           System.out.println("修改了一个用户");
       }
   
       @Override
       public void query() {
           System.out.println("查询了一个用户");
       }
   }
   ```

3. 需求来了，现在我们需要增加一个日志功能，怎么实现！

   思路1 ：在实现类上增加代码 【麻烦！】

   思路2：使用代理来做，能够不改变原来的业务情况下，实现此功能就是最好的了！

4. 设置一个代理类来处理日志！ 代理角色

   ```java
   public class UserServiceProxy implements UserService {
       private UserService userService;
   
       public void setUserService(UserService userService) {
           this.userService = userService;
       }
   
       @Override
       public void add() {
           log("add");
           userService.add();
       }
   
       @Override
       public void delete() {
           log("delete");
           userService.delete();
       }
   
       @Override
       public void update() {
           log("update");
           userService.update();
       }
   
       @Override
       public void query() {
           log("query");
           userService.query();
       }
   
       //日志方法
       public void log(String msg) {
           System.out.println("[Debug]使用了" + msg + "方法");
       }
   }
   ```

5. 测试访问类：

   ```java
   public class Client {
       public static void main(String[] args) {
           UserServiceImpl userService = new UserServiceImpl();
   
           UserServiceProxy proxy = new UserServiceProxy();
           proxy.setUserService(userService);
           proxy.add();
       }
   }
   ```

重点大家需要理解其中的思想：`我们在不改变原来的代码的情况下，实现了对原有功能的增强`，这是AOP中最核心的思想

【聊聊AOP：纵向开发，横向开发】

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240429142756203-101162789.png" alt="image-20240429142805253" width="400" />

## 动态代理

### 简介

1. 动态代理的角色和静态代理的一样
2. 动态代理的`代理类是动态生成`的 . 静态代理的代理类是我们提前写好的
3. 动态代理分为两类 : 一类是`基于接口`动态代理 , 一类是`基于类`的动态代理
   - 基于接口的动态代理----JDK动态代理【我们使用】
   - 基于类的动态代理--cglib
   - 现在用的比较多的是 javasist -Java字节码来生成动态代理
   - 我们这里使用JDK的原生代码来实现，其余的道理都是一样的！

JDK的动态代理需要了解两个类核心 : InvocationHandler 和 Proxy

### InvocationHandler调用处理程序

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240503134603959-1478274911.png" alt="image-20240503134604790" width="600" />

invoke方法

```java
//处理代理实例上的方法调用并返回结果
Object invoke(Object proxy,方法 method,Object[] args)

/*：参数
proxy - 调用该方法的代理实例
method -所述方法对应于调用代理实例上的接口方法的实例。 
		方法对象的声明类将是该方法声明的接口，它可以是代理类继承该方法的代理接口的超级接口。
args -包含的方法调用传递代理实例的参数值的对象的阵列，或null如果接口方法没有参数。 
*/
```

### Proxy代理

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240503135339054-284127216.png" alt="image-20240503135339838" width="500" />

newProxyInstance方法

```java
//返回指定接口的代理类的实例，该接口将方法调用分派给指定的调用处理程序
public static Object newProxyInstance(ClassLoader loader,
                                      类<?>[] interfaces,
                                      InvocationHandler h)
/*参数 
loader - 类加载器来定义代理类 
interfaces - 代理类实现的接口列表 
h - 调度方法调用的调用处理函数 
*/
```

### 代码实现

抽象角色和真实角色和之前的一样！

1. Rent . java 即抽象角色

   ```java
   //租房的接口
   public interface Rent {
       public void rent();
   }
   ```

2. Host . java 即真实角色

   ```java
   //房东
   public class Host implements Rent {
   
       @Override
       public void rent() {
           System.out.println("房东要出租房子");
       }
   }
   ```

3. ProxyInvocationHandler. java 即代理角色

   ```java
   //动态生成代理类
   public class ProxyInvocationHandler implements InvocationHandler {
       //被代理的接口
       private Rent rent;
   
       public void setRent(Rent rent) {
           this.rent = rent;
       }
   
       //1.生成得到 代理类
       public Object getProxy() {
           return Proxy.newProxyInstance(this.getClass().getClassLoader(), rent.getClass().getInterfaces(), this);
       }
   
       //2.处理代理实例上的方法调用，并返回结果。
       @Override
       public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
           //动态代理的本质，就是使用反射机制实现
           seeHouse();
           Object result = method.invoke(rent, args);
           fare();
   
           return result;
       }
   
       //中介带你看房子
       public void seeHouse() {
           System.out.println("中介带你看房子！");
       }
   
       //收中介费
       public void fare() {
           System.out.println("收中介费！");
       }
   }
   ```

4. Client . java

   ```java
   public class Client {
       public static void main(String[] args) {
           //真实角色
           Host host = new Host();
   
           //代理角色：现在没有，只有调用处理程序
           //1.获得调用处理程序
           ProxyInvocationHandler pih = new ProxyInvocationHandler();
           //2.通过调用处理程序 来 处理我们要调用的接口对象
           pih.setRent(host);  //相当于Rent rent=new Host()
           //3.获得动态生成的代理类    我们没有写
           Rent proxy = (Rent) pih.getProxy();
           //4.通过代理类调用接口
           proxy.rent();
       }
   }
   ```

核心：一个动态代理 , 一般代理某一类业务 , 一个动态代理可以代理多个类，代理的是接口！

## 动态代理理解

我们来使用动态代理实现代理我们后面写的UserService！

我们也可以编写一个通用的动态代理实现的类！所有的代理对象设置为Object即可！

```java
//动态生成代理类
public class ProxyInvocationHandler implements InvocationHandler {
    //1.被代理的接口
    private Object target;

    //代理谁
    public void setTarget(Object target) {
        this.target = target;
    }

    //2.生成 代理类
    public Object getProxy() {
        return Proxy.newProxyInstance(this.getClass().getClassLoader(), target.getClass().getInterfaces(), this);
    }

    //3.调用代理类要执行的方法
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        //动态代理的本质，就是使用反射机制实现
        log(method.getName());  //通过反射获得方法的名字
        Object result = method.invoke(target, args);

        return result;
    }

    //增加方法
    public void log(String msg) {
        System.out.println("执行了" + msg + "方法");
    }
}
```

测试！  

```java
public class Client {
    public static void main(String[] args) {
        //真实角色
        UserServiceImpl userService = new UserServiceImpl();

        //代理角色
        //1.获取调用处理程序
        ProxyInvocationHandler pih = new ProxyInvocationHandler();
        //2.设置要代理的接口-抽象角色  真实对象UserServiceImpl实现了接口
        pih.setTarget(userService);
        //3.动态生成代理类  默认生成Object类型，强转为UserService
        UserService proxy = (UserService) pih.getProxy();

        //通过代理类使用真实对象的方法
        proxy.delete();
    }
}
```

## 动态代理的好处

静态代理有的它都有

- 可以使得我们的真实角色更加纯粹 . 不再去关注一些公共的事情 .
- 公共的业务由代理来完成 . 实现了业务的分工
- 公共业务发生扩展时，变得更加集中和方便 

静态代理没有的，它也有！

当有多个类实现的是同一个接口的情况下，访问的客户只需要修改真实角色和代理的对象，而在获得代理对象的时候不用改，因为代理角色是动态生成的。

```java
public class Client {
    public static void main(String[] args) {
        //真实角色
        //UserServiceImpl userService = new UserServiceImpl();
        //修改一：
        UserServiceImpl2 userServiceImpl2 = new UserServiceImpl2();

        //代理角色
        ProxyInvocationHandler pih = new ProxyInvocationHandler();
		//修改二：
        pih.setTarget(userServiceImpl2);//设置要代理的接口-抽象角色
        UserService proxy = (UserService) pih.getProxy();

        proxy.delete();
    }
}
```

- 一个动态代理 , 代理的是接口！一般代理某一类业务
- 一个动态代理可以代理多个类，只要是实现了同一个接口

# AOP

## 简介

AOP（Aspect Oriented Programming）意为：`面向切面编程`，通过`预编译`方式和`运行期动态代理`实现程序功能的统一维护的一种技术。

- 基于接口实现动态代理- - JDK InvocationHandler 【Spring默认】

- 基于类实现动态代理- - cdlib

  ```xml
  <!--基于接口实现动态代理--JDK-->
  <aop:aspectj-autoproxy proxy-target-class="false"/>
  ```

AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率  

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240429143231014-206802399.png" alt="image-20240429143240167" width="500" />

## Aop在Spring中的作用

`提供声明式事务；允许用户自定义切面`

- 横切关注点：跨越应用程序多个模块的方法或功能。即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点。如日志 , 安全 , 缓存 , 事务等等 ....（为真实对象添加日志功能）
- 切面（ASPECT）：横切关注点 被模块化 的特殊对象。即，它是一个类。（Log.java）
- 通知（Advice）：切面必须要完成的工作。即，它是类中的一个方法。（Log类中的方法）
- 目标（Target）：被通知对象。（接口/方法）
- 代理（Proxy）：向目标对象应用通知之后创建的对象。（生成的代理类）
- 切入点（PointCut）：切面通知 执行的 “地点”的定义。
- 连接点（JointPoint）：与切入点匹配的执行点。

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240429143302465-1304615585.png" alt="image-20240429143311525" width="400" />

## 使用Spring实现Aop

【重点】使用AOP织入，需要导入一个依赖包！

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.22</version>
</dependency>
```

### 方式一

`通过 Spring API接口 实现`【主要是SringAPI接口实现】

SpringAOP中，通过Advice定义横切逻辑。Spring中支持5种类型的Advice:  即 Aop 在 不改变原有代码的情况下 , 去增加新的功能 .  

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240429143320619-1069542517.png" alt="image-20240429143329732" width="400" />

1. 首先编写我们的业务接口和实现类

   ```java
   public interface UserService {
       public void add();
   
       public void delete();
   
       public void update();
   
       public void select();
   }
   
   public class UserServiceImpl implements UserService {
       @Override
       public void add() {
           System.out.println("增加了一个用户");
       }
   
       @Override
       public void delete() {
           System.out.println("删除了一个用户");
       }
   
       @Override
       public void update() {
           System.out.println("修改了一个用户");
       }
   
       @Override
       public void select() {
           System.out.println("查询了一个用户");
       }
   }
   ```

2. 然后去写我们的增强类 , 我们编写两个 , 一个前置增强 一个后置增强  

   ```java
   public class Log implements MethodBeforeAdvice {
       //method : 要执行的目标对象的方法
       //args : 参数
       //target : 目标对象
       @Override
       public void before(Method method, Object[] args, Object target) throws Throwable {
           System.out.println(target.getClass().getName() + "的" + method.getName() + "方法将要被执行了-Before!");
       }
   }
   
   public class AfterLog implements AfterReturningAdvice {
       //returnValue : 返回值
       @Override
       public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
           System.out.println(target.getClass().getName() + "的" + method.getName() + "方法已经被执行了-AfterReturning!返回结果为：" + returnValue);
       }
   }
   ```

3. 最后去spring的文件中注册 , 并实现aop切入实现 , 注意导入约束 .  

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
   http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">
   
       <!--注册bean-->
       <bean id="userService" class="com.sun.service.UserServiceImpl"/>
       <bean id="log" class="com.sun.log.Log"/>
       <bean id="afterlog" class="com.sun.log.AfterLog"/>
   
       <!--配置aop:需要导入aop的约束-->
       <aop:config>
           <!--切入点:expression表达式
           expression(要执行的位置!*修饰词*返回值类型*类名*方法名*参数) *代表任意的东西
           execution(* com.sun.service.UserServiceImpl.*(..))
           表示com.sun.service.UserServiceImpl类下所有的方法，方法的参数不限(..)
           -->
   
           <aop:pointcut id="pointcut" expression="execution(* com.sun.service.UserServiceImpl.*(..))"/>
   
           <!--执行环绕增强
           表示把log类切入到pointcut处
           -->
           <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
           <aop:advisor advice-ref="afterlog" pointcut-ref="pointcut"/>
       </aop:config>
   </beans>
   ```

4. 测试  

   ```java
   public class MyTest {
       public static void main(String[] args) {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           //动态代理的是接口 这里需要获得接口对象【注意】
           UserService userService = context.getBean("userService", UserService.class);
   
           userService.add();
       }
   }
   /*
   com.sun.service.UserServiceImpl的add方法将要被执行了-Before!
   增加了一个用户
   com.sun.service.UserServiceImpl的add方法已经被执行了-AfterReturning!返回结果为：null
   */
   ```

Spring的Aop就是将公共的业务 (日志 , 安全等) 和领域业务结合起来 , 当执行领域业务时 , 将会把公共业务加进来 . 实现公共业务的重复利用 . 领域业务更纯粹 , 程序猿专注领域业务 , 其本质还是动态代理 .

### 方式二

`自定义类来实现Aop`【主要是切面定义】

目标业务类不变依旧是userServiceImpl

1. 写我们自己的一个切入类去spring中配置测试：

   ```java
   public class DiyPointCut {
       public void beFore() {
           System.out.println("====方法执行前====");
       }
   
       public void after() {
           System.out.println("====方法执行后====");
       }
   }
   ```

2. 去Spring中配置

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
   http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">
       
       <!--注册bean-->
       <bean id="userService" class="com.sun.service.UserServiceImpl"/>
       <bean id="diy" class="com.sun.diy.DiyPointCut"/>
   
       <!--配置aop:需要导入aop的约束-->
       <aop:config>
           <!--自定义切面 ref:要引用的类-->
           <aop:aspect ref="diy">
               <!--切点-->
               <aop:pointcut id="pointcut" expression="execution(* com.sun.service.UserServiceImpl.*(..))"/>
               <!--通知-->
               <aop:before method="beFore" pointcut-ref="pointcut"/>
               <aop:after method="after" pointcut-ref="pointcut"/>
           </aop:aspect>
       </aop:config>
   </beans>
   ```

3. 测试


### 方式三

`使用注解实现`

1. 编写一个注解实现的增强类

   ```java
   //标注这个类是一个切面
   @Aspect
   public class AnnotationPointCut {
       @Before("execution(* com.sun.service.UserServiceImpl.*(..))")
       public void before() {
           System.out.println("======方法执行前======");
       }
   
       @After("execution(* com.sun.service.UserServiceImpl.*(..))")
       public void after() {
           System.out.println("======方法执行后======");
       }
   
       //在环绕增强中，可以给定一个参数，代表连接点，与切入点匹配的执行点，我们要获取处理切入的点
       @Around("execution(* com.sun.service.UserServiceImpl.*(..))")
       public void around(ProceedingJoinPoint pjp) throws Throwable {
           System.out.println("======环绕前======");
           System.out.println("pjp.getSignature() : " + pjp.getSignature());
           //执行方法
           Object proceed = pjp.proceed();
           System.out.println("proceed : " + proceed);
           System.out.println("======环绕后======");
       }
   }
   ```

2. 在Spring配置文件中，注册bean，并增加支持注解的配置

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
   http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">
   
       <!--注册bean-->
       <bean id="userService" class="com.sun.service.UserServiceImpl"/>
       <bean id="annotationPointCut" class="com.sun.diy.AnnotationPointCut"/>
       
       <!--开启注解支持-->
       <aop:aspectj-autoproxy/>
   </beans>
   ```

aop:aspectj-autoproxy：说明

```java
	通过aop命名空间的<aop:aspectj-autoproxy />声明自动为spring容器中那些配置@aspectJ切面的bean创建代理，织入切面。当然，spring 在内部依旧采用AnnotationAwareAspectJAutoProxyCreator进行自动代理的创建工作，但具体实现的细节已经被<aop:aspectj-autoproxy />隐藏起来了
    <aop:aspectj-autoproxy />有一个proxy-target-class属性，默认为false，表示使用jdk动态代理织入增强，当配为<aop:aspectj-autoproxy poxy-target-class="true"/>时，表示使用CGLib动态代理技术织入增强。不过即使proxy-target-class设置为false，如果目标类没有声明接口，则spring将自动使用CGLib动态代理
```

# 整合Mybatis

## 步骤

1. 导入相关jar包

   - junit
   - mybatis
   - mysql-connector-java
   - spring相关
   - spring操作数据库还需要一个spring-jdbc包【要使用Spring的事务管理器就得到这个】
   - aspectJ AOP 织入器aspectjweaver
   - mybatis-spring整合包 【重点】

2. 配置Maven静态资源过滤问题！

   ```xml
   <dependencies>
       <!-- - junit-->
       <dependency>
           <groupId>junit</groupId>
           <artifactId>junit</artifactId>
           <version>4.12</version>
           <scope>test</scope>
       </dependency>
       <!-- - mybatis-->
       <dependency>
           <groupId>org.mybatis</groupId>
           <artifactId>mybatis</artifactId>
           <version>3.5.16</version>
       </dependency>
       <!-- - mysql-connector-java-->
       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
           <version>5.1.47</version>
       </dependency>
       <!-- - spring相关-->
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-webmvc</artifactId>
           <version>6.1.6</version>
       </dependency>
       <!-- - spring操作数据库还需要一个spring-jdbc-->
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-jdbc</artifactId>
           <version>6.1.6</version>
       </dependency>
       <!-- - aspectJ AOP 织入器aspectjweaver-->
       <dependency>
           <groupId>org.aspectj</groupId>
           <artifactId>aspectjweaver</artifactId>
           <version>1.9.22</version>
       </dependency>
       <!-- - mybatis-spring整合包 【重点】-->
       <dependency>
           <groupId>org.mybatis</groupId>
           <artifactId>mybatis-spring</artifactId>
           <version>3.0.3</version>
       </dependency>
   </dependencies>
   
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

3. 编写配置文件

4. 代码实现

## Mybatis

1. 编写pojo实体类

   ```java
   @Data
   public class User {
       private int id;
       private String name;
       private String pwd;
   }
   ```

2. 编写mybatis的核心配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
       <typeAliases>
           <package name="com.sun.pojo"/>
       </typeAliases>
       <environments default="development">
           <environment id="development">
               <transactionManager type="JDBC"/>
               <dataSource type="POOLED">
                   <property name="driver" value="com.mysql.jdbc.Driver"/>
                   <property name="url"
                             value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
                   <property name="username" value="root"/>
                   <property name="password" value="root"/>
               </dataSource>
           </environment>
       </environments>
       <mappers>
           <mapper class="com.sun.mapper.UserMapper"/>
       </mappers>
   
   </configuration>
   ```

3. 编写接口Mapper

   ```java
   public interface UserMapper {
       public List<User> selectUser();
   }
   ```

4. 编写接口对应的Mapper.xml映射文件

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.sun.mapper.UserMapper">
       <select id="selectUser" resultType="user">
           select *
           from mybatis.user;
       </select>
   </mapper>
   ```

5. 测试类

   ```java
   ublic class MyTest {
       @Test
       public void test() throws IOException {
           String resource = "mybatis-config.xml";
           InputStream inputStream = Resources.getResourceAsStream(resource);
   
           SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(inputStream);
           SqlSession sqlSession = factory.openSession(true);
   
           UserMapper mapper = sqlSession.getMapper(UserMapper.class);
           List<User> users = mapper.selectUser();
           for (User user : users) {
               System.out.println(user);
           }
   
           sqlSession.close();
       }
   }
   ```

## Mybatis-Spring

1. 引入Spring之前需要了解mybatis-spring包中的一些重要类；

   - 官方文档：https://mybatis.org/spring/zh_CN/index.html
   - 民间（更快,版本: 2.0.4）：http://doc.vrd.net.cn/mybatis/spring/zh/index.html

2. 什么是 MyBatis-Spring？

   - MyBatis-Spring 会帮助你将 MyBatis 代码无缝地整合到 Spring 中。
   - 它将允许 MyBatis 参与到 Spring 的事务管理之中，
   - 创建映射器 mapper 和 `SqlSession` 并注入到 bean 中，
   - 以及将 Mybatis 的异常转换为 Spring 的 `DataAccessException`。
   - 最终，可以做到应用代码不依赖于 MyBatis，Spring 或 MyBatis-Spring。      

3. 在开始使用 MyBatis-Spring 之前，你需要先熟悉 Spring 和 MyBatis 这两个框架和有关它们的术语。

4. MyBatis-Spring 需要以下版本：

   | MyBatis-Spring | MyBatis  | Spring Framework | Spring Batch | Java        |
   | -------------- | -------- | ---------------- | ------------ | ----------- |
   | **3.0**        | 3.5+     | 6.0+             | 5.0+         | Java 17+    |
   | **2.1**        | 3.5+     | 5.x              | 4.x          | Java 8+     |
   | ~~**2.0**~~    | ~~3.5+~~ | ~~5.x~~          | ~~4.x~~      | ~~Java 8+~~ |
   | ~~**1.3**~~    | ~~3.4+~~ | ~~3.2.2+~~       | ~~2.1+~~     | ~~Java 6+~~ |

## 整合实现一:SqlSessionTemplate

1. 新建一个Spring配置文件spring-dao.xml  

   1. 配置数据源替换mybaits的数据源
   2. 配置SqlSessionFactory，关联MyBatis
   3. 注册sqlSessionTemplate，关联sqlSessionFactory

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <!--DataSource：使用Spring的数据源替换Mybatis的配置 c3p0 dbcp druid
       这里使用Spring提供的JDBC   导包spring-jdbc
       -->
       <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
           <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
           <property name="url"
                     value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
           <property name="username" value="root"/>
           <property name="password" value="root"/>
       </bean>
   
       <!--SqlSessionFactory-->
       <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
           <property name="dataSource" ref="dataSource"/>
           <!--绑定Mybatis配置文件 也可自己设置property-->
           <property name="configLocation" value="classpath:mybatis-config.xml"/>
           <!--注册mapper-->
           <property name="mapperLocations" value="classpath:com/sun/mapper/*.xml"/>
       </bean>
   
       <!--SqlSessionTemplate : 就是我们使用的SqlSession  Template模版-->
       <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
           <!--只能使用构造器注入sqlSessionFactory  因为没有set方法-->
           <constructor-arg index="0" ref="sqlSessionFactory"/>
       </bean>
   </beans>
   ```

2. 需要增加接口userMapper的实现类；私有化sqlSessionTemplate

   ```java
   public class UserMapperImpl implements UserMapper {
       //我们所有的操作，在原来都使用sqlSession执行，现在都使用SqlSessionTemplate
       private SqlSessionTemplate sqlSession;
   
       public void setSqlSession(SqlSessionTemplate sqlSession) {
           this.sqlSession = sqlSession;
       }
   
       @Override
       public List<User> selectUser() {
           UserMapper mapper = sqlSession.getMapper(UserMapper.class);
           return mapper.selectUser();
       }
   }
   ```

3. 新建一个总的applicationContext.xml配置

   1. 导入Mybatis的配置
   2. 将接口实现类，注入到Spring

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
       <!--导入Mybatis的配置-->
       <import resource="spring-dao.xml"/>
   
       <!--注册UserMapperImpl类-->
       <bean id="userMapper" class="com.sun.mapper.UserMapperImpl">
           <property name="sqlSession" ref="sqlSession"/>
       </bean>
   </beans>
   ```

4. 测试

   ```java
   public class MyTest {
       @Test
       public void test() throws IOException {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
   
           UserMapper userMapper = context.getBean("userMapper", UserMapper.class);
   
           List<User> users = userMapper.selectUser();
           for (User user : users) {
               System.out.println(user);
           }
       }
   }
   ```

结果成功输出！现在我们的Mybatis配置文件的状态！发现都可以被Spring整合！

## 整合实现二:SqlSessionDaoSupport

mybatis-spring1.2.3版以上的才有这个 

dao继承Support类 , 直接利用 getSqlSession() 获得 , 然后直接注入SqlSessionFactory . 比起方式1 , 不需要管理SqlSessionTemplate , 而且对事务的支持更加友好 . 可跟踪源码查看

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240429165214646-605209513.png" alt="image-20240429165223290" width="600" />

测试：

1. 将我们上面写的UserDaoImpl修改一下

   ```java
   public class UserMapperImpl2 extends SqlSessionDaoSupport implements UserMapper {
       @Override
       public List<User> selectUser() {
           return getSqlSession().getMapper(UserMapper.class).selectUser();
       }
   }
   ```

2. 修改bean的配置

   ```xml
   <bean id="userMapper2" class="com.sun.mapper.UserMapperImpl2">
       <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
   </bean>
   ```

3. 测试

总结 : 整合到spring中以后可以完全不要mybatis的配置文件，除了这些方式可以实现整合之外，我们还可以使用注解来实现，这个等我们后面学习SpringBoot的时候还会测试整合！

# 声明式事务

## 事务

- 事务在项目开发过程非常重要，涉及到数据的一致性的问题，不容马虎！
- 事务管理是企业级应用程序开发中必备技术，用来`确保数据的完整性和一致性`。
- 事务就是把一系列的动作当成一个独立的工作单元，这些动作要么全部完成，要么全部不起作用。

事务四个属性ACID

1.  原子性（atomicity）事务是原子性操作，由一系列动作组成，事务的原子性确保动作要么全部完成，要么完全不起作用
2. 一致性（consistency）一旦所有事务动作完成，事务就要被提交。数据和资源处于一种满足业务规则的一致性状态中
3. 隔离性（isolation）可能多个事务会同时处理相同的数据，因此每个事务都应该与其他事务隔离开来，防止数据损坏
4. 持久性（durability）事务一旦完成，无论系统发生什么错误，结果都不会受到影响。通常情况下，事务的结果被写到持久化存储器中

## 测试

1. 将上面的代码拷贝到一个新项目中

   1. 拷pom.xml
   2. 拷实体类
   3. 拷mapper接口和mapper.xml配置
   4. 拷mybatis配置文件和spring-dao.xml配置文件
   5. 拷mapper接口的实现类
   6. 拷总的配置类，appliactionContext.xml（注册实现类）

2. 在之前的案例中，我们给userDao接口新增两个方法，删除和增加用户；

   ```java
   public interface UserMapper {
       public List<User> selectUser();
   
       public int addUser(User user);
   
       public int deleteUser(int id);
   }
   ```

3. mapper文件，我们故意把 deletes 写错，测试！

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.sun.mapper.UserMapper">
       <select id="selectUser" resultType="user">
           select *
           from mybatis.user;
       </select>
       <insert id="addUser" parameterType="user">
           insert into mybatis.user (id, name, pwd)
           VALUES (#{id}, #{name}, #{pwd})
       </insert>
       <delete id="deleteUser" parameterType="int">
           deletes from mybatis.user where id=
           #{id}
       </delete>
   </mapper>
   ```

4. 编写接口的实现类，在实现类中，我们去操作一波

   ```java
   public class UserMapperImpl extends SqlSessionDaoSupport implements UserMapper {
   
       @Override
       public List<User> selectUser() {
           User user = new User(5, "小武", "456456");
   
           UserMapper mapper = getSqlSession().getMapper(UserMapper.class);
           mapper.addUser(user);
           mapper.deleteUser(5);
   
           return mapper.selectUser();
       }
   
       @Override
       public int addUser(User user) {
           return getSqlSession().getMapper(UserMapper.class).addUser(user);
       }
   
       @Override
       public int deleteUser(int id) {
           return getSqlSession().getMapper(UserMapper.class).deleteUser(id);
       }
   }
   ```

5. 测试

   ```java
   public class MyTest {
       public static void main(String[] args) {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           UserMapper userMapper = context.getBean("userMapper", UserMapper.class);
           for (User user : userMapper.selectUser()) {
               System.out.println(user);
           }
       }
   }
   ```

报错：sql异常，delete写错了

结果 ：插入成功！

- 没有进行事务的管理；我们想让他们都成功才成功，有一个失败，就都失败，我们就应该需要事务！


- 以前我们都需要自己手动管理事务，十分麻烦！但是Spring给我们提供了事务管理，我们只需要配置即可；


## Spring中的事务管理

一个使用 MyBatis-Spring 的其中一个主要原因是

- 它允许 MyBatis 参与到 Spring 的事务管理中。
- 而不是给 MyBatis 创建一个新的专用事务管理器，
- MyBatis-Spring 借助了 Spring 中的 `DataSourceTransactionManager` 来实现事务管理。

Spring在不同的事务管理API之上定义了一个抽象层，使得开发人员不必了解底层的事务管理API就可以使用Spring的事务管理机制。Spring支持编程式事务管理和声明式的事务管理。

### 编程式事务管理

- 将事务管理代码嵌到业务方法中来控制事务的提交和回滚
- `需要在代码中进行事务的管理`
- 缺点：必须在每个事务操作业务逻辑中包含额外的事务管理代码

```java
try {
    userMapper.insertUser(user);
} catch (Exception e) {
    transactionManager.rollback(txStatus);
    throw e;
}
transactionManager.commit(txStatus);
//insertUser时利用try-cathch包围起来，捕获到异常就回滚
```

### 声明式事务管理

- 一般情况下比编程式事务好用。
- 将事务管理代码从业务方法中分离出来，以声明的方式来实现事务管理。
- `将事务管理作为横切关注点，通过aop方法模块化`。Spring中通过Spring AOP框架支持声明式事务管理。

## 交由容器管理事务

也就是声明式事务管理

### 事务管理器

- 无论使用Spring的哪种事务管理策略（编程式或者声明式）事务管理器DataSourceTransactionManager都是必须的。
- 就是 Spring的核心事务管理抽象，管理封装了一组独立于技术的方法。
- 传入的 DataSource 可以是任何能够与 Spring 兼容的 JDBC DataSource。包括连接池和通过 JNDI 查找获得的 DataSource。
- 注意：`为事务管理器指定的 DataSource 必须和用来创建 SqlSessionFactoryBean 的是同一个数据源`，否则事务管理器就无法工作了。

```xml
<!--开启 Spring 的事务处理功能 在 Spring 的配置文件中创建一个 DataSourceTransactionManager 对象-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <constructor-arg name="dataSource" ref="dataSource"/>
</bean>
```

### spring事务传播特性

事务传播行为就是多个事务方法相互调用时，事务如何在这些方法间传播。spring支持7种事务传播行为：

1. propagation_requierd【默认】：如果当前没有事务，就新建一个事务，如果已存在一个事务中，加入到这个事务中，这是最常见的选择。
2. propagation_supports：支持当前事务，如果没有当前事务，就以非事务方法执行。
3. propagation_mandatory：使用当前事务，如果没有当前事务，就抛出异常。
4. propagation_required_new：新建事务，如果当前存在事务，把当前事务挂起。
5. propagation_not_supported：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
6. propagation_never：以非事务方式执行操作，如果当前事务存在则抛出异常。
7. propagation_nested：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与propagation_required类似的操作

### 代码实现

- 使用Spring管理事务，注意头文件的约束导入 : tx
- 配置事务切入，注意头文件的约束导入 : aop

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--DataSource：使用Spring的数据源替换Mybatis的配置 c3p0 dbcp druid
    这里使用Spring提供的JDBC   导包spring-jdbc
    -->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url"
                  value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
        <property name="username" value="root"/>
        <property name="password" value="root"/>
    </bean>

    <!--SqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!--绑定Mybatis配置文件 也可自己设置property-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <!--注册mapper-->
        <property name="mapperLocations" value="classpath:com/sun/mapper/*.xml"/>
    </bean>

    <!--SqlSessionTemplate : 就是我们使用的SqlSession  Template模版-->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
        <!--只能使用构造器注入sqlSessionFactory  因为没有set方法-->
        <constructor-arg index="0" ref="sqlSessionFactory"/>
    </bean>

    <!--1.配置声明式事务  开启 Spring 的事务处理功能-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <constructor-arg name="dataSource" ref="dataSource"/>
    </bean>

    <!--2.结合AOP实现事务的织入-->
    <!--配置事务通知：-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <!--给哪些方法配置事务-->
        <tx:attributes>
            <!--配置事务的传播特性：propagation默认REQUIRED-->
            <tx:method name="add" propagation="REQUIRED"/>
            <tx:method name="delete" propagation="REQUIRED"/>
            <tx:method name="update" propagation="REQUIRED"/>
            <tx:method name="query" read-only="true"/>
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>

    <!--3.配置事务切入-->
    <aop:config>
        <!--com.sun.mapper包下的所有类的所有方法都会被织入txAdvice方法-->
        <aop:pointcut id="pointcut" expression="execution(* com.sun.mapper.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="pointcut"/>
    </aop:config>
</beans>
```

进行测试，删掉刚才插入的数据，再次测试！发现报错的同时，并没有再插入数据了。

## 为什么需要配置事务

- 如果不配置，就需要我们手动提交控制事务；
- 事务在项目开发过程非常重要，涉及到数据的一致性的问题，不容马虎