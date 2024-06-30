# Shiro简介

## 简介

Apache Shiro 是一个Java 的安全（权限）框架。

Shiro 可以非常容易的开发出足够好的应用，其不仅可以用在JavaSE环境，也可以用在JavaEE环境。

Shiro可以完成，认证，授权，加密，会话管理，Web集成，缓存等。

- 官网：http://shiro.apache.org/
- 中文文档：https://www.docs4dev.com/docs/zh/apache-shiro/1.5.3/reference/

## 功能模块

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240520175826389-814972450.png" alt="image-20240520175832436" style="zoom:43%;" />

1. Authentication：身份`认证`、登录，验证用户是不是拥有相应的身份； 
2. Authorization：`授权`，即权限验证，验证某个已认证的用户是否拥有某个权限，即判断用户能否进行什么操作，如：验证某个用户是否拥有某个角色，或者细粒度的验证某个用户对某个资源是否具有某个权限！
3. Session Manager：`会话管理`，即用户登录后就是第一次会话，在没有退出之前，它的所有信息都在会话中；会话可以是普通的JavaSE环境，也可以是Web环境；
4. Cryptography：`加密`，保护数据的安全性，如密码加密存储到数据库中，而不是明文存储；
5. Web Support：`Web支持`，可以非常容易的集成到Web环境；
6. Caching：`缓存`，比如用户登录后，其用户信息，拥有的角色、权限不必每次去查，这样可以提高效率
7. Concurrency：Shiro支持多线程应用的`并发验证`，即，如在一个线程中开启另一个线程，能把权限自动的传播过去
8. Testing：提供测试支持； 
9. Run As：允许一个用户`伪装`为另一个用户（如果他们允许）的身份进行访问；
10. Remember Me：`记住我`，这个是非常常见的功能，即一次登录后，下次再来的话不用登录了

## Shiro架构

### 外部

从外部来看Shiro，即从应用程序角度来观察如何使用shiro完成工作：

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240520180029926-1877678385.png" alt="image-20240520180035753" style="zoom:50%;" />

- subject： 应用代码`直接交互的对象`是Subject，也就是说Shiro的对外API核心就是Subject，Subject代表了当前的用户，这个用户不一定是一个具体的人，与当前应用交互的任何东西都是Subject，如网络爬虫，机器人等，与Subject的所有交互都会委托给
- SecurityManager；Subject其实是一个门面，SecurityManageer 才是实际的执行者SecurityManager：安全管理器，即所有与安全有关的操作都会与SercurityManager交互，并且它`管理着所有的Subject`，可以看出它是Shiro的核心，它负责与Shiro的其他组件进行交互，它相当于SpringMVC的DispatcherServlet的角色
- Realm：Shiro从Realm`获取安全数据`（如用户，角色，权限），就是说SecurityManager 要验证用户身份，那么它需要从Realm 获取相应的用户进行比较，来确定用户的身份是否合法；也需要从Realm得到用户相应的角色、权限，进行验证用户的操作是否能够进行，可以把Realm看成DataSource；

### 内部

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240520180049386-509161818.png" alt="image-20240520180055621" style="zoom:50%;" />

- Subject：任何可以与应用交互的 ‘用户’；
- Security Manager：相当于SpringMVC中的DispatcherServlet；是Shiro的心脏，所有具体的交互都通过Security Manager进行控制，它管理者所有的Subject，且负责进行认证，授权，会话，及缓存的管理。
- Authenticator：负责Subject认证，是一个扩展点，可以自定义实现；可以使用认证策略（Authentication Strategy），即什么情况下算用户认证通过了； 
- Authorizer：授权器，即访问控制器，用来决定主体是否有权限进行相应的操作；即控制着用户能访问应用中的那些功能； 
- ealm：可以有一个或者多个的realm，可以认为是安全实体数据源，即用于获取安全实体的，可以用JDBC实现，也可以是内存实现等等，由用户提供；所以一般在应用中都需要实现自己的realm 
- SessionManager：管理Session生命周期的组件，而Shiro并不仅仅可以用在Web环境，也可以用在普通的JavaSE环境中
- CacheManager：缓存控制器，来管理如用户，角色，权限等缓存的；因为这些数据基本上很少改变，放到缓存中后可以提高访问的性能；
- Cryptography：密码模块，Shiro 提高了一些常见的加密组件用于密码加密，解密等

