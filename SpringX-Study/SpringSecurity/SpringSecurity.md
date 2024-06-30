# 简介

## web开发安全

### 概述

在 Web 开发中，安全一直是非常重要的一个方面。安全虽然属于应用的非功能性需求，但是应该在应用开发的初期就考虑进来。如果在应用开发的后期才考虑安全的问题，就可能陷入一个两难的境地：

- 一方面，应用存在严重的安全漏洞，无法满足用户的要求，并可能造成用户的隐私数据被攻击者窃取；
- 另一方面，应用的基本架构已经确定，要修复安全漏洞，可能需要对系统的架构做出比较重大的调整，因而需要更多的开发时间，影响应用的发布进程。

因此，从应用开发的第一天就应该把安全相关的因素考虑进来，并在整个应用的开发过程中。市面上存在比较有名的安全框架：Shiro，Spring Security ！

### 分类

Web 应用的安全性包括`用户认证`（Authentication）和`用户授权`（Authorization）两个部分：

- 用户认证指的是：验证某个用户是否为系统中的合法主体，也就是说用户能否访问该系统。用户认证一般要求用户提供用户名和密码。系统通过校验用户名和密码来完成认证过程。
- 用户授权指的是：验证某个用户是否有权限执行某个操作。在一个系统中，不同用户所具有的权限是不同的。比如对一个文件来说，有的用户只能进行读取，而有的用户可以进行修改。一般来说，系统会为不同的用户分配不同的角色，而每个角色则对应一系列的权限。

## SpringSecurity

### 文档

- 中文文档：https://springdoc.cn/spring-security/
- Spring Security官网：https://docs.spring.io/spring-security/reference/index.html

### 概述

Spring Security 是针对Spring项目的安全框架，也是Spring Boot底层安全模块默认的技术选型，他提供了一套Web 应用安全性的完整解决方案。对于安全控制，我们仅需要引入 spring-boot-starter-security 模块，进行少量的配置，即可实现强大的安全管理！

- Spring  Security是一个Java框架，用于保护应用程序的安全性。
- 它提供了一套全面的安全解决方案，包括`身份验证、授权、防止攻击`等功能。
- Spring  Security基于过滤器链的概念，可以轻松地集成到任何基于Spring的应用程序中。
- 它支持多种身份验证选项和授权策略，开发人员可以根据需要选择适合的方式。
- 此外，Spring Security还提供了一些附加功能，如集成第三方身份验证提供商和单点登录，以及会话管理和密码编码等。
- 总之，Spring  Security是一个强大且易于使用的框架，可以帮助开发人员提高应用程序的安全性和可靠性。

### 权限分类

- 功能权限
- 访问权限
- 菜单权限
- 之前用拦截器/过滤器：代码繁琐，冗余

### 主要目标

1. “认证”（Authentication）

   身份验证是关于验证您的凭据，如用户名/用户ID和密码，以验证您的身份。身份验证通常通过用户名和密码完成，有时与身份验证因素结合使用。

2. “授权” （Authorization）（访问控制）

   授权发生在系统成功验证您的身份后，最终会授予您访问资源（如信息，文件，数据库，资金，位置，几乎任何内容）的完全权限。这个概念是通用的，而不是只在Spring Security 中存在。

对于用户认证和用户授权，Spring Security 框架都有很好的支持：

- `用户认证`方面：Spring Security 框架支持主流的认证方式，包括 HTTP 基本认证、HTTP 表单验证、HTTP 摘要认证、OpenID和 LDAP 等。
- `用户授权`方面，Spring Security 提供了基于角色的访问控制和访问控制列表（Access Control List，ACL），可以对应用中的领域对象进行细粒度的控制。

### 代码分析

- WebSecurityConfigurerAdapter： 自定义Security策略[已经弃用]
- AuthenticationManagerBuilder：自定义认证策略
- @EnableWebSecurity：开启WebSecurity模式

# 测试环境搭建

1. 新建一个初始的springboot项目

   选择web模块 ， thymeleaf模块

