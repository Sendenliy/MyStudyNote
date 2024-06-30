# MVC

## 什么是MVC

- MVC是模型(Model)、视图(View)、控制器(Controller)的简写，是一种软件设计规范。
- `最典型的MVC就是JSP + servlet + javabean的模式`
- 是将业务逻辑、数据、显示分离的方法来组织代码。
- MVC主要作用是降低了视图与业务逻辑间的双向偶合。
- MVC不是一种设计模式，MVC是一种架构模式。当然不同的MVC存在差异

### Model模型

1. 数据模型，提供要展示的数据，因此包含数据和行为，可以认为是领域模型或JavaBean组件（包含数据和行为）
2. 不过现在一般都分离开来：Value Object（数据Dao） 和 服务层（行为Service）。
3. 模型提供了：模型数据查询和模型数据的状态更新等功能，包括数据和业务。

### View视图

负责进行模型的展示，一般就是我们见到的用户界面，客户想看到的东西。（jsp）

- POJO实体类有时为了减少浪费，会封装一些属性较少的对象来使用
- 比如vo,viewObject；dto,dataObject

### Controller控制器

- 接收用户请求，委托给模型进行处理（状态改变），处理完毕后把返回的模型数据返回给视图，由视图负责展示。 
- 也就是说控制器做了个调度员的工作。
- （Servlet）

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240504134424520-1598206334.png" alt="image-20240504134425045" width="400" />

## Model模型发展

### Model1

- 在web早期的开发中，通常采用的都是Model1。
- Model1中，主要分为两层，视图层(jsp)和模型层(dao/service)。
- jsp本质就是个Servlet

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240504134432468-2066676901.png" alt="image-20240504134433471" width="400" />

Model1优点：架构简单，比较适合小型项目开发；

Model1缺点：JSP职责不单一，职责过重，不便于维护；

### Model2

Model2把一个项目分成三部分，包括视图、控制、模型  

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240504134451513-1026964299.png" alt="image-20240504134452835" width="400" />

1. 用户发请求
2. Servlet接收请求数据，并调用对应的业务逻辑方法
3.  业务处理完毕，返回更新后的数据给servlet
4.  servlet转向到JSP，由JSP来渲染页面
5.  响应给前端更新后的页面

职责分析： 

1. Controller：控制器
   1. 取得表单数据
   2. 调用业务逻辑
   3. 转向指定的页面
2. Model：模型
   1. 业务逻辑
   2. 保存数据的状态
3. View：视图
   1. 显示页面

Model2这样不仅提高的代码的复用率与项目的扩展性，且大大降低了项目的维护成本。Model 1模式的实现比较简单，适用于快速开发小规模项目，Model1中JSP页面身兼View和Controller两种角色，将控制逻辑和表现逻辑混杂在一起，从而导致代码的重用性非常低，增加了应用的扩展性和维护的难度。Model2消除了Model1的缺点。

## 回顾Servlet

1. 新建一个Maven工程当做父工程！ pom依赖！

   ```xml
   <dependencies>
       <dependency>
           <groupId>junit</groupId>
           <artifactId>junit</artifactId>
           <version>4.12</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>javax.servlet</groupId>
           <artifactId>servlet-api</artifactId>
           <version>2.5</version>
       </dependency>
       <dependency>
           <groupId>javax.servlet.jsp</groupId>
           <artifactId>jsp-api</artifactId>
           <version>2.1</version>
       </dependency>
       <!-- https://mvnrepository.com/artifact/javax.servlet/jstl -->
       <dependency>
           <groupId>javax.servlet</groupId>
           <artifactId>jstl</artifactId>
           <version>1.2</version>
       </dependency>
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-webmvc</artifactId>
           <version>6.1.6</version>
       </dependency>
   </dependencies>
   ```

2. 建立一个Moudle：springmvc-01-servlet ， 添加Web app的支持！

   - 如果使用Maven直接创建一个webapp的项目，版本过低需要手动修改
   - 可以添加框架支持，选择webapplication，会自动生成4.0版本

3. 导入servlet 和 jsp 的 jar 依赖

4. 编写一个HelloServlet类，用来处理用户的请求

   注意点：

   - new一个类继承HttpServlet
   - 在web.xml中配置该类

   ```java
   public class HelloServlet extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           //1.获取前端参数
           String method = req.getParameter("method");
           if (method.equals("add")) {
               req.getSession().setAttribute("msg","执行了add方法");
           }
           if (method.equals("delete")) {
               req.getSession().setAttribute("msg","执行了delete方法");
           }
           //2.调用业务层
           
           //3.视图转发/重定向
           req.getRequestDispatcher("/WEB-INF/jsp/test.jsp").forward(req,resp);
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doGet(req, resp);
       }
   }
   ```

5. 编写test.jsp，在WEB-INF目录下新建一个jsp的文件夹，新建test.jsp

   ```jsp
   <body>
   	${msg}
   </body>
   ```

6. 在web.xml中注册Servlet

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <servlet>
           <servlet-name>HelloServlet</servlet-name>
           <servlet-class>com.sun.HelloServlet</servlet-class>
       </servlet>
       <servlet-mapping>
           <servlet-name>HelloServlet</servlet-name>
           <url-pattern>/hello</url-pattern>
       </servlet-mapping>
   
       <!--    <session-config>-->
       <!--        <session-timeout>15</session-timeout>-->
       <!--    </session-config>-->
       <!--    -->
       <!--    <welcome-file-list>-->
       <!--        <welcome-file>index.jsp</welcome-file>-->
       <!--    </welcome-file-list>-->
   </web-app>
   ```

7. 配置Tomcat，并启动测试

MVC框架要做哪些事情

1.  将url映射到java类或java类的方法
2. 封装用户提交的数据
3. 处理请求--调用相关的业务处理--封装响应数据
4. 将响应的数据进行渲染 . jsp / html 等表示层数据

说明：

1. 常见的服务器端MVC框架有：Struts、Spring MVC、ASP.NET MVC、Zend Framework、JSF；
2. 常见前端MVC框架：vue、angularjs、react、backbone；
3. 由MVC演化出了另外一些模式如：MVP、MVVM(M V VM-view model:双向绑定)等等....

# 什么是SpringMVC

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240504134815789-727582768.png" alt="image-20240504134817007" width="300" />

## 概述

1. Spring MVC是Spring Framework的一部分，是基于Java实现MVC的轻量级Web框架。
2. 查看官方文档：https://docs.spring.io/spring-framework/reference/web/webmvc.html

我们为什么要学习SpringMVC呢?

Spring MVC的特点：

1. 轻量级，简单易学
2. 高效 , 基于请求响应的MVC框架
3. 与Spring兼容性好，无缝结合
4.  约定优于配置【Maven】
5. 功能强大：RESTful、数据验证、格式化、本地化、主题等
6. 简洁灵活

Spring的web框架围绕DispatcherServlet [ 调度Servlet ] 设计。

DispatcherServlet的作用是将请求分发到不同的处理器。从Spring 2.5开始，使用Java 5或者以上版本的用户可以采用基于注解形式进行开发，十分简洁；

## 中心控制器

Spring的web框架围绕`DispatcherServlet`设计。 DispatcherServlet的作用是`将请求分发到不同的处理器`。从Spring 2.5开始，使用Java 5或者以上版本的用户可以采用基于注解的controller声明方式。

Spring MVC框架像许多其他MVC框架一样, 以请求为驱动 , 围绕一个中心Servlet分派请求及提供其他功能，`DispatcherServlet是一个实际的Servlet `(它继承自HttpServlet 基类)。

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240505141500570-1044138627.png" alt="image-20240505141501153" width="500" />

SpringMVC的原理如下图所示：

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240504135021737-541637657.png" alt="image-20240504135022689" width="500" />

1. 当发起请求时被前置的控制器(jsp/Servlet/DispatcherServlet)拦截到请求，
2. 根据请求参数生成代理请求，找到请求对应的实际控制器，
3. 控制器(java程序/方法)处理请求，创建数据模型(service层/dao层)，访问数据库，
4. 将模型响应给中心控制器，
5. 控制器使用模型与视图ModelAndView渲染视图结果，
6. 将结果返回给前端控制器，
7. 再将结果返回给请求者。

## SpringMVC执行原理

![image-20240505161831365](https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240505161830255-1954461423.png)

简要分析执行流程

1.  DispatcherServlet表示前置控制器，是整个SpringMVC的控制中心。用户发出请求，DispatcherServlet接收请求并拦截请求。

   我们假设请求的url为 : http://localhost:8080/SpringMVC/hello如上url拆分成三部分：

   - http://localhost:8080服务器域名
   - SpringMVC部署在服务器上的web站点
   - hello表示控制器

   通过分析，如上url表示为：请求位于服务器localhost:8080上的SpringMVC站点的hello控制器。

2.  HandlerMapping为处理器映射。DispatcherServlet调用HandlerMapping,HandlerMapping根据请求url查找Handler。

3. HandlerExecution表示具体的Handler,其主要作用是根据url查找控制器，如上url被查找控制器为：hello。

4.  HandlerExecution将解析后的信息传递给DispatcherServlet,如解析控制器映射等。

5. HandlerAdapter表示处理器适配器，其按照特定的规则去执行Handler。

6. Handler让具体的Controller执行。

7. Controller将具体的执行信息返回给HandlerAdapter,如ModelAndView。

8. HandlerAdapter将视图逻辑名或模型传递给DispatcherServlet。

9. DispatcherServlet调用视图解析器(ViewResolver)来解析HandlerAdapter传递的逻辑视图名。

10. 视图解析器将解析的逻辑视图名传给DispatcherServlet。

11. DispatcherServlet根据视图解析器解析的视图结果，调用具体的视图。

12.  最终视图呈现给用户。

# HelloSpringMVC

## 配置版

前期准备

- 新建一个Moudle ， springmvc-02-hello ， 添加web的支持！
- 确定导入了SpringMVC 的依赖！
- 确定发布的项目有lib依赖！

### 步骤

1. 配置web.xml ， 注册DispatcherServlet

   【注意点】/ 和 /* 的区别：

   - < url-pattern > / </ url-pattern > 不会匹配到.jsp， 只针对我们编写的请求；即：.jsp 不会进入spring的 DispatcherServlet类 。
   - < url-pattern > /* </ url-pattern > 会匹配 *.jsp，会出现返回 jsp视图 时再次进入spring的DispatcherServlet 类，导致找不到对应的controller所以报404错。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <!--1.配置DispatcherServlet  SpringMVC的核心；请求分发器，前端控制器-->
       <servlet>
           <servlet-name>springmvc</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <!--2.DispatcherServlet要绑定SpringMVC的配置文件-->
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <!--配置文件命名建议：servlet的name-servlet.xml-->
               <param-value>classpath:springmvc-servlet.xml</param-value>
           </init-param>
           <!--3.设置启动级别 1:和服务器一起启动-->
           <load-on-startup>1</load-on-startup>
       </servlet>
       <!--在SpringMVC中
       /：只匹配所有的请求，不会去匹配jsp页面
       /*：匹配所有的请求，包括jsp页面
       -->
       <servlet-mapping>
           <servlet-name>springmvc</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   
   </web-app>
   ```

2. 编写SpringMVC 的 配置文件！名称：springmvc-servlet.xml 

   [servletname]-servlet.xml说明，这里的名称要求是按照官方来的

   - 添加 处理映射器
   - 添加 处理器适配器
   - 添加 视图解析器

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <!--1.处理器映射器 HandlerMapping-->
       <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
   
       <!--2.处理器适配器 HandlerAdapter-->
       <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
   
       <!--3.视图解析器 ViewResolver 模版引擎：Thymeleaf、Freemarker-->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
           <!--前缀 prefix-->
           <property name="prefix" value="/WEB-INF/jsp/"/>
           <!--前缀 suffix-->
           <property name="suffix" value=".jsp"/>
       </bean>
   
       <!--4.BeanNameUrlHandlerMapping特性：该处理器映射器要根据bean的名字来找-->
       <!--Handler 将自己的类交给SpringIOC容器，注册bean-->
       <bean id="/hello" class="com.sun.controller.HelloController"/>
   
   </beans>
   ```

3. 编写我们要操作业务Controller 

   - 要么实现Controller接口，要么增加注解；
   - 需要返回一个ModelAndView：装数据，封视图；

   ```java
   package com.sun.controller;
   
   import org.springframework.web.servlet.ModelAndView;
   import org.springframework.web.servlet.mvc.Controller;
   
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   
   public class HelloController implements Controller {
       @Override
       public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
           //1.ModelAndView 模型和视图
           ModelAndView mv = new ModelAndView();
   
           //业务代码
           //2.封装对象，放在ModelAndView中。Model  存数据
           String result = "helloSpringMVC";
           mv.addObject("msg", result);
           
           //视图跳转
           //3.封装要跳转的视图，放在ModelAndView中。View  要跳转的地方
           mv.setViewName("test"); //视图解析器会自动拼接为跳转到/WEB-INF/jsp/test.jsp
   
           return mv;
       }
   }
   
   ```

4. 将自己的类交给SpringIOC容器，注册bean

   ```xml
   <!--Handler-->
   <bean id="/hello" class="com.sun.controller.HelloController"/>
   ```

5. 写要跳转的jsp页面，显示ModelandView存放的数据，以及我们的正常页面；

   ```jsp
   <body>
   	${msg}
   </body>
   ```

6. 配置Tomcat 启动测试！  

### 可能遇到的问题

访问出现404。排查步骤：

1. 查看控制台输出，看一下是不是缺少了什么jar包。

2. 如果jar包存在，显示无法输出，就在IDEA的项目发布中，添加lib依赖！

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240505144129362-664389899.png" alt="image-20240505144130619" width="500" />

3. 重启Tomcat 即可解决！

### 小结

看这个估计大部分同学都能理解其中的原理了，但是我们实际开发才不会这么写，不然就疯了，还学这个玩意干嘛！我们来看个注解版实现，这才是SpringMVC的精髓，到底有多么简单

## 注解版

1. 新建一个Moudle，springmvc-03-annotation 。添加web支持！

2. 在pom.xml文件引入相关的依赖：主要有Spring框架核心库、Spring MVC、servlet , JSTL等。我们在父依赖中已经引入了！

3. 由于Maven可能存在资源过滤的问题，我们将配置完善。防止静态资源过滤

   ```xml
   <build>
       <resources>
           <resource>
               <directory>src/main/java</directory>
               <includes>
                   <include>**/*.properties</include>
                   <include>**/*.xml</include>
               </includes>
               <filtering>false</filtering>
           </resource>
           <resource>
               <directory>src/main/resources</directory>
               <includes>
                   <include>**/*.properties</include>
                   <include>**/*.xml</include>
               </includes>
               <filtering>false</filtering>
           </resource>
       </resources>
   </build>
   ```

4. 注意项目结构引入lib库

5. 配置web.xml

   【注意点：】

   - 注意web.xml版本问题，要最新版！
   - 注册DispatcherServlet
   - 关联SpringMVC的配置文件
   - 启动级别为1
   - 映射路径为 / 【不要用/*，会404】

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <!--1.配置DispatcherServlet  SpringMVC的核心；请求分发器，前端控制器-->
       <servlet>
           <servlet-name>springmvc</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <!--2.DispatcherServlet要绑定SpringMVC的配置文件-->
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <!--配置文件命名建议：servlet的name-servlet.xml-->
               <param-value>classpath:springmvc-servlet.xml</param-value>
           </init-param>
           <!--3.设置启动级别 1:和服务器一起启动-->
           <load-on-startup>1</load-on-startup>
       </servlet>
       <!--在SpringMVC中
       /：只匹配所有的请求，不会去匹配jsp页面
       /*：匹配所有的请求，包括jsp页面
       -->
       <servlet-mapping>
           <servlet-name>springmvc</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   </web-app>
   ```