# QuickStart

- 查看官网文档：http://shiro.apache.org/tutorial.html
- 官方的quickstart：https://github.com/apache/shiro/tree/master/samples/quickstart/  

## 源码

1. 创建一个maven父工程，用于学习Shiro，删掉不必要的东西

2.  创建一个普通的Maven子工程：shiro-01-helloworld

3. 导入Shiro的依赖

   ```xml
   <dependencies>
       <dependency>
           <groupId>org.apache.shiro</groupId>
           <artifactId>shiro-core</artifactId>
           <version>2.0.0</version>
       </dependency>
       <dependency>
           <groupId>org.slf4j</groupId>
           <artifactId>jcl-over-slf4j</artifactId>
           <version>2.0.13</version>
   
       </dependency>
       <dependency>
           <groupId>org.apache.logging.log4j</groupId>
           <artifactId>log4j-slf4j2-impl</artifactId>
           <version>2.23.1</version>
       </dependency>
       <dependency>
           <groupId>org.apache.logging.log4j</groupId>
           <artifactId>log4j-core</artifactId>
           <version>2.23.1</version>
       </dependency>
   </dependencies>
   ```

4. 观察配置文件log4j.xml

   ```xml
   <Configuration name="ConfigTest" status="ERROR" monitorInterval="5">
       <!--ERROR信息打印-->
       <Appenders>
           <!--控制台输出格式-->
           <Console name="Console" target="SYSTEM_OUT">
               <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
           </Console>
       </Appenders>
       <Loggers>
           <!--springframework-warn-->
           <Logger name="org.springframework" level="warn" additivity="false">
               <AppenderRef ref="Console"/>
           </Logger>
           <!--springframework-warn-->
           <Logger name="org.apache" level="warn" additivity="false">
               <AppenderRef ref="Console"/>
           </Logger>
           <!--ehcache-warn-->
           <Logger name="net.sf.ehcache" level="warn" additivity="false">
               <AppenderRef ref="Console"/>
           </Logger>
           <!--shiro-ThreadContext-warn-->
           <Logger name="org.apache.shiro.util.ThreadContext" level="warn" additivity="false">
               <AppenderRef ref="Console"/>
           </Logger>
           <!--info-->
           <Root level="info">
               <AppenderRef ref="Console"/>
           </Root>
       </Loggers>
   </Configuration>
   ```

5. 观察shiro.ini

   ```ini
   [users]
   # user 'root' with password 'secret' and the 'admin' role
   root = secret, admin
   # user 'guest' with the password 'guest' and the 'guest' role
   guest = guest, guest
   # user 'presidentskroob' with password '12345' ("That's the same combination on
   # my luggage!!!" ;)), and role 'president'
   presidentskroob = 12345, president
   # user 'darkhelmet' with password 'ludicrousspeed' and roles 'darklord' and 'schwartz'
   darkhelmet = ludicrousspeed, darklord, schwartz
   # user 'lonestarr' with password 'vespa' and roles 'goodguy' and 'schwartz'
   lonestarr = vespa, goodguy, schwartz
   
   # -----------------------------------------------------------------------------
   # Roles with assigned permissions
   # 
   # Each line conforms to the format defined in the
   # org.apache.shiro.realm.text.TextConfigurationRealm#setRoleDefinitions JavaDoc
   # -----------------------------------------------------------------------------
   [roles]
   # 'admin' role has all permissions, indicated by the wildcard '*'
   admin = *
   # The 'schwartz' role can do anything (*) with any lightsaber:
   schwartz = lightsaber:*
   # The 'goodguy' role is allowed to 'drive' (action) the winnebago (type) with
   # license plate 'eagle5' (instance specific id)
   goodguy = winnebago:drive:eagle5
   ```