2. 创建后，模块导入成功

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-thymeleaf</artifactId>
   </dependency>
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-web</artifactId>
   </dependency>
   ```

3. 导入静态资源

   - static
   - templates

4. 修改全局配置

   ```properties
   #关闭缓存
   spring.thymeleaf.cache=false
   ```

5. controller跳转！

   ```java
   @Controller
   public class RouterController {
       @RequestMapping({"/", "/index"})
       public String index() {
           return "index";
       }
   
       @RequestMapping("/toLogin")
       public String toLogin() {
           return "views/login";
       }
   
       @RequestMapping("/level1/{id}")
       public String level1(@PathVariable("id") int id) {
           return "views/level1/" + id;
       }
   
       @RequestMapping("/level2/{id}")
       public String level2(@PathVariable("id") int id) {
           return "views/level2/" + id;
       }
   
       @RequestMapping("/level3/{id}")
       public String level3(@PathVariable("id") int id) {
           return "views/level3/" + id;
       }
   }
   ```

6. 测试实验环境OK！  

# 授权

目前，我们的测试环境，是谁都可以访问的，我们使用 Spring Security 增加上认证和授权的功能

1. 引入 Spring Security 模块

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-security</artifactId>
   </dependency>
   ```

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240519173836707-868610481.png" alt="image-20240519173842339" style="zoom:53%;" />

2. 引入成功后，此时访问页面会被拦截.

   1. ，没有认证去到默认登录页面/login
   2. 如果认证失败去/login?error

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240519194356912-1673232176.png" alt="image-20240519194402782" style="zoom:33%;" />

3. 编写 Spring Security 配置类

   ```java
   @Configuration
   @EnableWebSecurity //让WebSecurity 生效
   public class SecurityConfig {
       //链式编程
       @Bean
       public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
           //首页所有人可以访问
           http
                   .authorizeHttpRequests(authorize -> authorize
                           .requestMatchers("/").permitAll())
                   //功能页，指定用户访问
                   .authorizeHttpRequests(authorize -> authorize
                           .requestMatchers("/level1/*").hasRole("vip1"))
                   .authorizeHttpRequests(authorize -> authorize
                           .requestMatchers("/level2/*").hasRole("vip2"))
                   .authorizeHttpRequests(authorize -> authorize
                           .requestMatchers("/level3/*").hasRole("vip3"))
                   //没有权限就去到默认登录页面
                   .formLogin(withDefaults());
   
           return http.build();
       }
   }
   ```

   

4. 测试一下

   1. 发现除了首页都进不去了！
   2. 因为请求需要登录的角色拥有对应的权限才可以！

# 认证

## 用户名/密码存储

### 内存

1. 编写认证代码-在内存中查询认证信息

   ```java
   //认证
   @Bean
   public UserDetailsService users() {
       //withDefaultPasswordEncoder 在保存进内存之前对密码进行编码
       User.UserBuilder users = User.withDefaultPasswordEncoder();
       UserDetails admin = users
               .username("admin")
               .password("111")
               .roles("vip1", "vip2", "vip3")
               .build();
       UserDetails guest = users
               .username("guest")
               .password("222")
               .roles("vip1")
               .build();
       //InMemoryUserDetailsManager在内存中查看用户信息
       return new InMemoryUserDetailsManager(admin, guest);
   ```

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240519204403909-1244593404.png" alt="image-20240519204409454" style="zoom:33%;" />

2. 访问/level1/1无权限，此时登录admin的用户信息测试

   - 进入/level1/1成功
   - 因为admin的roles中包含了vip1的角色
   - 每个角色只能访问自己认证下的规则！

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240519204403909-1244593404.png" alt="image-20240519204409454" style="zoom:33%;" /><img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240519204548693-1921616421.png" alt="image-20240519204554906" style="zoom:33%;" />

3. 清除浏览器缓存，用guest的用户信息访问/level2/1

   - 进入失败
   - 因为guest的roles中不含有vip2的角色

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240519205026592-1499223177.png" alt="image-20240519205032161" style="zoom:33%;" />

### JDBC

