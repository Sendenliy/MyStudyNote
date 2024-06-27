# 初识CSS

## 概述

1. CSS：Cascading Style Sheet 级联样式表。  
2. 表现HTML或XHTML文件样式的计算机语言。包括对字体、颜色、边距、高度、宽度、背景图片、网页定位等设定。
3. 优势
   - 内容与表现分离
   - 网页的表现统一，容易修改
   - 丰富的样式，使得页面布局更加灵活
   - 减少网页的代码量，增加网页的浏览速度，节省网络带宽
   - 运用独立于页面的CSS，有利于网页被搜索引擎收录

## 基本语法

1. CSS:由选择器和声明构成。声明（属性：值;）

   ```css
   选择器{
       声明1；
       声明2；
       ...
   }
   h1{
       font-size:12px;
       color:#F00;
   }
   /*最后一条声明后的;可写可不写，但是基于W3C标准规范考虑，建议写
   （结构化标准语言HTML、表现标准语言CSS、行为标准JS）
   ```

2. Style标签

   ```css
   <style type="text/css">
       h1{
           font-size:12px;
           color:#F00;
       }
   </style>
   ```

## CSS引入方式

CSS样式优先级

- 行内样式	>	内部样式表	>	外部样式表
- 就近原则：越接近标签的样式优先级越高。(行内最近，内部和外部看谁最靠下谁就近)

1. 行内样式

   - 使用style属性设置CSS样式仅对当前的HTML标签起作用
   - 不能起到内容与表现相分离，本质上没有体现出CSS的优势，因此不推荐使用    

   ```css
   /*	使用style属性引入CSS样式*/
   <h1 style="color:red;">style属性的应用</h1>
   <p style="font-size:14px; color:green;">直接在HTML标签中设置的样式</p>
   ```

2. 内部样式表 

   - 优点：方便在同页面中修改样式
   - 缺点：不利于在多页面间共享复用代码及维护，对内容与样式的分离也不够彻底

   ```css
   /*	CSS代码写在 <head> 的 <style> 标签 */
   <style>
   	h1{
           color: green; 
   	}
   </style>
   ```

3. 外部样式表

   CSS代码保存在扩展名为.css的样式表中，HTML文件引用扩展名为.css的样式表。

   - 链接式（常用）

   ```css
   /*	使用 <link> 标签链接外部样式表， <link> 标签必须放在<head> 标签中 */
   /* href=文件路径  rel=使用外部样式表 type=文件类型 */
   <head>
   	<link href="style.css" rel="stylesheet" type="text/css">
   </head>
   ```
   
   - 导入式

   ```css
   /*	使用@import导入外部样式表 */
   <head>
   	<style type="text/css">
   		<!--@import url("style.css")-->
       </style>
   </head>
   ```

链接式与导入式的区别

1. ` link`标签是属于XHTML范畴的；@import是属于CSS2.1中特有的。
2. 使用` link` 链接的CSS是客户端浏览网页时先将外部CSS文件加载到网页当中，然后再进行编译显示，所以这种情况下显示出来的网页与用户预期的效果一样，即使网速再慢也一样的效果。
3. 使用`@import`导入的CSS文件，客户端在浏览网页时是先将HTML结构呈现出来，再把外部CSS文件加载到网页当中，当然最终的效果也与使用 `link` 链接文件效果一样，只是当网速较慢时会先显示没有CSS统一布局的HTML网页，这样就会给用户很不好的感觉。这个也是现在目前大多少网站采用链接外部样式表的主要原因。
4. 由于@import是属于CSS2.1中特有的，因此对于不兼容CSS2.1的浏览器来说就是无效的

## CSS基本选择器

CSS基本选择器的优先级

- ID选择器	>	类选择器	>	标签选择器  
- 不遵循就近原则
- 无论是哪种方式引入CSS样式，一般都遵循该优先级

1. 标签选择器

   ```css
   /*
   HTML标签作为标签选择器的名称
   <h1>…<h6>、<p>、<img/>
   */
   p{
       font-size:16px;
   }
   ```

2. 类选择器

   ```css
   /*	可以复用，多个标签写同一个class类名	*/
   <标签名 class="类名称">标签内容</标签名>
   .class{
       font-size:16px;
   }
   
   /* ----例子---- */
   .biaoqian{
               color: #279466;
           }
   <h1 class="biaoqian">标签</h1>
   ```

3. ID选择器

   ```css
   /* ID选择器的名称就是HTML中标签的ID名称，
   	ID全局唯一，不能重复使用 */
   #id{
       font-size:16px;
   }
   
   /* ----例子---- */
   #biaoqian{
               color: #279466;
           }
   <h1 id="biaoqian">标签</h1>
   ```