6. 导入QuickStrat.java

   ```java
   public class Quickstart {
       //使用日志门面来输出 同sout
       private static final transient Logger log = LoggerFactory.getLogger(Quickstart.class);
   
       public static void main(String[] args) {
   
           //通过ini配置来创建安全管理SecurityManager  ini:realms, users, roles and permissions
           SecurityManager securityManager = new BasicIniEnvironment("classpath:shiro.ini").getSecurityManager();
   
           //单例 全局唯一(一般在web.xml中配置)
           SecurityUtils.setSecurityManager(securityManager);
   
           //Shiro环境开始
   
           //1.获取当前用户Subject
           Subject currentUser = SecurityUtils.getSubject();
   
           //2.通过当前用户获得Session(不需要 web or EJB 容器)
           Session session = currentUser.getSession();
           //Session中存取值
           session.setAttribute("someKey", "aValue");
           String value = (String) session.getAttribute("someKey");
           if (value.equals("aValue")) {
               log.info("Retrieved the correct value! [" + value + "]");
               //[main] INFO Quickstart - Retrieved the correct value! [aValue]
           }
   
           //3.判断当前用户是否被认证
           if (!currentUser.isAuthenticated()) {
               //生成Token令牌：通过账号和密码
               UsernamePasswordToken token = new UsernamePasswordToken("lonestarr", "vespa");
               //记住我
               token.setRememberMe(true);
               try {
                   //执行登录操作
                   currentUser.login(token);
               } catch (UnknownAccountException uae) {//用户名不存在
                   log.info("There is no user with username of " + token.getPrincipal());
               } catch (IncorrectCredentialsException ice) {//密码不对的异常！
                   log.info("Password for account " + token.getPrincipal() + " was incorrect!");
               } catch (LockedAccountException lae) {//用户被锁定的异常
                   log.info("The account for username " + token.getPrincipal() + " is locked.  " +
                           "Please contact your administrator to unlock it.");
               }
               // ... catch more exceptions here (maybe custom ones specific to your application?
               catch (AuthenticationException ae) { //认证异常，上面的异常都是它的子类
                   //unexpected condition?  error?
               }
           }
   
           //4.打印当前用户的标识主体（在本例中为用户名）
           log.info("User [" + currentUser.getPrincipal() + "] logged in successfully.");
           //[main] INFO Quickstart - User [lonestarr] logged in successfully.
   
           //5.测试角色：如果当前用户有“schwartz”角色
           if (currentUser.hasRole("schwartz")) {
               log.info("May the Schwartz be with you!");
               //[main] INFO Quickstart - May the Schwartz be with you!
           } else {
               log.info("Hello, mere mortal.");
               //[main] INFO Quickstart - You may use a lightsaber ring.  Use it wisely.
           }
   
           //6.粗粒度-权限检查 (不包含实例等级)
           if (currentUser.isPermitted("lightsaber:wield")) {
               log.info("You may use a lightsaber ring.  Use it wisely.");
               //[main] INFO Quickstart - You may use a lightsaber ring.  Use it wisely.
           } else {
               log.info("Sorry, lightsaber rings are for schwartz masters only.");
           }
   
           //7.细粒度-
           if (currentUser.isPermitted("winnebago:drive:eagle5")) {
               log.info("You are permitted to 'drive' the winnebago with license plate (id) 'eagle5'.  " +
                       "Here are the keys - have fun!");
               //[main] INFO Quickstart - You are permitted to 'drive' the winnebago with license plate (id) 'eagle5'.  Here are the keys - have fun!
           } else {
               log.info("Sorry, you aren't allowed to drive the 'eagle5' winnebago!");
           }
   
           //9.注销
           currentUser.logout();
   		//10.退出系统
           System.exit(0);
       }
   }
   ```

7. 测试运行一下  

   ```java
   10:08:33.558 [main] INFO  Quickstart - Retrieved the correct value! [aValue]
   10:08:33.561 [main] INFO  Quickstart - User [lonestarr] logged in successfully.
   10:08:33.561 [main] INFO  Quickstart - May the Schwartz be with you!
   10:08:33.561 [main] INFO  Quickstart - You may use a lightsaber ring.  Use it wisely.
   10:08:33.561 [main] INFO  Quickstart - You are permitted to 'drive' the winnebago with license plate (id) 'eagle5'.  Here are the keys - have fun!
   ```

## 问题

如果使用官方文档提供的依赖