6. 添加Spring MVC配置文件springmvc-servlet.xml

   - 让IOC的注解生效，为了支持基于注解的IOC，设置了自动扫描包的功能
   - 静态资源过滤 ：HTML . JS . CSS . 图片 ， 视频 ..... 
   - MVC的注解驱动
   - 配置视图解析器。在视图解析器中我们把所有的视图都存放在/WEB-INF/目录下，这样可以保证视图安全，因为这个目录下的文件，客户端不能直接访问。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
               http://www.springframework.org/schema/beans/spring-beans.xsd
               http://www.springframework.org/schema/context
               https://www.springframework.org/schema/context/springcontext.xsd
               http://www.springframework.org/schema/mvc
               https://www.springframework.org/schema/mvc/spring-mvc.xsd">
       <!-- 1.自动扫描包，让指定包下的注解生效,由IOC容器统一管理 -->
       <context:component-scan base-package="com.sun.controller"/>
       <!-- 2.让Spring MVC不处理静态资源 .css .html .js .mp3 .mp4-->
       <mvc:default-servlet-handler/>
       
       <!--
       支持mvc注解驱动
           在spring中一般采用@RequestMapping注解来完成映射关系
           要想使@RequestMapping注解生效
           必须向上下文中注册DefaultAnnotationHandlerMapping和一个AnnotationMethodHandlerAdapter实例
           这两个实例分别在类级别和方法级别处理。
       3.  annotation-driven配置帮助我们自动完成上述两个实例的注入。
       -->
       <mvc:annotation-driven/>
       <!--4. 视图解析器 -->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
           <!-- 前缀 -->
           <property name="prefix" value="/WEB-INF/jsp/"/>
           <!-- 后缀 -->
           <property name="suffix" value=".jsp"/>
       </bean>
   </beans>
   ```

7. 创建Controller编写一个Java控制类： com.sun.controller.HelloController , 注意编码规范

   - @Controller：是为了让Spring IOC容器初始化时自动扫描到该组件；
   - @RequestMapping：是为了映射请求路径，这里因为类与方法上都有映射所以访问时应该是/hello/h1
   - 方法中声明Model类型的参数是：为了把Action中的数据带到视图中；
   - 方法返回的结果是视图的名称hello，加上配置文件中的前后缀变成WEB-INF/jsp/hello.jsp

   ```java
   @Controller
   @RequestMapping("hello")
   public class HelloController {
       //真实访问地址:  项目名/hello/h1
       @RequestMapping("/h1")
       public String hello(Model model){
           //封装数据  向模型中添加属性msg和值，可以在jsp页面中取出并渲染
           model.addAttribute("msg","Hello,SpringMVCAnnotation");
           
           //返回视图
           return "hello";//会被视图解析器处理
       }
   }
   ```

8. 创建视图层        在WEB-INF/ jsp目录中创建hello.jsp 

   -  视图可以直接取出并展示从Controller带回的信息；
   - 可以通过EL表示取出Model中存放的值，或者对象；

   ```jsp
   <body>
   	${msg}
   </body>
   ```

9. 配置Tomcat运行

   配置Tomcat ， 开启服务器 ， 访问 对应的请求路径！

## 小结

实现步骤：

1. 新建一个web项目
2. 导入相关jar包
3. 编写web.xml , 注册DispatcherServlet
4. 编写springmvc配置文件
5. 接下来就是去创建对应的控制类 , controller
6. 最后完善前端视图jsp和controller之间的对应
7. 测试运行调试

使用springMVC必须配置的三大件：

- 处理器映射器、
- 处理器适配器、
- 视图解析器

通常，我们只需要手动配置视图解析器，而处理器映射器和处理器适配器只需要开启注解驱动即可，而省去了大段的xml配置

# Controller及RestFull

## 控制器Controller

### 概述

1. 控制器提供访问应用程序的复杂行为，通常通过`接口定义或注解定义`两种方法实现。注解方式是平时使用的最多的方式！除了这两种之外还有其他的方式
2. 控制器负责解析用户的请求并将其转换为一个模型。【接收到请求后，应该做什么事情，相当于Servlet】
3. 在Spring MVC中，一个控制器类可以包含多个方法
4. 在Spring MVC中，对于Controller的配置方式有很多种

### 实现Controller接口

- Controller是一个接口，在org.springframework.web.servlet.mvc包下，接口中只有一个方法；  

- 只要实现了Controller接口的类，说明这就是一个控制器了

测试

1. 新建一个Moudle，springmvc-04-controller 。

2. 编写web.xml配置文件！

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <!--1.配置DispatchServlet-->
       <servlet>
           <servlet-name>springmvc</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:springmvc-servlet.xml</param-value>
           </init-param>
           <load-on-startup>1</load-on-startup>
       </servlet>
       <servlet-mapping>
           <servlet-name>springmvc</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   </web-app>
   ```

3. 编写Spring的配置文件springmvc-servlet.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          https://www.springframework.org/schema/context/spring-context.xsd
          http://www.springframework.org/schema/mvc
          https://www.springframework.org/schema/mvc/spring-mvc.xsd">
   	<!--
       <context:component-scan base-package="com.sun.controller"/>
       <mvc:default-servlet-handler/>
       <mvc:annotation-driven/>
   	-->
   
       <!--3.视图解析器 ViewResolver 模版引擎：Thymeleaf、Freemarker-->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
           <!--前缀 prefix-->
           <property name="prefix" value="/WEB-INF/jsp/"/>
           <!--前缀 suffix-->
           <property name="suffix" value=".jsp"/>
       </bean>
   
   </beans>
   ```

4. 编写一个Controller类，ControllerTest1

   ```java
   public class ControllerTest1 implements Controller {
       //返回一个ModelAndView对象
       @Override
       public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
           ModelAndView modelAndView = new ModelAndView();
   
           modelAndView.addObject("msg", "ControllerTest1");
           modelAndView.setViewName("test");
   
           return modelAndView;
       }
   }
   ```

5. 编写完毕后，去Spring配置文件中注册请求的bean；name对应请求路径，class对应处理请求的类

   ```xml
   <bean name="/t1" class="com.sun.controller.ControllerTest1"/>
   ```

6. 编写前端test.jsp，注意在WEB-INF/jsp目录下编写，对应我们的视图解析器

7. 配置Tomcat运行测试，我这里没有项目发布名配置的就是一个 / ，所以请求不用加项目名，OK！

说明：

- 实现接口Controller定义控制器是较老的办法
- 缺点是：一个控制器中只有一个方法，如果要多个方法则需要定义多个Controller；定义的方式比较麻烦；

### 使用注解@Controller

- @Controller：代表这个类会被Spring接管
- 里面所有的方法，如果返回的是String字符串，并且有具体页面可以跳转，那么就会被视图解析器解析

测试：

1. @Controller注解类型用于声明Spring类的实例是一个控制器（在讲IOC时还提到了另外3个注解@Component-组件、@Service-service、@Repository-dao ）；

2. Spring可以使用扫描机制来找到应用程序中所有基于注解的控制器类，为了保证Spring能找到你的控制器，需要在配置文件中声明组件扫描。

   ```xml
   <!--自动扫描指定的包，下面所有注解类交给IOC容器管理-->
   <context:component-scan base-package="com.sun.controller"/>
   ```

3. 增加一个ControllerTest2类，使用注解实现；

   ```java
   //@Controller：代表这个类会被Spring接管，里面所有的方法，
   // 如果返回的是String字符串，并且有具体页面可以跳转，那么就会被视图解析器解析
   @Controller
   public class ControllerTest2 {
       @RequestMapping("/t2")
       public String test1(Model model) {
           model.addAttribute("msg", "ControllerTest2");
   
           return "test";  //去/WEB-INF/jsp/test.jsp
       }
   }
   ```

4. 运行tomcat测试

   - 可以发现，我们的两个请求都可以指向一个视图，但是页面结果的结果是不一样的
   - 从这里可以看出视图是被复用的，而控制器与视图之间是弱偶合关系。

注解方式是平时使用的最多的方式！除了这两种之外还有其他的方式

## @RequestMapping

### 注解的作用

1. 用于映射url到控制器类或一个特定的处理程序方法。
2. 可用于类或方法上。

### 注解的使用范围

1. 用于类上

   - 表示类中的所有响应请求的方法都是以该地址作为父路径。
   - 为了测试结论更加准确，我们可以加上一个项目名测试 myweb

   ```java
   @Controller
   @RequestMapping("/c3")
   public class ControllerTest3 {
       @RequestMapping("/t1")
       public String test1() {
           return "test";
       }
   }
   ```

2. 只注解在方法上面

   访问路径：http://localhost:8080 / 项目名 / 方法路径

3. 同时注解类与方法

   访问路径：http://localhost:8080 / 项目名/ 类路径/方法路径 , 需要先指定类的路径再指定方法的路径；

## RestFul风格

### 概念

Restful就是一个资源定位url及资源操作的风格。不是标准也不是协议，只是一种风格。

- 资源：互联网所有的事物都可以被抽象为资源
- 资源操作：使用POST、DELETE、PUT、GET，使用不同方法对资源进行操作。分别对应 添加、 删除、修改、查询。

基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制，也更安全。

思考：使用路径变量的好处？

- 使路径变得更加简洁；
- 获得参数更加方便，框架会自动进行类型转换。
- 通过路径变量的类型可以约束访问参数，如果类型不一样，则访问不到对应的请求方法，如这里访问是的路径是/commit/1/a，则路径与方法不匹配，而不会是参数转换失败。

### 操作资源的方式

1. 传统方式：

   通过不同的`参数/链接`来实现不同的效果！方法单一，post 和 get

   - http://127.0.0.1/item/queryItem.action?id=1 查询,GET
   - http://127.0.0.1/item/saveItem.action 新增,POST
   - http://127.0.0.1/item/updateItem.action 更新,POST
   - http://127.0.0.1/item/deleteItem.action?id=1 删除,GET或POST

2. 使用Restful操作资源 ：可以通过不同的`请求方式`来实现不同的效果！如下：请求地址一样，但是功能可以不同！

   - http://127.0.0.1/item/1 查询,GET 
   - http://127.0.0.1/item 新增,POST
   - http://127.0.0.1/item 更新,PUT
   - http://127.0.0.1/item/1 删除,DELETE

### 学习测试

1. 在新建一个类 RestFulController

   ```java
   @Controller
   public class RestFullController {
       //传统方式：http://localhost:8080/add?a=1&b=2
       @RequestMapping("/add")
       public String test1(int a, int b, Model model) {
           int res = a + b;
           model.addAttribute("msg", "结果为：" + res);
           return "admin/test";
       }
   }
   ```

2. 在Spring MVC中可以使用 @PathVariable 路径变量注解，让方法参数的值对应绑定到一个URL模板变量上。

   ```java
   @Controller
   public class RestFullController {
       //RestFul：http://localhost:8080/add/1/2
       @RequestMapping("/add/{a}/{b}")
       public String test1(@PathVariable int a, @PathVariable int b, Model model) {
           int res = a + b;
           model.addAttribute("msg", "结果为：" + res);
           return "admin/test";
       }
   }
   ```

3. 我们来测试请求查看下

   我们来修改下对应的参数类型int可以变为String，再次测试


### 使用method属性指定请求类型

用于约束请求的类型，可以收窄请求范围。

指定请求方法的类型如GET, POST, HEAD, OPTIONS, PUT, PATCH, DELETE, TRACE等

我们来测试一下：

1. 增加一个方法

   ```java
   @Controller
   public class RestFullController {
       //传统方式：http://localhost:8080/add?a=1&b=2
       //RestFul：http://localhost:8080/add/1/2
       @RequestMapping(value = "/add/{a}/{b}", method = RequestMethod.DELETE)
       public String test1(@PathVariable int a, @PathVariable int b, Model model) {
           int res = a + b;
           model.addAttribute("msg", "结果为：" + res);
           return "admin/test";
       }
   }
   ```

2. 我们使用浏览器地址栏进行访问默认是Get请求，会报错405：

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240505182734337-2064314026.png" alt="image-20240505182735293" width="300" />

3. 如果将POST修改为GET，reload重新部署，则正常了；

根据method的不同会返回不同的结果，哪怕方法内容一样

- 如果两个方法的内容和method都一样，会报500错误，模棱两可，无法映射
- http浏览器默认get方法，可以写个form表单的post方法验证

```jsp
<body>
    <form action="/add/1/2" method="post">
        <input type="text" name="a">
        <input type="text" name="b">
        <input type="submit">
    </form>