4. 小结

   - 标签选择器：直接应用于HTML标签
   - 类选择器：可在页面中多次使用
   - ID选择器：在同一个页面中只能使用一次

## CSS高级选择器

### 层次选择器

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410122509148-1990239988.png" alt="image-20240410122511732" width="300" />

1. 后代选择器

   ```css
   body p{
   	background: red;
   }
   /*	后代选择器两个选择符之间必须要以空格隔开，中间不能有任何其他的符号插入 */
   /*	所有后代*/
   ```

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410123139150-1749169008.png" alt="image-20240410123142103" width="400" />

2. 子选择器

   ```css
   body>p{
   	background: pink;
   }
   /*	只有下一代 即子代 */
   ```

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410123322590-959223222.png" alt="image-20240410123325576" width="400" />

3. 相邻兄弟选择器

   ```css
   /*	此处是类选择器 也可以用id选择器*/
   .active+p{
   	background: green;
   }
   /*	只有相邻的兄弟 对下不对上*/
   ```

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410123336643-52503041.png" alt="image-20240410123339884" width="400" />

4. 通用兄弟选择器

   ```css
   .active~p{
   	background: yellow;
   }
   /*	所有兄弟*/
   ```

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410123348805-1962066452.png" alt="image-20240410123351998" width="400" />

### 结构伪类选择器

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410122603632-310465000.png" alt="image-20240410122606786" width="300" />

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410122716630-2108262669.png" alt="image-20240410122719892" width="300" />

1. E:first-child	E的第一个子元素

   ```css
   ul li:first-child{ 
       background: red;
   } 
   /*	ul第一个子元素，变为红色*/
   ```

2. E:last-child     E的最后一个子元素

   ```css
   ul li:last-child{ 
       background: green;
   } 
   /*	ul最后一个子元素，变为绿色*/
   ```

3. E:nth-child(n)   E元素的第n个元素

   ```css
   p:nth-child(1){ 
       background: yellow;
   }
   /*	p的父元素的第一个子元素，变为黄色*/
   ```

4. E:nth-of-child(n)   E元素的指定类型的第n个元素

   ```css
   p:nth-of-type(2){ 
       background: blue;
   } 
   /*	p的父元素的第二个子元素且必须是p元素类型，变为蓝色*/
   ```

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410123926233-38490348.png" alt="image-20240410123928851" width="400" />

5. 使用E F:nth-child(n)和E F:nth-of-type(n)的 关键点

   1. E F:nth-child(n)在父级里从一个元素开始查找，不分类型
   2. E F:nth-of-type(n)在父级里先看类型，再看位置

### 属性选择器

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410122817952-1707773600.png" alt="image-20240410122820866" width="300" />

1. E[attr]属性选择器 

    ```css
    a[id] {
    	background: yellow;
    }
    /*	含有id属性的a元素，变为黄色*/
    ```

2. E[attr=val]属性选择器  

    ```css
   a[id="first"] {
   	background: red;
   }
   /*	id属性值为first的a元素，变为红色*/
   ```

3. E[attr^=val]属性选择器

    ```css
   a[href^=http] {
   	background: red;
   }
   /*	a元素中含有href属性，且属性值是以http开头的，变为红色*/
   ```

4. E[attr$=val]属性选择器  

    ```css
   a[href$=png] {
   	background: red;
   }
   /*	a元素中含有href属性，且属性值是以png结尾的，变为红色*/
   ```

5. E[attr*=val]属性选择器

    ```css
   a[class*=links] {
   	background: red;
   }
   /*	a元素中含有class属性，且属性值中包含links的，变为红色*/
   ```

## 小结

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410122936856-828216154.png" alt="image-20240410122939789" width="400" />

# 美化网页元素

span标签：能让某几个文字或者某个词语凸显出来，从而添加对应的样式  

```css
 <p>
	好好学习，<span>天天向上</span>
</p>
```

## 字体样式

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410130016480-1449234987.png" alt="image-20240410130019348" width="300" />

1. 字体类型 font-family

   ```css
   p{
       font-family:Verdana,"楷体";
   }
   body{
       font-family: Times,"Times New Roman", "楷体";
   }
   ```

2. 字体大小 font-size

   ```css
   /*
   单位:
   1.px（像素）
   2.em(缩进)、rem、cm、mm、pt、pc
   */
   h1{font-size:24px;}
   h2{font-size:16px;}
   
   h3{font-size:2em;}
   
   span{font-size:12pt;}
   
   strong{font-size:13pc;}
   ```

3. 字体风格 font-style

   ```css
   /* normal、italic和oblique */
   /*	标准、斜体、倾斜(inherit继承父元素字体样式)*/
   p{
       font-style:normal;
   }
   body{
       font-style: italic;
   }
   /*
   italic的意思就是使用字体的斜体属性，进行倾斜，
   oblique的意思就是让本不会倾斜的字变得倾斜
   视觉效果上来说他们一般可能并没有什么区别
   */
   ```