```xml
<dependencies>
    <dependency>
        <groupId>org.apache.shiro</groupId>
        <artifactId>shiro-core</artifactId>
        <version>2.0.0</version>
    </dependency>
    <!-- Shiro uses SLF4J for logging.  We'll use the 'simple' binding
         in this example app.  See https://www.slf4j.org for more info. -->
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-simple</artifactId>
        <version>1.7.21</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>jcl-over-slf4j</artifactId>
        <version>1.7.21</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

1. 报错: NoClassDefFoundError: org/apache/commons/logging/LogFactory

   - 日志工厂会去匹配日志，QuickStart中可以去匹配logn4j
   - 此处没有可以匹配的，`默认使用commons/logging`

2. 导入一下 commons-logging 的依赖

   ```xml
   <dependency>
       <groupId>commons-logging</groupId>
       <artifactId>commons-logging</artifactId>
       <version>1.2</version>
   </dependency>
   ```

3. 测试执行

   - 可能是maven依赖中的作用域问题，我们需要将scope作用域删掉，默认是在test

   ```java
   SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
   SLF4J: Defaulting to no-operation (NOP) logger implementation
   SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
   ```

4. 重启，那么我们的quickstart就结束了，默认的日志消息！

   ```java
   [main] INFO org.apache.shiro.session.mgt.AbstractValidatingSessionManager - Enabling session validation scheduler...
   [main] INFO Quickstart - Retrieved the correct value! [aValue]
   [main] INFO Quickstart - User [lonestarr] logged in successfully.
   [main] INFO Quickstart - May the Schwartz be with you!
   [main] INFO Quickstart - You may use a lightsaber ring.  Use it wisely.
   [main] INFO Quickstart - You are permitted to 'drive' the winnebago with license plate (id) 'eagle5'.  Here are the keys - have fun!
   ```

## 总结

```java
//1.使用日志门面来输出
        private static final transient Logger log = LoggerFactory.getLogger(Quickstart.class);
//2.创建SecurityManager
        SecurityManager securityManager = new BasicIniEnvironment("classpath:shiro.ini").getSecurityManager();
       SecurityUtils.setSecurityManager(securityManager);

//3.获取当前用户Subject
        Subject currentUser = SecurityUtils.getSubject();
//4.获得Session
        Session session = currentUser.getSession();
//Session中存取值
        session.setAttribute("someKey", "aValue");
        session.getAttribute("someKey");
//5.打印信息
        log.info("Retrieved the correct value! [" + value + "]");
//6.判断当前用户是否被认证
        currentUser.isAuthenticated()
//7.生成Token令牌：通过账号和密码
        UsernamePasswordToken token = new UsernamePasswordToken("lonestarr", "vespa");
//8.记住我
        token.setRememberMe(true);
//9.执行登录操作
        currentUser.login(token);
//10.当前用户的标识主体
        currentUser.getPrincipal()