```java
@Autowired
private DataSource dataSource;

//认证
@Bean
public UserDetailsService users() {
    // The builder will ensure the passwords are encoded before saving in memory
    //在内存中
    User.UserBuilder users = User.withDefaultPasswordEncoder();
    UserDetails admin = users
            .username("admin")
            .password("111")
            .roles("vip1", "vip2", "vip3")
            .build();
    UserDetails guest = users
            .username("guest")
            .password("222")
            .roles("vip1")
            .build();
    
    JdbcUserDetailsManager users = new JdbcUserDetailsManager(dataSource);
    users.createUser(admin);
    users.createUser(guest);
    
    return users;
}
```

## 注销

1. 开启自动配置的注销的功能

   ```java
   http.logout(withDefaults());
   ```

2. 我们在前端，增加一个注销的按钮， index.html 导航栏中

   ```html
   <!--注销 图标可在semanticUI中获取，只需要修改class的内容-->
   <a class="item" th:href="@{/logout}">
       <i class="sign-out icon"></i> 注销
   </a>
   ```

3. 我们可以去测试一下，登录成功后点击注销

   - 发现注销完毕会跳转到默认的/logout页面

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240520084127439-135808439.png" alt="image-20240520084133407" style="zoom:33%;" />

   - 点击Log Out，注销成功后进入/login?logout页面

     <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240520084239995-359514704.png" alt="image-20240520084246111" style="zoom:33%;" />

4. 设置注销成功后，进入首页

   ```java
   .logout((logout) ->
           logout.logoutSuccessUrl("/"));
   ```

5. logout源码

   ```java
   logout((logout) ->
          logout. deleteCookies("remove")
          .invalidateHttpSession(false)
          .logoutUrl("/ custom-logout")	//注销页面的url
          .logoutSuccessUrl("/ logout-success")	//注销成功后的跳转URL
         );
   ```

## 权限控制

### 分析

需求：设置特定用户只能看到权限以内的界面，看不到其他用户的界面

解决方法：我们需要结合thymeleaf中的一些功能sec：authorize="isAuthenticated()":是否认证登录！ 来显示不同的页面

- thymeleaf整合springSecurity的Maven依赖(注意版本问题)

```xml
<dependency>
    <groupId>org.thymeleaf.extras</groupId>
    <artifactId>thymeleaf-extras-springsecurity3</artifactId>
    <version>3.0.5.RELEASE</version>
</dependency>
```

### 代码实现

修改我们的index页面

1. 导入命名空间

   ```html
   <html lang="en" xmlns:th="http://www.thymeleaf.org"
   xmlns:sec="http://www.thymeleaf.org/extras/spring-security">
   ```

2. 修改导航栏，增加认证判断

   thymeleaf-extras-springsecurity3.X，支持thymeleaf3.X，只支持spring5.1.X以前的，springboot3.X开始就不支持spring5

   - !isAuthenticated(): 被认证
   - principal.authorities： 当前认证角色

   ```html
   <!--如果未登录，只显示登录按钮-->
   <div sec:authentication="!isAuthenticated()">
       <!--登录-->
       <a class="item" th:href="@{/toLogin}">
           <i class="address card icon"></i> 登录
       </a>
   </div>
   
   <!--如果登录，显示用户名和登录按钮-->
   <div sec:authentication="isAuthenticated()">
       <!--用户名-->
       <a class="item">
           用户名：<span sec:authentication="name"></span>
           角色：<span sec:authentication="principal.authorities"></span>
       </a>
   </div>
   <div>
       <!--注销-->
       <a class="item" th:href="@{/logout}">
           <i class="sign-out icon"></i> 注销
       </a>
   </div>
   ```

如果注销404了，就是因为它默认防止csrf跨站请求伪造，因为会产生安全问题

- 我们可以将请求改为post表单提交

- 或者在spring security中关闭csrf功能

  ```java
  .csrf(csrf -> csrf.disable())
  ```

## 记住我认证

现在的情况，我们只要登录之后，关闭浏览器，再登录，就会让我们重新登录，但是很多网站的情况，就是有一个记住密码的功能，这个该如何实现呢？很简单  

