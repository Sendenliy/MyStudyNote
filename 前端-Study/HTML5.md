# HTML基础

## 概述

1. HTML：Hyper Text Markup Language（超文本标记语言）  

   超文本包括：文字、图片、音频、视频、动画等  

2. 发展

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240409184502863-241662925.png" alt="image-20240409184505445" width="300" />

3. 优势

   - 世界知名浏览器厂商对HTML5的支持

     微软、Google、苹果、Opera、mozilla firefox

   - 市场的需求

   - 跨平台

## W3C标准

1. W3C

   - World Wide Web Consortium（万维网联盟）

   - 成立于1994年，Web技术领域最权威和具影响力的国际中立性技术标准机构
   - http://www.w3.org/
   - http://www.chinaw3c.org/

2. W3C标准包括

   - **结构化**标准语言（XHTML 、XML）
   - **表现**标准语言（CSS）
   - **行为**标准（DOM、ECMAScript ）

## HTML基本结构

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240409185743768-1625408728.png" alt="image-20240409185746601" width="300" />

1. 强调HTML标签都以“< >”开始、“</ >”结束
2. 说明网页基本结构中这几个标签的用法
3. 网页中所有的内容都放在之间

```html
<!--1.告诉浏览器，我们要使用的规范-->
<!DOCTYPE html>
<html lang="en">

<!-- 2.head标签 代表网页头部 -->
<head>
    <!-- 3.meta描述性标签 用来描述我们网站的一些信息-->
    <!-- meta一般用来做SEO 即搜索引擎优化   -->
    <meta charset="UTF-8">
    <meta name="keywords" content="我是，关键字">
    <meta name="description" content="这是，一个网站，练习">

    <!-- 4.title标签 网站标题-->
    <title>我是标题</title>
</head>

<!--5.body标签 代表网页主体-->
<body>
Hello,world!
</body>
</html>
```

## 网页的基本标签

1. 标题标签

   ```html
   标题标签-<h1>...</h1>
   <!-- h1最大，h6最小-->
   <h1>一级标题</h1>
   <h2>二级标题</h2>
   <h3>三级标题</h3>
   <h4>四级标题</h4>
   <h5>五级标题</h5>
   <h6>六级标题</h6>
   ```

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410082232040-768846846.png" alt="image-20240410082233999" width="150" />

2. 段落标签

   ```html
   段落标签-<p>...</p>
   快捷键：p + Tab键
   
   这是前半句，
   这是后半句
   
   <p>这是前半句，</p>
   <p>这是后半句</p>
   
   <p>这是前半句，这是后半句</p>
   ```

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410082554637-519879012.png" alt="image-20240410082557688" width="150" />

3. 换行标签

   ```html
   换行标签-...<br/>
   只是换行了，还是同一个段落（比较紧凑，间距小）
   <p>
       这是前半句，<br/>
   	这是后半句
   </p>
   ```
   
   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410082852606-495202166.png" alt="image-20240410082855930" width="100" />
   
4. 水平线标签 

   ```html
   水平线标签-<hr/>
   <p>
       这是前半句，<br/>
       <hr/>
   	这是后半句
   </p>
   ```
   
   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410083031934-372496742.png" alt="image-20240410083035223" width="250" />
   
5. 字体样式标签

   ```html
   加粗-<strong>...</strong>
   斜体-<em>...</em>
   <p>
       <em>这是前半句，</em>
       <br/>
       <strong>这是后半句</strong>
   </p>
   ```

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410083308183-155551728.png" alt="image-20240410083311089" width="100" />

6. 注释和特殊符号

   ```html
   <!--我是注释-->
   快捷键：Ctrl键 + /
   ```
   
   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240409193522851-1617859821.png" alt="image-20240409193525732" width="300" />
   
   ```html
   <!--特殊符号-->
   &开头 + 字母 + ;
   <!--空格-->
   空       格：
   <br/>
   空&nbsp;&nbsp;&nbsp;&nbsp;格:
   <br/>
   
   <!--大于-->
   &gt;
   <br/>
   
   <!--小于-->
   &lt;
   <br/>
   
   <!--版权符号-->
   &copy;
   ```
   
   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410084229639-1115951528.png" alt="image-20240410084232499" width="100" />

## 图像标签