</body>
```

```java
@Controller
public class RestFullController {
    //传统方式：http://localhost:8080/add?a=1&b=2
    //RestFul：http://localhost:8080/add/1/2
    @RequestMapping(value = "/add/{a}/{b}", method = RequestMethod.GET)
    public String test1(@PathVariable int a, @PathVariable int b, Model model) {
        int res = a + b;
        model.addAttribute("msg", "结果1为：" + res);
        return "admin/test";
    }

    @RequestMapping(value = "/add/{a}/{b}", method = RequestMethod.POST)
    public String test2(@PathVariable int a, @PathVariable int b, Model model) {
        int res = a + b;
        model.addAttribute("msg", "结果2为：" + res);
        return "admin/test";
    }
}
```

### 小结

1. Spring MVC 的 @RequestMapping 注解能够处理 HTTP 请求的方法, 比如 GET, PUT, POST, DELETE 以及 PATCH。
2. 所有的地址栏请求默认都会是 HTTP GET 类型的。
3. 方法级别的注解变体有如下几个： 组合注解
   - @PostMapping 
   - @PutMapping 
   - @DeleteMapping 
   - @PatchMapping  
   - @GetMapping 是一个组合注解，它所扮演的是 @RequestMapping(method =RequestMethod.GET) 的一个快捷方式。平时使用的会比较多！

# 结果跳转方式

## ModelAndView

设置ModelAndView对象 , 根据`view的名称和视图解析器`，跳到指定的页面

页面 : {视图解析器前缀} + viewName +{视图解析器后缀}

```xml
<!--3.视图解析器 ViewResolver 模版引擎：Thymeleaf、Freemarker-->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
    <!--前缀 prefix-->
    <property name="prefix" value="/WEB-INF/jsp/"/>
    <!--前缀 suffix-->
    <property name="suffix" value=".jsp"/>
</bean>
```

对应的controller类

```java
public class ControllerTest1 implements Controller {
    //返回一个ModelAndView对象
    @Override
    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
        ModelAndView modelAndView = new ModelAndView();

        modelAndView.addObject("msg", "ControllerTest1");
        modelAndView.setViewName("admin/test");

        return modelAndView;
    }
}
```

## ServletAPI

通过设置ServletAPI , 不需要视图解析器

1. 通过HttpServletResponse进行输出
2. 通过HttpServletResponse实现重定向
3. 通过HttpServletRequest实现转发

```java
@Controller
public class ResultGo {
    @RequestMapping("/result/t1")
    public void test1(HttpServletRequest req, HttpServletResponse rsp)
        throws IOException {
        //使用rsp输出
        rsp.getWriter().println("Hello,Spring BY servlet API");
    }

    @RequestMapping("/result/t2")
    public void test2(HttpServletRequest req, HttpServletResponse rsp)
        throws IOException {
        //使用rsp重定向
        rsp.sendRedirect("/index.jsp");
    }

    @RequestMapping("/result/t3")
    public void test3(HttpServletRequest req, HttpServletResponse rsp)
        throws Exception {
        //使用req转发
        req.setAttribute("msg", "/result/t3");
        req.getRequestDispatcher("/WEB-INF/jsp/test.jsp").forward(req, rsp);
    }
}
```

## SpringMVC

通过SpringMVC来实现转发和重定向 - 无需视图解析器；

- 测试前，需要将视图解析器注释掉

```java
@Controller
public class ResultSpringMVC {
    @RequestMapping("/rsm/t1")
    public String test1(){
        //转发
        return "/WEB-INF/jsp/admin/test.jsp";
    } 
    @RequestMapping("/rsm/t2")
    public String test2(){
        //转发二
        return "forward:/index.jsp";
    } 
    @RequestMapping("/rsm/t3")
    public String test3(){
        //重定向
        return "redirect:/index.jsp";
    }
}
```

通过SpringMVC来实现转发和重定向 - 有视图解析器；

- 重定向：不需要视图解析器。本质就是重新请求一个新地方嘛 , 所以注意路径问题
- 可以重定向到另外一个请求实现

```java
@Controller
public class ResultSpringMVC2 {
    @RequestMapping("/rsm2/t1")
    public String test1(){
        //转发[默认就是转发]
        return "test";
    } 
    @RequestMapping("/rsm2/t2")
    public String test2(){
        //重定向
        return "redirect:/index.jsp";
        //return "redirect:hello.do"; //hello.do为另一个请求/
    }
}
```

# 数据处理

以前的Servlet使用session.getParameters()获取前端表单参数

## 处理提交数据

1. 提交的域名称name和处理方法的参数名name一致

   提交数据 :http://localhost:8080/user/t1?name=qwq

   处理方法 :

   ```java
   @Controller
   @RequestMapping("/user")
   public class UserController {
       @GetMapping("/t1")
       public String test1(String name, Model model) {
           //1.接收前端参数
           System.out.println("接受到的参数为：" + name);
           //2.将返回的结果传递给前端
           model.addAttribute("msg", name);
           //3.视图跳转
           return "admin/test";
       }
   }
   ```

   后台输出 : qwq

2. 提交的域名称username和处理方法的参数名name不一致

   提交数据 : http://localhost:8080/user/t1?username=qwq

   处理方法：@RequestParam("username")

   ```java
   @Controller
   @RequestMapping("/user")
   public class UserController {
       @GetMapping("/t1")
       public String test1(@RequestParam("username") String name, Model model) {
           //1.接收前端参数
           System.out.println("接受到的参数为：" + name);
           //2.将返回的结果传递给前端
           model.addAttribute("msg", name);
           //3.视图跳转
           return "admin/test";
       }
   }
   ```

   后台输出 : qwq

3. 提交的是一个对象

   - 要求提交的表单域和对象的属性名一致 , 参数使用对象即可
   - 说明：如果使用对象的话，前端传递的参数名和对象名必须一致，否则就是null。
   
   实体类
   
   ```java
   public class User {
       private int id;
       private String name;
       private int age;
       //构造-get/set-tostring()
   }
   ```
   
   提交数据 : http://localhost:8080/user/t2?id=1&name=qwq&age=18
   
   处理方法 :
   
   ```java
   /*
       1.接收前端用户传递的参数，判断参数的名字，假设名字直接在方法上，可以直接使用
       2.假设传递的是一个对象User,匹配User对象中的字段名；如果名字一致则OK，否则匹配不到
   */
   @GetMapping("/t2")
   public String test2(User user, Model model) {
       //1.接收前端参数
       System.out.println("接受到的参数为：" + user);
       //2.将返回的结果传递给前端
       model.addAttribute("msg", user);
       //3.视图跳转
       return "admin/test";
   }
   ```
   
   后台输出 : User(id=1, name=qwq, age=18)

## 数据显示到前端

### 三种方式

1. 第一种 : 通过ModelAndView我们前面一直都是如此 . 就不过多解释

   ```java
   public class ControllerTest1 implements Controller {
       //返回一个ModelAndView对象
       @Override
       public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
           ModelAndView modelAndView = new ModelAndView();
   
           modelAndView.addObject("msg", "ControllerTest1");
           modelAndView.setViewName("admin/test");
   
           return modelAndView;
       }
   }
   ```

2. 第二种 : 通过Model

   ```java
   @GetMapping("/t2")
   public String test2(User user, Model model) {
       //1.接收前端参数
       System.out.println("接受到的参数为：" + user);
       //2.将返回的结果传递给前端
       model.addAttribute("msg", user);
       //3.视图跳转
       return "admin/test";
   }
   ```

3. 第三种 : 通过ModelMap

   ```java
   @GetMapping("/t3")
   public String test3(User user, ModelMap modelMap) {
       //1.接收前端参数
       System.out.println("接受到的参数为：" + user);
       //2.将返回的结果传递给前端
       modelMap.addAttribute("msg", user);
       //3.视图跳转
       return "admin/test";
   }
   //User(id=1, name=qwq, age=10) 
   ```

### 对比

就对于新手而言简单来说使用区别就是：

1. Model ：精简版，只有寥寥几个方法只适合用于储存数据，简化了新手对于Model对象的操作和理解；
2. ModelMap ：继承了 LinkedMap ，除了实现了自身的一些方法，同样的继承了 LinkedMap 的全部方法和特性；
3. ModelAndView ：可以在储存数据的同时，可以进行设置返回的逻辑视图，进行控制展示层的跳转。

请使用80%的时间打好扎实的基础，剩下18%的时间研究框架，2%的时间去学点英文，框架的官方文档永远是最好的教程。

## 乱码问题

### 环境搭建

1. 我们可以在首页编写一个提交的表单

   ```jsp
   <body>
       <form action="/e/t1" method="post">
           <input type="text" name="name">
           <input type="submit">
       </form>
   </body>
   ```

2. 后台编写对应的处理类

   ```java
   @Controller
   public class EncodingController {
       @PostMapping("e/t1")
       public String test1(String name, Model model) {
           System.out.println(name);
           model.addAttribute("msg", "name : " + name);
           return "admin/test";
       }
   }
   ```

3. 输入中文测试，发现乱码

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240505195316793-241059042.png" alt="image-20240505195317820" width="150" /><img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240505195328087-657520599.png" alt="image-20240505195329337" width="150" />

不得不说，乱码问题是在我们开发中十分常见的问题，也是让我们程序猿比较头大的问题！

### 通过HttpServletRequest

```java
@Controller
public class EncodingController {
    @PostMapping("e/t1")
    public String test1(String name, Model model, HttpServletRequest request) throws UnsupportedEncodingException {
        request.setCharacterEncoding("UTF-8");
        System.out.println(name);
        model.addAttribute("msg", "name : " + name);
        return "admin/test";
    }
}
```

### 通过过滤器

- 有时Post方式无效，Get方式可以
- 【注意】记得过滤的是/*

```java
public class EncodingFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        servletRequest.setCharacterEncoding("UTF-8");
        servletResponse.setCharacterEncoding("UTF-8");

        filterChain.doFilter(servletRequest, servletResponse);
    }

    @Override
    public void destroy() {

    }
}
```

```xml
<filter>
    <filter-name>encoding</filter-name>
    <filter-class>com.sun.filter.EncodingFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>encoding</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

### 通过SpringMVC

SpringMVC给我们提供了一个过滤器 , 可以在web.xml中配置 .

- 修改了xml文件需要重启服务器！但是我们发现 , 有些极端情况下，这个过滤器对post的支持不好 .

