<img src="https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240313125522878-1816726568.png" alt="image-20240307185543233" style="zoom:25%;" />

# Java预科

## Typora编码

 n级标题：n个## +空格 +回车

**字体（两边加）**： ** 加粗    *  斜体   ***加粗斜体  ~~删除线  

**引用：**>  + 空格 

**分割线：** - - - 或者***

**图片：**！+  [名字]  （路径）

**超链接：** [名字]  + （路径）

**列表：**有序   X + .  +空格   无序  * + 空格

**表格：**  | | |

**代码：** ``` + 语言名称

`阴影`  ：`  + 内容 +  `

## 打开cmd的方式

1. 开始里面
2. shift+ 右键点击文件夹=打开powershell
3. 资源管理器中的路径前  + cmd +空格

## 常用Dos命令

```bash
#查看目录 
dir
#切换目录 
cd  ：/d可以跨盘切换   ../返回上一级
#清理屏幕 
cls：clear screen
#退出终端
exit
#查看IP配置
ipconfig
#应用
calc计算器 mspaint 绘图 notepad 记事本
#Ping
#文件操作
创建目录 md  删除目录rd  
创建文件 cd >a.txt  删除文件  del a.txt
```

## JDK，JRE，JVM

JDK：java development kit

JRE:java runtime environment

JVM:java virtual machine   规范，模拟CPU

## 卸载JDK

1. 删除目录文件
2. 删除高级系统设置的环境变量，Javahome，Path里的Java
3. Java -version

## 安装JDK

1. 搜索JDK8
2. 同意协议
3. 找到对应版本下载安装
4. 记住安装的路径
5. 配置环境变量（JAVA_HOME=java的安装地址..\jdk8）
6. 配置Path变量（%%JAVA_HOM%\bin  和%%JAVA_HOM%\jre\bin ）
7. java-version

## HelloWord

1. 新建一个文件夹

2. 新建一个JAVA文件

   ```java
   //大小写敏感
   //文件名和类名必须一致，首字母大写
   public class hello{
       
   	public static void main(String[] args){
   		System.out.print("hello,word!");
   	}
   }
   ```

3. 在文件目录打开cmd

4. javac  hell.java编译  java hello运行

## Java程序运行机制

1. 编译型compile：有中间代码生成，边解释边执行
2. 解释型

# Java基础语法

## 注释

1. 单行注释： //注释

2. 多行注释：/*  多行注释  */

3. 文档注释：

   ```java
   /**JavaDoc文档注释
   * @Description
   */
   ```

## 标识符和关键字

<img src="https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240313125523631-1428763241.png" alt="image-20240308143927649" style="zoom: 50%;" />

1. 标识符：类名、变量名、方法名。由字母、数字、下划线或$组成。
2. 标识符只能以`$、字母、下划线`开始。`大小写敏感`

## 数据类型讲解

1. （强类型语言）分类

   浮点类型：小数

   long类型：整数后面要加L。与int区别

   float类型：小数后面要加F。与double区别

   char类型：只能一个汉字或一个字母

   `String是引用类型，是类`

   <img src="https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240313125524097-1179834229.png" alt="image-20240308154443290" style="zoom:50%;" />

   ```java
   char='a';
   //字符串不是关键字，是类
   String name="你好";
   ```

2. 一个字节 = 8位 = 8 bit  

   bit -> Byte -> KB -> MB -> GB

## 数据类型扩展

1. 整数扩展： 进制：二进制 0b   十进制   八进制 0  十六进制0x

2. 浮点数扩展：  float：有限 离散 舍入误差  大约 接近但不等于

   可以使用BigDecimal类  

   `最好完全避免使用浮点数进行比较`

3. 字符char扩展:所有字符本质上还是数字 

4. 编码：Unicode  2字节   0 - 65536

   ```java
   //U0000-UFFFF    
   char i = '\u0061';
   ```

5. 转义字符： \t 制表符  \n 换行 

   <img src="https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240313125524525-1485794523.png" alt="image-20240308153527224" style="zoom:50%;" />

6. 比较基本数据类型，则==比较的是数值是否一致

   比较引用数据类型，则==比较的是对象的地址是否一致（false;true）

   equal可以用来比较内容，基本类型不能使用equal进行比较

   new是创建了一个地址

   <img src="https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240313125524873-220196296.png" alt="image-20240308154017754" style="zoom:50%;" />

## 类型转换

1.  （低->高）byte,short,char -> int ->long -> float -> double

   小数优先级 >  整数

2. 不同类型数据先转化为同一类型，然后再进行计算

3. 强制类型转换：高->低

   ```java
   //(类型)变量名
   int i=128;
   byte b = (byte)i; -128   //内存溢出 byte:-128~127
   ```

4. 自动类型转换：低->高

   ```java
   //不需要加任何东西
   int i=128;
   double b = i; 128.0
   ```

5. 注意点：
           `不能对布尔boolean进行转换`
           `不能把对象类型转换为不相干的类型`
          ` 高容量到低容量时，强制转换，可能会内存溢出，或者精度问题`

   ```java
   //操作比较大的数时，注意溢出问题
   //(JDK7 数字之间可以用_分隔)
   int money = 10_0000_0000;
   int year =20;
   int total =money * year;  //-1474836480，溢出
   long total2 =money * year;  //溢出,默认是int，转换之前已经存在问题了
   
   long total3 =money * (long)year;  //先把一个数转换为long
   System.out.println(total3);
   ```

## 变量、常量、作用域

1. Java变量是最基本的存储单元，包含：变量名，变量类型和`作用域`

   ```JAVA
   	type  varName [=value]  [{,varName[=value]}];
   	int a=1,b=2,c=3;//不推荐
   int a=1;
   int b=2;
   int c=3;
   //数据类型 变量名 = 值；可以使用都好隔开来声明多个同类型变量
   ```

   注意：`每个变量都有类型，类型可以是基本类型，也可以是引用类型`

   ​			`变量名必须先是合法的标识符`

   ​			 `变量声明是一条完整的语句，因此每一个声明都必须以分号结束`

2. 变量作用域：

   类名：首字母大写和驼峰原则。

   方法名和变量名：首字母小写和驼峰原则。

   - 类变量（写在类里面）：static。（所有方法可以调用静态变量，非静态方法可以调用静态变量和非静态变量）
   - 实例变量（写在类里面，方法的外面，从属于对象）：无static，如果不初始化会输出默认值（int=0, String =null, boolean =false）
   - 局部变量（方法里面）：必须声明和初始化值。

   ```java
   public class Variable{
       static int all=0;	//类变量
       string str = "hello world!";	//实例变量
       public void method(){
           int i=0;	//局部变量
       }
   }
   ```

3. 常量**final**：初始化后不能再改变值。（特殊的变量）

   ```java
   //常量名一般使用大写字符和下划线
   final 常量名 = 值；
   final double PI =3.14;
   ```

4. 修饰符**static** ，不存在先后顺序

## 基本运算符

全部选中：点第一个，shift不放，点最后一个。

直接复制当前行到下一行：ctrl + D

1. 算术运算符：+，-，*，/，%，++，- -

   注意：`b = a++: b=a a=a+1  执行这行代码后，先给b赋值，再自增`

   ​			`c = ++a: a=a+1 c=a 执行这行代码前，先给a自增，在给c赋值`

   ```java
   //幂运算：a^b 
   pow = Math.pow(a,b)
   ```

2. 赋值运算符：=

3. 关系运算符：>，<， >=，<=，==，!=，instance of

4. 逻辑运算符：&&，||，！

   ```java
   //短路运算
   int c=5;
   boolean d =(c<4) && (c++<4);
   //d=false;c=5
   //d为假，后面不被执行，c值不变
   ```

5. 位运算符：&，|，^异或，~，>>右移，<<左移，>>>

   ```java
   <<  *2
   >>  /2
   ```

6. 条件运算符：?:（三元运算符）

   ```java
   //x ? y : z
   如果x==true，则结果为y，否则为z
   ```

7. 扩展赋值运算符：+=，-=，*=，/=

8. 字符串连接符：+ 

   ```java
   //+前侧有String,就会被当成字符串连接
   int a=10;
   int b=20;
   System.out.println(a+b);	//30
   System.out.println(""+a+b);	//1020
   System.out.println(a+b+"");	//30 先计算a+b后与字符串连接
   ```

## JavaDoc生成文档

1. javadoc 命令式用来自己生成API文档的

2. 文档注释/**可以自动显示信息

3. 在IDEA可以利用Tool里面的Generate JavaDoc

   在Doc.java打开终端生成doc文档，打开index.html

   ```shell
   javadoc -encoding UTF-8 -charset UTF-8 Doc.java
   ```

   | @author  | 作者名                    |
   | -------- | ------------------------- |
   | @version | 版本号                    |
   | @since   | 指明需要最早使用的JDK版本 |
   | @param   | 参数名                    |
   | @returns | 返回值情况                |
   | @throws  | 异常抛出情况              |

# 流程控制

## 用户交互Scanner

1. java.util.Scanner获取用户的输入

   ```java
   public static void main(String[] args) {
           //创建一个扫描器对象，用于接受键盘数据
           Scanner scanner = new Scanner(System.in);
           System.out.println("使用next方式接收：");
           //判断用户有没有接受字符串
           if(scanner.hasNext()){
               String str=scanner.next();
               System.out.println("输入的内容为："+str);
           }
           //凡是属于IO流的类如果不关闭会一直占用资源，用完就关掉
           scanner.close();
       }
   ```

2. Scanner对象

   ```java
   Scanner scanner = new Scanner(System.in);
   String str=scanner.next();	//输入hello world会变成hello
   System.out.println("输入的内容为："+str);
    scanner.close();
   
   //判断是否
   scanner.hasNextLine(); //有下一行
   scanner.hasNextInt();  //是整数
   ```

   <img src="https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240313125525281-1714945654.png" alt="image-20240310144657070" style="zoom:50%;" />

## 顺序结构

## if选择结构

1. if单选择结构

   ```java
   if(布尔表达式){
       //如果布尔表达式为true将执行的语句
   }
   ```

2. if双选择结构

   ```java
   if(布尔表达式){
       //如果布尔表达式为true将执行的语句
   }else{
       //如果布尔表达式为false将执行的语句
   }
   ```

3. if多选择结构

   ```java
   //一个为true，其他的就跳过不执行
   if(布尔表达式1){
       //如果布尔表达式1为true将执行的语句
   }else if(布尔表达式2){
       //如果布尔表达式2为true将执行的语句
   }else if(布尔表达式3){
       //如果布尔表达式3为true将执行的语句
   }else{
       //如果布尔表达式都不为true将执行的语句
   }
   ```

4. 嵌套的if结构

   ```java
   if(布尔表达式1){
       //如果布尔表达式1为true将执行的语句
       if(布尔表达式2){
       //如果布尔表达式2为true将执行的语句 
   	}
   }
   ```

5. Switch多选择结构

   ```java
   //变量可以是byte,short,int,char,String
   //case必须是字符串常量或字面量
   //case穿透：每一个case后要加break
   switch(expression){
       case value:
           //语句；
           break;//可选
       case value:
           //语句；
           break;//可选
       default://可选
           //语句
   }
   ```

## while 

```java
//布尔表达式为进入循环的标志
while(布尔表达式){
    //循环内容
}
```

## Do-while  

```java
//即使不满足条件也至少执行一次
do{
    //循环内容
}while(布尔表达式)
```

## For 

```java
//循环次数在执行前就已经确定
//初始化：可一个或多个循环控制变量，也可以是空语句
for(int x:array){}