1. 常见的图像格式：jpg、gif、png、bmp(位图)。

   说明JPG、gif是网页中最常用的格式，PNG受浏览器兼容性的限制

2. alt属性：地址错误时可以看到替代文字（alt属性常和src配合使用）

   title属性：悬停时可以看到提示文字

3. img标签的与之前学习的< br/>标签一样，不是成对的标签，直接在最后以“/”闭合  

```html
<!-- src=图像地址 alt=图像的替代文字 title=鼠标悬停提示文字 width=图像宽度 height图像高度-->
<img src="path" alt="text" title="text" width="x" height="y" />
<!--
    ../上一级目录
    相对地址：../resources/image/1.jpg（推荐！）
    绝对地址：D:\JAVA Class\HTML\resources\image\1.jpg
    src:图片地址
-->

<img src="../resources/image/1.jpg" alt="星猫图片" title="我是悬停文字">
```

地址错误时显示alt<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410085151383-263811263.png" alt="image-20240410085154012" width="100" />

## 链接标签

1. 链接类型

   1. 页面间链接：从一个页面链接到另外一个页面
   2. 锚链接
   3. 功能性链接

2. 相对路径和绝对路径  

3. target常用值为_self（默认）和_blank，还有其他值

   更改target的参数，会使目标窗口打开的不同位置    

4. 图片经常保存在image或images目录下，以保证网站目录清晰

```html
<!--href=链接地址  target=链接在哪个窗口打开-->
<a href="path" target="目标窗口位置">链接文本或图像</a>

<!--文本超链接-->
<a href="detail.html" target="_blank">我是文本链接</a>

<!--图像超链接-->
<a href="detail.html" target="_blank">
    <img src="image/img1.png" alt="图像" title="我是图像"/>
</a>
```

1. 锚链接

   - 从A页面的甲位置跳转到本页的乙位置
   - 从A页面的甲位置跳转到B页面的乙位置

   ```html
   <!-- 1.创建跳转标记 -->
   <a name="maker">乙位置</a>
   <!-- 2.创建跳转链接 -->
   <a herf="B页面的链接#maker">甲位置</a>
   ```

2. 功能性链接

   - 例如在网上单击一些QQ图标直接弹出QQ对话框，或单击MSN图标直接弹出MSN对话框，这些都是使用了功能性链接  
   - 电子邮件、QQ、MSN

   ```html
   <!-- 邮件链接：mailto -->
   <a href="mailto:bdqnWebmaster@bdqn.cn">联系我们</a>
   ```
   
   <img src="C:\Users\32354\AppData\Roaming\Typora\typora-user-images\image-20240410091520807.png" alt="image-20240410091520807" width="200" />

## 行内元素和块元素

1. 行内元素

   内容撑开宽度，左右都是行内元素的可以排在一行(a、strong、em…)

2. 块元素

   无论内容多少，该元素独占一行(p、h1-h6)

## 总结

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240409195758346-1880598960.png" alt="image-20240409195801045" width="400" />

# 列表、表格与媒体元素

## 列表

列表就是信息资源的一种展示形式。它可以使信息结构化和条理化，并以列表的样式显示出来，以便浏览者能更快捷地获得相应的信息  

1. 无序列表

   - 没有顺序，每个< li>标签独占一行（块元素）
   - 默认< li>标签项前面有个实心小圆点
   - 一般用于无序类型的列表，如导航、侧边栏新闻、有规律的图文组合模块等

   ```html
   <!--ul 声明无序列表-->
   <ul>
       <!--li 声明列表项-->
       <li>语文</li>
       <li>数学</li>
       <li>英语</li>
       <li>计算机</li>
   </ul>
   列表项中可以包含图片、文本，还可以嵌套列表、其他标签等
   ```

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410092637860-1666734607.png" alt="image-20240410092640841" width="100" />

2. 有序列表

   - 有顺序，每个< li>标签独占一行（块元素）
   - 默认< li>标签项前面有顺序标记,有序列表默认以数字序号显示
   - 一般用于排序类型的列表，如试卷、问卷选项等

   ```html
   <!--ol 声明有序列表-->
   orderlist-list
   <ol>
       <li>语文</li>
       <li>数学</li>
       <li>英语</li>
       <li>计算机</li>
   </ol>
   有序列表与无序列表一样，也可以嵌套列表、可以包含图片、文本、其他标签等
   ```

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410092612276-540688177.png" alt="image-20240410092615058" width="80" />

