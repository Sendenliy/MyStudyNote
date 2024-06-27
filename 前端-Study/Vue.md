# 前端体系

想要成为真正的“互联网 Java 全栈工程师”还有很长的一段路要走，其中“我大前端”是绕不开的一门必修课。

本阶段课程的主要目的就是带领我 Java 后台程序员认识前端、了解前端、掌握前端，为实现成为 “互联网 Java 全栈工程师” 再向前迈进一步。

## 前端三要素

### 前端三要素

- HTML（结构）：超文本标记语言（Hyper Text Markup Language），决定网页的结构和内容
- CSS（表现）：层叠样式表（Cascading Style Sheets），设定网页的表现样式
- JavaScript（行为）：是一种弱类型脚本语言，其源代码不需经过编译，而是由浏览器解释运行，用于控制网页的行为

### 结构层（HTML）

太简单，略

### 表现层（CSS）

CSS 层叠样式表是一门标记语言，并不是编程语言，因此不可以自定义变量，不可以引用等，换句话说就是不具备任何语法支持，它主要缺陷如下：

- 语法不够强大，比如无法嵌套书写，导致模块化开发中需要书写很多重复的选择器；
- 没有变量和合理的样式复用机制，使得逻辑上相关的属性值必须以字面量的形式重复输出，导致难以维护；

这就导致了我们在工作中无端增加了许多工作量。为了解决这个问题，前端开发人员会使用一种称之为【CSS 预处理器】 的工具，提供 CSS 缺失的样式层复用机制、减少冗余代码，提高样式代码的可维护性。大大提高了前端在样式上的开发效率。

什么是 CSS 预处理器?

CSS 预处理器定义了一种新的语言，其基本思想是，用一种专门的编程语言，为 CSS 增加了一些编程的特性，将 CSS 作为目标生成文件，然后开发者就只要使用这种语言进行 CSS 的编码工作。转化成通俗易懂的话来说就是“用一种专门的编程语言，进行 Web 页面样式设计，再通过编译器转化为正常的 CSS 文件，以供项目使用”。

常用的 CSS 预处理器有哪些：

- SASS：基于 Ruby，通过服务端处理，功能强大。解析效率高。需要学习 Ruby 语言，上手难度高于 LESS。
- LESS：基于 NodeJS，通过客户端处理，使用简单。功能比 SASS 简单，解析效率也低于 SASS，但在实际开发中足够了，所以我们后台人员如果需要的话，建议使用 LESS。

### 行为层（JavaScript）

JavaScript 一门弱类型脚本语言，其源代码在发往客户端运行之前不需经过编译，而是将文本格式的字符代码发送给浏览器由浏览器解释运行。

**Native 原生 JS 开发**

原生 JS 开发，也就是让我们按照 【ECMAScript】 标准的开发方式，简称是 ES，特点是所有浏览器都支持。ES 标准已发布如下版本：

- ES3
- ES4（内部，未正式发布） 
- ES5（全浏览器支持）
- ES6（常用，当前主流版本：webpack打包成为ES5支持！） 
- ES7
- ES8
- ES9（草案阶段）

从 ES6 开始每年发布一个版本，以年份作为名称，区别就是逐步增加新特性。

**TypeScript 微软的标准**

TypeScript 是一种由微软开发的自由和开源的编程语言。它是 JavaScript 的一个超集，而且本质上向这个语言添加了可选的静态类型和基于类的面向对象编程。由安德斯·海尔斯伯格（C#、Delphi、TypeScript 之父；.NET 创立者）主导。

## JavaScript 框架

### jQuery库

大家最熟知的 JavaScript 库，优点是简化了 DOM 操作，缺点是 DOM 操作太频繁，影响前端性能；在前端眼里使用它仅仅是为了兼容 IE6、7、8；

### Angular

Google 收购的前端框架，由一群 Java 程序员开发。

其特点是`将后台的 MVC 模式搬到了前端并增加了模块化开发的理念`，与微软合作，采用 TypeScript 语法开发；对后台程序员友好，对前端程序员不太友好；

最大的缺点是版本迭代不合理（如：1代 -> 2代，除了名字，基本就是两个东西；已推出了Angular6）

### React

Facebook 出品，一款高性能的 JS 前端框架；特点是`提出了新概念 【虚拟 DOM】` 用于减少真实 DOM操作，在`内存`中模拟 DOM 操作，有效的提升了前端渲染效率；

缺点是使用复杂，因为需要额外学习一门 【JSX】 语言；

### Vue

一款渐进式 JavaScript 框架，所谓渐进式就是逐步实现新特性的意思，如实现模块化开发、路由、状态管理等新特性。

其特点是`综合了 Angular（模块化MVVM） 和 React（虚拟 DOM）` 的优点；

### Axios

前端通信框架；

因为 Vue 的边界很明确，就是为了处理 DOM，所以并不具备通信能力，此时就需要额外使用一个通信框架与服务器交互；当然也可以直接选择使用 jQuery 提供的 A JAX 通信功能；

## UI 框架

## 常用

- Ant-Design：阿里巴巴出品，基于 React 的 UI 框架
- ElementUI、iview、ice：饿了么出品，基于 Vue 的 UI 框架
- Bootstrap：Twitter 推出的一个用于前端开发的开源工具包
- AmazeUI：又叫“妹子 UI”，一款 HTML5 跨屏前端框架
- Layui：轻量级框架

## JavaScript 构建工具

- Babel：JS 编译工具，主要用于浏览器不支持的 ES 新特性，比如用于编译 TypeScript
- WebPack：模块打包器，主要作用是打包、压缩、合并及按序加载

## 三端统一