```xml
<filter>
    <filter-name>encoding</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>utf-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>encoding</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

### 更多的处理方法

1. 修改tomcat配置文件 ： 设置编码！

   ```xml
   <Connector URIEncoding="utf-8" port="8080" protocol="HTTP/1.1"
       connectionTimeout="20000"
       redirectPort="8443" />
   ```

2. 自定义过滤器  

   ```java
   package com.sun.filter;
   
   import javax.servlet.*;
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletRequestWrapper;
   import javax.servlet.http.HttpServletResponse;
   import java.io.IOException;
   import java.io.UnsupportedEncodingException;
   import java.util.Map;
   
   /**
    * 解决get和post请求 全部乱码的过滤器
    */
   public class GenericEncodingFilter implements Filter {
       @Override
       public void destroy() {
       }
   
       @Override
       public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
           //处理response的字符编码
           HttpServletResponse myResponse = (HttpServletResponse) response;
           myResponse.setContentType("text/html;charset=UTF-8");
           // 转型为与协议相关对象
           HttpServletRequest httpServletRequest = (HttpServletRequest) request;
           // 对request包装增强
           HttpServletRequest myrequest = new MyRequest(httpServletRequest);
           chain.doFilter(myrequest, response);
       }
   
       @Override
       public void init(FilterConfig filterConfig) throws ServletException {
           
       }
   } 
   
   //自定义request对象，HttpServletRequest的包装类
   class MyRequest extends HttpServletRequestWrapper {
       private HttpServletRequest request;
       //是否编码的标记
       private boolean hasEncode;
   
       //定义一个可以传入HttpServletRequest对象的构造函数，以便对其进行装饰
       public MyRequest(HttpServletRequest request) {
           super(request);// super必须写
           this.request = request;
       } // 对需要增强方法 进行覆盖
   
       @Override
       public Map getParameterMap() {
           // 先获得请求方式
           String method = request.getMethod();
           if (method.equalsIgnoreCase("post")) {
               // post请求
               try {
                   // 处理post乱码
                   request.setCharacterEncoding("utf-8");
                   return request.getParameterMap();
               } catch (UnsupportedEncodingException e) {
                   e.printStackTrace();
               }
           } else if (method.equalsIgnoreCase("get")) {
               // get请求
               Map<String, String[]> parameterMap =
                       request.getParameterMap();
               if (!hasEncode) { // 确保get手动编码逻辑只运行一次
                   for (String parameterName : parameterMap.keySet()) {
                       String[] values = parameterMap.get(parameterName);
                       if (values != null) {
                           for (int i = 0; i < values.length; i++) {
                               try {
                                   // 处理get乱码
                                   values[i] = new String(values[i].getBytes("ISO-8859-1"), "utf-8");
                               } catch (UnsupportedEncodingException e) {
                                   e.printStackTrace();
                               }
                           }
                       }
                   }
                   hasEncode = true;
               }
               return parameterMap;
           }
           return super.getParameterMap();
       }
   
       //取一个值
       @Override
       public String getParameter(String name) {
           Map<String, String[]> parameterMap = getParameterMap();
           String[] values = parameterMap.get(name);
           if (values == null) {
               return null;
           }
           return values[0]; // 取回参数的第一个值
       }
   
       //取所有值
       @Override
       public String[] getParameterValues(String name) {
           Map<String, String[]> parameterMap = getParameterMap();
           String[] values = parameterMap.get(name);
           return values;
       }
   }
   ```

大神写的，一般情况下，SpringMVC默认的乱码处理就已经能够很好的解决了！然后在web.xml中配置这个过滤器即可！

乱码问题，需要平时多注意，在尽可能能设置编码的地方，都设置为统一编码 UTF-8！

# Json

前后端分离时代

- 后端部署后端，提供接口，提供数据
- JSON
- 前端独立部署，负责渲染后端数据

## 概述

- JSON(JavaScript Object Notation, JS 对象标记) 是一种轻量级的`数据交换格式`，目前使用特别广泛。
- 采用完全独立于编程语言的`文本格式`来存储和表示数据。
- 简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。
- 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。

在 JavaScript 语言中，一切都是对象。因此，任何JavaScript 支持的类型都可以通过 JSON 来表示，例如字符串、数字、对象、数组等。

1. JSON的要求和语法格式：

   对象：表示为键值对，数据由逗号分隔

   - 键/值对组合中的键名写在前面并用双引号 "" 包裹，使用冒号 : 分隔，然后紧接着值：

   ```java
   {"name": "QinJiang"}
   {"age": "3"}
   {"sex": "男"}
   ```

   - 花括号：保存对象
   - 方括号：保存数组

2. JSON 和 JavaScript 对象的关系

   - JSON 是 JavaScript 对象的字符串表示法，它使用文本表示一个 JS 对象的信息，本质是一个字符串。

   ```java
   var obj = {a: 'Hello', b: 'World'}; //这是一个对象，注意键名也是可以使用引号包裹的 
   
   var json = '{"a": "Hello", "b": "World"}'; //这是一个 JSON 字符串，本质是一个字符串
   ```

3. JSON 和 JavaScript 对象互转

   - 要实现从JSON字符串转换为JavaScript 对象，使用 JSON.parse() 方法：

     ```java
     var obj = JSON.parse('{"a": "Hello", "b": "World"}');
     //结果是 {a: 'Hello', b: 'World'}
     ```

   - 要实现从JavaScript 对象转换为JSON字符串，使用 JSON.stringify() 方法：

     ```java
     var json = JSON.stringify({a: 'Hello', b: 'World'});
     //结果是 '{"a": "Hello", "b": "World"}'
     ```


## 前端JSON测试

1. 新建一个module ，springmvc-05-json ， 添加web的支持

2. 在web目录下新建一个 json-1.html ， 编写测试内容

   ```html
   <!--script不能使用自闭合标签！！！-->
   <script type="text/javascript">
       //1.编写一个JavaScript对象
       var user = {
           name: "张三",
           age: 3,
           sex: "男"
       };
   
       //2.将JS对象转换为JSON对象
       var json = JSON.stringify(user);
       
       //3.将JSON对象转换为JS对象
       var js = JSON.parse(json);
       
       console.log(user);
       console.log(json);
       console.log(js);
   </script>
   ```

3. 在IDEA中使用浏览器打开，查看控制台输出！  

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240506092145110-1503378291.png" alt="image-20240506092146057" width="400" />

## 后端Controller返回JSON数据

Jackson应该是目前比较好的json解析工具了，当然工具不止这一个，比如还有阿里巴巴的 fastjson 等等。

我们这里使用Jackson

1. 使用它需要导入Jackson的jar包；

   ```xml
   <dependencies>
       <dependency>
           <groupId>com.fasterxml.jackson.core</groupId>
           <artifactId>jackson-databind</artifactId>
           <version>2.17.0</version>
       </dependency>
   </dependencies>
   ```

2. 配置SpringMVC需要的配置

   web.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
   
       <!--1.注册servlet-->
       <servlet>
           <servlet-name>SpringMVC</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <!--通过初始化参数指定SpringMVC配置文件的位置，进行关联-->
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:springmvc-servlet.xml</param-value>
           </init-param>
           <!-- 启动顺序，数字越小，启动越早 -->
           <load-on-startup>1</load-on-startup>
       </servlet>
       <!--2.所有请求都会被springmvc拦截 -->
       <servlet-mapping>
           <servlet-name>SpringMVC</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
       <filter>
           <filter-name>encoding</filter-name>
           <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
           <init-param>
               <param-name>encoding</param-name>
               <param-value>utf-8</param-value>
           </init-param>
       </filter>
       <filter-mapping>
           <filter-name>encoding</filter-name>
           <url-pattern>/</url-pattern>
       </filter-mapping>
   </web-app>
   ```

   springmvc-servlet.xml  

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
   
       <!-- 1.自动扫描指定的包，下面所有注解类交给IOC容器管理 -->
       <context:component-scan base-package="com.sun.controller"/>
       <!-- 2.视图解析器 -->
       <bean  class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
           <!-- 前缀 -->
           <property name="prefix" value="/WEB-INF/jsp/"/>
           <!-- 后缀 -->
           <property name="suffix" value=".jsp"/>
       </bean>
   </beans>
   ```

3. 我们随便编写一个User的实体类

   ```java
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class User {
       private String name;
       private int age;
       private String sex;
   }
   ```

4. 然后我们去编写我们的测试Controller；

   - @ResponseBody：加上该注解，他就不会走视图解析器；会直接返回真实返回的东西，一般返回字符串  
   - ObjectMapper对象：利用ObjectMapper对象的writeValueAsString()方法将user对象转换为JSON字符串

   ```java
   @Controller
   public class UserController {
       @RequestMapping("/j1")
       @ResponseBody
       public String json1() throws JsonProcessingException {
           //1.jackson 创建ObjectMapper对象
           ObjectMapper mapper = new ObjectMapper();
   
           //2.创建一个对象
           User user = new User("李四", 45, "男");
           
           //3.将user对象转换为JSON字符串
           String string = mapper.writeValueAsString(user);
   
           //return user.toString();   //User(name:??,age:45,sex:?)
           return string;  //{"name":"??","age":45,"sex":"?"}
       }
   }
   ```

5. 配置Tomcat ， 启动测试一下！

6. 发现出现了乱码问题，我们需要设置一下他的编码格式为utf-8，以及它返回的类型；

   通过@RequestMaping的produces属性来实现，修改下代码

   ```java
   //produces:指定响应体返回类型和编码
   @RequestMapping(value = "/j1", produces = "application/json;charset=utf-8")
   ```

7. 再次测试http://localhost:8080/j1 ， 乱码问题OK！

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240506094929251-1008653297.png" alt="image-20240506094930359" width="200" />

## 代码优化

### 乱码统一解决

上一种方法比较麻烦，如果项目中有许多请求则每一个都要添加，可以通过`Spring配置统一指定`，这样就不用每次都去处理了！我们可以在springmvc的配置文件上添加一段消息StringHttpMessageConverter转换配置！

```xml
<!--乱码问题配置-->
<mvc:annotation-driven>
    <mvc:message-converters register-defaults="true">
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <constructor-arg value="UTF-8"/>
        </bean>
        <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
            <property name="objectMapper">
                <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                    <property name="failOnEmptyBeans" value="false"/>
                </bean>
            </property>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```

### 返回json字符串统一解决

在类上直接使用 `@RestController `，这样子，里面所有的方法都只会返回 json 字符串了，不用再每一个都添加@ResponseBody ！

@ResponseBody配合@Controller使用

```java
@RestController
public class UserController {
    @RequestMapping(value = "/j1")
    public String json1() throws JsonProcessingException {
        //1.jackson 创建ObjectMapper对象
        ObjectMapper mapper = new ObjectMapper();

        //2.创建一个对象
        User user = new User("李四", 45, "男");

        //3.将user对象转换为JSON字符串
        String string = mapper.writeValueAsString(user);

        //return user.toString();   //User(name:??,age:45,sex:?)
        return string;  //{"name":"??","age":45,"sex":"?"}
    }
}
```

我们在前后端分离开发中，一般都使用 @RestController ，十分便捷！

## 输出集合List对象

增加一个新的方法

```java
@RequestMapping(value = "/j2")
public String json2() throws JsonProcessingException {
    List<User> users = new ArrayList<>();
    
    User user = new User("李1", 45, "男");
    User user2 = new User("李2", 45, "男");
    User user3 = new User("李3", 45, "男");
    User user4 = new User("李四", 45, "男");
    
    users.add(user);
    users.add(user2);
    users.add(user3);
    users.add(user4);

    return new ObjectMapper().writeValueAsString(users);
    /*
    [
         {"name":"李1","age":45,"sex":"男"},
         {"name":"李2","age":45,"sex":"男"},
         {"name":"李3","age":45,"sex":"男"},
         {"name":"李四","age":45,"sex":"男"}
     ]
     */
}
```

运行结果 : 十分完美，没有任何问题！

## 输出时间Date对象

增加一个新的方法，运行结果 :

- 默认日期格式会变成一个数字，是1970年1月1日到当前日期的毫秒数！
- Jackson 默认是会把时间转成timestamps形式

```java
@RequestMapping(value = "/j3")
public String json3() throws JsonProcessingException {
    Date date = new Date();

    //ObjectMapper，时间解析后的默认格式为：Timestamp时间戳
    return new ObjectMapper().writeValueAsString(date);
    //1714961134614 
}
```

### 解决方案

取消timestamps形式 ， 自定义时间格式  

1. 纯java方式

   - SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(date)

   ```java
   @RequestMapping(value = "/j3")
       public String json3() throws JsonProcessingException {
           Date date = new Date();
           //自定义日期的格式
           String format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(date);
           
           return new ObjectMapper().writeValueAsString(format);
           //"2024-05-06 10:08:18"
       }
   }
   ```

2. 使用ObjectMapper来格式化输出

   - mapper.setDateFormat(new SimpleDateFormat("yyyy-MM-dd HH:mm:ss"))

   ```java
   @RequestMapping(value = "/j3")
   public String json3() throws JsonProcessingException {
   
       ObjectMapper mapper = new ObjectMapper();
       //不使用时间戳的方式
       mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
       //自定义日期的格式
       mapper.setDateFormat(new SimpleDateFormat("yyyy-MM-dd HH:mm:ss"));
   
       Date date = new Date();
   
       return mapper.writeValueAsString(date);
       //"2024-05-06 10:08:18"
   }
   ```

3. 抽取为工具类

   如果要经常使用的话，这样是比较麻烦的，我们可以将这些代码封装到一个工具类JsonUtils中；

   ```java
   public class JsonUtils {
       //实现代码复用
       public static String getJson(Object object){
           //直接返回方法，dateFormat设为默认值
           return getJson(object,"yyyy-MM-dd HH:mm:ss");
       }
       
       
       public static String getJson(Object object, String dateFormat) {
           ObjectMapper mapper = new ObjectMapper();
   
           //不使用时间戳的方式
           mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
           //自定义日期的格式
           mapper.setDateFormat(new SimpleDateFormat(dateFormat));
   
           try {
               return mapper.writeValueAsString(object);
           } catch (JsonProcessingException e) {
               e.printStackTrace();
           }
           return null;
       }
   }
   ```

   使用

   ```java
   @RequestMapping(value = "/j2")
   public String json2() throws JsonProcessingException {
       List<User> users = new ArrayList<>();
   
       User user = new User("李1", 45, "男");
       User user2 = new User("李2", 45, "男");
       User user3 = new User("李3", 45, "男");
       User user4 = new User("李四", 45, "男");
   
       users.add(user);
       users.add(user2);
       users.add(user3);
       users.add(user4);
   
       return JsonUtils.getJson(users);
   
   }
   
   @RequestMapping(value = "/j3")
   public String json3() throws JsonProcessingException {
       Date date = new Date();
   
       return JsonUtils.getJson(date, "yyyy-MM-dd HH:mm:ss");
       //"2024-05-06 10:08:18"
   }
   ```

## FastJson

fastjson.jar是阿里开发的一款专门用于Java开发的包，可以方便的实现json对象与JavaBean对象的转换，实现JavaBean对象与json字符串的转换，实现json对象与json字符串的转换。实现json的转换方法很多，最后的实现结果都是一样的。

添加fastjson 的 pom依赖！

```xml
<!-- https://mvnrepository.com/artifact/com.alibaba.fastjson2/fastjson2 -->
<dependency>
    <groupId>com.alibaba.fastjson2</groupId>
    <artifactId>fastjson2</artifactId>
    <version>2.0.49</version>
</dependency>
```

fastjson 三个主要的类：

1. JSONObject 代表 json 对象

   - JSONObject实现了Map接口, 猜想 JSONObject底层操作是由Map实现的。
   - JSONObject对应json对象，通过各种形式的get()方法可以获取json对象中的数据，也可利用诸如size()，isEmpty()等方法获取"键：值"对的个数和判断是否为空。其本质是通过实现Map接口并调用接口中的方法完成的。

2. JSONArray 代表 json 对象数组

   内部是有List接口中的方法来完成操作的。

3. JSON 代表 JSONObject和JSONArray的转化

   - JSON类源码分析与使用
   - 仔细观察这些方法，主要是实现json对象，json对象数组，javabean对象，json字符串之间的相互转化。

代码测试，我们新建一个FastJsonDemo 类  

```java
@RequestMapping(value = "/j4")
    public String json4() throws JsonProcessingException {
        List<User> users = new ArrayList<>();

        User user = new User("李1", 45, "男");
        User user2 = new User("李2", 45, "男");
        User user3 = new User("李3", 45, "男");
        User user4 = new User("李四", 45, "男");

        users.add(user);
        users.add(user2);
        users.add(user3);
        users.add(user4);
        System.out.println("*******Java对象 转 JSON字符串*******");
        String str1 = JSON.toJSONString(users);
        System.out.println("JSON.toJSONString(list)==>" + str1);
        String str2 = JSON.toJSONString(user);
        System.out.println("JSON.toJSONString(user1)==>" + str2);

        System.out.println("\n****** JSON字符串 转 Java对象*******");
        User jp_user1 = JSON.parseObject(str2, User.class);
        System.out.println("JSON.parseObject(str2,User.class)==>" + jp_user1);

        System.out.println("\n****** Java对象 转 JSON对象 ******");
        JSONObject jsonObject1 = (JSONObject) JSON.toJSON(user2);
        System.out.println("(JSONObject) JSON.toJSON(user2)==>" + jsonObject1.getString("name"));

        System.out.println("\n****** JSON对象 转 Java对象 ******");
        User to_java_user = JSON.toJavaObject(jsonObject1, User.class);
        System.out.println("JSON.toJavaObject(jsonObject1, User.class)==>" + to_java_user);
        
        /*
        *******Java对象 转 JSON字符串*******
        JSON.toJSONString(list)==>[{"age":45,"name":"李1","sex":"男"},{"age":45,"name":"李2","sex":"男"},{"age":45,"name":"李3","sex":"男"},{"age":45,"name":"李四","sex":"男"}]
        JSON.toJSONString(user1)==>{"age":45,"name":"李1","sex":"男"}
        
        ****** JSON字符串 转 Java对象*******
        JSON.parseObject(str2,User.class)==>User(name=李1, age=45, sex=男)
        
        ****** Java对象 转 JSON对象 ******
        (JSONObject) JSON.toJSON(user2)==>李2
        
        ****** JSON对象 转 Java对象 ******
        JSON.toJavaObject(jsonObject1, User.class)==>User(name=李2, age=45, sex=男)

         */

        return JSON.toJSONString(users);
    }