3. 自定义列表

   - 没有顺序，每个< dt>标签、< dd>标签独占一行（块元素）
   - 默认没有标记
   - 一般用于一个标题下有一个或多个列表项的情况
   - 以后的网页制作中经常会用到定义列表，特别是图文混排的情况  

   ```html
   <!--dl 声明定义列表-->
   <dl>
       <!--dt 声明列表项-->
       <dt>水果</dt>
           <!--dd 定义列表项内容-->
           <dd>苹果</dd>
           <dd>桃子</dd>
           <dd>李子</dd>
   </dl>
   定义列表也可以嵌套列表、包含图片、文本、其他标签等
   ```

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410092832230-788589954.png" alt="image-20240410092835393" width="100" />

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240409200330533-164765171.png" alt="image-20240409200333200" width="500" />

4. 小结

   列表之间可以互相嵌套，进行页面的局部布局 

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240409200539809-373198566.png" alt="image-20240409200542448" width="400" />

## 表格

1. 特点：简单通用、结构稳定
2. 基本结构：单元格、行、列

```html
<!--table表格标签-->
border：边框的宽度
<table border="1px">
    <!--tr 行标签-->
    <tr>
        <!--td 单元格标签-->
        <td>第1个单元格的内容[1,1]</td>
        <td>第2个单元格的内容[1,2]</td>
        <td>第2个单元格的内容[1,2]</td>
        ……
    </tr>
    <tr>
        <td>第1个单元格的内容[2,1]</td>
        <td>第2个单元格的内容[2,2]</td>
        ……
    </tr>
</table>
```

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410093358524-1280499809.png" alt="image-20240410093401125" width="300" />

1. 表格的跨列-合并左右两格

   ```html
   <table>
       <tr>
           <!--colspan 所跨的列数-->
           <td colspan="n">单元格内容</td>
       </tr>
       <tr>
           <td>单元格内容</td>
           ……
       </tr>
   ......
   </table>
   ========================================
   好像只能写在一列的格子
   <table border="1px">
       <!--tr 行标签-->
       <tr>
           <!--td 单元格标签-->
           <td colspan="4">第1个单元格的内容[1,1]</td>
           <td>第2个单元格的内容[1,2]</td>
           <!--跨列-->
           <td >第2个单元格的内容[1,3]</td>
           ……
       </tr>
       <tr>
           <td>第1个单元格的内容[2,1]</td>
           <td>第2个单元格的内容[2,2]</td>
           ……
       </tr>
   </table>
   ```

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240409201333640-322859124.png" alt="image-20240409201336117" width="200" /><img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410093706849-201476664.png" alt="image-20240410093709496" width="300" />

2. 表格的跨行-合并上下两格

   ```html
   <table >
       <tr>
           <!--rowspan 所跨的行数-->
           <td rowspan="n">&nbsp;</td>
           <td>&nbsp;</td>
       </tr>
       <tr>
       	<td>&nbsp;</td>
       </tr>
   </table>
   ```

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240409201346117-1670008151.png" alt="image-20240409201349051" width="200" />

3. 表格的跨列和跨行

   ```html
   <table border="1px">
       <tr>
           <!--跨列-->
           <td colspan="3">学生成绩</td>
       </tr>
       <tr>
           <!--跨行-->
           <td rowspan="2">张三</td>
           <td>语文</td>
           <td>98</td>
       </tr>
       <tr>
           <td>数学</td>
           <td>95</td>
       </tr>
       <tr>
           <td rowspan="2">李四</td>
           <td>语文</td>
           <td>88</td>
       </tr>
       <tr>
           <td>数学</td>
           <td>91</td>
       </tr>
   </table>
   五行三列
   ```

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240409201355312-150479609.png" alt="image-20240409201358327" width="100" />

## 音频、视频

1. 现在网页上播放视频和音频  的方式

   - 第三方自主开发的播放器
   - Flash(需要插件，速度慢)
   - HTML5媒体元素

2. 视频标签

   ```html
   src：指定要播放的视频文件的路径
   controls：提供播放、暂停和音量的控件(不写就看不见播放选项)
   autoplay：自动播放属性(打开网页就自动播放)
   loop：视频的循环播放
   <video src="视频路径" controls autoplay></video>
   ```