1. 混合开发（Hybrid App)

   主要目的是实现一套代码三端统一(PC、Android :  .apk , iOS: .ipa ）并能够调用到设备底层硬件(如:传感器、GPS、摄像头等)，打包方式主要有以下两种:

   - 云打包:HBuild -> HBuildX,DCloud出品;
   - API Cloud·本地打包:Cordova(前身是PhoneGap)

2. 微信小程序

   详见微信官网，这里就是介绍一个方便微信小程序U开发的框架:WeUI

## 后端技术

前端人员为了方便开发也需要掌握一定的后端技术，但我们 Java 后台人员知道后台知识体系极其庞大复杂，所以为了方便前端人员开发后台应用，就出现了 NodeJS 这样的技术。

NodeJS 的作者已经声称放弃 NodeJS（说是架构做的不好再加上笨重的 node_modules，可能让作者不爽了吧），开始开发全新架构的 Deno。

既然是后台技术，那肯定也需要框架和项目管理工具，NodeJS 框架及项目管理工具如下：

- Express：NodeJS 框架
- Koa：Express 简化版
- NPM：项目综合管理工具，类似于 Maven
- YARN：NPM 的替代方案，类似于 Maven 和 Gradle 的关系

## 主流前端框架

Vue.js

### iView

iview 是一个强大的基于 Vue 的 UI 库，有很多实用的基础组件比 elementui 的组件更丰富，主要服务于PC 界面的中后台产品。使用单文件的 Vue 组件化开发模式 基于 npm + webpack + babel 开发，支持ES2015 高质量、功能丰富 友好的 API ，自由灵活地使用空间。

- [官网地址] https://www.iviewui.com/
- [Github] https://github.com/TalkingData/iview-weapp 
- [iview-admin] https://github.com/iview/iview-admin

备注：属于前端主流框架，选型时可考虑使用，主要特点是移动端支持较多

### ElementUI

Element 是饿了么前端开源维护的 Vue UI 组件库，组件齐全，基本涵盖后台所需的所有组件，文档讲解详细，例子也很丰富。主要用于开发 PC 端的页面，是一个质量比较高的 Vue UI 组件库。

- [官网地址]https://element-plus.org/zh-CN/#/zh-CN
- [Github] https://github.com/ElementUI/element-starter
- [vue-element-admin] https://github.com/PanJiaChen/vue-element-admin

备注：属于前端主流框架，选型时可考虑使用，主要特点是桌面端支持较多

### ICE

飞冰是阿里巴巴团队基于 React/Angular/Vue 的中后台应用解决方案，在阿里巴巴内部，已经有 270 多个来自几乎所有 BU 的项目在使用。飞冰包含了一条从设计端到开发端的完整链路，帮助用户快速搭建属于自己的中后台应用。

- [官网地址] https://alibaba.github.io/ice
- [Github] https://github.com/alibaba/ice

备注：主要组件还是以 React 为主，截止 2019 年 02 月 17 日更新博客前对 Vue 的支持还不太完善，目前尚处于观望阶段

### VantUI

Vant UI 是有赞前端团队基于有赞统一的规范实现的 Vue 组件库，提供了一整套 UI 基础组件和业务组件。通过 Vant，可以快速搭建出风格统一的页面，提升开发效率。

- [官网地址] https://youzan.github.io/vant/#/zh-CN/intro
- [Github] https://github.com/youzan/vant

### AtUI

at-ui 是一款基于 Vue 2.x 的前端 UI 组件库，主要用于快速开发 PC 网站产品。 它提供了一套 npm + webpack + babel 前端开发工作流程，CSS 样式独立，即使采用不同的框架实现都能保持统一的 UI 风格

- [官网地址] https://at-ui.github.io/at-ui/#/zh
- [Github] https://github.com/at-ui/at-ui

### CubeUI

cube-ui 是滴滴团队开发的基于 Vue.js 实现的精致移动端组件库。支持按需引入和后编译，轻量灵活；扩展性强，可以方便地基于现有组件实现二次开发。

- [官网地址] https://didi.github.io/cube-ui/#/zh-CN
- [Github] https://github.com/didi/cube-ui/

### Flutter

Flutter 是谷歌的移动端 UI 框架，可在极短的时间内构建 Android 和 iOS 上高质量的原生级应用。Flutter 可与现有代码一起工作, 它被世界各地的开发者和组织使用, 并且 Flutter 是免费和开源的。

- [官网地址] https://flutter.dev/docs
- [Github] https://github.com/flutter/flutter

备注：Google 出品，主要特点是快速构建原生 APP 应用程序，如做混合应用该框架为必选框架

### Ionic

Ionic 既是一个 CSS 框架也是一个 Javascript UI 库，Ionic 是目前最有潜力的一款 HTML5 手机应用开发框架。通过 SASS 构建应用程序，它提供了很多 UI 组件来帮助开发者开发强大的应用。它使用JavaScript MVVM 框架和 AngularJS/Vue 来增强应用。提供数据的双向绑定，使用它成为 Web 和移动开发者的共同选择。

- [官网地址] https://ionicframework.com/
- [官网文档] https://ionicframework.com/docs/ 
- [Github] https://github.com/ionic-team/ionic

### mpvue

mpvue 是美团开发的一个使用 Vue.js 开发小程序的前端框架，目前支持 微信小程序、百度智能小程序，头条小程序 和 支付宝小程序。 框架基于 Vue.js，修改了的运行时框架 runtime 和代码编译器compiler 实现，使其可运行在小程序环境中，从而为小程序开发引入了 Vue.js 开发体验。

- [官网地址] http://mpvue.com/
- [Github] https://github.com/Meituan-Dianping/mpvue

备注：完备的 Vue 开发体验，并且支持多平台的小程序开发，推荐使用

### WeUI

WeUI 是一套同微信原生视觉体验一致的基础样式库，由微信官方设计团队为微信内网页和微信小程序量身设计，令用户的使用感知更加统一。包含 button、cell、dialog、toast、article、icon 等各式元素。

- [官网地址] https://weui.io/
- [Github] https://github.com/weui/weui.git

# 前后分离的演变史

## 后端为主的MVC时代

为了降低开发的复杂度，以后端为出发点，比如：Struts、SpringMVC 等框架的使用，就是后端的 MVC时代;

以 SpringMVC 流程为例：  

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240508200445547-840224709.png" alt="image-20240508200447125" width="600" />

- 发起请求到前端控制器( DispatcherServlet )
- 前端控制器请求 HandlerMapping 查找 Handler ，可以根据 xml 配置、注解进行查找
- 处理器映射器 HandlerMapping 向前端控制器返回 Handler
- 前端控制器调用处理器适配器去执行 Handler
- 处理器适配器去执行 Handler
- Handler 执行完成给适配器返回 ModelAndView
- 处理器适配器向前端控制器返回 ModelAndView ， ModelAndView 是 SpringMVC 框架的一个底层对象，包括 Model 和 View
- 前端控制器请求视图解析器去进行视图解析，根据逻辑视图名解析成真正的视图( JSP )
- 视图解析器向前端控制器返回 View
- 前端控制器进行视图渲染，视图渲染将模型数据(在 ModelAndView 对象中)填充到 request域
- 前端控制器向用户响应结果

优点：

MVC 是一个非常好的协作模式，能够有效降低代码的耦合度，从架构上能够让开发者明白代码应该写在哪里。为了让 View 更纯粹，还可以使用 Thymeleaf、Freemarker 等模板引擎，使模板里无法写入 Java代码，让前后端分工更加清晰。

缺点：

前端开发重度依赖开发环境，开发效率低，这种架构下，前后端协作有两种模式：

1. 第一种是前端写 DEMO，写好后，让后端去套模板。好处是 DEMO 可以本地开发，很高效。不足是还需要后端套模板，有可能套错，套完后还需要前端确定，来回沟通调整的成本比较大；
2. 另一种协作模式是前端负责浏览器端的所有开发和服务器端的 View 层模板开发。好处是 UI 相关的代码都是前端去写就好，后端不用太关注，不足就是前端开发重度绑定后端环境，环境成为影响前端开发效率的重要因素。

前后端职责纠缠不清：模板引擎功能强大，依旧可以通过拿到的上下文变量来实现各种业务逻辑。这样，只要前端弱势一点，往往就会被后端要求在模板层写出不少业务代码。还有一个很大的灰色地带是Controller ，页面路由等功能本应该是前端最关注的，但却是由后端来实现。 Controller 本身与 Model 往往也会纠缠不清，看了让人咬牙的业务代码经常会出现在 Controller 层。这些问题不能全归结于程序员的素养，否则 JSP 就够了。

对前端发挥的局限性：性能优化如果只在前端做空间非常有限，于是我们经常需要后端合作；

注：在这期间（2005 年以前），包括早期的 JSP、PHP 可以称之为 Web 1.0 时代。在这里想说一句，如果你是一名 Java 初学者，请你不要再把一些陈旧的技术当回事了，比如 JSP，因为时代在变、技术在变、什么都在变（引用扎克伯格的一句话：唯一不变的是变化本身）

## 基于AJAX带来的SPA时代

时间回到 2005 年 AJAX （Asynchronous JavaScript And XML，异步 JavaScript 和 XML，老技术新用法） 被正式提出并开始使用 CDN 作为静态资源存储，于是出现了 JavaScript 王者归来（在这之前JS 都是用来在网页上贴狗皮膏药广告的）的 SPA（Single Page Application）单页面应用时代。

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240508200535147-930694127.png" alt="image-20240508200536922" width="400" />

优点：

这种模式下，前后端的分工非常清晰，前后端的关键协作点是 A JAX 接口。看起来是如此美妙，但回过头来看看的话，这与 JSP 时代区别不大。复杂度从服务端的 JSP 里移到了浏览器的 JavaScript，浏览器端变得很复杂。类似 Spring MVC，这个时代开始出现浏览器端的分层架构

<img src="https://img2023.cnblogs.com/blog/3406637/202406/3406637-20240627193106633-1086899925.png" alt="image-20240508200540919" width="400" />

缺点

- 前后端接口的约定： 如果后端的接口一塌糊涂，如果后端的业务模型不够稳定，那么前端开发会很痛苦；不少团队也有类似尝试，通过接口规则、接口平台等方式来做。有了和后端一起沉淀的 接口规则，还可以用来模拟数据，使得前后端可以在约定接口后实现高效并行开发。
- 前端开发的复杂度控制： SPA 应用大多以功能交互型为主，JavaScript 代码过十万行很正常。大量JS 代码的组织，与 View 层的绑定等，都不是容易的事情。

## 前端为主的 MV* 时代

(大前端)此处的 MV* 模式如下：

- MVC（同步通信为主）：Model、View、Controller
- MVP（异步通信为主）：Model、View、Presenter
- MVVM（异步通信为主）：Model、View、ViewModel

为了降低前端开发复杂度，涌现了大量的前端框架，比如： AngularJS 、 React 、Vue.js 、 EmberJS 等，这些框架总的原则是先按类型分层，比如 Templates、Controllers、Models，然后再在层内做切分，如下图：

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240508201728655-275463355.png" alt="image-20240508201730773" width="400" />

优点：

- 前后端职责很清晰： 前端工作在浏览器端，后端工作在服务端。清晰的分工，可以让开发并行，测试数据的模拟不难，前端可以本地开发。后端则可以专注于业务逻辑的处理，输出 RESTful等接口。
- 前端开发的复杂度可控： 前端代码很重，但合理的分层，让前端代码能各司其职。这一块蛮有意思的，简单如模板特性的选择，就有很多很多讲究。并非越强大越好，限制什么，留下哪些自由，代码应该如何组织，所有这一切设计，得花一本书的厚度去说明。
- 部署相对独立： 可以快速改进产品体验

缺点：

- 代码不能复用。比如后端依旧需要对数据做各种校验，校验逻辑无法复用浏览器端的代码。如果可以复用，那么后端的数据校验可以相对简单化。
- 全异步，对 SEO(搜索引擎优化) 不利。往往还需要服务端做同步渲染的降级方案。
- 性能并非最佳，特别是移动互联网环境下。
- SPA 不能满足所有需求，依旧存在大量多页面应用。URL Design 需要后端配合，前端无法完全掌控。

## NodeJS带来的全栈时代

前端为主的 MV* 模式解决了很多很多问题，但如上所述，依旧存在不少不足之处。随着 NodeJS 的兴起，JavaScript 开始有能力运行在服务端。这意味着可以有一种新的研发模式：

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240508201807902-23079684.png" alt="image-20240508201810026" width="300" />

在这种研发模式下，前后端的职责很清晰。对前端来说，两个 UI 层各司其职：

- Front-end UI layer 处理浏览器层的展现逻辑。通过 CSS 渲染样式，通过 JavaScript 添加交互功能，HTML 的生成也可以放在这层，具体看应用场景。
- Back-end UI layer 处理路由、模板、数据获取、Cookie 等。通过路由，前端终于可以自主把控URL Design，这样无论是单页面应用还是多页面应用，前端都可以自由调控。后端也终于可以摆脱对展现的强关注，转而可以专心于业务逻辑层的开发。

通过 Node，Web Server 层也是 JavaScript 代码，这意味着部分代码可前后复用，需要 SEO 的场景可以在服务端同步渲染，由于异步请求太多导致的性能问题也可以通过服务端来缓解。前一种模式的不足，通过这种模式几乎都能完美解决掉。

与 JSP 模式相比，全栈模式看起来是一种回归，也的确是一种向原始开发模式的回归，不过是一种螺旋上升式的回归。

基于 NodeJS 的全栈模式，依旧面临很多挑战：

- 需要前端对服务端编程有更进一步的认识。比如 TCP/IP 等网络知识的掌握。
- NodeJS 层与 Java 层的高效通信。NodeJS 模式下，都在服务器端，RESTful HTTP 通信未必高效，通过 SOAP 等方式通信更高效。一切需要在验证中前行。
- 对部署、运维层面的熟练了解，需要更多知识点和实操经验。
- 大量历史遗留问题如何过渡。这可能是最大最大的阻力。

## 小结

综上所述，模式也好，技术也罢，没有好坏优劣之分，只有适合不适合；

前后分离的开发思想主要是基于 SoC （关注度分离原则），上面种种模式，都是让前后端的职责更清晰，分工更合理高效。

# MVVM模式

## 什么是MVVM模式

MVVM（Model-View-ViewModel）是一种软件架构设计模式，由微软 WPF（用于替代 WinForm，以前就是用这个技术开发桌面应用程序的）和 Silverlight（类似于 Java Applet，简单点说就是在浏览器上运行的 WPF） 的架构师 Ken Cooper 和 Ted Peters 开发，是一种简化用户界面的事件驱动编程方式。由 John Gossman（同样也是 WPF 和 Silverlight 的架构师）于 2005 年在他的博客上发表。

MVVM 源自于经典的 MVC（Model-View-Controller）模式。`MVVM 的核心是 ViewModel 层`，负责转换 Model 中的数据对象来让数据变得更容易管理和使用，其作用如下：

- 该层向上：与视图层view进行双向数据绑定
- 向下：与 Model 层通过接口请求进行数据交互

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240508202048935-1052595547.png" alt="image-20240508202050896" width="500" />

MVVM 已经相当成熟了，当下流行的 MVVM 框架有 Vue.js ， AngularJS 等。

## 为什么要使用 MVVM

MVVM 模式和 MVC 模式一样，主要目的是分离视图（View）和模型（Model），有几大好处：

- 低耦合： 视图（View）可以独立于 Model 变化和修改，一个 ViewModel 可以绑定到不同的 View上，当 View 变化的时候 Model 可以不变，当 Model 变化的时候 View 也可以不变。
- 可复用： 你可以把一些视图逻辑放在一个 ViewModel 里面，让很多 View 重用这段视图逻辑。
- 独立开发： 开发人员可以专注于业务逻辑和数据的开发（ViewModel），设计人员可以专注于页面设计。
- 可测试： 界面素来是比较难于测试的，而现在测试可以针对 ViewModel 来写。

## MVVM 的组成部分  

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240508202144340-1414922844.png" alt="image-20240508202146478" width="400" />

### View

View 是视图层，也就是用户界面。前端主要由 HTML 和 CSS 来构建，为了更方便地展现ViewModel 或者 Model层的数据，已经产生了各种各样的前后端模板语言，比如 FreeMarker、Thymeleaf 等等，各大 MVVM 框架如 Vue.js，AngularJS，EJS 等也都有自己用来构建用户界面的内置模板语言。

### Model

Model 是指数据模型，泛指后端进行的各种业务逻辑处理和数据操控，主要围绕数据库系统展开。这里的难点主要在于需要和前端约定统一的 接口规则

### ViewModel

ViewModel 是由前端开发人员组织生成和维护的视图数据层。在这一层，前端开发者对从后端获取的Model 数据进行转换处理，做二次封装，以生成符合 View 层使用预期的视图数据模型。

需要注意的是 ViewModel 所封装出来的数据模型包括视图的状态和行为两部分，而 Model 层的数据模型是只包含状态的。

- 比如页面的这一块展示什么，那一块展示什么这些都属于视图状态（展示）
- 页面加载进来时发生什么，点击这一块发生什么，这一块滚动时发生什么这些都属于视图行为（交互）

视图状态和行为都封装在了 ViewModel 里。这样的封装使得 ViewModel 可以完整地去描述 View 层。由于实现了双向绑定，ViewModel 的内容会实时展现在 View 层，这是激动人心的，因为前端开发者再也不必低效又麻烦地通过操纵 DOM 去更新视图。

MVVM 框架已经把最脏最累的一块做好了，我们开发者只需要处理和维护 ViewModel，更新数据视图就会自动得到相应更新，真正实现 事件驱动编程 。

View 层展现的不是 Model 层的数据，而是 ViewModel 的数据，由 ViewModel 负责与Model 层交互，这就完全解耦了 View 层和 Model 层，这个解耦是至关重要的，它是前后端分离方案实施的重要一环。

# 第一个Vue程序

## 简介

Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的渐进式框架，发布于 2014 年 2 月。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。

Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库（如：vue-router，vue-resource，vuex）或既有项目整合。

## MVVM 模式的实现者

- Model：模型层，在这里表示 JavaScript 对象
- View：视图层，在这里表示 DOM（HTML 操作的元素） 
- ViewModel：连接视图和数据的中间件，Vue.js 就是 MVVM 中的 ViewModel 层的实现者

在 MVVM 架构中，是不允许 数据 和 视图 直接通信的，只能通过 ViewModel 来通信，而 ViewModel就是定义了一个 Observer 观察者。

- ViewModel 能够观察到数据的变化，并对视图对应的内容进行更新
- ViewModel 能够监听到视图的变化，并能够通知数据发生改变

至此，我们就明白了，Vue.js 就是一个 MVVM 的实现者，他的核心就是实现了 DOM 监听 与 数据绑定

## 为什么要使用 Vue.js

- 轻量级，体积小是一个重要指标。Vue.js 压缩后有只有 20多kb （Angular 压缩后 56kb+，React压缩后 44kb+）
- 移动优先。更适合移动端，比如移动端的 Touch 事件
- 易上手，学习曲线平稳，文档齐全
- 吸取了 Angular（模块化）和 React（虚拟 DOM）的长处，并拥有自己独特的功能，如：`计算属性`
- 开源，社区活跃度高

## 第一个Vue程序准备

注意：Vue 不支持 IE8 及以下版本，因为 Vue 使用了 IE8 无法模拟的 ECMAScript 5 特性。但它支持所有兼容 ECMAScript 5 的浏览器。

VScode、Hubilder、Sublime、WebStrom、IDEA

开发版本

- 包含完整的警告和调试模式：https://vuejs.org/js/vue.js
- 删除了警告，30.96KB min + gzip：https://vuejs.org/js/vue.min.js

CDN

```html
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.js"></script>
<script src="https://cdn.jsdelivr.net/npm/vue@3.4.27/dist/vue.global.min.js"></script>
```

## Hello，Vue

Vue.js 的核心是实现了 MVVM 模式，她扮演的角色就是 ViewModel 层，那么所谓的第一个应用程序就是展示她的 数据绑定 功能，操作流程如下：

1. 创建一个 HTML 文件 01-hello.html

2. 引入 Vue.js

3. 创建一个 Vue 的实例

   ```html
   <body>
       <!--view层 模版-->
       <div id="app">
           {{message}}
       </div>
   
       <!--1.导入vue.js-->
       <script src=" https://cdn.jsdelivr.net/npm/vue@2/dist/vue.min.js "></script>
       <script>
           var vm = new Vue({
               el: "#app",
               data: {
                   message: "hello,vue!"
               }
           });
       </script>
   </body>
   ```

   说明：

   - el:'#vue' ：绑定元素的 ID
   - data:{message:'Hello Vue!'} ：数据对象中有一个名为 message 的属性，并设置了初始值Hello Vue!

4. 将数据绑定到页面元素（视图层）

   说明：只需要在绑定的元素中使用 双花括号 将 Vue 创建的名为 message 属性包裹起来，即可实现数据绑定功能，也就实现了 ViewModel 层所需的效果，是不是和 EL 表达式非常像？

测试：

为了能够更直观的体验 Vue 带来的数据绑定功能，我们需要在浏览器测试一番，操作流程如下：

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240509173504720-1405278231.png" alt="image-20240509173506045" width="200" />

1. 在浏览器上运行第一个 Vue 应用程序，进入 开发者工具
2. 在控制台输入 vm.message = 'Hello World' ，然后 回车，你会发现浏览器中显示的内容会直接变成 Hello World

此时就可以在控制台直接输入 vm.message 来修改值，中间是可以省略 data 的，在这个操作中，我并没有主动操作 DOM，就让页面的内容发生了变化，这就是借助了 Vue 的 数据绑定 功能实现的；MVVM模式中要求 ViewModel 层就是使用 观察者模式 来实现数据的监听与绑定，以做到数据与视图的快速响应

# Vue

## 七大属性

1. el属性

2. - 用来指示vue编译器从什么地方开始解析 vue的语法，可以说是一个占位符。

3. data属性

4. - 用来组织从view中抽象出来的属性，可以说将视图的数据抽象出来存放在data中。

5. template属性

6. - 用来设置模板，会替换页面元素，包括占位符。

7. methods属性

8. - 放置页面中的业务逻辑，js方法一般都放置在methods中

9. render属性

10. - 创建真正的Virtual Dom

11. computed属性

12. - 用来计算

13. watch属性

14. - watch:function(new,old){}
    - 监听data中数据的变化
    - 两个参数，一个返回新值，一个返回旧值，

## 八个方法

初始化显示

- *beforeCreate()
- *created()
- *beforeMount() 挂载前
- *mounted() 挂载后

更新状态:this.xxx=value

- *beforeUpdate()
- *updated()

销毁 vue 实例:vm.$destory()

- *beforeDestory()
- *destoryed()

## 七个指令

1. v-if 

   条件渲染指令根据其后表达式的bool值进行判断是否渲染该元素

2. v-show

   与v-if类似，只是会渲染其身后表达式为false的元素，而且会给这样的元素添加css代码：style=“display:none”

3. v-else

   必须跟在v-if/v-show指令之后，不然不起作用；如果v-if/v-show指令的表达式为true，则else元素不显示；如果v-if/v-show指令的表达式为false，则else元素会显示在页面上

4. v-for

   类似JS的遍历，用法为 v-for=“item in items”, items是数组，item为数组中的数组元素

5. v-bind

   这个指令用于响应地更新 HTML 特性，比如绑定某个class元素或元素的style样式，它的语法糖为 ‘：’

6. v-on

   用于监听指定元素的DOM事件，比如点击事件，它的语法糖为 ‘@’ （诶哟不错哦）

7. v-model

   用于表单元素，进行双向数据绑定


# 基础语法

我们对于基础语法，说白了就是实现元素赋值，循环，判断，以及事件响应即可！

## v-bind ：

我们已经成功创建了第一个 Vue 应用！看起来这跟渲染一个字符串模板非常类似，但是 Vue 在背后做了大量工作。现在数据和 DOM 已经被建立了关联，所有东西都是响应式的。我们在控制台操作对象属性，界面可以实时更新！

我们还可以使用 v-bind 来绑定元素特性!

```html
<div id="app">
    <span v-bind:title="message">悬浮查看title绑定的message值</span>
</div>
```

你看到的 v-bind 特性被称为指令。指令带有前缀 v- ，以表示它们是 Vue 提供的特殊特性。他们会在渲染的DOM上应用特殊的响应式行为。在这里，他的意思是：将这个元素的title属性和Vue实例的message属性保持一致。

除了使用插值表达式{{}}进行数据渲染，也可以使用 v-bind指令，它的简写的形式就是一个冒号（:）

## v-if 系列

什么是条件判断语句，就不需要我说明了吧（￣▽￣）,以下两个属性！

- v-if
- v-else-if 
- v-else

```html
<body>
    <!--view层 模版-->
    <div id="app">
        <h1 v-if="ok">Yes</h1>
        <h1 v-else>No</h1>
    </div>

<!--1.导入vue.js-->
    <script src=" https://cdn.jsdelivr.net/npm/vue@2/dist/vue.min.js "></script>
    <script>
        var vm = new Vue({
            el: "#app",
            data: {
                ok: true
            }
        });
    </script>
</body>
```

测试：观察在控制台输入 vm.type = 'B'、'C'、'D' 的变化

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240509180040467-1753338462.png" alt="image-20240509180042843" width="100" />

```html
<!--view层 模版-->
<div id="app">
    <h1 v-if="type==='A'">A</h1>
    <h1 v-else-if="type==='B'">B</h1>
    <h1 v-else>c</h1>
</div>

<!--1.导入vue.js-->
<script src=" https://cdn.jsdelivr.net/npm/vue@2/dist/vue.min.js "></script>
<script>
    var vm = new Vue({
        el: "#app",
        data: {
            type: 'A'
        }
    });
</script>
```

## v-for

语法格式：

```html
<div id="app">
    <li v-for="item in items">
        {{item.message}}
    </li>
</div>
```

注：items 是数组，item是数组元素迭代的别名。和Thymeleaf模板引擎的语法和这个十分的相似！

代码：03-v-for.html

```html
<!--view层 模版-->
<div id="app">
    <li v-for="item in items">
        {{item.message}}
    </li>
</div>

<!--1.导入vue.js-->
<script src=" https://cdn.jsdelivr.net/npm/vue@2/dist/vue.min.js "></script>
<script>
    var vm = new Vue({
        el: "#app",
        data: {
            items: [
                {message: '消息1'},
                {message: '消息2'}
            ]
        }
    });
</script>
```

测试 ：在控制台输入 vm.items.push({message: '狂神说运维'}) ，尝试追加一条数据，你会发现浏览器中显示的内容会增加一条 狂神说运维 .

加上index下标

```html
<div id="app">
    <li v-for="(item,index) in items">
        {{item.message}}--{{index}}
    </li>
</div>
```

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240509181008259-2089614919.png" alt="image-20240509181010513" width="200" />

## v-on监听事件 @

v-on 监听事件：事件有Vue的事件、和前端页面本身的一些事件！

我们这 click 是vue的事件，可以绑定到Vue中的methods 中的方法事件！

代码：04-v-on.html

```html
<!--view层 模版-->
<div id="app">
    <!--点击触发事件-->
    <button v-on:click="sayHi">click me to sayHi</button>
</div>

<!--1.导入vue.js-->
<script src=" https://cdn.jsdelivr.net/npm/vue@2/dist/vue.min.js "></script>
<script>
    var vm = new Vue({
        el: "#app",
        data: {
            message: "Hi"
        },
        methods: {//方法必须定义在Vue的Methods对象中
            sayHi: function () {
                alert(this.message)
            }
        }
    });
</script>
```

## v-model双向数据绑定

### 什么是双向数据绑定

Vue.js 是一个 MVVM 框架，即数据双向绑定，即当数据发生变化的时候，视图也就发生变化，当视图发生变化的时候，数据也会跟着同步变化。这也算是 Vue.js 的精髓之处了。

值得注意的是，我们所说的数据双向绑定，一定是对于 UI 控件来说的，非 UI 控件不会涉及到数据双向绑定。单向数据绑定是使用状态管理工具的前提，如果我们使用Vuex，那么数据流也是单向的，这时就会和双向数据绑定有冲突。

就像是在输入框中输入数据，下面的元素值显示出来

### 为什么要实现数据的双向绑定

在Vue.js 中，如果使用vuex ，实际上数据还是单向的，之所以说是数据双向绑定，这是用的UI控件来说，对于我们处理表单，Vue.js的双向数据绑定用起来就特别舒服了。

即两者并不互斥，在全局性数据流使用单项，方便跟踪；局部性数据流使用双向，简单易操作。

### 在表单中使用双向数据绑定

你可以用`v-model`指令在表单input、testarea、select元素上创建双向数据绑定。

它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇，但v-model本质上不过是语法糖。它负责监听户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。

 注意：

- v-model会忽略所有元素的value、checked、selected特性的初始值
- 而总是将Vue实例的数据作为数据来源，
- 你应该通过JavaScript在组件的data选项中声明

### 测试代码

文本框

```html
<!--view层 模版-->
<div id="app">
    输入的文本：<input type="text" v-model="message">{{message}}
</div>

<!--1.导入vue.js-->
<script src=" https://cdn.jsdelivr.net/npm/vue@2/dist/vue.min.js "></script>
<script>
    var vm = new Vue({
        el: "#app",
        data: {
            message: ""
        }
    });
</script>
```

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240509183715558-663988324.png" alt="image-20240509183717631" width="200" />

文本域

```html
<textarea v-model="message"></textarea>{{message}}
```

单选按钮

```html
<!--view层 模版-->
<div id="app">
    <!--    输入的文本：<input type="text" v-model="message">{{message}}-->
    <!--    <textarea v-model="message"></textarea>{{message}}-->
    性别：
    <input type="radio" name="sex" value="男" v-model="zhangsan">男
    <input type="radio" name="sex" value="女" v-model="zhangsan">女
    <p>选中了谁：{{zhangsan}}</p>
</div>

<!--1.导入vue.js-->
<script src=" https://cdn.jsdelivr.net/npm/vue@2/dist/vue.min.js "></script>
<script>
    var vm = new Vue({
        el: "#app",
        data: {
            zhangsan: '',
            checked:false	//也可以绑定默认值 v-model="checked"
        }
    });
</script>
```

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240509184442099-1402045725.png" alt="image-20240509184444475" width="200" />

单复选框

```html
<div id="app">
    单复选框：
    <input type="checkbox" id="checkbox" v-model="checked">
    &nbsp;&nbsp;
    <label for="checkbox">{{ checked }}</label>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.js">
</script>
<script>
    new Vue({
        el: '#app',
        data: {
            checked: false
        }
    })
</script>
```

多复选框

```html
<body>
<div id="app">
    多复选框：
    <input type="checkbox" id="jack" value="Jack" vmodel="checkedNames">
    <label for="jack">Jack</label>
    <input type="checkbox" id="john" value="John" vmodel="checkedNames">
    <label for="john">John</label>
    <input type="checkbox" id="mike" value="Mike" vmodel="checkedNames">
    <label for="mike">Mike</label>
    <span>选中的值: {{ checkedNames }}</span>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.js">
</script>
<script>
    new Vue({
        el: '#app',
        data: {
            checkedNames: []
        }
    })
</script>
</body>
```

下拉框

```html
<div id="app">
    下拉框：
    <select v-model="selected">
        <option value="" disabled>----请选择---</option>
        <option>A</option>
        <option>B</option>
        <option>C</option>
    </select>
    <span>value:{{selected}}</span>
</div>

<!--1.导入vue.js-->
<script src=" https://cdn.jsdelivr.net/npm/vue@2/dist/vue.min.js "></script>
<script>
    var vm = new Vue({
        el: "#app",
        data: {
            selected: ''
        }
    });
</script>
```

注意：

- 如果 v-model 表达式的初始值未能匹配任何选项，< select> 元素将被渲染为“未选中”状态。
- 在 iOS 中，这会使用户无法选择第一个选项。因为这样的情况下，iOS 不会触发 change 事件。
- 因此，更推荐像上面这样提供一个值为空的禁用选项

# 组件

## 什么是组件

组件是`可复用的 Vue 实例`，说白了就是一组可以重复使用的模板，跟 JSTL 的自定义标签、Thymeleaf 的 th:fragment 等框架有着异曲同工之妙。

通常一个应用会以一棵嵌套的组件树的形式来组织

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240508203306131-708267895.png" alt="image-20240508203307957" width="500" />

例如，你可能会有页头、侧边栏、内容区等组件，每个组件又包含了其它的像导航链接、博文之类的组件。

## 第一个 Vue 组件

注意：在实际开发中，我们并不会用以下方式开发组件，以下方法只是为了让大家理解什么是组件。

使用 Vue.component() 方法注册组件：

```html
<!--view层 模版-->
<div id="app">
    <zujian></zujian>
</div>

<!--1.导入vue.js-->
<script src=" https://cdn.jsdelivr.net/npm/vue@2/dist/vue.min.js "></script>
<script>
    //定义一个Vue组件 component
    Vue.component("zujian", {
        template: '<li>hello</li>'
    });

    var vm = new Vue({
        el: "#app",
        data: {
            selected: ''
        }
    });
</script>
```

说明：

- Vue.component()：注册组件
- zujian：自定义组件的名字
- template：组件的模板

## 使用 props 属性传递参数

像上面那样用组件没有任何意义，所以我们是需要传递参数到组件的，此时就需要使用 props 属性了！

注意：默认规则下 props 属性里的值不能为大写：

```html
<!--view层 模版-->
<div id="app">
    <!--组件：循环 遍历传递组件中的值 
    通过绑定，把items的每一项给组件的参数props-->
    <zujian v-for="item in items" v-bind:jieshou="item"></zujian>
</div>

<!--1.导入vue.js-->
<script src=" https://cdn.jsdelivr.net/npm/vue@2/dist/vue.min.js "></script>
<script>
    //定义一个Vue组件 component
    Vue.component("zujian", {
        //props接收值
        props: ['jieshou'],
        template: '<li>{{jieshou}}</li>'
    });

    var vm = new Vue({
        el: "#app",
        data: {
            items: ["组件一", "组件2", "组件3"]
        }
    });
</script>
```

说明：

- v-for="item in items" ：遍历 Vue 实例中定义的名为 items 的数组，并创建同等数量的组件；
- v-bind:item="item" ：将遍历的 item 项绑定到组件中 props 定义的名为 item 属性上；
- = 号左边的 item 为 props 定义的属性名，右边的为 item in items 中遍历的 item 项的值；

# Axios异步通信

## 什么是Axios

Axios 是一个开源的可以用在浏览器端和 NodeJS 的异步通信框架，她的主要作用就是实现 A JAX 异步通信。

其功能特性如下：

- 从浏览器创建 [XMLHttpRequests](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
- 从 node.js 创建 [http](http://nodejs.org/api/http.html) 请求
- 支持 [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) API
- 拦截请求和响应
- 转换请求和响应数据
- 取消请求
- 超时处理
- 查询参数序列化支持嵌套项处理
- 自动将请求体序列化为：
  - JSON (`application/json`)
  - Multipart / FormData (`multipart/form-data`)
  - URL encoded form (`application/x-www-form-urlencoded`)
- 将 HTML Form 转换成 JSON 进行请求
- 自动转换JSON数据
- 获取浏览器和 node.js 的请求进度，并提供额外的信息（速度、剩余时间）
- 为 node.js 设置带宽限制
- 兼容符合规范的 FormData 和 Blob（包括 node.js）
- 客户端支持防御[XSRF]

GitHub：https://github.com/axios/axios

中文文档：https://www.axios-http.cn/

## 为什么要使用 Axios

由于 Vue.js 是一个 视图层框架 并且作者（尤雨溪）严格准守 SoC （关注度分离原则），所以Vue.js 并不包含 A JAX 的通信功能，为了解决通信问题，作者单独开发了一个名为 vue-resource 的插件，不过在进入 2.0 版本以后停止了对该插件的维护并推荐了 Axios 框架。

少用jQuery，因为它操作Dom太频繁！

## 第一个 Axios 应用程序

咱们开发的接口大部分都是采用 JSON 格式，可以先在项目里模拟一段 JSON 数据，数据内容如下：创建一个名为 data.json 的文件并填入上面的内容，放在项目的根目录下。

```json
{
  "name": "张三",
  "url": "https://blog.kuangstudy.com",
  "page": 1,
  "isNonProfit": true,
  "address": {
    "street": "洪崖洞",
    "city": "重庆市",
    "country": "中国"
  },
  "links": [
    {
      "name": "王5",
      "url": "https://blog.kuangstudy.com"
    },
    {
      "name": "王6",
      "url": "https://blog.kuangstudy.com"
    },
    {
      "name": "王7",
      "url": "https://blog.kuangstudy.com"
    }
  ]
}
```

测试代码

```html
<!DOCTYPE html>
<html lang="en" xmlns:v-bind="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--解决闪烁问题 没有加载之前白屏-->
    <style>
        [v-clock] {
            display: none;
        }
    </style>
</head>
<body>
<div id="vue" v-clock>
    <div>
        info.name：{{info.name}}
        <br>
        info：{{info}}
    </div>
    <div>
        <a v-bind:href="info.url">点击</a>
    </div>
</div>

<!--引入 JS 文件-->
<script src=" https://cdn.jsdelivr.net/npm/vue@2/dist/vue.min.js "></script>
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
<script type="text/javascript">
    var vm = new Vue({
        el: '#vue',
        //data:属性 vm
        data() {
            //return的是格式
            return {
                //请求的返回参数格式 必须和JSON字符串一样
                info: {
                    name: null,
                    address: {
                        street: null,
                        city: null,
                        country: null
                    },
                    url: null
                }
            }
        },
        mounted() {//钩子函数 链式编程 基于ES6的新特性
            axios.get('../data.json').then(response => (this.info = response.data));
        }
    });
</script>
</body>
</html>
```

说明 :

1. 在这里使用了 v-bind 将 a:href 的属性值与 Vue 实例中的数据进行绑定
2. 使用 axios 框架的 get 方法请求 A JAX 并自动将数据封装进了 Vue 实例的数据对象中
3. 我们在data中的数据结构必须要和 Ajax 响应回来的数据格式匹配！

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240509201548655-1317633907.png" alt="image-20240509201550470" width="500" />

## Vue的生命周期

官方文档：https://cn.vuejs.org/v2/guide/instance.html#生命周期图示

​		Vue 实例有一个完整的生命周期，也就是从开始创建、初始化数据、编译模板、挂载 DOM、渲染→更新→渲染、卸载等一系列过程，我们称这是 Vue 的生命周期。通俗说就是 Vue 实例从创建到销毁的过程，就是生命周期。

​		在 Vue 的整个生命周期中，它提供了一系列的事件，可以让我们在事件触发时注册 JS 方法，可以让我们用自己注册的 JS 方法控制整个大局，在这些事件响应方法中的 this 直接指向的是 Vue 的实例。

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240508204328359-1109219367.png" alt="image-20240508204330097" width="500" />

# computed计算属性

​	计算属性的重点突出在 `属性` 两个字上（属性是名词）。首先它是个 属性，其次这个属性有 计算 的能力（计算是动词），这里的 `计算` 就是个函数。

简单点说，它就是一个能够将计算结果缓存起来的属性（将行为转化成了静态的属性），仅此而已；可以想象为缓存！

计算属性：计算出来的结果，保存在属性中，在内存中运行：虚拟DOM

```html
<div id="vue">
    <!--currentTime1() : 1715257311137-->
    <p>currentTime1() : {{currentTime1()}}</p>
    <!--currentTime1 : function () { [native code] }-->
    <p>currentTime1 : {{currentTime1}}</p>

    <!--TypeError: currentTime2 is not a function-->
    <p>currentTime2() : {{currentTime2()}}</p>
    <!--currentTime2 : 1715257610663-->
    <p>currentTime2 : {{currentTime2}}</p>
</div>

<!--引入 JS 文件-->
<script src=" https://cdn.jsdelivr.net/npm/vue@2/dist/vue.min.js "></script>
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
<script type="text/javascript">
    var vm = new Vue({
        el: '#vue',
        data: {
            message: "hello,vue"
        },
        methods: {
            currentTime1: function () {
                return Date.now();//返回一个时间戳
            }
        },
        computed: {//计算属性：methods和computed里的方法不能重名 重名之后，只会调用methods中的方法
            currentTime2: function () {
                return Date.now();//返回一个时间戳
            }
        }
    });
</script>
```

注意：methods 和 computed 里的东西不能重名

说明：

- methods：定义方法，调用方法使用 currentTime1()，需要带括号；
- computed：定义计算属性，调用属性使用 currentTime2，不需要带括号；this.message 是为了能够让 currentTime2 观察到数据变化而变化；

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240509203032491-891078355.png" alt="image-20240509203034829" width="200" />

- 如果在方法中的值发生了变化，则缓存就会刷新（类似于Mybatis的CRUD后一级缓存会失效）！方法外的值修改，缓存则不会刷新。

结论：

- 调用方法时，每次都需要进行计算，既然有计算过程则必定产生系统开销，那如果这个结果是不经常变化的呢？此时就可以考虑将这个结果缓存起来，采用计算属性可以很方便的做到这一点
- 计算属性的主要特性就是为了将不经常变化的计算结果进行缓存，以节约我们的系统开销;

# slot插槽-内容分发

在 Vue 中我们使用 <slot 元素，作为承载分发内容的出口，作者称其为 插槽，可以应用在组合组件的场景中;

测试

比如准备制作一个待办事项组件（todo），该组件由待办标题（todo-title）和待办内容（todo-items）组成，但这三个组件又是相互独立的，该如何操作呢？

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240509223704940-752107817.png" alt="image-20240509223707323" width="150" />

```html
<!--view层 模拟-->
<div id="vue">
    <!--6.将这些值,通过插槽插入-->
    <todo>
        <todo-title slot="todo-title" :title="title"></todo-title>
        <todo-items slot="todo-items" v-for="item in todoItems" :item="item"></todo-items>
    </todo>
</div>

<!--引入 Vue.js 文件-->
<script src=" https://cdn.jsdelivr.net/npm/vue@2/dist/vue.min.js "></script>
<script>
    //1.定义一个组件todo
    Vue.component("todo", {
        //2.留出 插槽slot
        template:
            '<div>\
                <slot name="todo-title"></slot>\
                <ul>\
                    <slot name="todo-items"></slot>\
                </ul>\
            </div>'
    });
    //3.定义一个名为 todo-title 的待办标题组件
    Vue.component("todo-title", {
        props: ["title"],
        template: '<div>{{title}}</div>'
    });
    //4.定义一个名todo-items 的待办内容组件
    Vue.component("todo-items", {
        props: ["item"],
        template: '<li>{{item}}</li>'
    });
    //5.实例化 Vue 并初始化数据
    var vm = new Vue({
        el: '#vue',
        data: {
            title: "我是标题",
            todoItems: ["我是一号", "我是2号", "我是3号"]
        }
    });
</script>
```

说明:

- 我们的 todo-title 和 todo-items 组件分别被分发到了 todo 组件的 todo-title 和 todo-items 插槽中

# 自定义事件

通过以上代码不难发现，数据项在 Vue 的实例中，但删除操作要在组件中完成，那么组件如何才能删除Vue 实例中的数据呢？

此时就涉及到参数传递与事件分发了，Vue 为我们提供了自定义事件的功能很好的帮助我们解决了这个问题；使用 this.$emit('自定义事件名', 参数)。

## 测试

```html
<!--view层 模拟-->
<div id="vue">
    <todo>
        <todo-title slot="todo-title" :title="title"></todo-title>
        <!--3.-->
        <todo-items slot="todo-items" v-for="(item,index) in todoItems"
                    :item="item" :index="index"
                    @rremove="removeItems(index)"></todo-items>
    </todo>
</div>

<!--引入 Vue.js 文件-->
<script src=" https://cdn.jsdelivr.net/npm/vue@2/dist/vue.min.js "></script>
<script>
    //定义一个组件todo
    Vue.component("todo", {
        //插槽slot
        template:
            '<div>\
                <slot name="todo-title"></slot>\
                <ul>\
                    <slot name="todo-items"></slot>\
                </ul>\
            </div>'
    });
    Vue.component("todo-title", {
        props: ["title"],
        template: '<div>{{title}}</div>'
    });
    Vue.component("todo-items", {
        props: ["item", "index"],
        //1.只能绑定当前组件的方法
        template: '<li>{{index}} --- {{item}}  <button @click="remove">删除</button></li>',
        methods: {
            remove: function (index) {
                //4.this.$emit自定义事件分发(名字，参数)
                this.$emit('rremove', index);
            }
        }
    });
    var vm = new Vue({
        el: '#vue',
        data: {
            title: "我是标题",
            todoItems: ["我是一号", "我是2号", "我是3号"]
        },
        //2.定义方法
        methods: {
            removeItems: function (index) {
                console.log("删除了：" + this.todoItems[index] + " OK");
                this.todoItems.splice(index, 1);//一次删除一个元素
            }
        }
    });
</script>
```

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240509230819022-952417136.png" alt="image-20240509230821035" width="200" />

## 逻辑理解  

- 前端可以直接调用Vue实例对象中的方法methods [removeItems(index)]
- 前端可以绑定组件属性(items,index)，可以监听事件(rremove)
- 前端可以循环遍历Vue实例对象中的data数据(todoItems)
- 组件可以自定义事件$emit(rremove)

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240508203844836-2033937132.png" alt="image-20240508203847010" width="500" />

# Vue 入门小结  

核心 : 数据驱动 , 组件化

优点 : 借鉴了 AngulaJS 的模块化开发 和 React 的虚拟Dom , 虚拟Dom就是把Dom操作放到内存中执行;

常用的属性 :

- v-if
- v-else-if 
- v-else 
- v-for
- v-on 绑定事件 , 简写 @
- v-model 数据双向绑定
- v-bind 给组件绑定参数,简写 :

组件化 :

- 组合组件， slot 插槽。
- 组件内部绑定事件需要使用到 this.$emit("事件名",参数) ;
- 计算属性的特色,缓存计算数据

网络通信：axios

- 遵循SoC 关注度分离原则，Vue是纯粹的视图框架，并不包含比如Ajax之类的通信功能
- 为了解决通信问题，我们需要使用Axios 框架做异步通信。

实际开发

- 基于NodeJS
- 采用vue-cli脚手架开发
- vue-router路由
- vuex做状态管理
- VueUI 界面用ElementUI或者ICE

# Vue-Cli

## 什么是vue-cli

vue-cli 官方提供的一个脚手架,用于快速生成一个 vue 的项目模板 ;

预先定义好的目录结构及基础代码，就好比咱们在创建 Maven 项目时可以选择创建一个骨架项目，这个骨架项目就是脚手架,我们的开发更加的快速;

主要的功能 :

- 统一的目录结构
- 本地调试
- 热部署
- 单元测试
- 集成打包上线

## 需要的环境

### Node.js

下载：

- 下载/官网：https://nodejs.org/en/
- 中文网：https://nodejs.cn/en
- LTS：长期支持版本
- Current：最新版

安装：无脑下一步

- 软件已经自动配置了环境变量
- 如果移动文件夹，请手动修改环境变量

查看版本

```bash
node -v

C:\Users\32354>node -v
v20.12.0

# 由于新版的nodejs已经集成了npm，所以之前npm也一并安装好了。同样可以使用cmd命令行输入“npm -v”来测试是否安装成功。
npm -v

C:\Users\32354>npm -v
10.5.0

# 安装淘宝镜像，防止下载较慢
npm install cnpm -g #少用
#全局安装 位置 C:\Users\32354\AppData\Roaming\npm\node_modules

npm config get registry #查看镜像地址
https://registry.npmmirror.com #默认官方

#解除镜像并恢复到官方源，请执行以下命令：
npm config set registry https://registry.npmjs.org
```

### Git

基本自带的有

### 安装 vue-cli

```bash
cnpm install vue-cli -g
#全局安装 位置 C:\Users\32354\AppData\Roaming\npm\node_modules

# 测试是否安装成功

# 查看可以基于哪些模板创建 vue 应用程序，通常我们选择 webpack
#Vue基于ES6 webapck可以打包降级
vue list
```

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240510091509498-365602371.png" alt="image-20240510091512626" width="500" />

## 第一个 vue-cli 应用程序

1. 我们新建一个文件夹 vue-cli

2. 创建一个基于 webpack 模板的 vue 应用程序

   ```bash
   # 这里的 myvue 是项目名称，可以根据自己的需求起名
   vue init webpack myvue
   # 一路都选择no即可;
   ```

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240510092404010-203176492.png" alt="image-20240510092407218" width="500" />

说明 :

- Project name：项目名称，默认 回车 即可
- Project description：项目描述，默认 回车 即可
- Author：项目作者，默认 回车 即可
- VueBuild：构建。选择Runtime + Compiler: recommended for most users，运行时编译standalone
- Install vue-router：是否安装 
- vue-router，选择 n 不安装（后期需要再手动添加）
- Use ESLint to lint your code：是否使用 ESLint 做代码检查，选择 n 不安装（后期需要再手动添加）
- Set up unit tests：单元测试相关，选择 n 不安装（后期需要再手动添加）
- Setup e2e tests with Nightwatch：单元测试相关，选择 n 不安装（后期需要再手动添加） 
- Should we run npm install for you after the project has been created：创建完成后直接初始化，选择 n，我们手动执行;运行结果

## 初始化并运行

在终端依次输入

```bash
cd myvue	#进入目录
npm install	#安装依赖[根据package.json]
npm run dev	#运行启动命令
Ctrl+C #停止运行
```

- 执行完成后,目录多了很多依赖

- 安装并运行成功后在浏览器输入：http://localhost:8080

效果

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240510093048856-320705899.png" alt="image-20240510093051542" width="500" />

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240510093144572-757004702.png" alt="image-20240510093146890" width="500" />

## Vue-cli目录结构

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240510095104399-740825528.png" alt="image-20240510095107172" width="200" />

- build 和 config：WebPack 配置文件
- node_modules：用于存放 npm install 安装的依赖文件
- src： 项目源码目录
- static：静态资源文件
- .babelrc：Babel 配置文件，主要作用是将 ES6 转换为 ES5 
- .editorconfig：编辑器配置eslintignore：需要忽略的语法检查配置文件
- .gitignore：git 忽略的配置文件
- .postcssrc.js：css 相关配置文件，其中内部的 module.exports 是 NodeJS 模块化语法
- index.html：首页，仅作为模板页，实际开发时不使用
- package.json：项目的配置文件
  - name：项目名称
  - version：项目版本
  - description：项目描述
  - author：项目作者
  - scripts：封装常用命令
  - dependencies：生产环境依赖
  - devDependencies：开发环境依赖

## src 目录

src 目录是项目的源码目录，所有代码都会写在这里！

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240510094426391-584285082.png" alt="image-20240510094429261" width="200" />

### main.js

项目的入口文件，我们知道所有的程序都会有一个入口

```js
import Vue from 'vue'
import App from './App'

Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
  components: { App },
  template: '<App/>'
})
```

- import Vue from 'vue' ：ES6 写法，会被转换成 require("vue"); （require 是 NodeJS 提供的模块加载器）
- import App from './App' ：意思同上，但是指定了查找路径，./ 为当前目录
- Vue.config.productionTip = false ：关闭浏览器控制台关于环境的相关提示
- new Vue({...}) ：实例化 Vue
  - el: '#app' ：查找 index.html 中 id 为 app 的元素
  - template: '' ：模板，会将 index.html 中替换为
  - components: { App } ：引入组件，使用的是 import App from './App' 定义的 App 组件

### App.vue

```vue
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <HelloWorld/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

- template：HTML 代码模板，会替换 < App /> 中的内容
- import HelloWorld from './components/HelloWorld'：引入 HelloWorld 组件用于替换 template中的 < HelloWorld/>
- export default{...}：导出 NodeJS 对象，作用是可以通过 import 关键字导入
  - name: 'App'：定义组件的名称
  - components: { HelloWorld }：定义子组件
- 在helloWorld.Vue中,关于 < style scoped> 的说明：CSS 样式仅在当前组件有效，声明了样式的作用域,是当前的界面私有的!

# Webpack

## 什么是Webpack

​		本质上， webpack是一个`现代JavaScript应用程序的静态模块打包器`(module bundler) 。当webpack处理应用程序时， 它会递归地构建一个依赖关系图(dependency graph) ， 其中包含应用程序需要的每个模块， 然后将所有这些模块打包成一个或多个bundle.

  Webpack是当下最热门的前端资源模块化管理和打包工具， 它可以将许多松散耦合的模块按照依赖和规则打包成符合生产环境部署的前端资源。还可以将按需加载的模块进行代码分离，等到实际需要时再异步加载。通过loader转换， 任何形式的资源都可以当做模块， 比如Commons JS、AMD、ES 6、CSS、JSON、Coffee Script、LESS等；

  伴随着移动互联网的大潮， 当今越来越多的网站已经从网页模式进化到了WebApp模式。它们运行在现代浏览器里， 使用HTML 5、CSS 3、ES 6等新的技术来开发丰富的功能， 网页已经不仅仅是完成浏览器的基本需求； WebApp通常是一个SPA(单页面应用) ， 每一个视图通过异步的方式加载，这导致页面初始化和使用过程中会加载越来越多的JS代码，这给前端的开发流程和资源组织带来了巨大挑战。

  前端开发和其他开发工作的主要区别，首先是前端基于多语言、多层次的编码和组织工作，其次前端产品的交付是基于浏览器的，这些资源是通过增量加载的方式运行到浏览器端，如何在开发环境组织好这些碎片化的代码和资源，并且保证他们在浏览器端快速、优雅的加载和更新，就需要一个模块化系统，这个理想中的模块化系统是前端工程师多年来一直探索的难题。

## 模块化的演进

### Script标签

```html
<script src = "module1.js"></script>
<script src = "module2.js"></script>
<script src = "module3.js"></script>
```

这是最原始的JavaScript文件加载方式，如果把每一个文件看做是一个模块，那么他们的接口通常是暴露在全局作用域下，也就是定义在window对象中，不同模块的调用都是一个作用域。
  这种原始的加载方式暴露了一些显而易见的弊端：

- 全局作用域下容易造成变量冲突
- 文件只能按照script>的书写顺序进行加载
- 开发人员必须主观解决模块和代码库的依赖关系
- 在大型项目中各种资源难以管理，长期积累的问题导致代码库混乱不堪

### CommonsJS

服务器端的NodeJS遵循CommonsJS规范，该规范核心思想是允许模块通过require方法来同步加载所需依赖的其它模块，然后通过exports或module.exports来导出需要暴露的接口。

```js
require("module");
require("../module.js");

export.doStuff = function(){};
module.exports = someValue;
```

优点：

- 服务器端模块便于重用
- NPM中已经有超过45万个可以使用的模块包
- 简单易用

缺点：

- 同步的模块加载方式不适合在浏览器环境中，同步意味着阻塞加载，浏览器资源是异步加载的
- 不能非阻塞的并行加载多个模块

实现：

- 服务端的NodeJS
- Browserify，浏览器端的CommonsJS实现，可以使用NPM的模块，但是编译打包后的文件体积较大
- modules-webmake，类似Browserify，但不如Browserify灵活
- wreq，Browserify的前身

### AMD

Asynchronous Module Definition规范其实主要一个主要接口define(id?,dependencies?,factory)；它要在声明模块的时候指定所有的依赖dependencies，并且还要当做形参传到factory中，对于依赖的模块提前执行。

```js
define("module",["dep1","dep2"],functian(d1,d2){
	return someExportedValue;
});
require（["module","../file.js"],function(module，file){});
```

优点

- 适合在浏览器环境中异步加载模块
- 可以并行加载多个模块

缺点

- 提高了开发成本，代码的阅读和书写比较困难，模块定义方式的语义不畅
- 不符合通用的模块化思维方式，是一种妥协的实现

实现

- RequireJS
- curl

### CMD

Commons Module Definition规范和AMD很相似，尽保持简单，并与CommonsJS和NodeJS的Modules规范保持了很大的兼容性。

```js
define(function(require,exports,module){
	var $=require("jquery");
	var Spinning = require("./spinning");
	exports.doSomething = ...;
	module.exports=...;
});
```

优点：

- 依赖就近，延迟执行
- 可以很容易在NodeJS中运行缺点
- 依赖SPM打包，模块的加载逻辑偏重

实现

- Sea.js
- coolie

### ES6模块

EcmaScript 6标准增加了JavaScript语言层面的模块体系定义。

ES 6模块的设计思想`是尽量静态化`， 使编译时就能确定模块的依赖关系， 以及输入和输出的变量。Commons JS和AMD模块，都只能在运行时确定这些东西。

```js
import "jquery"
export function doStuff(){}
module "localModule"{}
```

优点

- 容易进行静态分析
- 面向未来的Ecma Script标准

缺点

- 原生浏览器端还没有实现该标准
- 全新的命令，新版的Node JS才支持

实现

- Babel

### 大家期望的模块

- 系统可以兼容多种模块风格
- 尽量可以利用已有的代码
- 不仅仅只是JavaScript模块化，还有CSS、图片、字体等资源也需要模块化

## 安装Webpack

​	WebPack是一款模块加载器兼打包工具， 它能把各种资源， 如JS、JSX、ES 6、SASS、LESS、图片等都作为模块来处理和使用。

### 安装

```bash
npm install webpack -g
npm install webpack-cli -g
```

测试安装成功

```bash
webpack -v
#出现的是电脑信息

C:\Users\32354>npm info webpack version
#5.91.0

webpack-cli -v
#出现的是电脑信息

C:\Users\32354>npm info webpack-cli version
#5.1.4
```

### 配置

创建 webpack.config.js配置文件

```js
module.exports = {
	entry:"",
	output:{
		path:"",
		filename:""
	},
	module:{
		loaders:[
			{test:/\.js$/,;\loade:""}
		]
	},
	plugins:{},
	resolve:{},
	watch:true
}
```

- entry：入口文件， 指定Web Pack用哪个文件作为项目的入口
- output：输出， 指定WebPack把处理完成的文件放置到指定路径
- module：模块， 用于处理各种类型的文件
  - rules : 规则
- plugins：插件， 如：热更新、代码重用等
- resolve：设置路径指向
- watch：监听， 用于设置文件改动后直接打包

直接运行webpack命令打包

## 使用webpack

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240510104401944-1289830140.png" alt="image-20240510104405189" width="200" />

1. 创建项目

2. 创建一个名为modules的目录，用于放置JS模块等资源文件

3. 在modules下创建模块文件，如hello.js，用于编写JS模块相关代码

   ```js
   //暴露一个方法
   exports.sayHi=function () {
       document.write("<h1>Hello ES6!</h1>");
   }
   ```

4. 在modules下创建一个名为main.js的入口文件，用于打包时设置entry属性

   ```js
   //接收一个方法
   var hello = require("./hello");
   hello.sayHi();
   ```

5. 在项目目录下创建webpack.config.js配置文件，使用`webpack`命令打包

   ```js
   module.exports = {
       entry: './modules/main.js',
       output: {
           filename: './js/bundle.js'
       },
       mode: 'development'
   };
   ```

6. 在项目目录下创建HTML页面，如index.html，导入webpack打包后的JS文件

   ```html
   <body>
   
   <script src="dist/js/bundle.js"></script>
   
   </body>
   ```

7. 在IDEA控制台中直接执行webpack；如果失败的话，就使用管理员权限运行即可！

8. 运行HTML看效果

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240510104220251-1453975119.png" alt="image-20240510104222884" width="200" />

说明

参数--watch 用于监听变化

```bash
webpack --watch
```

# 路由

## 说明

Vue Router 是 Vue.js 官方的路由管理器。它和 Vue.js 的核心深度集成，让构建单页面应用变得易如反掌。包含的功能有：

- 嵌套的路由/视图表
- 模块化的、基于组件的路由配置
- 路由参数、查询、通配符
- 基于 Vue.js 过渡系统的视图过渡效果
- 细粒度的导航控制
- 带有自动激活的 CSS class 的链接
- HTML5 历史模式或 hash 模式，在 IE9 中自动降级
- 自定义的滚动条行为

Vue.js 路由允许我们通过不同的 URL 访问不同的内容。通过 Vue.js 可以实现多视图的单页Web应用（single page web application，SPA）

## 安装

基于第一个 vue-cli 进行测试学习;先查看node_modules中是否存在 vue-router

vue-router 是一个插件包，所以我们还是需要用 npm/cnpm 来进行安装的。打开命令行工具，进入你的项目目录，输入下面命令。

```bash
cnpm install vue-router --save-dev
#版本太高会报错
cnpm install vue-router@3 --save-dev
```

如果在一个模块化工程中使用它，必须要通过 Vue.use() 明确地安装路由功能：

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

//必须要显示声明使用VueRouter
Vue.use(VueRouter);
```

## 测试

可以先清理掉多余的文件：Src 下面就很干净了

1. components 目录下存放我们自己编写的组件

2. 定义组件 Content.vue 组件

   ```vue
   <template>
     <h1>内容页</h1>
   </template>
   
   <script>
   export default {
     name: 'Content'
   }
   </script>
   
   <style scoped>
   
   </style>
   ```

3. 我们在新建一个 Main.vue 组件

   ```vue
   <template>
     <h1>首页</h1>
   </template>
   
   <script>
   export default {
     name: "main"
   }
   </script>
   
   <style scoped>
   </style>
   ```

4. 安装路由,在src目录下,新建一个文件夹 : router ,专门存放路由，写入 index.js

   ```js
   import Vue from "vue";
   import VueRouter from "vue-router";
   //导入定义好的组件
   import Content from "../components/Content.vue";
   import Main from "../components/Main.vue";
   
   //安装路由
   Vue.use(VueRouter);
   
   //配置导出路由
   export default new VueRouter({
     mode: "history",
     routes: [
       {
         //路由的路径 @RequestMapping
         path: '/content',
         name: 'content',
         //跳转的组件
         component: Content
       },
       {
         //路由的路径
         path: '/main',
         name: 'main',
         //跳转的组件
         component: Main
       }
     ]
   });
   ```

5. 在 main.js 中配置路由

   ```js
   import Vue from 'vue'
   import App from './App'
   //导入上面创建的路由配置目录
   import router from "./router";//自动扫描里面的路由配置
   
   //关闭生产模式下给出的提示
   Vue.config.productionTip = false
   
   new Vue({
     el: '#app',
     //配置路由
     router,
     components: {App},
     template: '<App/>'
   })
   ```

6. 在 App.vue 中使用路由

   ```vue
   <template>
     <div id="app">
       <h1>Vue-Router</h1>
         <!--
   			router-link：默认会被渲染成一个<a>标签，to属性为指定链接
   			router-view：用于渲染路由匹配到的组件
   		-->
       <router-link to="/main">首页</router-link>
       <router-link to="/content">内容页</router-link>
       
       <!--展示template模版-->
       <router-view></router-view>
     </div>
   </template>
   
   <script>
   export default {
     name: 'App'
   }
   </script>
   
   <style>
   #app {
     font-family: 'Avenir', Helvetica, Arial, sans-serif;
     -webkit-font-smoothing: antialiased;
     -moz-osx-font-smoothing: grayscale;
     text-align: center;
     color: #2c3e50;
     margin-top: 60px;
   }
   </style>
   ```

7. 启动测试一下 ： npm run dev

练习： 在现有的基础上，在增加一个路由组件，优化一下  

1. 自定义一个组件sun.vue

   ```vue
   <template>
     <h1>sun</h1>
   </template>
   
   <script>
   export default {
     name: "sun"
   }
   </script>
   
   <style scoped>
   </style>
   ```

2. 配置组件到路由

   ```js
   import Vue from "vue";
   import VueRouter from "vue-router";
   
   import Content from "../components/Content.vue";
   import Main from "../components/Main.vue";
   import sun from "../components/sun.vue";
   
   //安装路由
   Vue.use(VueRouter);
   
   //配置导出路由
   export default new VueRouter({
     mode: "history",
     routes: [
       {
         //路由的路径 @RequestMapping
         path: '/content',
         name: 'content',
         //跳转的组件
         component: Content
       },
       {
         //路由的路径
         path: '/main',
         name: 'main',
         //跳转的组件
         component: Main
       },
       {
         //路由的路径
         path: '/sun',
         //跳转的组件
         component: sun
       }
     ]
   });
   ```

3. 在App.vue中加入跳转链接

   ```vue
   <router-link to="/sun">自定义页</router-link>
   ```

## vue+elementUI

我们采用实战教学模式并结合ElementUI组件库，将所需知识点应用到实际中，以最快速度带领大家掌握Vue的使用；

### 创建工程

注意：命令行都要使用管理员模式运行

1. 创建一个名为hello-vue的工程

   ```bash
   vue init webpack hello-vue
   ```

2. 安装依赖， 我们需要安装vue-router、element-ui、sass-loader和node-sass四个插件

   ```bash
   #进入工程目录
   cd hello-vue
   #安装vue-routern 
   npm install vue-router@3 --save-dev
   #安装element-ui
   npm i element-ui -S	#旧版
   #安装依赖
   npm install
   
   #启功测试
   npm run dev
   #可能需要下载热部署，cnpm下就完了
   ```

3. Npm命令解释：

   - npm install moduleName：安装模块到项目目录下
   - npm install -g moduleName：-g的意思是将模块安装到全局，具体安装到磁盘哪个位置要看npm config prefix的位置
   - npm install -save moduleName：–save的意思是将模块安装到项目目录下， 并在package文件的dependencies节点写入依赖，-S为该命令的缩写
   - npm install -save-dev moduleName:–save-dev的意思是将模块安装到项目目录下，并在package文件的devDependencies节点写入依赖，-D为该命令的缩写

### 创建登录页面

把src没有用的初始化东西删掉！
   在源码目录中创建如下结构：

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240510122625619-1978535931.png" alt="image-20240510122628155" width="200" />

- assets：用于存放资源文件
- components：用于存放Vue功能组件
- views：用于存放Vue视图组件
- router：用于存放vue-router配置

编写代码

1. 创建首页视图，在views目录下创建一个名为Main.vue的视图组件：

   ```vue
   <template>
     <title>首页</title>
   </template>
   
   <script>
   export default {
     name: "Main"
   }
   </script>
   
   <style scoped>
   </style>
   ```

2. 创建登录页视图在views目录下创建名为Login.vue的视图组件，其中el-*的元素为ElementUI组件；

   ```vue
   <template>
     <div>
       <el-form ref="loginForm" :model="form" :rules="rules" label-width="80px" class="login-box">
         <h3 class="login-title">欢迎登录</h3>
         <el-form-item label="账号" prop="username">
           <el-input type="text" placeholder="请输入账号" v-model="form.username"/>
         </el-form-item>
         <el-form-item label="密码" prop="password">
           <el-input type="password" placeholder="请输入密码" v-model="form.password"/>
         </el-form-item>
         <el-form-item>
           <el-button type="primary" v-on:click="onSubmit('loginForm')">登录</el-button>
         </el-form-item>
       </el-form>
   
       <el-dialog title="温馨提示" :visible.sync="dialogVisible" width="30%" :before-close="handleClose">
         <span>请输入账号和密码</span>
         <span slot="footer" class="dialog-footer">
             <el-button type="primary" @click="dialogVisible = false">确定</el-button>
           </span>
       </el-dialog>
     </div>
   </template>
   
   <script>
   export default {
     name: "Login",
     data() {
       return {
         form: {
           username: '',
           password: ''
         },
         //表单验证，需要在 el-form-item 元素中增加prop属性
         rules: {
           username: [
             {required: true, message: "账号不可为空", trigger: "blur"}
           ],
           password: [
             {required: true, message: "密码不可为空", trigger: "blur"}
           ]
         },
   
         //对话框显示和隐藏
         dialogVisible: false
       }
     },
     methods: {
       onSubmit(formName) {
         //为表单绑定验证功能
         this.$refs[formName].validate((valid) => {
           if (valid) {
             //使用vue-router路由到指定界面，该方式称为编程式导航
             this.$router.push('/main');
           } else {
             this.dialogVisible = true;
             return false;
           }
         });
       }
     }
   }
   </script>
   
   <style scoped>
   .login-box {
     border: 1px solid #DCDFE6;
     width: 350px;
     margin: 180px auto;
     padding: 35px 35px 15px 35px;
     border-radius: 5px;
     -webkit-border-radius: 5px;
     -moz-border-radius: 5px;
     box-shadow: 0 0 25px #909399;
   }
   
   .login-title {
     text-align: center;
     margin: 0 auto 40px auto;
     color: #303133;
   }
   </style>
   
   ```

3. 创建路由，在router目录下创建一个名为`index.js`的vue-router路由配置文件

   ```js
   import Vue from "vue";
   import Router from "vue-router";
   
   import Main from "../views/Main.vue";
   import Login from "../views/Login.vue";
   
   Vue.use(Router);
   
   export default new Router({
     routes:[
       {
         path:'/main',
         component:Main
       },
       {
         path: '/login',
         component: Login
       }
     ]
   });
   ```

4. 把路由和Element加到main.js

   ```js
   import Vue from 'vue'
   import App from './App'
   import router from './router'
   
   //引入Element
   import ElementPlus from 'element-plus'
   import 'element-plus/dist/index.css'
   
   Vue.use(router);
   Vue.use(ElementPlus);
   
   new Vue({
     el: '#app',
     router,
     render: h => h(App)
   })
   ```

5. APP.vue：把路由展示到页面上

   ```vue
   <template>
     <div id="app">
       <router-view></router-view>
     </div>
   </template>
   
   <script>
   
   export default {
     name: 'App',
     components: {}
   }
   </script>
   ```

测试：在浏览器打开 http://localhost:8080/#/login

如果出现错误: 

- 可能是因为sass-loader的版本过高导致的编译错误，当前最高版本是8.0.2，需要退回到7.3.1 ；
- 去package.json文件里面的 "sass-loader"的版本更换成7.3.1，然后重新cnpm install就可以了

## 路由嵌套

嵌套路由又称子路由，在实际应用中，通常由多层嵌套的组件组合而成。同样的，URL中各段动态路由也按某种结构对应嵌套的各层组件，例如：

<img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240510143751397-1585615649.png" alt="image-20240510143754211" width="500" />

1. 创建用户信息组件，在 views/user 目录下创建一个名为 Profile.vue 的视图组件；Profile.vue

   ```vue
   <template>
     <h1>个人信息</h1>
   </template>
   
   <script>
   export default {
     name: "UserProfile"
   }
   </script>
   
   <style scoped>
   </style>
   ```

2. 在用户列表组件在 views/user 目录下创建一个名为 List.vue 的视图组件；
   List.vue

   ```vue
   <template>
     <h1>用户列表</h1>
   </template>
   
   <script>
   export default {
     name: "UserList"
   }
   </script>
   
   <style scoped>
   </style>
   ```

3. 修改首页视图，我们修改 Main.vue 视图组件，此处使用了 ElementUI 布局容器组件，代码如下：
   Main.vue

   ```vue
   <template>
     <div>
       <el-container>
         <el-aside width="200px">
           <el-menu :default-openeds="['1']">
             <!--用户管理-->
             <el-submenu index="1">
               <template slot="title"><i class="el-icon-caret-right"></i>用户管理</template>
               <el-menu-item-group>
                 <el-menu-item index="1-1">
                   <!--插入的地方-->
                   <router-link to="/user/profile">个人信息</router-link>
                 </el-menu-item>
                 <el-menu-item index="1-2">
                   <!--插入的地方-->
                   <router-link to="/user/list">用户列表</router-link>
                 </el-menu-item>
               </el-menu-item-group>
             </el-submenu>
             <!--内容管理-->
             <el-submenu index="2">
               <template slot="title"><i class="el-icon-caret-right"></i>内容管理</template>
               <el-menu-item-group>
                 <el-menu-item index="2-1">分类管理</el-menu-item>
                 <el-menu-item index="2-2">内容列表</el-menu-item>
               </el-menu-item-group>
             </el-submenu>
           </el-menu>
         </el-aside>
   
         <el-container>
           <!--顶部栏 向右对齐-->
           <el-header style="text-align: right; font-size: 12px">
             <el-dropdown>
               <i class="el-icon-setting" style="margin-right: 15px"></i>
               <el-dropdown-menu slot="dropdown">
                 <el-dropdown-item>个人信息</el-dropdown-item>
                 <el-dropdown-item>退出登录</el-dropdown-item>
               </el-dropdown-menu>
             </el-dropdown>
           </el-header>
           <el-main>
             <!--在这里展示视图-->
             <router-view/>
           </el-main>
         </el-container>
       </el-container>
     </div>
   </template>
   <script>
   export default {
     name: "Main"
   }
   </script>
   <style scoped>
   .el-header {
     background-color: pink;
     color: #333;
     line-height: 60px;
   }
   
   .el-aside {
     color: #333;
   }
   </style>
   
   ```

4. 配置嵌套路由修改 router 目录下的 index.js 路由配置文件，使用children放入main中写入子模块，代码如下
   index.js

   ```js
   //导入vue
   import Vue from 'vue';
   import VueRouter from 'vue-router';
   //导入组件
   import Main from "../views/Main";
   import Login from "../views/Login";
   
   import UserList from "../views/user/List";
   import UserProfile from "../views/user/Profile";
   
   //使用
   Vue.use(VueRouter);
   //导出
   export default new VueRouter({
     routes: [
       {
         //登录页
         path: '/main',
         component: Main,//嵌套路由
         children: [
           {
             path: '/user/profile',
             component: UserProfile
           },
           {
             path: '/user/list',
             component: UserList
           }
         ]
       },
       //首页
       {
         path: '/login',
         component: Login
       }
     ]
   })
   ```

5. 路由嵌套实战效果图

   <img src="https://img2023.cnblogs.com/blog/3406637/202405/3406637-20240510144852495-20778932.png" alt="image-20240510144855560" width="500" />

## 参数传递及重定向

这里演示如果请求带有参数该怎么传递，用的还是上述例子的代码 修改一些代码 这里不放重复的代码了

###  第一种取值方式

1. 修改路由配置, 主要是router下的index.js中的 path 属性中增加了 :id 这样的参数

   ```js
   export default new VueRouter({
     routes: [
       {
         //登录页
         path: '/main',
         component: Main,//嵌套路由
         children: [
           {
             path: '/user/profile/:id',
             name: 'UserProfile',
             component: UserProfile
           },
           {
             path: '/user/list',
             component: UserList
           }
         ]
       },
       //首页
       {
         path: '/login',
         component: Login
       }
     ]
   })
   ```

2. 传递参数

   - 此时我们在Main.vue中的route-link位置处 to 改为了 :to，是为了将这一属性当成对象使用，
   - 注意  router-link 中的 name 属性名称 一定要和 路由中的 name 属性名称 匹配，因为这样 Vue 才能找到对应的路由路径；

   ```vue
   <el-menu-item index="1-1">
     <!--插入的地方 name:地址,传组件名 params:传递参数-->
     <router-link :to="{name:'UserProfile',params:{id:1}}">个人信息</router-link>
   </el-menu-item>
   ```

3. 在要展示的组件Profile.vue中接收参数 使用 {{$route.params.id}}来接收
   Profile.vue 部分代码

   - 所有的元素，必须不能直接在根节点下
   - 要放入同一个标签内

   ```vue
   <template>
     <div>
       <h1>个人信息</h1>
       {{ $route.params.id }}
     </div>
   </template>
   ```

### 第二种取值方式

使用props 减少耦合

1. 修改路由配置 , 主要在router下的index.js中的路由属性中增加了 props: true 属性

   ```js
   children: [
     {
       path: '/user/profile/:id',
       props:true,
       name: 'UserProfile',
       component: UserProfile
     },
   ```

2. 传递参数和之前一样 在Main.vue中修改route-link地址

   ```vue
   <el-menu-item index="1-1">
     <!--插入的地方 name:地址,传组件名 params:传递参数-->
     <router-link :to="{name:'UserProfile',params:{id:1}}">个人信息</router-link>
   </el-menu-item>
   ```

3. 在Profile.vue接收参数为目标组件增加 props 属性
   Profile.vue

   ```vue
   <template>
     <div>
       <h1>个人信息</h1>
       {{ id }}
     </div>
   </template>
   
   <script>
   export default {
     props: ["id"],
     name: "UserProfile"
   }
   </script>
   
   <style scoped>
   </style>
   ```

### 组件重定向

重定向的意思大家都明白，但 Vue 中的重定向是作用在路径不同但组件相同的情况下，比如：
在router下面index.js的配置

```js
{
  path: '/main',
  name: 'Main',
  component: Main
},
{
  path: '/goHome',
  redirect: '/main'
}
```

说明：这里定义了两个路径，一个是 /main ，一个是 /goHome，其中 /goHome 重定向到了 /main 路径，由此可以看出重定向不需要定义组件；

使用的话，只需要在Main.vue设置对应路径即可；

```vue
<el-menu-item index="1-3">
    <router-link to="/goHome">回到首页</router-link>
</el-menu-item>
```

## 优化

### 路由模式有两种

- hash：路径带 # 符号，如 http://localhost/#/login
- history：路径不带 # 符号，如 http://localhost/login

修改路由配置，去除#，代码如下：

```js
export default new VueRouter({
  mode: 'history',
  routes: []
})
```

### 404处理

处理 404 创建一个名为 NotFound.vue 的视图组件，代码如下：

```vue
<template>
  <div>
    <h1>404，你的页面走丢了</h1>
  </div>
</template>

<script>
export default {
  name: "NotFound"
}
</script>

<style scoped>
</style>
```

修改路由配置，代码如下：

```js
import NotFound from "../views/NotFound.vue";

{
    path: '*',
    component: NotFound
}
```

## 路由钩子与异步请求

### 钩子函数

- beforeRouteEnter：在进入路由前执行
- beforeRouteLeave：在离开路由前执行

在Profile.vue中写

```vue
<script>
export default {
  props: ["id"],
  name: "UserProfile",
  //类似于过滤器/拦截器 req,resp,chain
  beforeRouteEnter: (to, from, next) => {
    console.log("路由进入之前");
    next();
  },
  beforeRouteLeave: (to, from, next) => {
    console.log("路由离开之前");
    next();
  }
}
</script>
```

参数说明：

- to：路由将要跳转的路径信息
- from：路径跳转前的路径信息
- next：路由的控制参数
  - next() 跳入下一个页面
  - next(’/path’) 改变路由的跳转方向，使其跳到另一个路由
  - next(false) 返回原来的页面
  - next((vm)=>{}) 仅在 beforeRouteEnter 中可用，vm 是组件实例

### 在钩子函数中使用异步请求

1. 安装 Axios

   ```bash
   cnpm install --save vue-axios
   ```

2. main.js引用 Axios

   ```js
   import axios from 'axios'
   import VueAxios from 'vue-axios'
   Vue.use(VueAxios, axios)
   ```

3. 准备数据 

   - 只有我们的 static 目录下的文件是可以被访问到的，所以我们就把静态文件放入该目录下。
   - 数据和之前用的json数据一样 需要的去上述axios例子里

   ```java
   静态数据存放的位置
   static/mock/data.json
   ```

4. 在 beforeRouteEnter 中进行异步请求
   Profile.vue

   ```vue
   <script>
   export default {
     //第二种取值方式
     // props:['id'],
     name: "UserProfile",
     //钩子函数 过滤器
     beforeRouteEnter: (to, from, next) => {
       //加载数据
       console.log("进入路由之前")
       next(vm => {
         //进入路由之前执行getData方法
         vm.getData()
       });
     },
     beforeRouteLeave: (to, from, next) => {
       console.log("离开路由之前")
       next();
     },
     //axios
     methods: {
       getData: function () {
         this.axios({
           method: 'get',
           url: 'http://localhost:8080/static/mock/data.json'
         }).then(function (response) {
           console.log(response)
         })
       }
     }
   }
   </script>
   ```

5. 路由钩子和axios结合图
   <img src="https://img-blog.csdnimg.cn/20200624143534392.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L29rRm9ycmVzdDI3,size_16,color_FFFFFF,t_70#pic_center" alt="" width="600" />