4. 字体粗细 font-weight

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410131126492-1611582951.png" alt="image-20240410131129263" width="300" />

5. 字体属性 font

   ```css
   /*	字体属性的顺序：
   	字体风格→字体粗细→字体大小→字体类型
   */
   p span{
   	font:oblique bold 12px "楷体";
   }
   ```

## 文本样式

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410130027163-1440891870.png" alt="image-20240410130030394" width="300" />

1. 文本颜色 color

   - RGB

     - 十六进制方法表示颜色：前两位表示红色分量，中间两位表示绿色分量，最后两位表示蓝色分量 【#A983D8】

     - rgb(r,g,b) : 正整数的取值为0～255 

   - RGBA

     在RGB基础上增加了控制alpha透明度的参数，其中这个透明通道值为0～1

   ```css
   color:#A983D8;
   color:#EEFF66;
   
   color:rgb(0,255,255);
   color:rgba(0,0,255,0.5);
   ```

2. 排版文本段落

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410130044482-1857316929.png" alt="image-20240410130047443" width="300" />

   ```css
   /*水平对齐方式：*/
   text-align属性如上图
   /*首行缩进：*/
   text-indent：em或px
   /*行高：*/	
   line-height：px 
   /* 居中：行高=块高 line-height = height */
   ```

3. 文本修饰text-decoration和垂直对齐vertical-align

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410130052184-312389052.png" alt="image-20240410130055319" width="300" />

   ```css
   /*文本装饰：*/
   text-decoration属性如上图
   
   /*
   垂直对齐方式：
   vertical-align属性：middle、top、bottom
   */
   img, span {vertical-align: middle;}
   ```

## 文本阴影

```css
/*	原本处于原点位置
阴影颜色 X轴位置 Y轴位移 阴影模糊半径
X轴位移：用来指定阴影水平位移量px
Y轴位移：用来指定阴影垂直位移量px
阴影模糊半径：代表阴影向外模糊的模糊范围px
*/
text-shadow:color x-offset y-offset blur-radius;
```

## 超链接伪类

1. 语法

   ```css
   标签名：伪类名{
       声明；
   }
   a:hover{
       color:#B46210;
       text-decoration:underline;
   }
   ```