```

这种工具类，我们只需要掌握使用就好了，在使用的时候在根据具体的业务去找对应的实现。和以前的commons-io那种工具包一样，拿来用就好了！  

# 整合SSM

## 环境要求

环境：

- IDEA
- MySQL 5.7.19
- Tomcat 9
- Maven 3.6

要求：

- 需要熟练掌握MySQL数据库(建表、增删改查)，Spring，JavaWeb及MyBatis知识，
- 简单的前端知识；

## 数据库环境

创建一个存放书籍数据的数据库表  

```mysql
CREATE DATABASE `ssmbuild`;
USE `ssmbuild`;
DROP TABLE IF EXISTS `books`;
CREATE TABLE `books`
(
    `bookID`     INT(10)      NOT NULL AUTO_INCREMENT COMMENT '书id',
    `bookName`   VARCHAR(100) NOT NULL COMMENT '书名',
    `bookCounts` INT(11)      NOT NULL COMMENT '数量',
    `detail`     VARCHAR(200) NOT NULL COMMENT '描述',
    KEY `bookID` (`bookID`)
) ENGINE = INNODB DEFAULT CHARSET = utf8;
INSERT INTO `books`(`bookID`, `bookName`, `bookCounts`, `detail`)
VALUES (1, 'Java', 1, '从入门到放弃'),
       (2, 'MySQL', 10, '从删库到跑路'),
       (3, 'Linux', 5, '从进门到进牢');
```

## 基本环境搭建

1. 新建一Maven项目！ ssmbuild ， 添加web的支持

2. 导入相关的pom依赖！和Maven资源过滤设置

   ```xml
   <!--依赖-->
   <dependencies>
       <!-- junit -->
       <dependency>
           <groupId>junit</groupId>
           <artifactId>junit</artifactId>
           <version>4.12</version>
           <scope>test</scope>
       </dependency>
       <!-- 数据库驱动 -->
       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
           <version>5.1.47</version>
       </dependency>
       <!-- 数据库连接池:c3p0 -->
       <dependency>
           <groupId>com.mchange</groupId>
           <artifactId>c3p0</artifactId>
           <version>0.10.0</version>
       </dependency>
       <!-- servlet -->
       <dependency>
           <groupId>javax.servlet</groupId>
           <artifactId>servlet-api</artifactId>
           <version>2.5</version>
       </dependency>
       <!-- jsp -->
       <dependency>
           <groupId>javax.servlet.jsp</groupId>
           <artifactId>jsp-api</artifactId>
           <version>2.1</version>
       </dependency>
       <!-- jstl -->
       <dependency>
           <groupId>javax.servlet</groupId>
           <artifactId>jstl</artifactId>
           <version>1.2</version>
       </dependency>
       <!-- mybatis -->
       <dependency>
           <groupId>org.mybatis</groupId>
           <artifactId>mybatis</artifactId>
           <version>3.5.16</version>
       </dependency>
       <!-- mybatis-spring Spring6以下不支持3.0x版本-->
       <dependency>
           <groupId>org.mybatis</groupId>
           <artifactId>mybatis-spring</artifactId>
           <version>3.0.3</version>
       </dependency>
       <!-- spring -->
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-webmvc</artifactId>
           <version>5.3.18</version>
       </dependency>
       <!-- spring-jdbc -->
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-jdbc</artifactId>
           <version>5.3.18</version>
       </dependency>
       <!-- lombok -->
       <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <version>1.18.32</version>
           </dependency>
   </dependencies>
   
   <!--静态资源导出-->
   <build>
       <resources>
           <resource>
               <directory>src/main/java</directory>
               <includes>
                   <include>**/*.properties</include>
                   <include>**/*.xml</include>
               </includes>
               <filtering>false</filtering>
           </resource>
           <resource>
               <directory>src/main/resources</directory>
               <includes>
                   <include>**/*.properties</include>
                   <include>**/*.xml</include>
               </includes>
               <filtering>false</filtering>
           </resource>
       </resources>
   </build>
   ```

3. 建立基本结构和配置框架！
   - com.sun.pojo

   - com.sun.mapper

   - com.sun.service

   - com.sun.controller

   - mybatis-config.xml

     ```xml
     <?xml version="1.0" encoding="UTF-8" ?>
     <!DOCTYPE configuration
             PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
             "http://mybatis.org/dtd/mybatis-3-config.dtd">
     <configuration>
     
     </configuration>
     ```

   - applicationContext.xml

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
     
     </beans>
     ```

## Mybatis层编写

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240506211530601-1833313043.png" alt="image-20240506211531396" width="250" />

1. 数据库配置文件 database.properties

   ```properties
   jdbc.driver=com.mysql.jdbc.Driver
   #如果使用的是MySQL8.0+  需要增加时区的配置serverTimezone=Asia/Shanghai
   jdbc.url=jdbc:mysql://localhost:3306/ssmbuild?useSSL=true&useUnicode=true&characterEncoding=utf8
   jdbc.username=root
   jdbc.password=root
   ```

2. IDEA关联数据库

3. 编写MyBatis的核心配置文件mybatis-config.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
       <!--配置数据源，交给Spring去做-->
   
       <!--起别名-->
       <typeAliases>
           <package name="com.sun.pojo"/>
       </typeAliases>
   
       <!--注册mapper-->
       <mappers>
           <mapper class="com.sun.mapper.BookMapper"/>
       </mappers>
   </configuration>
   ```

4. 编写数据库对应的实体类 com.sun.pojo.Books使用lombok插件！

   ```java
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class Books {
       private int bookID;
       private String bookName;
       private int bookCounts;
       private String detail;
   }
   ```

5. 编写Mapper层的 Mapper接口！

   ```java
   public interface BookMapper {
       //增加一本书
       int addBook(Books books);
   
       //删除一本书
       int deleteBookById(@Param("bookId") int id);
   
       //更新一本书
       int updateBook(Books books);
   
       //查询一本书
       Books queryBookById(@Param("bookId") int id);
   
       //查询全部的书
       List<Books> queryAllBook();
   }
   ```

6. 编写接口对应的 Mapper.xml 文件。需要导入MyBatis的包；【记得去mybatis-config.xml注册mapper】

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.sun.mapper.BookMapper">
       <insert id="addBook" parameterType="books">
           insert into ssmbuild.books(bookName, bookCounts, detail)
           values (#{bookName}, #{bookCounts}, #{detail});
       </insert>
       <delete id="deleteBookById" parameterType="_int">
           delete
           from ssmbuild.books
           where bookID = #{bookId};
       </delete>
       <update id="updateBook" parameterType="books">
           update ssmbuild.books
           set bookName=#{bookName},
               bookCounts=#{bookCounts},
               detail=#{detail}
           where bookID = #{bookID};
       </update>
       <select id="queryBookBtId" parameterType="_int" resultType="books">
           select *
           from ssmbuild.books
           where bookID = #{bookId};
       </select>
       <select id="queryAllBook" resultType="books">
           select *
           from ssmbuild.books
       </select>
   </mapper>
   ```

7. 编写Service层

   接口：和BookMapper基本类似

   ```java
   public interface BookService {
       //增加一本书
       int addBook(Books books);
   
       //删除一本书
       int deleteBookById(int id);
   
       //更新一本书
       int updateBook(Books books);
   
       //查询一本书
       Books queryBookById(int id);
   
       //查询全部的书
       List<Books> queryAllBook();
   }
   ```

   实现类：

   ```java
   public class BookServiceImpl implements BookService {
       //service层调Dao层
       private BookMapper bookMapper;
   
       public void setBookMapper(BookMapper bookMapper) {
           this.bookMapper = bookMapper;
       }
   
       @Override
       public int addBook(Books books) {
           return bookMapper.addBook(books);
       }
   
       @Override
       public int deleteBookById(int id) {
           return bookMapper.deleteBookById(id);
       }
   
       @Override
       public int updateBook(Books books) {
           return bookMapper.updateBook(books);
       }
   
       @Override
       public Books queryBookById(int id) {
           return bookMapper.queryBookById(id);
       }
   
       @Override
       public List<Books> queryAllBook() {
           return bookMapper.queryAllBook();
       }
   }
   ```

## Spring层

注意应用上下文的依赖，否则spring-service.xml无法识别到spring-dao.xml中的配置，会报错

- 可以直接在ApplicationContext.xml中引入

  ```xml
  <import resource="classpath:spring-dao.xml"/>
  <import resource="classpath:spring-service.xml"/>
  ```

- 也可以手动设置

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240507111856636-1172285571.png" alt="image-20240507111858273" width="500" />

1. 配置Spring整合MyBatis，我们这里数据源使用c3p0连接池；

2. 我们去编写Spring整合Mybatis的相关的配置文件； spring-dao.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
   
       <!--1.关联数据库配置文件  import引入的是Spring文件-->
       <context:property-placeholder location="classpath:database.properties"/>
   
       <!--2.连接池
           org.springframework.jdbc.datasource.DriverManagerDataSource原生
           dbcp：半自动化操作，不能自动连接
           c3p0：自动化操作（自动化的加载配置文件，可以自动设置到对象中）
           druid/hikari
       -->
       <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
           <property name="driverClass" value="${jdbc.driver}"/>
           <property name="jdbcUrl" value="${jdbc.url}"/>
           <property name="user" value="${jdbc.username}"/>
           <property name="password" value="${jdbc.password}"/>
   
           <!-- c3p0连接池的私有属性 -->
           <property name="maxPoolSize" value="30"/>
           <property name="minPoolSize" value="10"/>
           <!-- 关闭连接后不自动commit -->
           <property name="autoCommitOnClose" value="false"/>
           <!-- 获取连接超时时间 -->
           <property name="checkoutTimeout" value="10000"/>
           <!-- 当获取连接失败重试次数 -->
           <property name="acquireRetryAttempts" value="2"/>
       </bean>
   
       <!--3.SqlSessionFactory-->
       <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
           <!--引入数据源-->
           <property name="dataSource" ref="dataSource"/>
           <!--绑定Mybatis的配置-->
           <property name="configLocation" value="classpath:mybatis-config.xml"/>
       </bean>
   
       <!--4.配置Dao接口扫描包，动态的实现Dao接口可以注入到Spring容器中
           通过反射，不用再写实现类Impl了
       -->
       <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
           <!--1.注入SqlSessionFactory
           通过继承的方式：sqlSessionFactoryBeanName  加载的value是步骤3中的SqlSessionFactory
           通过私有化构造：sqlSessionTemplateBeanName
           -->
           <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
           <!--2.要扫描的dao包-->
           <property name="basePackage" value="com.sun.mapper"/>
       </bean>
   
   </beans>
   ```

3. Spring整合service层  spring-service.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
       <!--1.扫描service层的包-->
       <context:component-scan base-package="com.sun.service"/>
   
       <!--2.把所有的业务类注入到Spring，可以通过配置/注解实现@Service  @Autowired-->
       <bean id="BookServiceImpl" class="com.sun.service.BookServiceImpl">
           <property name="bookMapper" ref="bookMapper"/>
       </bean>
   
       <!--3.声明式事务配置-->
       <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
           <!--注入数据源-->
           <property name="dataSource" ref="dataSource"/>
       </bean>
   
       <!--4.AOP事务支持-->
   
   </beans>
   ```

## SpringMVC层