//11.权限检查
        if (currentUser.isPermitted("lightsaber:wield")

//12.注销
        currentUser.logout();
//13.退出系统
        System.exit(0);
```

# SpringBoot集成

## 环境搭建

1. 搭建一个SpringBoot项目、选中web和thymeleaf

2. 编写一个页面 index.html 在templates

   ```html
   <body>
   <h1>首页</h1>
   <p th:text="${msg}"></p>
   </body>
   ```

3. 编写controller进行访问测试

   ```java
   @Controller
   public class MyController {
       @RequestMapping({"/", "/index"})
       public String toIndex(Model model) {
           model.addAttribute("msg", "hello shiro");
           return "index";
       }
   }
   ```

4. 测试访问首页！

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240521112507383-215063070.png" alt="image-20240521112513527" style="zoom:33%;" />

## 整合Shiro

回顾核心API：

1. Subject：用户主体
2. SecurityManager：安全管理器
3. Realm：Shiro 连接数据

步骤：

1. 导入Shiro 和 spring整合的依赖

   ```xml
   <dependency>
       <groupId>org.apache.shiro</groupId>
       <artifactId>shiro-spring</artifactId>
       <version>2.0.0</version>
   </dependency>
   ```

2. 先自定义创建一个 realm 对象

   1. 用来编写一些查询的方法
   2. 或者认证与授权的逻辑

   ```java
   //自定义的realm对象
   public class UserRealm extends AuthorizingRealm {
       //1.授权
       @Override
       protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
           System.out.println("已授权=>doGetAuthorizationInfo");
           return null;
       }
       //2.认证
       @Override
       protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
           System.out.println("已认证=>doGetAuthorizationInfo");
           return null;
       }
   }
   ```

3. 编写Shiro 配置类 config包

   1. 将这个自定义realm类注册到我们的Bean中！ 
   2. 创建 DefaultWebSecurityManager 
   3.  创建 ShiroFilterFactoryBean 

   ```java
   @Configuration
   public class ShiroConfig {
       //1.自定义创建realm对象
       @Bean
       public UserRealm userRealm() {
           return new UserRealm();
       }
   
       //2.DefaultWebSecurityManager
       @Bean
       public DefaultWebSecurityManager securityManager(@Qualifier("userRealm") UserRealm userRealm) {
           DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
   
           //关联realm
           //不可以：securityManager.setRealm(userRealm());
           securityManager.setRealm(userRealm);
   
           return securityManager;
       }
   
       //3.ShiroFilterFactoryBean
       @Bean
       public ShiroFilterFactoryBean shiroFilter(@Qualifier("securityManager") DefaultWebSecurityManager securityManager) {
           ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();
           //关联securityManager-设置安全管理器
           bean.setSecurityManager(securityManager);
           return bean;
       }
   }
   ```

4. 编写两个页面、在templates目录下新建一个 user 目录 add.html update.html

   ```html
   <body>
   <h1>add</h1>
   </body>
   
   <body>
   <h1>update</h1>
   </body>
   ```

5. 编写跳转到页面的controller

   ```java
   @RequestMapping("/user/add")
   public String add() {
       return "user/add";
   }
   
   @RequestMapping("/user/update")
   public String update() {
       return "user/update";
   }
   ```

6. 在index页面上，增加跳转链接

   ```html
   <a th:href="@{/user/add}">add</a> | <a th:href="@{/user/update}">update</a>
   ```

7. 测试页面跳转是否OK

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240521123327743-1894180846.png" alt="image-20240521123333814" style="zoom:33%;" />

## 问题

报错：找不到javax.servlet.filter类

原因：在Spring Boot中使用Servlet Filter，需要引入servlet-api或javax.servlet相关的依赖。例如，在pom.xml中添加以下依赖项：

```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>4.0.1</version>
    <scope>provided</scope>