1. 开启记住我功能

   ```java
   http.rememberMe(withDefaults());
   ```

2. 我们再次启动项目测试一下，发现登录页多了一个记住我功能

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240520113938902-50807726.png" alt="image-20240520113944923" style="zoom:33%;" />

   我们登录之后关闭 浏览器，然后重新打开浏览器访问，发现用户依旧存在！

思考：如何实现的呢？

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240520120856109-1933259199.png" alt="image-20240520120901963" style="zoom:33%;" />

1. 其实非常简单查看浏览器的cookie。登录成功后，将cookie发送给浏览器保存，以后登录带上这个cookie，只要通过检查就可以免登录了
2. 我们点击注销的时候，可以发现，spring security 帮我们自动删除了这个 cookie

## 定制登录页

现在这个登录页面都是spring security 默认的，怎么样可以使用我们自己写的Login界面呢？

1. 在刚才的登录页配置后面指定 loginpage

   ```java
   http.formLogin(formLogin -> formLogin.loginPage("/toLogin")
                           .loginProcessingUrl("/login"))
   ```

2. 然后前端login.html也需要指向我们自己定义的 login请求

   ```html
   <form th:action="@{/login}" method="post">
   ```

3. Spring Security配置：确保你的安全配置没有错误地重定向到登录页面或者其他需要认证的URL。

   给登录页授权

   ```java
   http.authorizeHttpRequests(authorize -> authorize
           .requestMatchers("/toLogin").permitAll())
   ```

4. 我们登录，需要将这些信息发送到哪里，我们也需要配置

   - login.html 配置提交请求方式必须为post
   - 默认输入的用户名和密码的name为username和username

   ```java
   //formLogin的默认值
   .formLogin((formLogin) ->
     				formLogin
     					.usernameParameter("username")
     					.passwordParameter("username")
     					.loginPage("/ authentication/ login")
     					.failureUrl("/ authentication/ login?failed")
     					.loginProcessingUrl("/ authentication/ login/ process")
     			);
     		return http. build();
     	}
   ```

5. 在登录页增加记住我的多选框

   ```html
   <input type="checkbox" name="remember">记住我
   ```

   ```java
   .rememberMe(rememberMe-> 
           rememberMe.rememberMeParameter("remember"));
   ```

过滤静态资源

```java
http
        .authorizeHttpRequests(authorize -> authorize
                .requestMatchers("/", "/index", "/toLogin", "/sunm/**").permitAll())
```

# 完整配置代码

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    //授权
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        //首页所有人可以访问
        http
                .authorizeHttpRequests(authorize -> authorize
                        .requestMatchers("/").permitAll())
                .authorizeHttpRequests(authorize -> authorize
                        .requestMatchers("/toLogin").permitAll())
                //功能页，指定用户访问
                .authorizeHttpRequests(authorize -> authorize
                        .requestMatchers("/level1/*").hasRole("vip1"))
                .authorizeHttpRequests(authorize -> authorize
                        .requestMatchers("/level2/*").hasRole("vip2"))
                .authorizeHttpRequests(authorize -> authorize
                        .requestMatchers("/level3/*").hasRole("vip3"))
                //没有权限就去到默认登录页面
                .formLogin(formLogin -> formLogin.loginPage("/toLogin")
                        .loginProcessingUrl("/login"))
                .logout((logout) ->
                        logout.logoutSuccessUrl("/"))
                .csrf(AbstractHttpConfigurer::disable)
                .rememberMe(rememberMe ->
                        rememberMe.rememberMeParameter("remember"));

        return http.build();
    }

    //认证
    @Bean
    public UserDetailsService users() {
        // The builder will ensure the passwords are encoded before saving in memory
        //在内存中
        User.UserBuilder users = User.withDefaultPasswordEncoder();
        UserDetails admin = users
                .username("admin")
                .password("111")
                .roles("vip1", "vip2", "vip3")
                .build();
        UserDetails guest = users
                .username("guest")
                .password("222")
                .roles("vip1")
                .build();
        return new InMemoryUserDetailsManager(admin, guest);
    }
}
```