2. 设置伪类的顺序(默认- - 点击后- - 悬浮- - 点击未释放时)

   a:link	->	a:visited	->	a:hover	->	a:active

   实际网页开发中通常只设置两种状态

   - 一是a{color:#333;}，
   - 一是a:hover{ color:#B46210;}  

3. 使用CSS设置超链接

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410130119187-1177241942.png" alt="image-20240410130122383" width="300" />

## 列表样式

1. 分类
   - list-style-type
   - list-style-image 
   - list-style-position
   - list-style
   
   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410130124730-2078219789.png" alt="image-20240410130127901" width="300" />
   
2. 很少使用CSS自带的列表标记，而是使用设计的图标，会想使用list-style-image

3. list-style-position不能准确地定位图像标记的位置，通常，网页中图标的位置都是非常精确的。

4. 在实际的网页制作中，通常使用list-style或list-style-type设置项目无标记符号，然后通过背景图像的方式把设计的图标设置成列表项标记。在网页制作中，`list-style和list-style-type`两个属性是大家经常用到的，而另两个属性则不太常用

```css
li{
    list-style:none;
}
```

## 背景样式

常见的背景样式

- 背景图像background-image
- 背景颜色background-color

1. 设置背景图像

   ```css
   background-image:url;
   ```

2. 背景重复方式

   ```css
   background-repeat属性
   - repeat：沿水平和垂直两个方向平铺（默认）
   - no-repeat：不平铺，只显示一次
   - repeat-x：沿水平方向平铺
   - repeat-y：沿垂直方向平铺
   ```

3. 背景定位

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410130133833-134680363.png" alt="image-20240410130137065" width="300" />

4. 设置背景

   ```css
   background属性
   .title{
       font-size:18px;
       font-weight:bold;
       color:#FFF;
       text-indent:1em;
       line-height:35px;
       background:#C00 url(../image/arrow-down.gif) 205px 10px no-repeat;
       /*	背景颜色 背景图像 背景定位 背景不重复显示*/
       background-color
       background-image
       background-position
       background-repeat
   }
   ```

5. 背景尺寸

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410130142186-907194034.png" alt="image-20240410130145404" width="300" />

## 渐变样式

网站推荐：http://color.oulu.me/  、https://www.grabient.com/

background-image、background-color、background

- 线性渐变

  颜色沿着一条直线过渡：从左到右、从右到左、从上到下等

- 径向渐变

  圆形或椭圆形渐变，颜色不再沿着一条直线变化，而是从一个起点朝所有方向混合

1. CSS3渐变兼容

   - IE浏览器是Trident内核，加前缀：-ms
   - Chrome浏览器是Webkit内核，加前缀：-webkit
   - Safari浏览器是Webkit内核，加前缀：-webkit
   - Opera浏览器是Blink内核，加前缀：-o
   - Firefox浏览器是Mozilla内核，加前缀：-moz-  

2. 线性渐变

   ```css
   /*	渐变方向  第一种颜色值  第二种颜色值  */
   linear-gradient(position,color1,color2,...)
   
   /*	兼容Webkit内核的浏览器	*/
   -webkit-linear-gradient ( position, color1, color2,…)  
   ```

## 小结

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410130153140-228697415.png" alt="image-20240410130156210" width="400" />

# 盒子模型

## 概述

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411151651758-2022600649.png" alt="image-20240411151654872" width="300" /><img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411151810510-166897025.png" alt="image-20240411151813860" width=300 />

## 边框border

1. 边框颜色 border-color

   - 边框颜色设置方式与文本颜色都是使用十六进制
   - 强调同时设置4个边框颜色时，顺序为`上右下左`
   - 分别上、下、左、右各边框颜色的不同设置方式，及属性值的顺序

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411152020216-850672231.png" alt="image-20240411152023397" style="zoom:33%;" />

2. 边框粗细 border-width

   thin、medium、thick、像素值

   ```css
   border-top-width:5px;
   border-right-width:10px;
   border-bottom-width:8px;
   border-left-width:22px;
   
   border-width:5px ;
   border-width:20px 2px;
   border-width:5px 1px 6px;
   border-width:1px 3px 5px 2px;
   ```

3. 边框样式 border-style

   none、hidden、dotted、dashed虚线、solid实线、double

   ```css
   border-top-style:solid;
   border-right-style:solid;
   border-bottom-style:solid;
   border-left-style:solid;
   
   border-style:solid ;
   border-style:solid dotted;
   border-style:solid dotted dashed;
   border-style:solid dotted dashed double;
   ```

4. border简写

   同时设置边框的颜色、粗细和样式

   ```css
   /* 粗细 样式 颜色 */
   border:1px solid #3a6587;
   border: 1px dashed red;
   ```

## 内外边距

1. 外边距 margin

   margin-top、margin-right、margin-bottom、margin-left

   ```css
   margin-top: 1 px
   margin-right : 2 px
   margin-bottom : 2 px
   margin-left : 1 px
   /*顺时针：上 右 下 左*/
   margin :3px 5px 7px 4px;
   margin :3px 5px;
   /*	只有两个是：上下，左右	*/
   margin :3px 5px 7px;
   margin :8px;
   ```

2. 网页居中对齐的必要条件

   - 块元素，处于div中
   - 固定宽度，块元素有固定宽度
   - `网页居中对齐  ：margin:0px auto;  `

   ```css
   <div style="width: 500px;margin: 0 auto">
       <img alt="" src="../resources/image/1.jpg">
   </div>
   ```

3. 内边距 padding

   padding-left、padding-right、padding-top、padding-bottom

   ```css
   padding-left:10px;
   padding-right: 5px;
   padding-top: 20px;
   padding-bottom:8px;
   
   padding:20px 5px 8px 10px ;
   padding:10px 5px;
   padding:30px 8px 10px ;
   padding:10px;
   ```

## 盒子模型尺寸

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411152620027-193949384.png" alt="image-20240411152622935" width="300" />

**box-sizing**  

```css
/*
默认值，盒子的总尺度
盒子的宽度或高度=元素内容的宽度或高度
元素继承父元素的盒子模型模式
*/
box-sizing:content-box | border-box | inherit;

<style>
    div{
        width: 100px;
        height: 100px;
        padding: 5px;
        margin: 10px;
        border: 1px solid #000000;
        box-sizing: border-box;
        /*box-sizing: content-box; /!* 默认值*!/*/
    }
</style>
```

## 圆角边框

```css
/* 四个属性值按顺时针排列(左上 右上 右下 左下) */
border-radius: 20px 10px 50px 30px;
/* 两个：左上右下-右上左下*/
```

1. 制作特殊图形：圆形  

   - 元素的宽度和高度必须相同
   - 圆角的半径为元素宽度的一半(border=1px时，若border=10px，半径=宽度一半+10)，或者直接设置圆角半径值为50%

   ```css
   div{
       width: 100px;
       height: 100px;
       border: 4px solid red;
       border-radius: 50%;
   }
   ```

2. 制作特殊图形：半圆形  

   - 制作上半圆或下半圆时，元素的宽度是高度的2倍，而且圆角半径为元素的高度值
   - 制作左半圆或右半圆时，元素的高度是宽度的2倍，而且圆角半径为元素的宽度值

   ```css
   div{
       background: red;
       margin: 30px;
   } 
   /* 上半圆 */
   div:nth-of-type(1){
       width: 100px;
       height: 50px;
       border-radius: 50px 50px 0 0;
   } 
   /* 下半圆 */
   div:nth-of-type(2){
       width: 100px;
       height: 50px;
       border-radius:0 0 50px 50px;
   } 
   /* 右半圆 */
   div:nth-of-type(3){
       width: 50px;
       height: 100px;
       border-radius:0 50px 50px 0;
   } 
   /* 左半圆 */
   div:nth-of-type(4){
       width: 50px;
       height: 100px;
       border-radius: 50px 0 0 50px;
   }
   ```

3. 制作特殊图形：扇形 

   遵循“三同，一不同”原则  

   -  “三同”是：元素宽度、高度、圆角半径相同
   - “一不同”是：圆角取值位置不同

   ```css
   div{
       background: red;
       margin: 30px;
   } 
   /* 左上-扇形 */
   div:nth-of-type(1){
       width: 50px;
       height: 50px;
       border-radius: 50px 0 0 0;
   }
   /* 右上-扇形 */
   div:nth-of-type(2){
       width: 50px;
       height: 50px;
       border-radius:0 50px 0 0;
   } 
   /* 右下-扇形 */
   div:nth-of-type(3){
       width: 50px;
       height: 50px;
       border-radius:0 0 50px 0;
   } 
   /* 左下-扇形 */
   div:nth-of-type(4){
       width: 50px;
       height: 50px;
       border-radius: 0 0 0 50px;
   }
   ```

## 盒子阴影

```css
/* 
阴影类型-内阴影
X轴位移，指定阴影水平位移量
Y轴位移，指定垂直水平位移量
阴影模糊半径，阴影向外模糊的模糊范围
阴影颜色，定义绘制阴影时所使用的颜色
*/
box-shadow:inset x-offset y-offset blur-radius color;

div{
    width: 100px;
    height: 100px;
    border: 1px solid red;
    border-radius: 8px;
    margin: 20px;
/*box-shadow: 20px 10px 10px #06c; /!*内阴影*!/*/
/*box-shadow: 0px 0px 20px #06c; /!*只设置模糊半径的阴影
*!/*/
	box-shadow: inset 3px 3px 10px #06c; /*内阴影*/
}
```

## 小结

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411154500694-1695456611.png" alt="image-20240411154502865" width="400" />

# 浮动

## 标准文档流

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240413131643405-1022221660.png" alt="image-20240413131646468" width="400" />

标准文档流：指元素根据块元素或行内元素的特性按从上到下，从左到右的方式自然排列。这也是元素默认的排列方式  

标准文档流组成：

内联标签可以包含于块级标签中，成为它的子元素，而反过来则不成立

1. 块级元素（block）

  ```css
  <h1>…<h6>、<p>、<div>、列表li
  ```

2. 内联元素（inline）

  ```css
  <span>、<a>、<img/>、<strong>...
  ```

## display

1. display属性

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240413122124050-991381135.png" alt="image-20240413122126985" width="400" />

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411154829219-1497084526.png" alt="image-20240411154832193" width="300" />

   

2. display特性

   - 块级元素与行级元素的转变（block块元素、inline行内元素）
   - 控制块元素排到一行（inline-block 是块元素但是可以内联，在一行）
   - 控制元素的显示和隐藏（none）

3. 块元素排在一行的方法

   - inline-block
   - float

## 浮动

1. float属性

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411155041095-2109500723.png" alt="image-20240411155044433" width="300" />

2. 左浮动

   ```css
   .layer01 {
       border:1px #F00 dashed;
       float:left;
   } 
   /*	全部左浮（除文字）如下图*/
   ```
   
   ![image-20240413124007999](https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240413124005409-175829399.png)
   
3. 右浮动

   ```css
   .layer01 {
       border:1px #F00 dashed;
       float:right;
   }
   /*	全部右浮*/
   ```
   
   ![image-20240413124147268](https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240413124144508-472243436.png)

## 边框塌陷

设置宽度和右浮动后，边框塌陷了怎么解决？

浮动元素脱离标准文档流，清除浮动

1. clear属性

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411155714743-131403984.png" alt="image-20240411155718113" width="300" />

   ```css
   .layer04 {
   	clear:both; /*清除两侧浮动*/
   } 
   .layer04 {
   	clear:left; /*清除左侧浮动*/
   } 
   .layer04 {
   	clear:right; /*清除右侧浮动，不允许右侧有浮动元素，如果有就把自身排到下一行*/
   }
   ```

2. 解决父级边框塌陷的方法

   clear属性可以清除浮动对其他元素造成的影响，可是依然解决不了父级边框塌陷问题  

   - 设置父元素的高度

     ```css
     #father {
         /*	设置父元素的高度覆盖浮动范围*/
         height: 400px; 
         border:1px #000 solid; 
     }
     ```
     
   - 浮动元素后面加空div

     ```css
     <div id="father">
         <div class="layer01"><img alt="" height="100" src="../../resources/image/1.jpg" width="100"></div>
         <div class="layer02"><img alt="" height="150" src="../../resources/image/2.jpg" width="200"></div>
         <div class="layer03"><img alt="" height="200" src="../../resources/image/3.jpg" width="300"></div>
         <div class="layer04">浮动的盒子</div>
     	/* 增加一个空的div标签 */
         <div class="clear"></div>
     </div>
     /*	给div设置清楚浮动元素，内外边距为0*/
     .clear{ 
         clear: both; 
         margin: 0; 
         padding: 0;
     }
     ```

   - 父级添加overflow属性hidden(溢出处理)

     <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411160021147-1656994768.png" alt="image-20240411160023797" width=300 />

     ```css
     <div id="father">
         <div class="layer01"><img src="image/photo-1.jpg" alt="日用品" /></div>
         <div class="layer02"><img src="image/photo-2.jpg" alt="图书" /></div>
         <div class="layer03"><img src="image/photo-3.jpg" alt="鞋子" /></div>
         <div class="layer04">浮动的盒子……</div>
     </div>
     /*
     hidden属性值，通常与< div>宽度结合使用设置< div>自动扩展高度，或者隐藏超出的内容
     */
     #father {
         overflow: hidden;
         border:1px #000 solid; 
     }
     ```

   - 父级添加伪类：after（避免空div，最好的方式）

     ```css
     <div id="father" class="clear">
         <div class="layer01"><img src="image/photo-1.jpg" alt="日用品" /></div>
         <div class="layer02"><img src="image/photo-2.jpg" alt="图书" /></div>
         <div class="layer03"><img src="image/photo-3.jpg" alt="鞋子" /></div>
         <div class="layer04">浮动的盒子……</div>
     </div>
     .clear:after{
         content: ''; /*在clear类后面添加内容为空*/
         display: block; /*把添加的内容转化为块元素*/
         clear: both; /*清除这个元素两边的浮动*/
     }
     ```

3. 清除浮动，防止父级边框塌陷的四种方法

   1. 浮动元素后面加空div

      简单，空div会造成HTML代码冗余

   2. 设置父元素的高度

      简单，元素固定高会降低扩展性

   3. 父级添加overflow属性

      简单，下拉列表框的场景避免用

   4. 父级添加伪类after

      写法比上面稍微复杂一点，但是没有副作用，推荐使用

## inline-block和float区别

1. display:inline-block
   - 可以让元素排在一行，并且支持宽度和高度，代码实现起来方便
   - 位置方向不可控制，会解析空格
   - IE 6、IE 7上不支持
2. float
   - 可以让元素排在一行并且支持宽度和高度，可以决定排列方向
   - float 浮动以后元素脱离文档流，会对周围元素产生影响，必须在它的父级上添加清除浮动的样式

## 小结

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411155520618-1381588796.png" alt="image-20240411155523370" width=400 />

# 定位

## 应用

- 下拉菜单
- 不随滚动条移动的固定导航
- 鼠标移入弹出的消息框

## 相对定位

position属性：

1. static：默认值，没有定位，以标准流方式显示

2. relative：相对定位

   - 相对自身原来位置进行偏移
   - 偏移设置：top、left、right、bottom

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411160631539-1561517936.png" alt="image-20240411160634705" width=200 />

相对定位元素的规律

1. 设置相对定位的盒子会`相对它原来的位置`，通过指定偏移，到达新的位置
2. 设置相对定位的盒子`仍在标准文档流`中，它对父级盒子和相邻的盒子都没有任何影响
3. 设置相对定位的盒子`原来的位置会被保留`下来

设置第二个盒子右浮动，再设置第一、第二盒子相对定位

```css
#first {
    background-color:#FC9;
    border:1px #B55A00 dashed;
    
    position:relative;
    right:20px; 	/* 左移了20px */
    bottom:20px;	/* 下移了20px */
    left:20px;	/* 右移了20px */
    top:-20px;	/* 上移了20px */
} 
```

## 绝对定位

absolute属性：偏移设置： left、right、top、bottom

绝对定位：

1. 假设父级元素存在定位，则以它`最近的一个“已经定位”的“祖先元素” `为基准进行偏移
2. 如果没有已经定位的祖先元素，会以`浏览器窗口`为基准进行定位
3. 绝对定位的元素`从标准文档流中脱离`，这意味着它们对其他元素的定位不会造成影响
4. 元素位置发生偏移后，它`原来的位置不会被保留`下来
5. 设置了绝对定位，但没有设置偏移量的元素将保持在原来的位置。
6. 在网页制作中可以用于需要使某个元素脱离标准流，而仍然希望它保持在原来的位置的情况

```css
#second {
    background-color:#CCF;
    border:1px #0000A8 dashed;
    
    position:absolute;
    right:30px;
}
```

## 固定定位

position的fixed属性值：

- 偏移设置： left、right、top、bottom

- 类似绝对定位，不过区别在于定位的基准不是祖先元素，而是`浏览器窗口  `

```css
/*固定定位*/
div:nth-of-type(2) {
    width: 50px;
    height: 50px;
    background: #279466;
    
    position: fixed;
    right: 0;
    bottom: 0;
}
```

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411161700825-1761251832.png" alt="image-20240411161703944" width="300" />

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411161705867-1943820842.png" alt="image-20240411161709226" width="300" />

## 三种定位方式

**相对定位**

1. position：relative；

2. 相对定位的特性

   - 相对于自己的初始位置来定位
   - 元素位置发生偏移后，它原来的位置会被保留下来
   - 层级提高，可以把标准文档流中的元素及浮动元素盖在下边

3. 相对定位的使用场景

   相对定位一般情况下很少自己单独使用，都是配合绝对定位使用，为绝对定位创造定位父级而又不设置偏移量

**绝对定位**

1. position：absolute；

2. 绝对定位的特性

   - 绝对定位是相对于它的定位父级的位置来定位，如果没有设置定位父级，则相对浏览器窗口来定位
   - 元素位置发生偏移后，原来的位置不会被保留
   - 层级提高，可以把标准文档流中的元素及浮动元素盖在下边
   - 设置绝对定位的元素脱离文档流

3. 绝对定位的使用场景

   一般情况下，绝对定位用在下拉菜单、焦点图轮播、弹出数字气泡、特别花边等场景

**固定定位**

1. position：fixed；

2. 固定定位的特性

   - 相对浏览器窗口来定位
   - 偏移量不会随滚动条的移动而移

3. 固定定位的使用场景

   一般在网页中被用在窗口左右两边的固定广告、返回顶部图标、吸顶导航栏等

## z-index属性

调整元素定位时重叠层的上下位置

- z-index属性值：整数，默认值为0
- 设置了positon属性时，z-index属性可以设置各元素之间的重叠高低关系
- z-index值大的层位于其值小的层上方
- opacity：透明度0-1或者%

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411162321154-1627261464.png" alt="image-20240411162324462" width="300" />

**小结**

1. 网页中的元素都含有两个堆叠层级
2. 未设置绝对定位时所处的环境，z-index是0
3. 设置绝对定位时所处的堆叠环境，此时层的位置由z-index的值确定
4. 改变设置绝对定位和没有设置绝对定位的层的上下堆叠顺序，只需调整绝对定位层的z-index值即可

**网页元素透明度**

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411162239529-1518244317.png" alt="image-20240411162242363" width="500" />

## 小结

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411162456626-290450315.png" alt="image-20240411162459860" width="400" />

# 制作网页动画

如何在网页中实现动画效果？

1. 动态图片
2.  Flash：需要插件支持，文件体积大  
3. JavaScript

使用CSS代码来完成动画：存在兼容性问题  

## CSS变形

1. CSS3变形是一些效果的集合

   每个效果都可以称为变形（transform），它们可以分别操控元素发生平移、旋转、缩放、倾斜等变化

   - 如平移、旋转、缩放、倾斜效果
   - 简言之，平移就是一个变形，旋转就是一个变形…

   ```css
   /* 设置变形函数，可以是一个，也可以是多个，中间以空格分开 */
   transform:[transform-function]*;
   ```

2. 变形函数

   - translate()：平移函数，基于X、Y坐标重新定位元素的位置
   - scale()：缩放函数，可以使任意元素对象尺寸发生变化
   - rotate()：旋转函数，取值是一个度数值skew()：倾斜函数，取值是一个度数值  

3. 2D位移

   ```css
   /* X轴移动的向量长度，Y轴移动的向量长度 */
   translate(tx,ty);
   ```

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411163251938-1641868844.png" alt="image-20240411163255248" width="300" />

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411163236195-1042527172.png" alt="image-20240411163239435" width="300" />

4. 2D缩放

   - scale()函数能够用来缩放元素大小，该函数包含两个参数值，分别用来定义宽度和高度的缩放比例，默认值为1。0～0.99的任意值都可以使元素缩小，而任何大于1的值都能让元素放大。
   - scale()函数和translate()函数的语法非常相似，可以只接收一个值，也可以接收两个值，只有一个值时，第二个值默认和第一个值相等，例如，scale（1，1）元素不会有任何变化，而scale（2，2）会让元素放大2倍

   ```css
   /* 横向坐标方向的缩放量，高度方向的缩放量 */
   scale(sx,sy);
   ```

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411163322904-769127761.png" alt="image-20240411163326233" width="300" />

5. 2D倾斜

   可以仅设置沿着X轴或Y轴方向倾斜

   - skewX（ax）：表示只设置X轴的倾斜
   - skewY（ay）：表示只设置Y轴的倾斜

   ```css
   /* ax：水平方向的倾斜角度，ay：Y轴的倾斜角度 */
   skew(ax,ay);
   ```

6. 2D旋转

   - 参数a单位使用deg表示
   - 参数a取正值时元素相对原来中心顺时针旋转

   ```css
   rotate;
   ```

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411163339376-1970398767.png" alt="image-20240411163342785" width="200" />

7. 小结

   - rotate( )函数只是旋转，而不会改变元素的形状
   - skew( )函数是倾斜，元素不会旋转，会改变元素的形状  

## CSS过渡

transition呈现的是一种过渡，是一种动画转换的过程，如渐现、渐弱、动画快慢等

CSS3 transition的过渡功能更像是一种“黄油”，通过一些CSS的简单动作触发样式平滑过渡  

```css
/* 过渡或动态模拟的CSS属性 完成过渡所需的时间 指定过渡函数 过渡开始出现的延迟时间*/
transition:[transition-property transition-duration transition-timing-function transition-delay]
```

1. 过渡属性（ transition-property ）

   定义转换动画的CSS属性名称

   - IDENT：指定的CSS属性（width、height、background-color属性等） 
   - all：指定所有元素支持transition-property属性的样式，一般为了方便都会使用all

2. 过渡所需的时间（ transition-duration ）

   定义转换动画的时间长度，即从设置旧属性到换新属性所花费的时间，单位为秒（s）

3. 过渡动画函数（ transition-timing-function ）

   指定浏览器的过渡速度，以及过渡期间的操作进展情况，通过给过渡添加一个函数来指定动画的快慢方式

   - ease：速度由快到慢（默认值）
   - linear：速度恒速（匀速运动） 
   - ease-in：速度越来越快（渐显效果） 
   - ease-out：速度越来越慢（渐隐效果） 
   - ease-in-out：速度先加速再减速（渐显渐隐效果）

4. 过渡延迟时间（ transition-delay ）

   指定一个动画开始执行的时间，当改变元素属性值后多长时间去执行过渡效果

   - 正值：元素过渡效果不会立即触发，当过了设置的时间值后才会被触发
   - 负值：元素过渡效果会从该时间点开始显示，之前的动作被截断
   - 0：默认值，元素过渡效果立即执行

**过渡的触发机制**

1. 伪类触发
   - ：hover
   - ：active
   - ：focus
   - ：checked
2. 媒体查询：通过@media属性判断设备的尺寸，方向等
3. JavaScript触发：用JavaScript脚本触发

注意：重点需要掌握伪类触发的方法，这种方法也是实际开发中用的比较多的一种

**使用transition实现过渡动画的使用步骤**

1. 在默认样式中声明元素的初始状态样式
2. 声明过渡元素最终状态样式，如悬浮状态
3. 在默认样式中通过添加过渡函数，添加一些不同的样式

## CSS动画

animation动画简介

animation实现动画主要由两个部分组成：

1. 通过类似Flash动画的关键帧来声明一个动画
2. 在animation属性中调用关键帧声明的动画实现一个更为复杂的动画效果

**CSS3动画的使用过程**

1. 设置关键帧

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411164802022-2108254585.png" alt="image-20240411164804731" width="400" />

   @keyframes的浏览器兼容  

   写兼容的时候浏览器前缀是放在@keyframes中间。例如：@-webkit-keyframes、@-moz- keyframes  

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411164829252-658242067.png" alt="image-20240411164832700" width="400" />

2. 调用关键帧  

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411164929669-208093886.png" alt="image-20240411164932837" width="400" />

   - 动画的播放次数（animation-iteration-count）
     - 值通常为整数，默认值为1
     - 特殊值infinite，表示动画无限次播放
   - 动画的播放方向（animation-direction）
     - normal，动画每次都是循环向前播放
     - alternate，动画播放为偶数次则向前播放
   - 动画的播放状态（animation-play-state）
     - running将暂停的动画重新播放
     - paused将正在播放的元素动画停下来
   - 动画发生的操作（animation-fill-mode）
     - forwards表示动画在结束后继续应用最后关键帧的位置
     - backwards表示会在向元素应用动画样式时迅速应用动画的初始帧
     - both表示元素动画同时具有forwards和backwards的效果

## 总结

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411165053984-1110418190.png" alt="image-20240411165057199" width="400" />