3. 音频标签

   ```html
   src：指定要播放的音频文件的路径
   controls：提供播放、暂停和音量的控件
   <audio src="音频路径" controls autoplay></audio>
   ```

## 页面结构分析

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240409201836043-463869346.png" alt="image-20240409201838220" width="300" /><img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240409201843356-1787014014.png" alt="image-20240409201846265" width="300" />

## 内联框架

1. iframe 单页面内联

   ```html
   src：引用页面地址
   name：框架标识名
   <iframe src="path" name="mainFrame" ></iframe>
   ```

2. iframe 属性（实现页面间的相互跳转）

   ```html
   在被打开的框架上加name属性
   <iframe name="mainFrame"></iframe>
   在超链接上设置target目标窗口属性为希望显示的框架窗口名
   <a href="https://www.baidu.com/" target="mainFrame">加载</a>
   ```

## 总结

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240409202128039-1387849810.png" alt="image-20240409202130850" width="400" />

# 表单

## 表单语法

1. 分别把method的值设置为get和post，然后提交表单，查看页面效果；通过演示可看到method设置不同值时，表单数据在地址栏显示的不同情况
2. get和post两者的区别
   - get方式提交可以在url中看到输入的信息
   - get高效
   - post比较安全，可以传输大文件
   - post方式提交的数据安全性要明显高于get方式提交的数据。因此在实际开发中通常采用post方式提交表单数据。

```html
method: 规定如何发送表单数据。常用值：get post
在实际网页开发中通常采用post方式提交表单数据
action: 表示向何处发送表单数据
<form method="post" action="result.html">
    <p>名字：<input name="name" type="text" > </p>
    <p>密码：<input name="pass" type="password" > </p>
    <p>
        <input type="submit" name="Button" value="提交"/>
        <input type="reset" name="Reset" value="重填"/>
    </p>
</form>
```

## 表单元素

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410105331522-795693383.png" alt="image-20240410105333769" width="350" />

1. 文本框

   ```html
   <!--type="text"
   name：文本框名称(必填)
   value：文本框初始值
   size：文本框长度
   maxlength：文本框可输入最多字符
   -->
   <input type="text" name="userName" value="用户名" size="30" maxlength="20"/>
   ```

2. 密码框

   向密码框中输入字符时，显示的效果，密码字符以黑色实心的圆点来显示。

   ```html
   <!--type="password"
   name：密码框名称(必填)
   size：密码框长度
   -->
   <input type="password" name="pass" size="20"/>
   ```

3. 单选按钮

   同一组单选按钮，name属性值必须相同，才能在选中单选按钮时达到互斥

   ```html
   <!--type="radio"
   name：单选框名称(必填)，一组的名称需要相同
   checked：单选按钮选中状态
   value：单选框的值
   需要显示的值要在input外写出
   -->
   <input name="gen" type="radio" value="男" checked />男
   <input name="gen" type="radio" value="女" />女
   ```

4. 复选框

   同一组复选框，根据需要可设置name属性值相同

   ```html
   <!--type="checkbox"
   name：复选框名称(必填)，一组的名称需要相同
   checked：复选按钮选中状态
   value：复选框的值
   -->
   <input type="checkbox" name="interest" value="sports"/>运动
   <input type="checkbox" name="interest" value="talk" checked />聊天
   <input type="checkbox" name="interest" value="play"/>玩游戏
   ```

5. 下拉列表框

   希望在页面加载时有默认选中的选中项，则必须使用selected属性，如果没有默认选中项则第一个选项默认被选中；

   size的值和selected默认值，这两个属性的理解。

   ```html
   <!-- 一个<select>中至少包含一下<option> -->
   <!--select:下拉列表框-->
   <!--option：选项-->
   <!--selected：默认选中-->
   <select name="列表名称" size="行数">
       <option value="选项的值" selected>…</option >
       <option value="选项的值">…</option >
   </select>
   ```

6. 按钮

   普通按钮是需要添加onclick事件的

   有时会使用图片代替按钮，强调type和src属性，强调“type”属性没有设置为“submit”，但仍然具备提交表单的功能。

   ```html
   <!--普通按钮-->
   <input type="button" name="butButton" value="button按钮"/>
   <!--重置按钮-->
   <input type="reset" name="butReset" value="reset按钮">
   <!--提交按钮-->
   <input type="submit" name="butSubmit" value="submit按钮">
   <!--图片按钮-->
   <input type="image" src="images/login.gif" />
   ```