for(初始化;布尔表达式;更新){
    //代码语句
}
//死循环
for(;;){}
```

## 打印九九乘法表 

```java
//行
for (int j = 1; j <=9; j++) {
//列
	for (int i = 1; i <= j; i++) {
    	System.out.print(j+"*"+i+"="+(1*i)+"\t");
	}
    System.out.println();
}
```

## 增强For循环 

```java
//遍历数组中的元素
int [] numbers={10,20,30};
for(int x:numbers){
    System.out.println(x);
}
//等价于
for(int i=0;i<3;i++){
    System.out.println(numbers[i]);
}
```

## break、continue 

1. break:强行退出循环，不执行循环中剩余的语句
2. continue:终止某次循环过程，跳过循环体中尚未执行的语句，接着进行下一次是否执行循环的判定

## 打印三角形

```java
//打印五行三角形
for (int i = 1; i <= 5; i++) {
    for (int j = 5; j >= i; j--) {
        System.out.print(" ");
    }
    for (int j = 1; j <=i; j++) {
        System.out.print("*");
    }
    for (int j = 1; j < i; j++) {
        System.out.print("*");
    }
    System.out.println();
}
```

# 方法

## 定义和调用 

1. 方法是语句的集合，在一起执行`一个功能`。(方法的原子性)

2. 方法包含于类或对象中；方法在程序中被创建，在其他地方被引用。

3. 一个方法包含一个方法头和一个方法体

   ```java
   修饰符 返回值类型 方法名（参数类型  参数名）{
       ....
       方法体
       ...
       return 返回值；
   }
   ```

   - 修饰符（可选）：public，static，final
   - 返回值类型：void无返回值
   - 参数类型（可选）：实参（调用方法时实际传给方法的数据），
   - 形式参数（在方法被调用时用于接受外界输入的数据）。实参和形参类型要对应
   - return返回值，终止方法；

4. 调用的两种方式：对象名.方法名(实参列表)

   - 当方法有返回值时。方法调用通常被当作一个值
   - 当方法没有返回值时。方法调用一定是一条语句。

5. 值传递（Java）和引用传递。

6. 静态方法

   `被static修饰的内容会跟随类的加载而加载，`

   `所以静态化的内容可以不用实例化就直接调用。`

   `同时两个静态方法之间也可以相互调用。`

7. 值传递和引用传递

   ```java
   //值传递
   public static void main(String[] args) {
       int a = 1;
       Demo04.change(a);
       System.out.println(a);  //a=1
   }
   public static void change(int a) {
       a = 10;
   }
   
   //引用传递
   public class Demo05 {
       public static void main(String[] args) {
           Person person = new Person();
   
           System.out.println(person.name); //name=null
   
           Demo05.change(person);
   
           System.out.println(person.name); //name=nihao
       }
   
       public static void change(Person person) {
           //person是一个对象，指向的是Person person = new Person(); 具体的人 可以改变属性
           person.name = "nihao";
       }
   }
   
   //定义了一个person类，有一个属性：name
   class Person {
       String name;
   }
   ```

## 重载 

在一个类中，有相同的方法名，但是参数列表不同，返回类型可同可不同。

## 命令行传递参数 

希望运行一个程序时再传递给它消息，这要靠传递命令行参数给main()函数实现.

要在src目录下加载class文件

## 可变参数 

1. 不定向参数。在指定参数类型后加一个…

2. 一个方法中只能指定一个可变参数，它必须是方法的最后一个参数。

   ```java
   public static void main(String[] args) {
           Demo02 demo02 = new Demo02();
           demo02.printMax(1,2,3,5);
       }
   public static  void printMax(double ... max){
       System.out.println(max[3]);
   }
   //输出5.0
   ```

## 递归

1.  递归：自己调用自己。适用于基数比较小的

2. 递归包含：递归头（什么时候不调用自身方法）和递归体（什么时候调用自己方法）。

   ```java
   //阶乘
   public static void main(String[] args) {
           //阶乘
           System.out.println(f(3));
       }
   public static int f(int n){
       if(n==1) return 1;
       else{
           return n*f(n-1);
       }
   }
   ```

# 数组

## 数组的声明和创建 

1. 数组：相同类型数据的有序集合。必须声明数组，才能在程序中使用数组。

2. 声明数组

   ```java
   dataType[] arrayRefVar;	//首选
   dataType arrayRefVar[];
   ```

3. 创建数组

   ```java
   dataType[] arrayRefVar = new dataType[arraySize];
   int[] num=new int[10];
   int[] num={1,2,3};
   ```

4. 数组元素是通过索引访问的，数组索引从0开始。

5. 获取数组长度：arrays.length

## 三种初始化及内存分析 

1. 内存分析

   <img src="https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240313125525760-1392213261.png" alt="image-20240310184541839" style="zoom:50%;" /><img src="https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240313125526140-1413494935.png" alt="image-20240310185341476" style="zoom:50%;" />

2. 三种初始化：静态初始化，动态初始化，数组的默认初始化

   动态初始化：包括默认初始化

   <img src="https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240313125526572-1198048092.png" alt="image-20240310185517025" style="zoom:50%;" />

## 下标越界 

1. 数组的四个基本特点

   <img src="https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240313125527036-1616974752.png" alt="image-20240310184435077" style="zoom:50%;" />

2. 数组边界

   <img src="https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240313125527470-1329287806.png" alt="image-20240310190321214" style="zoom:53%;" />

3. 数组小结：

   - 数组是相同数据类型(数据类型可以为任意类型)的有序集合数组也是对象。
   - 数组元素相当于对象的成员变量
   - 数组长度是确定的，不可变的。如果越界，则报:ArraylndexOutofBounds

## 反转数组

```java
public static int[] reverse(int[] arrays){
        int[] result =new int[arrays.length];
        for (int i = 0,j=result.length-1; i < arrays.length; i++,j--) {
            result[j]=arrays[i];
        }
        return result;
    }