1. web.xml  

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <!--1.DispatcherServlet-->
       <servlet>
           <servlet-name>springmvc</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <!--注意绑定总的配置文件-->
               <param-value>classpath:applicationContext.xml</param-value>
           </init-param>
           <load-on-startup>1</load-on-startup>
       </servlet>
       <servlet-mapping>
           <servlet-name>springmvc</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   
       <!--2.乱码过滤-->
       <filter>
           <filter-name>encodingFilter</filter-name>
           <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
           <init-param>
               <param-name>encoding</param-name>
               <param-value>utf-8</param-value>
           </init-param>
       </filter>
       <filter-mapping>
           <filter-name>encodingFilter</filter-name>
           <url-pattern>/*</url-pattern>
       </filter-mapping>
   
       <!--3.Session过期时间-->
       <session-config>
           <session-timeout>15</session-timeout>
       </session-config>
   
   </web-app>
   ```

2. spring-mvc.xml  

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:mvc="http://www.springframework.org/schema/mvc"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
       
       <!--1.注解驱动-->
       <mvc:annotation-driven/>
   
       <!--2.静态资源过滤-->
       <mvc:default-servlet-handler/>
   
       <!--3.扫描包：controller-->
       <context:component-scan base-package="com.sun.controller"/>
   
       <!--4.视图解析器-->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
           <property name="prefix" value="/WEB-INF/jsp/"/>
           <property name="suffix" value=".jsp"/>
       </bean>
   </beans>
   ```

3. Spring配置整合文件，applicationContext.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <!--关联起来-->
       <import resource="classpath:spring-dao.xml"/>
       <import resource="classpath:spring-service.xml"/>
       <import resource="classpath:spring-mvc.xml"/>
   </beans>
   ```

配置文件，暂时结束！

## Controller和视图层编写

### 方法一：查询全部书籍

1. BookController 类编写 ，

   ```java
   @Controller
   @RequestMapping("/book")
   public class BookController {
       //controller调用service层
       @Autowired
       @Qualifier("BookServiceImpl")
       private BookService bookService;
   
       //查询全部的书籍，并且返回到一个书籍展示页面
       @RequestMapping("/allBook")
       public String list(Model model){
           List<Books> books = bookService.queryAllBook();
           model.addAttribute("books", books);
   
           return "allBook";
       }
   }
   ```

2. 编写首页 index.jsp  

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>首页</title>
       <style>
           a {
               text-decoration: none;
               color: black;
               font-size: 18px;
           }
   
           h3 {
               width: 180px;
               height: 38px;
               margin: 100px auto;
               text-align: center;
               line-height: 38px;
               background: pink;
               border-radius: 5px;
           }
       </style>
   </head>
   <body>
   <h3>
       <a href="${pageContext.request.contextPath}/book/allBook">进入书籍页面</a>
   </h3>
   </body>
   </html>
   
   ```

3. 书籍列表页面 allBook.jsp  

   ```jsp
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>书籍展示</title>
   
       <!--BootStrap美化界面-->
       <link href="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/5.3.3/css/bootstrap.min.css" rel="stylesheet">
   
   </head>
   <body>
   
   <%--使用bootstrap的栅格容器--%>
   <div class="container">
       <%--div是块级元素 清除浮动--%>
       <div class="row clearfix">
           <%--把中等屏幕md 等分为12份，此处写12是把一行都占满了--%>
           <div class="col-md-12 column">
               <div class="page-header">
                   <h1>
                       <small>书籍列表 ———— 显示所有书籍</small>
                   </h1>
               </div>
           </div>
           <!--拥有12列了-->
           <div class="row">
               <div class="col-md-4 column">
                   <!--toAddBook-->
                   <a class="btn btn-primary" href="${pageContext.request.contextPath}/book/toAddBook">新增书籍</a>
               </div>
           </div>
       </div>
       <div class="row clearfix">
           <%--把中等屏幕md 等分为12份，此处写12是把一行都占满了--%>
           <div class="col-md-12 column">
               <!--table样式 hover变色 隔行变色-->
               <table class="table table-hover table-striped">
                   <thead>
                   <tr>
                       <th>书籍编号</th>
                       <th>书籍名称</th>
                       <th>书籍数量</th>
                       <th>书籍详情</th>
                       <th>操作</th>
                   </tr>
                   </thead>
                   <!--书籍从数据库中查询出来，从Controller的books列表中遍历出来：c:foreach 也可以取{requestScope.get(books)}-->
                   <tbody>
                   <c:forEach var="book" items="${books}">
                       <tr>
                           <td>${book.bookID}</td>
                           <td>${book.bookName}</td>
                           <td>${book.bookCounts}</td>
                           <td>${book.detail}</td>
                           <td>
                               <a href="${pageContext.request.contextPath}/book/toUpdate?id=${book.bookID}">修改</a>
                               &nbsp;| &nbsp;
                               <a href="${pageContext.request.contextPath}/book/deleteBook/${book.bookID}">删除</a>
                           </td>
                       </tr>
                   </c:forEach>
                   </tbody>
               </table>
           </div>
       </div>
   </div>
   
   </body>
   </html>
   
   ```

排错思路

问题：bean不存在<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240507135626522-1131912285.png" alt="image-20240507135628109" width="600" />

步骤：

1. 查看bean注入是否成功：此处查看spring-service.xml中的BookService是否注入。成功

2. Junit单元测试，看代码是否能够查询出来结果。成功【此处注意，Mybatis-spring要注意版本问题】

   ```java
   @Test
   public void test() {
       ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
       BookServiceImpl bookServiceImpl = context.getBean("BookServiceImpl", BookServiceImpl.class);
   
       for (Books books : bookServiceImpl.queryAllBook()) {
           System.out.println(books);
       }
   }
   ```

3. 问题不一定在我们底层，可能是spring出了问题，手动注入试试

   ```java
   //原本自动注入
   @Autowired
   @Qualifier("BookServiceImpl")
   private BookService bookService;
   //现在直接new
   private BookService bookService = new BookServiceImpl();
   //还是空指针异常
   ```

4. SpringMVC整合的时候没有调用我们的service层的bean

   1. applicationContext.xml没有注册bean。事实是都已经import引入，没有问题

   2. web.xml中，绑定配置文件错误。应该绑定总的上下文

      ```xml
      <init-param>
          <param-name>contextConfigLocation</param-name>
          <param-value>classpath:applicationContext.xml</param-value>
      </init-param>
      ```

### 方法二：添加书籍  

1. 在前端allBook.jsp增加“添加”链接

2. BookController 类编写 ， 

   ```java
   //跳转到增加书籍页面
   @RequestMapping("/toAddBook")
   public String toAddPaper() {
       return "addBook";
   }
   
   //添加书籍的请求
   @RequestMapping("/addBook")
   public String addBook(Books book) {
       System.out.println("addBook=>" + book);
       bookService.addBook(book);
       return "redirect:/book/allBook";    //重定向到首页
   }
   ```

3. 添加书籍页面：addBook.jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>添加书籍</title>
       <!--BootStrap美化界面-->
       <link href="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/5.3.3/css/bootstrap.min.css" rel="stylesheet">
   </head>
   <body>
   
   <%--使用bootstrap的栅格容器--%>
   <div class="container">
       <%--div是块级元素 清除浮动--%>
       <div class="row clearfix">
           <%--把中等屏幕md 等分为12份，此处写12是把一行都占满了--%>
           <div class="col-md-12 column">
               <div class="page-header">
                   <h1>
                       <small>新增书籍</small>
                   </h1>
               </div>
           </div>
       </div>
       <form action="${pageContext.request.contextPath}/book/addBook" method="post">
           <div class="mb-3">
               <label for="bkname" class="form-label">书籍名称：</label>
               <!--点击label会跳转到id值与for值相同的地方 required要求输入不为空-->
               <input type="text" class="form-control" id="bkname" name="bookName" required>
           </div>
           <div class="mb-3">
               <label for="bkcount" class="form-label">书籍数量：</label>
               <input type="text" class="form-control" id="bkcount" name="bookCounts" required>
           </div>
           <div class="mb-3">
               <label for="bkde" class="form-label">书籍描述：</label>
               <input type="text" class="form-control" id="bkde" name="detail" required>
           </div>
           <div class="text-center">
               <button type="submit" class="btn btn-primary">添加</button>
           </div>
   
       </form>
   
   </div>
   
   </body>
   </html>
   ```

### 方法二：修改书籍  

1. 在前端allBook.jsp增加“修改”链接

2. BookController 类编写 ，

   ```java
   //跳转到修改页面
   @RequestMapping("/toUpdate")
   public String toUpdatePaper(int id, Model model) {
       Books book = bookService.queryBookById(id);
       model.addAttribute("Qbook", book);
       return "updateBook";
   }
   
   //修改书籍
   @RequestMapping("/updateBook")
   public String updateBook(Books book) {
       System.out.println("updateBook=>" + book);
       int i = bookService.updateBook(book);
       if (i > 0) {
           System.out.println("修改成功：" + book);
       }
       return "redirect:/book/allBook";
   }
   ```

3. 修改书籍页面 updateBook.jsp  

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>修改书籍</title>
       <!--BootStrap美化界面-->
       <link href="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/5.3.3/css/bootstrap.min.css" rel="stylesheet">
   </head>
   <body>
   
   <%--使用bootstrap的栅格容器--%>
   <div class="container">
       <%--div是块级元素 清除浮动--%>
       <div class="row clearfix">
           <%--把中等屏幕md 等分为12份，此处写12是把一行都占满了--%>
           <div class="col-md-12 column">
               <div class="page-header">
                   <h1>
                       <small>修改书籍</small>
                   </h1>
               </div>
           </div>
       </div>
       <form action="${pageContext.request.contextPath}/book/updateBook" method="post">
           <%--提交了修改的SQL请求，但是修改失败：
           考虑是事务，配置事务织入依旧失败。
           看SQL语句能否成功，发现SQL执行失败，修改未完成，%>
           <%--前端传递隐藏域--%>
           <input type="hidden" name="bookID" value="${Qbook.bookID}">
           <div class="mb-3">
               <label for="bkname" class="form-label">书籍名称：</label>
               <!--点击label会跳转到id值与for值相同的地方 required要求输入不为空-->
               <input type="text" class="form-control" id="bkname" name="bookName" value="${Qbook.bookName}" required>
           </div>
           <div class="mb-3">
               <label for="bkcount" class="form-label">书籍数量：</label>
               <input type="text" class="form-control" id="bkcount" name="bookCounts" value="${Qbook.bookCounts}" required>
           </div>
           <div class="mb-3">
               <label for="bkde" class="form-label">书籍描述：</label>
               <input type="text" class="form-control" id="bkde" name="detail" value="${Qbook.detail}" required>
           </div>
           <div class="text-center">
               <button type="submit" class="btn btn-primary">修改</button>
           </div>
       </form>
   </div>
   
   </body>
   </html>
   ```

### 方法四：删除书籍 

1. 在前端allBook.jsp增加“删除”链接

2. BookController 类编写 ， 

   ```java
   //删除书籍
   @RequestMapping("/deleteBook/{bookId}")
   public String deleteBook(@PathVariable("bookId") int id) {
       bookService.deleteBookById(id);
       return "redirect:/book/allBook";
   }
   ```

## 小结

这个是同学们的第一个SSM整合案例，一定要烂熟于心！

SSM框架的重要程度是不言而喻的，学到这里，大家已经可以进行基本网站的单独开发。但是这只是增删改查的基本操作。可以说学到这里，大家才算是真正的步入了后台开发的门。也就是能找一个后台相关工作的底线。

或许很多人，工作就做这些事情，但是对于个人的提高来说，还远远不够！我们后面还要学习一些 SpringMVC 的知识！

Ajax 和 Json文件上传和下载拦截器SpringBoot、SpringCloud开发

## 查询书籍功能

1. 前端页面增加一个输入框和查询按钮

   ```jsp
   <div class="col-md-4 column">
       <!--toAddBook-->
       <a class="btn btn-primary" href="${pageContext.request.contextPath}/book/toAddBook">新增书籍</a>
       <a class="btn btn-primary" href="${pageContext.request.contextPath}/book/allBook">显示全部书籍</a>
   </div>
   <div class="col-md-4 column">
       <%--查询书籍--%>
       <form action="${pageContext.request.contextPath}/book/queryBook" method="post">
           <input type="text" name="queryBookName" class="form-control" placeholder="请输入要查询的书籍名称">
           <input type="submit" class="btn btn-primary form-inline" value="查询">
       </form>
   </div>
   ```

2. 编写查询的Controller

   ```java
   //查询书籍
   @RequestMapping("/queryBook")
   public String queryBook(String queryBookName, Model model) {
       Books books = bookService.queryBookByName(queryBookName);
       System.err.println("queryBook=>" + books);
       List<Books> list = new ArrayList<>();
       list.add(books);
       model.addAttribute("books", list);
       return "allBook";
   }
   ```

3. 由于底层没有实现，所以我们要将底层代码先搞定

   1. Mapper接口

      ```java
      Books queryBookByName(@Param("bookName") String bookName);
      ```

   2. Mapper.xml

      ```xml
      <select id="queryBookByName" resultType="books">
          select *
          from ssmbuild.books
          where bookName = #{bookName};
      </select>
      ```

   3. Service接口

      ```java
      Books queryBookByName(String bookName);
      ```

   4. Service实现类

      ```java
      @Override
      public Books queryBookByName(String bookName) {
          return bookMapper.queryBookByName(bookName);
      }
      ```

4. 完善Controller

5. 测试，查询功能OK！

优化！我们发现查询的东西不存在的时候，查出来的页面是空的，我们可以提高一下用户的体验性！使得东西不存在的时候，提示错误信息并且返回全部书籍的信息
1. 在前端添加一个展示全部书籍的按钮

   ```jsp
   <a class="btn btn-primary" href="${pageContext.request.contextPath}/book/allBook">显示全部书籍</a>
   ```

2. 并在后台增加一些判断性的代码！

   ```java
   //查询书籍
   @RequestMapping("/queryBook")
   public String queryBook(String queryBookName, Model model) {
       Books books = bookService.queryBookByName(queryBookName);
       System.err.println("queryBook=>" + books);
       List<Books> list = new ArrayList<>();
       list.add(books);
       if (books == null) {
           list = bookService.queryAllBook();
           model.addAttribute("error", "未查到");
       }
       model.addAttribute("books", list);
       return "allBook";
   }
   ```

3. 将错误信息展示在前台页面！完整的查询栏代码

   ```jsp
   <%--查询书籍--%>
   <form class="form-inline" action="${pageContext.request.contextPath}/book/queryBook" method="post">
       <span style="color: red;font-weight: bold">${error}</span>
       <label class="sr-only">
           <input type="text" name="queryBookName" class="form-control"
                  placeholder="请输入要查询的书籍名称">
       </label>
       <input type="submit" class="btn btn-primary form-inline" value="查询">
   </form>
   ```

# Ajax

## 简介

1. A JAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）。

2. A JAX 是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。

3. Ajax 不是一种新的编程语言，而是一种用于创建更好更快以及交互性更强的Web应用程序的技术。

4. 在 2005 年，Google 通过其 Google Suggest 使 A JAX 变得流行起来。Google Suggest能够自动帮你完成搜索单词。Google Suggest 使用 A JAX 创造出动态性极强的 web 界面：当您在谷歌的搜索框输入关键字时， JavaScript 会把这些字符发送到服务器，然后服务器会返回一个搜索建议的列表。就和国内百度的搜索框一样：

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240506112310070-1312352884.png" alt="image-20240506112310773" width="500" />

5. 传统的网页(即不用ajax技术的网页)，想要更新内容或者提交一个表单，都需要重新加载整个网页。使用ajax技术的网页，通过在后台服务器进行少量的数据交换，就可以实现异步局部更新。

6. 使用Ajax，用户可以创建接近本地桌面应用的直接、高可用、更丰富、更动态的Web用户界面。

## 伪造Ajax

我们可以使用前端的一个标签来伪造一个ajax的样子。 iframe标签

1. 新建一个module ： sspringmvc-06-ajax ， 导入web支持！

   web.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <!--注册DispatcherServlet-->
       <servlet>
           <servlet-name>springmvc</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <!--绑定Spring配置-->
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:applicationContext.xml</param-value>
           </init-param>
           <!--设置加载开启等级-->
           <load-on-startup>1</load-on-startup>
       </servlet>
       <servlet-mapping>
           <servlet-name>springmvc</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
       
       <!--注册CharacterEncodingFilter-->
       <filter>
           <filter-name>encoding</filter-name>
           <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
           <!--设置编码-->
           <init-param>
               <param-name>encoding</param-name>
               <param-value>utf-8</param-value>
           </init-param>
       </filter>
       <filter-mapping>
           <filter-name>encoding</filter-name>
           <url-pattern>/*</url-pattern>
       </filter-mapping>
   </web-app>
   ```

   applicationContext.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
       <!-- 自动扫描指定的包，下面所有注解类交给IOC容器管理 -->
       <context:component-scan base-package="com.sun.controller"/>
   
       <!--注解驱动-->
       <mvc:annotation-driven/>
   
       <!-- 视图解析器 -->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
           <!-- 前缀 -->
           <property name="prefix" value="/WEB-INF/jsp/"/>
           <!-- 后缀 -->
           <property name="suffix" value=".jsp"/>
       </bean>
   </beans>
   ```

2. 编写一个 ajax-frame.html 使用 iframe 测试，感受下效果

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>iframe测试-体验页面无刷新</title>
       <script>
           function go() {
               //所有的值变量，提取获取
               let url = document.getElementById("url").value;
               document.getElementById("iframe1").src = url;
           }
       </script>
   </head>
   <body>
   <div>
       <p>请输入地址：</p>
       <p>
           <input type="text" id="url" value="https://www.csdn.net">
           <input type="button" value="提交" onclick="go()">
       </p>
   </div>
   <div>
       <iframe id="iframe1" style="width: 50%;height: 500px"></iframe>
   </div>
   
   </body>
   </html>
   ```

3. 使用IDEA开浏览器测试一下！

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240506130604045-1668539036.png" alt="image-20240506130604866" width="500" />

利用A JAX可以做：

1. 注册时，输入用户名自动检测用户是否已经存在。
2. 登陆时，提示用户名密码错误
3. 删除数据行时，将行ID发送到后台，后台在数据库中删除，数据库删除成功后，在页面DOM中将数据行也删除。....等等

## jQuery.ajax

jQuery是一个库；包含js的大量函数方法。

### 概述

1. Ajax的核心是XMLHttpRequest对象(XHR) - - -是JavaScript对象。XHR为向服务器发送请求和解析服务器响应提供了接口。能够以异步方式从服务器获取新数据。

   纯JS原生实现Ajax我们不去讲解这里，直接使用jquery提供的，方便学习和使用，避免重复造轮子，有兴趣的同学可以去了解下JS原生XMLHttpRequest ！

   ```html
   <!--XMLHttpRequest对象 -->
   <script type="text/javascript">
       var xmlhttp;
   
       function loadXMLDoc(url) {
           xmlhttp = null;
           if (window.XMLHttpRequest) {// code for all new browsers
               xmlhttp = new XMLHttpRequest();
           } else if (window.ActiveXObject) {// code for IE5 and IE6
               xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
           }
           if (xmlhttp != null) {
               xmlhttp.onreadystatechange = state_Change;
               xmlhttp.open("GET", url, true);
               xmlhttp.send(null);
           } else {
               alert("Your browser does not support XMLHTTP.");
           }
       }
   
       function state_Change() {
           if (xmlhttp.readyState == 4) {// 4 = "loaded"
               if (xmlhttp.status == 200) {// 200 = OK 加载成功
                   // ...our code here...
               } else {
                   alert("Problem retrieving XML data");
               }
           }
       }
   </script>
   ```

2. jQuery 提供多个与 A JAX 有关的方法。通过 jQuery A JAX 方法，能够使用 HTTP Get 和 HTTP Post 从远程服务器上请求文本、HTML、XML 或 JSON – 同时您能够把这些外部数据直接载入网页的被选元素中。

3. jQuery 不是生产者，而是大自然搬运工。jQuery Ajax本质就是 XMLHttpRequest，对他进行了封装，方便调用！

```java
jQuery.ajax(...)
部分参数：
    url：请求地址
    type：请求方式，GET、POST（1.9.0之后用method）
    headers：请求头
    data：要发送的数据
    contentType：即将发送信息至服务器的内容编码类型(默认: "application/x-www-form-urlencoded; charset=UTF-8")
    async：是否异步
    timeout：设置请求超时时间（毫秒）
    beforeSend：发送请求前执行的函数(全局)
    complete：完成之后执行的回调函数(全局)
    success：成功之后执行的回调函数(全局)
    error：失败之后执行的回调函数(全局)
    accepts：通过请求头发送给服务器，告诉服务器当前客户端可接受的数据类型
    dataType：将服务器端返回的数据转换成指定类型
        "xml": 将服务器端返回的内容转换成xml格式
        "text": 将服务器端返回的内容转换成普通文本格式
        "html": 将服务器端返回的内容转换成普通文本格式，在插入DOM中时，如果包含JavaScript标签，则会尝试去执行。
        "script": 尝试将返回值当作JavaScript去执行，然后再将服务器端返回的内容转换成普通文本格式
        "json": 将服务器端返回的内容转换成相应的JavaScript对象
        "jsonp": JSONP 格式使用 JSONP 形式调用函数时，如 "myurl?callback=?" jQuery 将自动替换 ? 为正确的函数名，以执行回调函数
```

### jQuery的使用

1. 直接下载，在项目中导入外部静态资源，并在要使用的页面上加上文件地址

   ```html
   <script src="${pageContext.request.contextPath}/statics/js/jquery-3.7.1.js"></script>
   ```

   官网下载地址：https://jquery.com/download/

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240506132029593-1336703006.png" alt="image-20240506132030338" width="400" />

2. 使用CDN(内容分发网络Content Distribution Network)

   ```html
   <head>
   	<script src="https://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js"></script>
   </head>
   ```

### 原始的HttpServletResponse处理

原理：

- url：请求地址
- data：要发送的数据
- success：成功之后执行的回调函数(全局)

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240506141041841-179964664.png" alt="image-20240506141042376" width="700" />

测试：

1. 配置web.xml 和 springmvc的配置文件，复制上面案例的即可 【记得静态资源过滤和注解驱动配置上】

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc" xmlns:mvn="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
       <!-- 自动扫描指定的包，下面所有注解类交给IOC容器管理 -->
       <context:component-scan base-package="com.sun.controller"/>
       <!--静态资源过滤 否则jQuery.js会无效-->
       <mvn:default-servlet-handler/>
       <!--注解驱动-->
       <mvc:annotation-driven/>
   
       <!-- 视图解析器 -->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
           <!-- 前缀 -->
           <property name="prefix" value="/WEB-INF/jsp/"/>
           <!-- 后缀 -->
           <property name="suffix" value=".jsp"/>
       </bean>
   </beans>
   ```

2. 导入jquery ， 可以使用在线的CDN ， 也可以下载导入

3. 编写index.jsp测试

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>$Title$</title>
       
       <script src="${pageContext.request.contextPath}/statics/js/jquery-3.7.1.js"></script>
       <script>
           function a() {
               $.post(
                   {
                       url: "${pageContext.request.contextPath}/a1",
                       data: {"name": $("#username").val()},
                       success: function (data) {
                           alert(data);
                       }
                   }
               );
           }
       </script>
   </head>
   <body>
       <%--失去焦点的时候，发送一个请求(携带信息)到后台--%>
       用户名：<input type="text" id="username" onblur=a()>
   </body>
   </html>
   ```

4. 编写一个AjaxController

   ```java
   @RequestMapping("/a1")
   public void a1(String name, HttpServletResponse response) throws IOException {
       System.out.println("a1:param=>" + name);
       if ("nihao".equals(name)) {
           response.getWriter().print("true");
       } else {
           response.getWriter().print("false");
       }
   }
   ```

5. 启动tomcat测试！ 打开浏览器的控制台，当我们鼠标离开输入框的时候，可以看到发出了一个ajax的请求！是后台返回给我们的结果！测试成功！

### Springmvc实现

1. 实体类user

   ```java
   @Data
   @AllArgsConstructor
   @RequestMapping
   public class User {
       private String name;
       private int age;
       private String sex;
   }
   ```

2. 我们来获取一个集合对象，展示到前端页面

   ```java
   @RequestMapping("/a2")
   public List<User> a2() throws IOException {
       ArrayList<User> list = new ArrayList<>();
   
       list.add(new User("张1", 5, "男"));
       list.add(new User("张1", 5, "男"));
       list.add(new User("张3", 5, "女"));
       list.add(new User("张4", 5, "男"));
   
       return list;
   }
   ```

3. 前端页面

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>test2</title>
   
       <script src="${pageContext.request.contextPath}/statics/js/jquery-3.7.1.js"></script>
       <script>
           $(function () {
               $("#btn").click(function () {
                   //简写：$.post( url , param[可以省略] , success )
                   $.post("${pageContext.request.contextPath}/a2", function (data) {
                       //console.log(data);
   
                       var html = "";
                       for (let i = 0; i < data.length; i++) {
                           html += "<tr>" +
                               "<td>" + data[i].name + "</td>" +
                               "<td>" + data[i].age + "</td>" +
                               "<td>" + data[i].sex + "</td>" +
                               "</tr>"
                       }
                       $("#content").html(html);
                   });
               })
           });
   
       </script>
   </head>
   <body>
       <input type="button" value="加载数据" id="btn">
       <br>
       <table>
           <tr>
               <td>姓名</td>
               <td>年龄</td>
               <td>性别</td>
           </tr>
           <tbody id="content">
           <!--数据-->
           </tbody>
       </table>
   </body>
   </html>
   ```

## 注册提示效果

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240506172118112-1456589294.png" alt="image-20240506172118999" width="300" />

我们再测试一个小Demo，思考一下我们平时注册时候，输入框后面的实时提示怎么做到的；如何优化

1. 我们写一个Controller

   ```java
   @RequestMapping("/a3")
   public String a3(String name, String pwd) throws IOException {
       String msg = "";
       if (name != null) {
           //"admin" 应该在数据库中查
           if ("admin".equals(name)) {
               msg = "OK";
           } else {
               msg = "name 有误";
           }
       }
   
       if (pwd != null) {
           if ("123456".equals(pwd)) {
               msg = "OK";
           } else {
               msg = "pwd 有误";
           }
       }
   
       return msg;
   }
   ```

2. 前端页面 login.jsp【记得处理json乱码问题】

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
       <script src="${pageContext.request.contextPath}/statics/js/jquery-3.7.1.js"></script>
   
       <script>
           function a1() {
               $.post({
                   url: "${pageContext.request.contextPath}/a3",
                   data: {"name": $("#name").val()},
                   success: function (data) {
                       if (data.toString() == "OK") {
                           $("#userInfo").css("color", "green");
                       } else {
                           $("#userInfo").css("color", "red");
                       }
                       $("#userInfo").text(data);
                   }
               })
           }
   
           function a2() {
               $.post({
                   url: "${pageContext.request.contextPath}/a3",
                   data: {"pwd": $("#pwd").val()},
                   success: function (data) {
                       if (data == "OK") {
                           $("#pwdInfo").css("color", "green");
                       } else {
                           $("#pwdInfo").css("color", "red");
                       }
                       $("#pwdInfo").text(data);
                   }
               })
           }
       </script>
   </head>
   <body>
   <p>
       用户名：<input type="text" id="name" onblur="a1()">
       <span id="userInfo"></span>
   </p>
   <p>
       密码：<input type="text" id="pwd" onblur="a2()">
       <span id="pwdInfo"></span>
   </p>
   
   </body>
   </html>
   ```

   中文乱码问题，在applicationContext.xml中添加

   ```xml
   <!--乱码问题配置-->
   <mvc:annotation-driven>
       <mvc:message-converters register-defaults="true">
           <bean class="org.springframework.http.converter.StringHttpMessageConverter">
               <constructor-arg value="UTF-8"/>
           </bean>
           <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
               <property name="objectMapper">
                   <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                       <property name="failOnEmptyBeans" value="false"/>
                   </bean>
               </property>
           </bean>
       </mvc:message-converters>
   </mvc:annotation-driven>
   ```

3. 测试一下效果，动态请求响应，局部刷新，就是如此！

## 获取baidu接口Demo

```jsp
<!DOCTYPE HTML>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <title>JSONP百度搜索</title>
    <style>
        #q {
            width: 500px;
            height: 30px;
            border: 1px solid #ddd;
            line-height: 30px;
            display: block;
            margin: 0 auto;
            padding: 0 10px;
            font-size: 14px;
        }

        #ul {
            width: 520px;
            list-style: none;
            margin: 0 auto;
            padding: 0;
            border: 1px solid #ddd;
            margin-top: -1px;
            display: none;
        }

        #ul li {
            line-height: 30px;
            padding: 0 10px;
        }

        #ul li:hover {
            background-color: #f60;
            color: #fff;
        }
    </style>
    <script>
        // 2.步骤二
        // 定义demo函数 (分析接口、数据)
        function demo(data) {
            var Ul = document.getElementById('ul');
            var html = '';
            // 如果搜索数据存在 把内容添加进去
            if (data.s.length) {
                // 隐藏掉的ul显示出来
                Ul.style.display = 'block';
                // 搜索到的数据循环追加到li里
                for (var i = 0; i < data.s.length; i++) {
                    html += '<li>' + data.s[i] + '</li>';
                }
                // 循环的li写入ul
                Ul.innerHTML = html;
            }
        }

        // 1.步骤一
        window.onload = function () {
            // 获取输入框和ul
            var Q = document.getElementById('q');
            var Ul = document.getElementById('ul');
            // 事件鼠标抬起时候
            Q.onkeyup = function () {
                // 如果输入框不等于空
                if (this.value != '') {
                    // ☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆JSONPz重点☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆
                    // 创建标签
                    var script = document.createElement('script');
                    //给定要跨域的地址 赋值给src
                    //这里是要请求的跨域的地址 我写的是百度搜索的跨域地址
                    script.src = 'https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su?wd = ' + this.value + ' & cb = demo';
                    // 将组合好的带src的script标签追加到body里
                    document.body.appendChild(script);
                }
            }
        }
    </script>
</head>
<body>
    <input type="text" id="q"/>
    <ul id="ul">
    </ul>
</body>
</html>
```

# 拦截器

## 概述

SpringMVC的处理器拦截器`类似于Servlet开发中的过滤器Filter`，用于对处理器进行预处理和后处理。开发者可以自己定义一些拦截器来实现特定的功能。

过滤器与拦截器的区别：`拦截器是AOP思想的具体应用`。

过滤器

- servlet规范中的一部分，任何java web工程都可以使用
- 在url-pattern中配置了/*之后，可以对所有要访问的资源进行拦截

拦截器

- 拦截器是SpringMVC框架自己的，只有使用了SpringMVC框架的工程才能使用
- 拦截器只会拦截访问的控制器Controller方法， 如果访问的是jsp/html/css/image/js是不会进行拦截的。自带静态资源过滤。

## 自定义拦截器

想要自定义拦截器，必须实现 HandlerInterceptor 接口。

1. 新建一个Moudule ， springmvc-07-Interceptor ， 添加web支持

2. 配置web.xml 和 springmvc-servlet.xml 文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <!--注册DispatcherServlet-->
       <servlet>
           <servlet-name>springmvc</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <!--绑定Spring配置-->
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:applicationContext.xml</param-value>
           </init-param>
           <!--设置加载开启等级-->
           <load-on-startup>1</load-on-startup>
       </servlet>
       <servlet-mapping>
           <servlet-name>springmvc</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   
       <!--注册CharacterEncodingFilter-->
       <filter>
           <filter-name>encoding</filter-name>
           <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
           <!--设置编码-->
           <init-param>
               <param-name>encoding</param-name>
               <param-value>utf-8</param-value>
           </init-param>
       </filter>
       <filter-mapping>
           <filter-name>encoding</filter-name>
           <url-pattern>/*</url-pattern>
       </filter-mapping>
   </web-app>
   ```

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc" xmlns:mvn="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
       <!-- 自动扫描指定的包，下面所有注解类交给IOC容器管理 -->
       <context:component-scan base-package="com.sun.controller"/>
       <!--静态资源过滤-->
       <mvn:default-servlet-handler/>
       <!--注解驱动-->
       <mvc:annotation-driven/>
   
       <!-- 视图解析器 -->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
           <!-- 前缀 -->
           <property name="prefix" value="/WEB-INF/jsp/"/>
           <!-- 后缀 -->
           <property name="suffix" value=".jsp"/>
       </bean>
   
       <!--乱码问题配置-->
       <mvc:annotation-driven>
           <mvc:message-converters register-defaults="true">
               <bean class="org.springframework.http.converter.StringHttpMessageConverter">
                   <constructor-arg value="UTF-8"/>
               </bean>
               <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
                   <property name="objectMapper">
                       <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                           <property name="failOnEmptyBeans" value="false"/>
                       </bean>
                   </property>
               </bean>
           </mvc:message-converters>
       </mvc:annotation-driven>
   </beans>
   ```

3. 编写一个拦截器

   ```java
   public class MyInterceptor implements HandlerInterceptor {
       //return true; 执行下一个拦截器，放行
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           System.out.println("==========处理前==========");
           return true;
       }
   
       @Override
       public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
           System.out.println("==========处理后==========");
       }
   
       @Override
       public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
           System.out.println("==========清理==========");
       }
   }
   ```

4. 在springmvc的配置文件中配置拦截器

   ```xml
   <!--拦截器配置-->
   <mvc:interceptors>
       <mvc:interceptor>
           <!--/**：包括这个请求下的所有请求-->
           <mvc:mapping path="/**"/>
           <bean class="com.sun.config.MyInterceptor"/>
       </mvc:interceptor>
   </mvc:interceptors>
   ```

5. 编写一个Controller，接收请求

   ```java
   @RestController
   public class TestController {
   
       @GetMapping("/i1")
       public String test() {
           System.out.println("TestController=>test()执行了");
           return "OK";
       }
   }
   ```

6. 启动tomcat 测试一下！

   ```java
   输出结果：
   ==========处理前==========
   TestController=>test()执行了
   ==========处理后==========
   ==========清理==========
   ```

## 验证用户是否登录

实现思路

1. 有一个登陆页面，需要写一个controller访问页面。
2. 登陆页面有一提交表单的动作。需要在controller中处理。判断用户名密码是否正确。如果正确，向session中写入用户信息。返回登陆成功。
3. 拦截用户请求，判断用户是否登陆。如果用户已经登陆。放行， 如果用户未登陆，跳转到登陆页面

测试

1. 编写一个登陆页面 login.jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>登录页面</title>
   </head>
   <body>
   <%--
       在WEB-INF下的所有页面或者资源，
       只能通过Controller或者Servlet进行访问
   --%>
   <h1>登录页面</h1>
   <form action="" method="post">
       用户名：<input type="text" name="username">
       密码：<input type="text" name="pwd">
       <input type="submit" value="提交">
   </form>
   </body>
   </html>
   ```

2. 登录后的首页main.jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>首页</title>
   </head>
   <body>
       <h1>首页</h1>
       <span>${username}</span>
       <span>${password}</span>
       
       <p>
           <a href="${pageContext.request.contextPath}/user/goOut">注销</a>
       </p>
   </body>
   </html>
   ```

3. index.jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>$Title$</title>
   </head>
   <body>
   <h1><a href="${pageContext.request.contextPath}/user/goLogin">登录页面</a></h1>
   <h1><a href="${pageContext.request.contextPath}/user/main">首页</a></h1>
   
   </body>
   </html>
   ```

4. 编写一个Controller处理请求  

   ```java
   @Controller
   @RequestMapping("/user")
   public class LoginController {
       @PostMapping("/Login")
       public String login(String username, String pwd, HttpSession session, Model model) {
           //登录之后，把用户信息存在Session中
           session.setAttribute("username", username);
           session.setAttribute("password", pwd);
   
           model.addAttribute("username", username);
           model.addAttribute("password", pwd);
   
           return "main";
       }
   
       @RequestMapping("/main")
       public String main() {
           return "main";
       }
   
       @RequestMapping("/goLogin")
       public String gologin() {
           return "login";
       }
   
       @RequestMapping("/goOut")
       public String goOut(HttpSession session) {
           //1.移除用户信息
           session.removeAttribute("username");
           session.removeAttribute("password");
   
           //2.销毁session
           //session.invalidate();
   
           return "login";
       }
   }
   ```

5. 在 index 页面上测试跳转！启动Tomcat 测试，未登录也可以进入主页！

6. 编写用户登录拦截器

   ```java
   public class LoginInterceptor implements HandlerInterceptor {
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           //放行：判断什么情况下登录
   
           //1.登录页面也放行
           if (request.getRequestURI().contains("Login")) {
               return true;
           }
           //2.session存在
           HttpSession session = request.getSession();
           if (session.getAttribute("username") != null || session.getAttribute("pwd") != null) {
               return true;
           }
   
           //判断什么情况下没有登录   没有session
           request.getRequestDispatcher("/WEB-INF/jsp/login.jsp").forward(request, response);
           return false;
       }
   }
   ```

7. 在Springmvc的配置文件中注册拦截器

   ```xml
   <mvc:interceptors>
       <mvc:interceptor>
           <!--/**：包括这个请求下的所有请求-->
           <mvc:mapping path="/user/**"/>
           <bean class="com.sun.config.LoginInterceptor"/>
       </mvc:interceptor>
   </mvc:interceptors>
   ```

8. 再次重启Tomcat测试

# 文件上传和下载

## 准备工作

​		文件上传是项目开发中最常见的功能之一 ,springMVC 可以很好的支持文件上传，但是SpringMVC上下文中默认没有装配MultipartResolver，因此默认情况下其不能处理文件上传工作。如果想使用Spring的文件上传功能，则需要在上下文中配置MultipartResolver。

​		前端表单要求：为了能上传文件，必须将表单的`method设置为POST`，并将`enctype设置为multipart/form-data`。只有在这样的情况下，浏览器才会把用户选择的文件以二进制数据发送给服务器；

对表单中的 enctype 属性做个详细的说明：

- application/x-www=form-urlencoded：默认方式，只处理表单域中的 value 属性值，采用这种编码方式的表单会将表单域中的值处理成 URL 编码方式。
- multipart/form-data：这种编码方式会以二进制流的方式来处理表单数据，这种编码方式会把文件域指定文件的内容也封装到请求参数中，不会对字符编码。
- text/plain：除了把空格转换为 "+" 号外，其他字符都不做编码处理，这种方式适用直接通过表单发送邮件。

```html
<form action="" enctype="multipart/form-data" method="post">
    <input type="file" name="file"/>
    <input type="submit">
</form>
```

​		一旦设置了enctype为multipart/form-data，浏览器即会采用二进制流的方式来处理表单数据，而对于文件上传的处理则涉及在服务器端解析原始的HTTP响应。

​		在2003年，Apache Software Foundation发布了开源的Commons FileUpload组件，其很快成为Servlet/JSP程序员上传文件的最佳选择。

- Servlet3.0规范已经提供方法来处理文件上传，但这种上传需要在Servlet中完成。而Spring MVC则提供了更简单的封装。
- Spring MVC为文件上传提供了直接的支持，这种支持是用即插即用的MultipartResolver实现的。
- Spring MVC使用Apache Commons FileUpload技术实现了一个MultipartResolver实现类： CommonsMultipartResolver。因此，SpringMVC的文件上传还需要依赖Apache Commons FileUpload的组件。

## 文件上传

1. 导入文件上传的jar包，commons-fileupload ， Maven会自动帮我们导入他的依赖包 commonsio包；

   ```xml
   <dependencies>
       <!-- 文件上传 -->
       <dependency>
           <groupId>commons-fileupload</groupId>
           <artifactId>commons-fileupload</artifactId>
           <version>1.5</version>
       </dependency>
       <!-- servlet-api -->
       <dependency>
           <groupId>javax.servlet</groupId>
           <artifactId>servlet-api</artifactId>
           <version>2.5</version>
       </dependency>
   </dependencies>
   ```

2. 编写前端页面index.jsp

   ```jsp
   <body>
     <form action="${pageContext.request.contextPath}/upload" enctype="multipart/form-data" method="post">
         <input type="file" name="file"/>
         <input type="submit" value="upload">
     </form>
   </body>
   ```

3. 配置bean：multipartResolver

   【注意！！！这个bena的id必须为：multipartResolver ， 否则上传文件会报400的错误！】

   ```xml
   <!--文件上传配置-->
   <bean id="multipartResolver"
         class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
       <!-- 请求的编码格式，必须和jSP的pageEncoding属性一致，以便正确读取表单的内容，
       默认为ISO-8859-1 -->
       <property name="defaultEncoding" value="utf-8"/>
       <!-- 上传文件大小上限，单位为字节（10485760=10M） -->
       <property name="maxUploadSize" value="10485760"/>
       <property name="maxInMemorySize" value="40960"/>
   </bean>
   ```

   CommonsMultipartFile 的 常用方法：

   - String getOriginalFilename()：获取上传文件的原名
   - InputStream getInputStream()：获取文件流void 
   - transferTo(File dest)：将上传文件保存到一个目录文件中我们去实际测试一下

4. Controller  

   ```java
   @RestController
   public class FileController {
       //@RequestParam("file") 将name=file控件得到的文件封装成CommonsMultipartFile 对象
       //批量上传CommonsMultipartFile则为数组即可
       @RequestMapping("/upload")
       public String fileUpload(@RequestParam("file") CommonsMultipartFile file, HttpServletRequest request) throws IOException {
           //1.获取文件名 : file.getOriginalFilename();
           String uploadFileName = file.getOriginalFilename();
   
           //如果文件名为空，直接回到首页！
           if ("".equals(uploadFileName)) {
               return "redirect:/index.jsp";
           }
           System.out.println("上传文件名 : " + uploadFileName);
   
           //2.上传路径保存设置
           String path = request.getSession().getServletContext().getRealPath("/upload");
   
           //如果路径不存在，创建一个
           File realPath = new File(path);
           if (!realPath.exists()) {
               realPath.mkdir();
           }
           System.out.println("上传文件保存地址：" + realPath);
           InputStream is = file.getInputStream(); //文件输入流
           OutputStream os = new FileOutputStream(new File(realPath, uploadFileName)); //文件输出流
           
           //3.读取写出
           int len = 0;
           byte[] buffer = new byte[1024];
           while ((len = is.read(buffer)) != -1) {
               os.write(buffer, 0, len);
               os.flush();
           }
           os.close();
           is.close();
           return "redirect:/index.jsp";
       }
   }
   ```

5. 测试

采用file.Transto 来保存上传的文件

1. 编写Controller

   ```java
   /*
    * 采用file.Transto 来保存上传的文件
    */
   @RequestMapping("/upload2")
   public String fileUpload2(@RequestParam("file") CommonsMultipartFile file, HttpServletRequest request) throws IOException {
       //1.上传路径保存设置
       String path = request.getSession().getServletContext().getRealPath("/upload");
       File realPath = new File(path);
       if (!realPath.exists()) {
           realPath.mkdir();
       }
   
       //2.上传文件地址
       System.out.println("上传文件保存地址：" + realPath);
   
       //3.通过CommonsMultipartFile的方法直接写文件（注意这个时候）
       file.transferTo(new File(realPath + "/" + file.getOriginalFilename()));
   
       return "redirect:/index.jsp";
   }
   ```

2. 前端表单提交地址修改

3. 访问提交测试，OK！

## 文件下载

文件下载步骤：

1. 设置 response 响应头
2. 读取文件 -- InputStream
3. 写出文件 -- OutputStream
4. 执行操作
5. 关闭流 （先开后关）

代码实现：

```java
@RequestMapping(value="/download")
public String downloads(HttpServletResponse response , HttpServletRequest
                        request) throws Exception{
    //要下载的图片地址
    String path = request.getSession().getServletContext().getRealPath("/upload");
    String fileName = "1.jpg";
    //1、设置response 响应头
    response.reset(); //设置页面不缓存,清空buffer
    response.setCharacterEncoding("UTF-8"); //字符编码
    response.setContentType("multipart/form-data"); //二进制传输数据
    //设置响应头
    response.setHeader("Content-Disposition",
                       "attachment;fileName="+ URLEncoder.encode(fileName, "UTF-8"));
    File file = new File(path,fileName);

    //2、 读取文件--输入流
    InputStream input=new FileInputStream(file);

    //3、 写出文件--输出流
    OutputStream out = response.getOutputStream();
    byte[] buff =new byte[1024];
    int index=0;

    //4、执行 写出操作
    while((index= input.read(buff))!= -1){
        out.write(buff, 0, index);
        out.flush();
    } 

    out.close();
    input.close();
    return null;
}
```

前端

```html
<a href="/download">点击下载</a>
<!--
<a href="${pageContext.request.contextPath}/statics/1.jpg">下载图片</a>
-->
```

测试，文件下载OK，大家可以和我们之前学习的JavaWeb原生的方式对比一下，就可以知道这个便捷多了!