7. 多行文本域

   改变cols和rows的值，让学员看到由于这两个值的改变，文本框内容显示的改变。强调多行文本域的内容是在标签之间。

   ```html
   <!--
   textarea：多行文本域
   cols：显示的列数
   rows：显示的行数
   文本内容在域内显示
   -->
   <textarea name="showText" cols="x" rows="y">文本内容 </textarea>
   ```

8. 文件域

   在表单中使用文件域时，必须设置表单的“enctype”编码属性为“multipart/form-data”，表示将表单数据分为多部分提交。

   ```html
   enctype：表单编码属性
   <form action="" method="post" enctype="multipart/form-data">
       <p>
           <!--type="file" 文件域-->
           <input type="file" name="files" />
           <input type="submit" name="upload" value="上传" />
       </p>
   </form>
   ```

9. 邮箱

   会自动验证Email地址格式是否正确

   ```html
    邮箱:<input type="email" name="email"/>
   ```

10. 网址

    会自动验证URL地址格式是否正确

    ```html
    请输入你的网址:<input type="url" name="userUrl"/>
    ```

11. 数字

    ```html
    min：最小值
    max：最大值
    step：步长（一次增加或减少的幅度）
    请输入数字:<input type="number" name="num" min="0" max="100" step="10"/>
    ```

12. 滑块

    type值为range即为滑块

    ```html
    请输入数字:<input type="range" name="range1" min="0" max="10" step="2"/>
    ```

13. 搜索框

    type值为search即为搜索框

    ```html
    请输入搜索的关键词:<input type="search" name="sousuo"/>
    ```

## 高级应用

在某些注册页面或本图片中订单信息页面，必须同意一些条款按钮才能使用等等  

1. 隐藏域(类型)

   在浏览器中看不到隐藏域，但是在提交表单时可以看到隐藏域的内容被提交至服务器

   ```html
   可以直接提交默认值value
   <input type="hidden" value="666" name="userid">
   ```

2. 只读、禁用(属性)

   W3C HTML5标准中，规定对于布尔类型的属性，属性值可以省略

   ```html
   <!--
   readonly和disabled
   强调不能单写readonly或disabled，
   必须写readonly＝”readonly”和disabled=“disabled”，
   -->
   <input name="name" type="text" value="张三" readonly>
   <input type="submit" disabled value="保存" >
   ```

3. 表单元素的标注（增强鼠标的可用性）

   自动将焦点转移到与该标注相关的表单元素上

   ```html
   <!--它的for属性对应的id与表单元素id一致-->
   <label for="male">标注的文本</label>
   <input type="radio" name="gender" id="male"/>
   效果：
   点击“标注的文本”，光标会移动到radio框
   ```

## 初级验证

如果用户填写的表单内容不进行验证就发给服务器，那么服务器发现填写的不合法，或是没有填写  - -> 就会返回响应给用户 - -> 用户重新填写再提交，如此多次持续直到用户输入正确。

它们之间的通信是通过网络进行的，如果网络很差，那么注册一个账号就得花很长时间，对用户来说是非常烦的，对服务器来说也增加了其工作压力。

要是有恶意的用户向服务器发送病毒或是有害于服务器安全的程序就更危险了。

1. 表单验证的好处

   - 减轻服务器的压力。
   - 保证数据的可行性和安全性。

   在客户端就对表单进行验证是非常有必要的

2. 表单初级验证的方法

   - placeholder提示语

     提示语默认显示，当文本框中输入内容时提示语消失

     ```html
     用在所有的输入框中
     <input type="search" name="sousuo" placeholder="请输入要搜索的关键字"/>
     ```
   
   - required非空
   
     规定文本框填写内容不能为空，否则不允许用户提交表单
   
     ```html
     <input type="text" name="username" required/>
     ```
   
   - pattern正则表达式
   
     用户输入的内容必须符合正则表达式所指的规则，否则就不能提交表单
   
     ```html
     <input type="text" name="tel" required pattern="^1[358]\d{9}" />
     ```

## 总结

<img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240409203632684-2017806631.png" alt="image-20240409203635255" width="400" />