</dependency>
```

# 功能实现

## 页面拦截

`SpringBoot版本2.7`

### Shiro权限参数

| Filter Name       | 功能                                                         |
| ----------------- | ------------------------------------------------------------ |
| **anno**          | 不需要授权、登录就可以访问。eg:/index                        |
| **authc**         | **需要登录授权才能访问。eg：/用户中心**                      |
| authcBasic        | Basic HTTP身份验证拦截器                                     |
| **logout**        | 退出拦截器。退出成功后，会 redirect到设置的/URI              |
| noSessionCreation | 不创建会话连接器                                             |
| perms             | 授权拦截器:perm['user:create']                               |
| port              | 端口拦截器.eg:port[80]                                       |
| rest              | rest风格拦截器                                               |
| roles             | 角色拦截器。eg：role[administrator]                          |
| ssl               | ssl拦截器。通过https协议才能通过                             |
| user              | 用户拦截器。eg：登录后（authc），第二次没登陆但是有记住我(remmbner)都可以访问。 |

### 拦截实现 

1. 准备添加Shiro的内置过滤器

   ```java
   //3.ShiroFilterFactoryBean
   @Bean
   public ShiroFilterFactoryBean shiroFilter(DefaultWebSecurityManager securityManager) {
       ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();
       //关联securityManager-设置安全管理器
       bean.setSecurityManager(securityManager);
   
       Map<String, String> filterMap = new LinkedHashMap<>();
       filterMap.put("/user/add", "authc");
       filterMap.put("/user/update", "authc");
       bean.setFilterChainDefinitionMap(filterMap);
   
       return bean;
   }
   ```

2. 访问链接进行测试！拦截OK！

   但是发现，点击后会跳转到一个Login.jsp页面，这个不是我们想要的效果，我们需要自己定义一个Login页面！

   ![image-20240521162032813](https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240521162026623-1442348085.png)

3.  我们编写一个自己的Login页面

   ```html
   <body>
   <h1>登录</h1>
   <form action="">
       <p>用户名： <input type="text" name="username"></p>
       <p>密码： <input type="text" name="password"></p>
       <p><input type="submit"></p>
   </form>
   </body>
   ```

4. 编写跳转的controller

   ```java
   @RequestMapping("/toLogin")
   public String toLogin() {
       return "login";
   }
   ```

5. 在shiro中配置一下！ ShiroFilterFactoryBean() 方法下面

   ```java
   //设置登录的请求
   bean.setLoginUrl("/toLogin");
   ```

6. 优化一下代码，我们这里的拦截可以使用 通配符来操作

   ```java
   filterMap.put("/user/*", "authc");
   ```

7. 测试，完全OK！

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240521162314023-980183246.png" alt="image-20240521162320384" style="zoom:33%;" />

## 伪造登录认证

1. 编写一个登录的controller  

   ```java
   @RequestMapping("/login")
   public String login(String username, String password, Model model) {
       //1.获得当前的用户
       Subject subject = SecurityUtils.getSubject();
       //2.封装用户的登录数据
       UsernamePasswordToken token = new UsernamePasswordToken(username, password, true);
       //3.执行登录的方法
       try {
           subject.login(token);
           return "index";
       } catch (UnknownAccountException e) {
           model.addAttribute("msg", "用户名不存在");
           return "login";
       } catch (IncorrectCredentialsException e) {
           model.addAttribute("msg", "密码错误");
           return "login";
       }
   }
   ```

2. 在前端修改对应的信息输出或者请求！登录页面增加一个 msg 提示：给表单增加一个提交地址：

   ```html
   <p th:text="${msg}" style="color: red"></p>
   <form th:action="@{/login}">
   ```

3.  提交测试一下

   确实执行了我们的认证逻辑！

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240521164216073-1996355036.png" alt="image-20240521164222340" style="zoom:33%;" />

4. 在 UserRealm 中编写用户认证的判断逻辑

   ```java
   //2.认证
   @Override
   protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
       System.out.println("已认证=>doGetAuthorizationInfo");
   
       //用户名，密码
       String name = "root";
       String password = "root";
   
       UsernamePasswordToken userToken = (UsernamePasswordToken) token;
       if (!userToken.getUsername().equals(name)) {
           //会自动抛出异常 UnknownAccountException
           return null;
       }
       //密码认证，shiro做
       return new SimpleAuthenticationInfo("", password, "");
   }
   ```

5. 测试

   1. 密码认证，shiro会自动判断
   2. 输入成功就会进入首页

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240521164837272-1318676904.png" alt="image-20240521164843500" style="zoom:33%;" />

## 整合数据库认证

### 环境搭建

1. 导入相关依赖

   ```xml
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <version>5.1.47</version>
   </dependency>
   <dependency>
       <groupId>log4j</groupId>
       <artifactId>log4j</artifactId>
       <version>1.2.17</version>
   </dependency>
   <dependency>
       <groupId>com.alibaba</groupId>
       <artifactId>druid</artifactId>
       <version>1.2.22</version>
   </dependency>
   <dependency>
       <groupId>org.mybatis.spring.boot</groupId>
       <artifactId>mybatis-spring-boot-starter</artifactId>
       <version>2.1.1</version>
   </dependency>
   <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
       <version>1.18.30</version>
   </dependency>
   ```

2. 编写配置文件-连接配置 application.yml

   ```yaml
   spring:
     datasource:
       username: root
       password: root
       url: jdbc:mysql://localhost:3306/mybatis?useUnicode=true&characterEncoding=utf-8&serverTimezone=UTC
       driver-class-name: com.mysql.jdbc.Driver
       type: com.alibaba.druid.pool.DruidDataSource
   ```

3. 编写mybatis的配置 application.properties

   ```properties
   mybatis.type-aliases-package=com.sunm.pojo
   mybatis.mapper-locations=classpath:mapper/*.xml
   ```

4.  编写实体类,引入Lombok

   ```java
   @Data
   @NoArgsConstructor
   @AllArgsConstructor
   public class User {
       private int id;
       private String name;
       private String pwd;
   }
   ```

5. 编写Mapper接口和Mapper配置文件

   ```java
   @Mapper
   @Repository
   public interface UserMapper {
       User queryUserById(int id);
       User queryUserByName(String name);
   }
   ```

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
   
   <!-- namespace=绑定一个对应的Dao/Mapper接口 -->
   <mapper namespace="com.sunm.mapper.UserMapper">
       <select id="queryUserById" resultType="user">
           select *
           from user
           where id = #{id}
       </select>
       <select id="queryUserByName" resultType="user">
           select *
           from user
           where name = #{name}
       </select>
   </mapper>
   ```

6. 编写UserService 层

   ```java
   public interface UserService {
       User queryUserById(int id);
       User queryUserByName(String name);
   }
   
   @Service
   public class UserServiceImpl implements UserService {
       @Autowired
       UserMapper userMapper;
   
       @Override
       public User queryUserById(int id) {
           return userMapper.queryUserById(id);
       }
       @Override
       public User queryUserById(String name) {
           return userMapper.queryUserByName(name);
       }
   }
   ```

7. 测试一下，保证能够从数据库中查询出来

   ```java
   @SpringBootTest
   class ShiroSpringbootApplicationTests {
       @Autowired
       UserServiceImpl userService;
   
       @Test
       void contextLoads() {
           System.out.println(userService.queryUserByName("root"));
       }
   
   }
   ```

### 从数据库认证

1. 改造UserRealm，连接到数据库进行真实的操作！

   ```java
   //2.认证
   @Override
   protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
       System.out.println("已认证=>doGetAuthorizationInfo");
   
       UsernamePasswordToken userToken = (UsernamePasswordToken) token;
       //连接真实数据库
       User user = userService.queryUserByName(userToken.getUsername());
       if (user == null) {
           return null;
       }
   
       //密码认证，shiro做
       return new SimpleAuthenticationInfo("", user.getPwd(), "");
   }
   ```

2. 测试，现在查询都是从数据库查询的了！

### 密码加密

这个Shiro，是怎么帮我们实现密码自动比对的呢？

- 在密码认证处打断点，观察代码
- 对密码，使用的默认简单加密方式

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240521175553768-1429840730.png" alt="image-20240521175559900" style="zoom: 33%;" />

我们可以去 realm的父类 AuthorizingRealm 的父类 AuthenticatingRealm 中找一个方法核心： `getCredentialsMatcher() ` - - ->获取证书匹配器

接口 CredentialsMatcher 有很多的实现类 :

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240521180010358-642041313.png" alt="image-20240521180016633" style="zoom: 33%;" />

我们的密码一般都不能使用明文保存？需要加密处理；思路分析

文章参考：https://blog.csdn.net/lucky_shanshan/article/details/119353198

1. 如何把一个字符串加密
2. 替换当前的Realm 的 CredentialsMatcher 属性，直接使用 加密对象，并设置加密算法

## 授权

### 在过滤器拦截

使用shiro的过滤器来拦截请求即可！

1. 在 ShiroFilterFactoryBean 中添加一个过滤器

   ```java
   Map<String, String> filterMap = new LinkedHashMap<>();
   
   //授权
   filterMap.put("/user/add", "perms[user:add]");    //只有user角色的add权限才可以
   
   filterMap.put("/user/*", "authc");
   bean.setFilterChainDefinitionMap(filterMap);
   ```

2. 我们再次启动测试一下，访问add，发现以下错误！`Unauthorized:未授权错误！`

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240521191111822-391447985.png" alt="image-20240521191117676" style="zoom: 33%;" />

   注意：当我们实现权限拦截后，shiro会自动跳转到未授权的页面，但我们没有这个页面，所有401了

3. 配置一个未授权的提示的页面，增加一个controller提示

   ```java
   @RequestMapping("/noauth")
   @ResponseBody
   public String Unauthorized() {
       return "未经授权，无法访问页面！";
   }
   ```

4. 然后再 shiroFilterFactoryBean 中配置一个未授权的请求页面

   ```java
   //设置未授权页面
   bean.setUnauthorizedUrl("/noauth");
   ```

5. 测试，现在没有授权，可以跳转到我们指定的位置了！

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240521191624675-1132201713.png" alt="image-20240521191630567" style="zoom:40%;" />

### 授权字符串

在UserRealm 中添加授权的逻辑，增加授权的字符串！

- 此时所有进来的用户都添加了授权字符串

```java
//1.授权
@Override
protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
    System.out.println("已授权=>doGetAuthorizationInfo");

    //给资源进行授权
    SimpleAuthorizationInfo info = new SimpleAuthorizationInfo();
    
    //添加资源的授权字符串
    info.addStringPermission("user:add");

    return info;
}
```

我们再次登录测试，发现登录的用户是可以进行访问add 页面了！授权成功！

### 问题

我们现在完全是硬编码，无论是谁登录上来，都可以实现授权通过，但是真实的业务情况应该是，每个用户拥有自己的一些权限，从而进行操作，所以说，`权限应该在用户的数据库中`。

正常的情况下，应该数据库中是由一个权限表的，我们需要联表查询，但是这里为了大家操作理解方便一些，我们直接在数据库表中增加一个字段来进行操作！

### 优化

1. 修改实体类，增加一个字段perms-varchar(100)

2. 我们给数据库中的用户增加一些权限

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240521192810914-958424360.png" alt="image-20240521192816772" style="zoom:43%;" />

3. 我们现在需要再自定义的授权认证中，获取登录的用户，从而实现动态认证授权操作！

   在用户登录授权的时候，将用户放在 Principal 中，改造下之前的代码

   然后再授权的地方获得这个用户，从而获得它的权限

   ```java
   //1.授权
       @Override
       protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
           System.out.println("已授权=>doGetAuthorizationInfo");
   
           //给资源进行授权
           SimpleAuthorizationInfo info = new SimpleAuthorizationInfo();
           //添加资源的授权字符串
   //        info.addStringPermission("user:add");
   
           //拿到当前登录对象
           Subject subject = SecurityUtils.getSubject();
           User currentUser = (User) subject.getPrincipal();
           //设置当前用户的权限
           info.addStringPermission(currentUser.getPerms());
   
           return info;
       }
   ```

4. 在过滤器中，将 update 请求也进行权限拦截下

5. 我们启动项目，登录不同的账户，进行测试一下！

6. 测试完美通过OK！

## 整合Thymeleaf

### 权限控制

1. 添加Maven的依赖； 

   ```xml
   <dependency>
       <groupId>com.github.theborakompanioni</groupId>
       <artifactId>thymeleaf-extras-shiro</artifactId>
       <version>2.0.0</version>
   </dependency>
   ```

2. 配置一个shiro的Dialect ，在shiro的配置中增加一个Bean

   ```java
   //整合shiro的dialect
   @Bean
   public ShiroDialect getShiroDialect(){
       return new ShiroDialect();
   }
   ```

3. 修改前端的配置

   ```html
   <body>
       <h1>首页</h1>
       <p th:text="${msg}"></p>
       <p>
           <a th:href="@{/toLogin}">登录</a>
       </p>
   
       <div shiro:hasPermission="user:add">
           <a th:href="@{/user/add}">add</a>
       </div>
       <div shiro:hasPermission="user:update">
           <a th:href="@{/user/update}">update</a>
       </div>
   
   </body>
   ```

4. 测试一下

   登录后，可以看到不同的用户，有不同的效果，现在就已经接近完美了

### 优化

1. 我们在用户登录后应该把信息放到Session中，我们完善下！

   在执行认证逻辑时候，加入session

   ```java
   //2.认证
   @Override
   protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
       System.out.println("已认证=>doGetAuthorizationInfo");
   
       UsernamePasswordToken userToken = (UsernamePasswordToken) token;
       //连接真实数据库
       User user = userService.queryUserByName(userToken.getUsername());
       if (user == null) {
           return null;
       }
   	//把user放入Session
       Subject subject = SecurityUtils.getSubject();
       Session session = subject.getSession();
       session.setAttribute("loginUser", user);
   
       //密码认证，shiro做
       //可以加密
   
       return new SimpleAuthenticationInfo(user, user.getPwd(), "");
   }
   ```

2. 前端从session中获取，然后用来判断是否显示登录

   ```html
   <div th:if="${session.loginUser==null}">
       <p>
           <a th:href="@{/toLogin}">登录</a>
       </p>
   </div>
   ```

3. 测试，效果完美~ 