```

## 二维数组 

定义

```java
int a[][]=new int[2][5];
int[][] array={{1,2}, {2,3}, {3,4}, {4,5}};
```

## Arrays类 

1. <img src="https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240313125527852-981486525.png" alt="image-20240310193017114" style="zoom:50%;" />

2. 常用功能

   <img src="https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240313125528295-1495651361.png" alt="image-20240310193049086" style="zoom:50%;" />

3. 常用方法

   ```java
   //打印数组元素
   System.out.println(Arrays.toString(array));
   //排序
   Arrays.sort(array);
   System.out.println(Arrays.toString(array));
   //填充
   Arrays.fill(array,2,4,0);
   System.out.println(Arrays.toString(array));
   ```

## 冒泡排序 

八大排序

```java
//冒泡排序
//1.比较数组中，两个相邻的元素，如果第一个数比第二个数大，就交换位置
//2.每次比较都会产生一个最大或最小的数字
//3.下一轮则可以少一次排序
public static int[] sort(int[] array){
    //临时变量
    int temp=0;

    //1.外层循环，判断要走多少次
    for (int i = 0; i < array.length-1; i++) {
/*优化
boolean flag=false;//通过flag标识位减少没有意义的比较
*/
        //2.内层循环，比较两个数，如果第一个数比第二个数大，就交换位置
        for (int j = 0; j < array.length-1-i; j++) {
            if(array[j]>array[j+1]){
                temp=array[j];
                array[j]=array[j+1];
                array[j+1]=temp;
            }
        }
/*优化
if(flag==false) break;  
*/    
    }
    return array;
}
```

## 稀疏数组 

1. 原始数组与稀疏数组

   <img src="https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240313125528803-946733156.png" alt="image-20240310195859712" style="zoom:50%;" />

```java
//1.创建一个二维数组11*11  0：没有棋子  1：黑棋  2：白棋
int[][] array1=new int[11][11];
array1[1][2]=1;
array1[2][3]=2;
System.out.println("输出原始的数组：");
for (int[] ints : array1) {
    for (int anInt : ints) {
        System.out.print(anInt+"\t");
    }
    System.out.println();
}

//转换为稀疏数组
//1.获取有效值的个数
int sum=0;
for (int i = 0; i < 11; i++) {
    for (int j = 0; j < 11; j++) {
        if(array1[i][j]!=0) sum++;
    }
}
System.out.println("有效值的个数为："+sum);
//2.创建一个有效数组
int[][] array2=new int[sum+1][3];
array2[0][0]=11;
array2[0][1]=11;
array2[0][2]=sum;
//3.遍历二维数组，将非零的值，存在稀疏数组中
int count=0;
for (int i = 0; i < array1.length; i++) {
    for (int j = 0; j < array1[i].length; j++) {
        if(array1[i][j]!=0){
            count++;
            array2[count][0]=i;
            array2[count][1]=j;
            array2[count][2]=array1[i][j];
        }
    }
}
//输出稀疏数组
for (int i = 0; i < array2.length; i++) {
    System.out.println(array2[i][0]+"\t"
                       +array2[i][1]+"\t"
                       +array2[i][2]+"\t");
}

//还原
//1.读取稀疏数组
int[][] array3=new int[array2[0][0]][array2[0][1]];
//2.给其中的元素还原值
for (int i = 1; i < array2.length; i++) {
    array3[array2[i][0]][array2[i][1]]=array2[i][2];
}
//
System.out.println("输出还原的数组：");
for (int[] ints : array3) {
    for (int anInt : ints) {
        System.out.print(anInt+"\t");
    }
    System.out.println();
}
```

# 面向对象编程OOP

面向对象编程的本质就是：`以类的方式组织代码，以对象的组织封装数据`。

面向对象的三大特性：`封装、继承、多态`。

## 类与对象的创建 

使用new关键字创建：会分配内存，给创建好的对象进行默认的初始化以及对类中构造器的调用

```java
public static void main(String[] args) {
    //类是抽象的，实例化
    //类实例化后会返回一个自己的对象
    //student对象就是一个Student类的具体实例
    Student xiaoming = new Student();
    Student xh = new Student();

    xiaoming.name = "xiaoming";
    xiaoming.age = 3;
    System.out.println(xiaoming.name); //xiaoming
    System.out.println(xiaoming.age);  //3
}
//学生类=属性+方法
public class Student {
    //属性：字段
    String name;
    int age;
    //方法
    public void study() {
        System.out.println(this.name + "在学习。");
    }
}
```

## 构造器详解 

1. 类中的构造器也称构造方法，是在创建对象时必须要调用的。

2. 并且构造器有以下两个特点

   - 必须和类的名字相同
   - 必须没有返回类型，也不能写void

   作用：

   - new本质在调用构造方法
   - 初始化对象的值

   ![image-20240312113020224](https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240313125529195-1546967420.png)

3. 定义有参构造之后，如果想使用无参构造，显示的定义一个无参的构造

4. 自动创建构造方法：alt+insert

   点击name是有参构造方法，点击Select None是无参构造函数

   <img src="https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240313125529580-553320740.png" alt="image-20240312130814449" style="zoom:33%;" />

   ```java
   this.x = x
   ```

## 创建对象内存分析 

<img src="https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240313125530005-1419953117.png" alt="image-20240312133046531" style="zoom:40%;" />

```java
public class Pet {
    public String name;
    public int age;

    //无参构造
    public void shout() {
        System.out.println("叫了一声！");
    }
}
public static void main(String[] args) {
    Pet dog = new Pet();
    dog.name = "旺财";
    dog.age = 3;
    dog.shout();

    System.out.println(dog.name);	//输出：叫了一声！  “旺财”  3
    System.out.println(dog.age);

    Pet cat = new Pet();

}
```

## 类与对象 

1. 类是一个模版，对象是一个具体的实例。

2. 方法：定义和调用。

3. 对象的引用

   引用类型：	基本类型(8)

   对象是通过引用来操作的：栈-->堆(地址)

4. 属性：字段 Field   成员变量

   默认初始化：数值-0/0.0   char-u0000  boolean-false  引用：null

   赋值：修饰符 属性类型 属性名 = 属性值

5. 对象的创建和使用

   -必须使用new关键字创建对象，构造器  Person xiaoming = new Person();

   -对象的属性  xiaoming.name

   -对象的方法  xiaoming.sleep()

6. 类：静态的属性和动态的行为(方法)

## 封装

1. 意义：

   - 提高程序的安全性，保护数据。
   - 隐藏代码的实现细节
   - 统一接口
   - 系统可维护性增加

2. 高内聚（类的内部数据操作细节自己完成），低耦合（仅暴露少量的方法给外部使用）

3. 封装（数据的隐藏）：通常，应禁止直接访问一个对象中数据的实际表示，而是应该通过操作接口来访问，这成为信息隐藏。

4. **属性私有：get/set。**（防止脏数据传给属性）

   读“脏”数据：事务1修改某一数据，并将其写回磁盘；事务2读取同一数据后，事务1由于某种原因被撤消，这时事务1已修改过的数据恢复原值，事务2读到的数据就与数据库中的数据不一致，是不正确的数据，又称为“脏”数据。

5.  alt + insert 自动生成方法

## 继承 extends

1. 继承的本质是对某一批类的抽象。
2. Java中只有单继承，没有多继承。 接口可以多继承。
3. 继承是类和类之间的一种关系。除此之外类和类之间的关系还有依赖、组合、聚合等。
4. 子类继承父类，extends。子类是父类的扩展。
5. 在Java中所有的类都默认直接或间接继承object类。
6. ctrl + H：打开层次结构

## Super 

1. super调用父类的构造方法，必须在构造方法的第一个。
2. super必须只能出现在子类的方法或构造方法中！
3. super和this不能同时调用构造方法。
4. super和this的区别：
   - 代表的对象不同：this-本身调用者这个对象。	super-代表父类对象的调用。
   - 前提：this-没有继承也可以使用。     super-只能在继承条件下才可以使用。
   - 构造方法：this()-本类的构造。     super()：父类的构造。

## 方法重写 Override

1. 重写都是方法的重写和属性无关。重写只和非静态方法有关。

2. Override重写：@Override   //注解：有功能的注释！

   ```Java
   //前提是：A和B里的test()都是静态方法
   public static void main(String[] args) {
       //静态方法：方法的调用只和左边，定义的数据类型有关
       A a = new A();
       a.test();  //A=>test()
   
       //父类的引用指向的子类实现
       //向上转型
       B b = new A();
       b.test(); //B=>test()
   }
   =================================================
   //前提是：A和B里的test()都不是静态方法
   public static void main(String[] args) {
       //静态方法和非静态的方法区别很大！
       A a = new A();
       a.test();  //A=>test()
   
       //子类重写了父类的方法
       B b = new A();
       b.test(); //A=>test()
   }
   ```

   - 有static时，b调用的是B类的方法，因为b是用B类定义的。静态方法在类里加载。
   - 没有static时，b调用的是对象的方法，而b是用A类new的

3. 重写需要有继承关系，子类重写父类的方法！

   子类和父类的方法要求：

   - 方法名必须相同
   - 参数列表必须相同
   - 修饰符：范围可以扩大，但不能缩小。public > protected > default > private
   - 抛出的异常：范围可以被缩小，但不能扩大；ClassNotFoundException - -> Exception（大）

4. 重写：子类的方法和父类必须要一致，方法体不同！

5. 重写的意义：

   - 父类的功能，子类不一定需要，或者不一定满足！
   - Alt + insert + Override 自动重写方法

6. 不能重写的：

   - static 方法，属于类，它不属于实例
   - final 常量
   - private方法；

## 多态 

1. 动态编译：类型：可扩展

2. 同一方法根据发送对象的不同而采用多种不同的行为方式。

3. 一个对象的实际类型是确定的，但可以指向对象的引用类型有很多

4. 多态存在的条件：

   - 有继承关系(否则会有类型转换异常，ClassCastException)
   - 子类重写父类方法
   - 父类引用指向子类的对象

5. 多态是方法的多态，属性没有多态性。

6. 父类的引用指向子类，子类重写了父类的方法就调用重写的方法，否则还是父类的方法

7. 

   ```java
   public static void main(String[] args) {
       //一个对象的实际类型是确定的
       // new Student();
   
       //可以指向的引用类型不确定:父类的引用指向子类
   
       //子类能调用的方法都是自己的，或者继承父类的
       Student s1 = new Student();
       //父类可以指向子类，但不能调用子类独有的方法
       Person s2 = new Student();
       Object s3 = new Student();
   
       //对象能执行哪些方法，主要看对象左边的类型，和右边关系不大！
       //子类重写了父类的方法，执行子类的方法
       s2.run();   //son
       s1.run();  //son
   }
   ```

## instanceof和引用类型转换 

instanceof

```java
System.out.println(X instanceof Y);
//能否编译通过，X和Y之间是否有父子关系
```

```java
public static void main(String[] args) {
    //Object > Person >Student
    //Object > Person >Teacher
    //Object > String
    Object object = new Student();
    System.out.println(object instanceof Student);  //true
    System.out.println(object instanceof Person);   //true
    System.out.println(object instanceof Object);   //true
    System.out.println(object instanceof Teacher);  //false
    System.out.println(object instanceof String);   //false
    System.out.println("=======================================");
    Person person = new Student();
    System.out.println(person instanceof Student);  //true
    System.out.println(person instanceof Person);   //true
    System.out.println(person instanceof Object);   //true
    System.out.println(person instanceof Teacher);  //false
    // System.out.println(person instanceof String);   //编译报错
    System.out.println("============================================");
    Student student = new Student();
    System.out.println(student instanceof Student);  //true
    System.out.println(student instanceof Person);   //true
    System.out.println(student instanceof Object);   //true
    //System.out.println(student instanceof Teacher);  //编译报错
    //System.out.println(student instanceof String);   //编译报错
}
```

高转低

```java
public static void main(String[] args) {
    //类型之间的转化：基本类型转换 高低64 32 16 8
    //父高 自低
    Person obj = new Student();
    //student.go(); Person里面没有go方法
    //把Person类型转换为Student类型

    //Student student = (Student) obj;
    //student.go();

    ((Student) obj).go();
}
```

低转高

```java
public static void main(String[] args) {
    Student student = new Student();
    student.go();
    //低转高
    Person person = student;
    //person.go();  错误
    //子类转换为父类，可能丢失自己本来的一些方法
}
```

1. 父类的引用指向子类的对象
2. 把子类转换为父类，向上转型；会丢失方法
3. 把父类转换为子类，向下转型；强制转换；

## static 

1. 静态的一直存在，非静态的需要打开（实例）了才能用

2. 代码块

   ```java
   public class Person {
       //2.赋初始值
       {
           System.out.println("匿名代码块");
       }
       //1.只执行一次
       static {
           System.out.println("静态代码块");
       }
   	//3.
       public Person() {
           System.out.println("构造方法");
       }
       public static void main(String[] args) {
           Person person1 = new Person();
           //静态代码块
           //匿名代码块
           //构造方法
           System.out.println("=====================");
           Person person2 = new Person();
           //匿名代码块
           //构造方法
       }
   }
   ```

3. 静态导入包

   ```java
   import static java.lang.Math.random;
   import static java.lang.Math.PI;
   
   public class Teacher {
       public static void main(String[] args) {
           System.out.println(random());
           System.out.println(PI);
       }
   }
   ```

4. final修饰的类，不可以被继承

## 抽象类 abstract

1. abstract修饰符可以用来修饰类，也可以用来修饰方法。

   如果修饰方法,那么该方法就是抽象方法；如果修饰类，那么该类就是抽象类。

2. 抽象类中可以没有抽象方法，但是有抽象方法的类一定要声明为抽象类。

3. 抽象类，不能使用new关键字来创建对象，它是用来让子类继承的。

4. 抽象方法，只有方法的声明，没有方法的实现，它是用来让子类实现的。

5. 子类**继承**抽象类（单继承），那么就必须要实现抽象类没有实现的抽象方法，否则该子类也要声明为抽象类。

<img src="https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240315124949958-327271069.png" alt="image-20240315124954004" style="zoom:33%;" />

## 接口 interface

1. 普通类：只有具体实现

2. 抽象类：具体实现和规范(抽象方法)都有!不能new

3. 接口：只有规范!自己无法写方法。专业的抽象。约束和实现分离：面向接口编程

4. 接口中的所有定义的方法都是抽象的，默认 public abstract

   接口中的所有定义的属性都是静态常量，默认public static final 

5. 接口就是规范，定义的是一组规则。

   体现了现实世界中“如果你是...则必须能...”的思想。

   **接口的本质是契约**，就像我们人间的法律一样。制定好后大家都遵守。

6. OO的精髓，是对对象的抽象，最能体现这一点的就是接口。为什么我们讨论设计模式都只针对具备了抽象能力的语言（比如c++、java、c#等)，就是因为设计模式所研究的，实际上就是如何合理的去抽象。

7. 声明类的关键字是class，声明接口的关键字是interface，实现接口的关键字是implements

8. 接口的作用：

   - 约束
   - 定义一些方法，让不同的人实现
   - 接口不能被实例化；接口中没有构造方法
   - implements可以实现多个接口
   - 必须要重写接口中的方法

<img src="https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240315131422401-764174521.png" alt="image-20240315131427618" style="zoom:40%;" />

## N种内部类 

1. 内部类就是在一个类的内部在定义一个类。

   比如，A类中定义一个B类，那么B类相对A类来说就称为内部类，而A类相对B类来说就是外部类了。

2. 内部类可以访问外部类的私有属性，私有方法

   <img src="https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240315143044508-2084173325.png" alt="image-20240315143050010" style="zoom:33%;" />

3. 分类：

   - 成员内部类：有一个隐式引用，指向外部类对象，可以访问外部类的成员；构造一个内部类对象new OuterClass().new Innerclass()；内部类声明的static字段必须都是final；内部类不能有static方法。

   - 静态内部类

   - 局部内部类：定义在方法内部的类；局部内部类不能有访问修饰符（private或public）

   - 匿名内部类：没有名字

     <img src="https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240315145008507-1758263221.png" alt="image-20240315145014141" style="zoom:33%;" />

4. 内部类可以访问static，但是static的内部类不能访问外部非static属性

# 异常Exception

1. 异常指程序运行中出现的不期而至的各种状况。

   如:文件找不到、网络连接失败、非法参数等。

2. 异常发生在程序运行期间，它影响了正常的程序执行流程。

3. 异常体系结构

   <img src="https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240315150909813-280062861.png" alt="image-20240315150915285" style="zoom:40%;" />

4. 异常分类：

   - 检查性异常：最具代表的检查性异常是用户错误或问题引起的异常，这是程序员无法预见的。例如要打开一个不存在文件时，一个异常就发生了，这些异常在编译时不能被简单地忽略。
   - 运行时异常：运行时异常是可能被程序员避免的异常。与检查性异常相反，运行时异常可以在编译时被忽略。
   - 错误ERROR：错误不是异常，而是脱离程序员控制的问题。错误在代码中通常被忽略。例如，当栈溢出时，一个错误就发生了，它们在编译也检查不到的。

## Error和Exception

Error

1. Error类对象由Java虚拟机生成并抛出，大多数错误与代码编写者所执行的操作无关。
2. Java虚拟机运行错误(Virtual MachineError)，当JVM不再有继续执行操作所需的内存资源时，将出现OutOfMemoryError。这些异常发生时，Java虚拟机(JVM)一般会选择线程终止;
3. 还有发生在虚拟机试图执行应用时，如类定义错误(NoClassDefFoundError)、链接错误(LinkageError)。这些错误是不可查的，因为它们在应用程序的控制和处理能力之外，而且绝大多数是程序运行时不允许出现的状况。

Exception

1. 在Exception分支中有一个重要的子类RuntimeException(运行时异常)
   这些异常是不检查异常，程序中可以选择捕获处理，也可以不处理。
2. 这些异常一般是由程序逻辑错误引起的，程序应该从逻辑角度尽可能避免这类异常的发生;

Error和Exception的区别:

1.  Error通常是灾难性的致命的错误，是程序无法控制和处理的，当出现这些异常时，Java虚拟机(JVM)一般会选择终止线程;
2. Exception通常情况下是可以被程序处理的，并且在程序中应该尽可能的去处理这些异常。

## 捕获和抛出异常

`选中-> Ctrl + Alt + T :自动生成包裹代码块`

try、catch、finally、throw、throws

```java
//finally 可以不要finally。 IO，资源，关闭。
public static void main(String[] args) {
    int a = 1;
    int b = 0;

    try {   //try监控区域
        System.out.println(a / b);
    } catch (ArithmeticException e) { //catch(想要捕获的异常类型)捕获异常
        System.out.println("程序出现异常，变量b不能为0！");
    } finally {  //处理善后工作
        System.out.println("finally");
    }
}
```

要捕获多个异常，需要从小到大的捕获！

```java
try {   //try监控区域
    System.out.println(a / b);
    //new Test().a();
} catch (Error e) { //catch(想要捕获的异常类型)捕获异常
    System.out.println("Error");
} catch (Exception e) {
    System.out.println("Exception！");
} catch (Throwable t) {
    System.out.println("Throwable");
} finally {  //处理善后工作
    System.out.println("finally");
}
```

throw和throws

```java
//假设这个方法中，处理不了这个异常。方法上抛出异常
public void test(int a, int b)throws ArithmeticException{
    if (b == 0) {
        //主动的抛出异常  一般用在方法中
        throw new ArithmeticException();
    }
    //System.out.println(a / b);
}
```

## 自定义异常

1. 用户自定义异常类，只需继承Exception类即可。

2. 在程序中使用自定义异常类，大体可分为以下几个步骤:

   - 创建自定义异常类。

     <img src="https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240315160413841-1531756888.png" alt="image-20240315160419536" style="zoom:33%;" />

   - 在方法中通过throw关键字抛出异常对象。

   - 如果在当前抛出异常的方法中处理异常，可以使用try-catch语句捕获并处理；否则在方法的声明处通过throws关键字指明要抛出给方法调用者的异常，继续进行下一步操作

     <img src="https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240315160338139-1241847922.png" alt="image-20240315160343333" style="zoom:33%;" />

   - 在出现异常方法的调用者中捕获并处理异常。

     <img src="https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240315160945188-914488703.png" alt="image-20240315160951036" style="zoom:33%;" /><img src="https://img2023.cnblogs.com/blog/3406637/202403/3406637-20240315161009166-103809263.png" alt="image-20240315161014982" style="zoom:43%;" />

3. 经验总结

   - 处理运行时异常时，采用逻辑去合理规避同时辅助try-catch处理

   - 在多重catch块后面，可以加一个catch (Exception)来处理可能会被遗漏的异常。 

     Error < Exception < Throwable

   - 对于不确定的代码，也可以加上try-catch，处理潜在的异常

   - 尽量去处理异常，切忌只是简单地调用printStackTrace()去打印输出

   - 具体如何处理异常，要根据不同的业务需求和异常类型去决定

   - 尽量添加finally语句块去释放占用的资源。IO-Scanner
