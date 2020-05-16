一、PHP 简介

PHP（全称 Hypertext Preprocessor，超文本预处理器的字母缩写）是一种服务器端脚本语言，它可嵌入到 HTML 中，尤其适合 WEB 开发。

一个简单的 PHP 文件示例

```php
<html>
  <head>
    <title>Example</title>
  </head>

  <body>
    <p>
    <?php echo 'Hello ShiYanLou!';?>
    </p>
  </body>
</html>
```

其中被 `` 包围的内容即是 PHP 程序，在装了 PHP 解释器的服务端运行该文件时，PHP 程序能够被解析到 HTML 页面中，上例中结果为

```html
<html>
  <head>
    <title>Example</title>
  </head>

  <body>
    <p>
    Hello ShiYanLou!
    </p>
  </body>
</html>
```



####       PHP 能做什么    

- **服务端脚本**。这是 PHP 最传统，也是最主要的目标领域。需要具备以下三点：PHP 解析器（CGI 或者服务器模块），WEB 服务器（如 Nginx，Apache）和浏览器。
- **命令行脚本**。可以编写一段 PHP 脚本，并且不需要任何服务器或者浏览器来运行它，仅需要 PHP 解析器来执行。
- **桌面应用程序**。桌面应用程序可以通过 PHP 高级特性 PHP-GTK 来编写。PHP-GTK 是 PHP 的一个扩展，在通常发布的 PHP 包中并不包含它。



- 命令行脚本文件

在目录 `/home/shiyanlou` 创建 hello.php，并编辑内容

```php
<?php 
echo 'Hello World';
```

命令行执行

```
$ php hello.php
Hello World
```

也可以在 PHP 脚本第一行加入 PHP 路径，通过 `./hello` 执行

编辑文件 hello

```
$ cd /home/shiyanlou
$ vim hello
#!/usr/bin/php
<?php

echo 'Hello World';
```

保存退出后，修改文件权限

```
$ chmod +x hello
$ ./hello
Hello World
```

- 交互模式

```
$ php -a
php > $a = 'Hello World';
php > echo $a;
Hello World
```

- 直接运行代码

```
$ php -r "echo 'Hello World!';"
Hello World
```

- 服务器端
  - cgi，如 Nginx 的 fast-cgi。
  - 模块，如 Apache 的 mod_php。

在开发和测试阶段还可以使用 PHP 内置服务器

```
$ cd /home/shiyanlou
$ php -S localhost:8080
```

端口号 8080 可以自定义

浏览器输入 http://localhost:8080/hello.php

```html
Hello World
```



# 二、基本语法

​      实验知识点    

- PHP 标记
- 从 HTML 中分离
- 指令分隔符
- 注释

 通常 PHP 标记为 ` 和 `?>`，输出内容可使用短格式 ` 和 `?>`。所有这些标签内的部分都会被 PHP 解析器解析。

如果文件内容是纯 PHP 代码，最好在文件末尾删除 PHP 结束标记。这可以避免在 PHP 结束标记之后万一意外加入了空格或者换行符，会导致 PHP 开始输出这些空白。

```php
<?php

$a = 'Hello';
echo "$a World"; 
```

注意文件末尾省略了结束标签 `?>`

在 PHP5.4 以后短标签无需任何设置，都是合法标签。

```php
<?="Hello Wolrd"?>
```

注意：在 PHP7 中以下两个标记方式已经不再适用

- <script language='php'></script>

- ```
  <% 和 %>
  ```



## 从 HTML 中分离    

通常情况下可以使用 `echo` 输出 HTML 页面

*Practice*

编辑 `/home/shiyanlou/html.php`

```php
<?php

$highlight = true;

echo "<html>
  <body>
    <p".($highlight ? " class='highlight'" : '').">
    This is a paragraph
    </p>
  </body>
</html>";
```

命名行执行

```
$ php html.php
```

由于在一对开始和结束标记之外的内容都会被 PHP 解析器忽略，我们可以在 HTML 中需要使用 PHP 的地方在执行 PHP 程序，因此，上面例子可以写成

```php
<?php 
$highlight = true;
?>
<html>
  <body>
    <p <?=$highlight ? "class='highlight'" : ''?>>
    This is a paragraph
    </p>
  </body>
</html>
```

命名行执行

```
$ php html.php
```

两次执行的结果是一样的，因此可以使用第二种方式将 PHP 代码嵌入到 HTML 中，避免使用 `echo` 输出整段 html 代码。

## 指令分隔符    

PHP 需要在每个语句后用分号（`;`）结束指令。如果后面还有新行，则代码段的结束标记包含了行结束。

## PHP 支持多种注释风格

- 单行注释。 `//` 和 `#` 仅仅注释到行末或者当前的 PHP 代码块。
- 多行注释。注释在碰到第一个 `*/` 时结束

```
<?php

#单行注释
echo 'Hello World';

$a = 'shiyanlou';//单行注释

/**
 * 多行注释
 * 注释内容
 *
 */
```

# 三、数据类型（一）

PHP 支持 8 种原始数据类型。 

四种标量类型： 

- boolean（布尔型）  
- integer（整型）
- float（浮点型，也称作 double) 
- string（字符串） 

两种复合类型：

- array（数组）  
- object（对象）

最后是两种特殊类型： 

- resource（资源）  
- NULL（无类型） 



​       类型检测    

- `var_dump()` 函数可以查看表达式的值和类型
- `gettype()` 函数用于检测变量类型
- `is_` 加类型，如 `is_int()`，`is_array()` 等，判断变量是否为该类型

在目录 `/home/shiyanlou` 创建 test.php，并编辑内容

```php
<?php
$a = TRUE;
$b  = "foo";
$c = 0.1;
$d = 12;

if (is_string($b)) {
    echo "$b 是字符串".PHP_EOL;
} 

if (is_int($c)) {
    echo "$c 是整型".PHP_EOL;
}

var_dump($a);
var_dump($b);
echo gettype($c).PHP_EOL;
echo gettype($d);
```

执行

```
$ php test.php
```

结果输出

```
foo 是字符串
bool(true)
string(3) "foo"
double
integer
```

布尔类型表达了真值，可以为 TRUE 或 FALSE，不区分大小写。 

```php
<?php

$a = True;
$b = False;

var_dump($a);// bool(true)
var_dump($b);// bool(false)
```

运算符返回 boolean 类型的结果:

```php
<?php

// == 是一个操作符，它检测两个变量值是否相等，并返回一个布尔值
if ($a == "say_hello") {
    echo "Hello World";
}
// $b 的值是否为 true：
if ($res) {
    echo "This is true";
}
```

**注意**当下列情况转换为 Boolean 时，其值为 FALSE

- 布尔值 FALSE 本身
- 整型值 0（零）
- 浮点型值 0.0（零）
- 空字符串，以及字符串 "0" 
- 不包括任何元素的数组
- 特殊类型 NULL（包括尚未赋值的变量）
- 从空标记生成的 SimpleXML 对象

在目录 `/home/shiyanlou` 编辑 test.php

```php
<?php

$a = '';
$b = 0;
$c = false;
$d = "0";

var_dump($a == $b);
var_dump($b == $d);
var_dump($a == $c);
var_dump($b == $c);
```

执行 php test.php

## 整数溢出

如果给定的一个数超出了 integer 的范围，将会被解释为 float。同样如果执行的运算结果超出了 integer 范围，也会返回 float。

在目录 `/home/shiyanlou` 编辑 test.php

```php
<?php

$a = 123445566;
$b = 9223372036854775807;
$c = 9223372036854775808;
$d = 50000000000000 * 1000000;

var_dump($a);
var_dump($b);
var_dump($c);
var_dump($d);
```

浮点型（也叫浮点数 float，双精度数 double 或实数 real）

```
<?php
$a = 1.234; 
$b = 1.2e3; 
$c = 7E-10;
```

浮点数的字长和平台相关，通常最大值是 1.8e308 并具有 14 位十进制数字的精度（64 位 IEEE 格式）



​       String 字符串类型    



一个字符串 string 就是由一系列的字符组成，其中每个字符等同于一个字节，PHP 中有 4 中表达方式

- **单引号**

单引号内特殊字符和变量不会被解析

```php
<?php
$a = 'Hello';
echo '$a \n World';//$a \n World
```

- **双引号**

双引号内的特殊字符和变量会被解析。

~~~php
```php
<?php
$a = 'Hello';
/**
 * Hello
 *   World
 */
echo "$a \n World";
~~~

- **Heredoc**

Heredoc 类似与双引号，内部转义字符和变量可以被解析，句法结构为

```php
<<<EOT

字符串

EOT;
```

其中 `EOT` 为标识符，可以自定义，但是首尾标识符必须相同。开始标识符 EOT 后需换行，结束标识符 EOT 必须独占一行，且前面不许有空格。

```php
<?php
$a ='Hello' ;
echo <<<EOT
$a Hello
EOT;
```

- **Nowdoc**

Nowdoc 类似于单引号，无法解析转移字符和变量。句法结构类似 Heredoc，但是需要在开始标识符加上单引号。

```php
<?php
$a ='Hello' ;
echo <<<'EOD'//和 Heredoc 不同点
$a Hello
EOD;
```

编辑 test.php

```php
<?php

$a = 'Hello';
$b = '$a World';
$c = "ShiYanLou";

$c = <<<EOT
$a \n World
EOT;

$d = <<<'EOT'
$a \n $c
EOT;

var_dump($b);
var_dump($c);
var_dump($d);
```

执行 php test.php



# 四、数据类型（二）

​      实验知识点    

- Array
- Object
- Resource
- Null
- 类型转换

实验中主要涉及以下几个步骤：

- PHP 复合数据类型
- PHP 特殊数据类型
- 类型转换

##        Array 数组    

数组实际上是一个有序映射。映射是一种把 values 关联到 keys 的类型。由于数组元素的值也可以是另一个数组，树形结构和多维数组也是允许的。

##### 语法

定义数组可以用 `array()` 或 `[]` 来新建一个数组。它接受任意数量用逗号分隔的键（key） => 值（value）对。key 可以是 integer（索引数组）或者 string（关联数组），value 可以是任意类型，如对象、数组。

在目录 `/home/shiyanlou` 创建 test.php，并编辑内容

```php
<?php

$a = [
    "b" => "bb",
    "c" => "cc",
];

$b = [
    "bb",
    "cc"
];
$c = [
    "bb",
    "cc",
    "a" => $a,
    "b" => $b,
];

var_dump($a);
var_dump($a[0]);
var_dump($b);
var_dump($b['b']);
var_dump($c['a']['b']);
```

执行 php test.php

从结果中可以看到

- 如果没有键名，则数组默认使用从 0 开始的数字键名
- 打印数组不存在的 key 的值时，直接返回 NULL
- 数组可以多维嵌套，通过键名可以获取特定值



##        Object 对象    

使用 `new` 可以创建一个新的对象

```php
<?php

class foo
{
    function do()
    {
        echo "Action do"; 
    }
}

$f = new foo;
$f->do();
```

#### 转换为对象

如果将一个对象转换成对象，它将不会有任何变化。如果其它任何类型的值被转换成对象，将会创建一个内置类 stdClass 的实例。如果该值为  NULL，则新的实例为空。 array 转换成 object 将使键名成为属性名并具有相对应的值，除了数字键，不迭代就无法被访问。

在目录 `/home/shiyanlou` 创建 a.php，并编辑内容

```php
<?php

class A
{
}

$a = new A();
$b = (object)$a;
$c = (object)'A';
$d = (object)NULL;
$e = (object)['hello'=>'world'];

var_dump($a);
var_dump($b);
var_dump($c->scalar);
var_dump($d);
var_dump($e->hello);
```

执行 php a.php

从结果中可以看出

- a是对象，转换为对象后不发生变化，所以a 等于 $b
- 字符串 "A" 转换为对象时，自动生成 scalar 属性
- 数组 ['hello'=>'world'] 转换为对象时，键名 hello 作为属性，键值 world 为属性值



##       Resource 资源    

资源 resource 是一种特殊变量，保存了外部资源的一个引用，如打开文件、数据库连接等，资源是通过专门的函数来建立和使用的

```php
<?php

$file = fopen($filename);//打开文件
$db = mysqli_connect();//数据库连接
```

### 转换为资源

由于资源类型变量保存有为打开文件、数据库连接、图形画布区域等的特殊句柄，因此将其它类型的值转换为资源没有意义。

#### 释放资源

引用计数系统是 Zend 引擎的一部分，可以自动检测到一个资源不再被引用了（和 Java 一样）。这种情况下此资源使用的所有外部资源都会被垃圾回收系统释放。因此，很少需要手工释放内存。



##       NULL    

特殊的 NULL 值表示一个变量没有值。NULL 类型唯一可能的值就是 NULL。 在下列情况下一个变量被认为是 NULL：

- 被赋值为 NULL
- 尚未被赋值
- 被 unset()

#### 语法

NULL 类型只有一个值，就是不区分大小写的常量 NULL。

```php
<?php

$a = NULL;
```

#### 转换到 NULL

使用 (unset) 将一个变量转换为 null 将不会删除该变量或 unset 其值。仅是返回 NULL 值而已



##        类型转换    



PHP 是弱语言类型，定义变量的时候不需要制定变量类型，根据上下文自动解成对于的变量类型。

编辑 `/home/shiyanlou/test.php`

```php
<?php

$foo = "0";
var_dump($foo);

$foo += 2;
var_dump($foo);

$foo = $foo + 1.3;
var_dump($foo);

$foo = 5 + "10 Little Piggies";
var_dump($foo);

$foo = 5 + "10 Small Pigs";
var_dump($foo);
```

命令行执行 php test.php

从结果中可以看出，PHP隐式转换的优先级为：浮点型 > 整型 > 字符串

### 类型强制转换

在要转换的变量之前加上用括号括起来的目标类型，如

```php
<?php

$foo = 10;   // $foo 是整数
$bar = (boolean) $foo;   // 转换成布尔类型
```

允许的强制转换有：

```
-  (int), (integer) - 转换为整形 integer
-  (bool), (boolean) - 转换为布尔类型 boolean
-  (float), (double), (real) - 转换为浮点型 float
-  (string) - 转换为字符串 string
-  (array) - 转换为数组 array
-  (object) - 转换为对象 object
-  (unset) - 转换为 NULL (PHP 5)
```

# 五、变量

## 变量定义和命名规范    

#### 变量定义

PHP 中的变量用一个美元符号 `$` 后面跟变量名来表示。

#### 命名规范

变量名区分大小写，一个有效的变量名由字母或者下划线开头，后面跟上任意数量的字母，数字，或者下划线。

```php
<?php
$var = 'Bob';
$Var = 'Joe';
echo "$var, $Var";      // 输出 "Bob, Joe"

$4site = 'not yet';     // 非法变量名；以数字开头
$_4site = 'not yet';    // 合法变量名；以下划线开头
$i站点is = 'mansikka';  // 合法变量名；可以用中文
?> 
```

## 传值和引用    

#### 传值

变量默认总是传值赋值。这意味着，例如，当一个变量的值赋予另外一个变量时，改变其中一个变量的值，将不会影响到另外一个变量。

```php
<?php

$a = 'hello';
$b = $a;
$a = 'hi';

var_dump($a,$b);
```

执行结果为

```
string(2) "hi"
string(5) "hello"
```

说明传值赋值，不会因为原变量改变二改变

#### 引用

PHP 也提供了另外一种方式给变量赋值：引用赋值。这意味着新的变量相当于原变量的别名，改动新的变量将影响到原始变量，反之亦然。 

使用引用赋值，简单地将一个 & 符号加到将要赋值的变量前

```php
<?php

$a = 'hello';
$b = &$a;
$a = 'hi';

var_dump($a,$b);
```

结果为

```
string(2) "hi"
string(2) "hi"
```

有一点重要事项必须指出，那就是只有有名字的变量才可以引用赋值。例如，`&(2 * 3)` 为非法形式。

编辑 `/home/shiyanlou/test.php`

```php
<?php

$a = 'a';
$b = $$a;
$c = &$a;
$d = &$b;

$a = 'b';

var_dump($a,$b,$c,$d);
```

执行 php test.php

##       预定义变量    



PHP 提供了大量的预定义变量。其中一些变量依赖于运行的服务器的版本和设置，及其它因素。

- `$GLOBALS` — 引用全局作用域中可用的全部变量
- `$_SERVER` — 服务器和执行环境信息
- `$_GET` — HTTP GET 变量
- `$_POST` — HTTP POST 变量
- `$_FILES` — HTTP 文件上传变量
- `$_REQUEST` — HTTP Request 变量
- `$_SESSION` — Session 变量
- `$_ENV` — 环境变量
- `$_COOKIE` — HTTP Cookies
- `$php_errormsg` — 前一个错误信息
- `$HTTP_RAW_POST_DATA` — 原生POST数据
- `$http_response_header` — HTTP 响应头

以下预定义变量只在命令行执行的时候生效

- `$argc` — 传递给脚本的参数数目
- `$argv` — 传递给脚本的参数数组

##        变量范围    



变量的范围即它定义的上下文背景（也就是它的生效范围）。大部分的 PHP 变量只有一个单独的范围。这个单独的范围跨度同样包含了 include 和 require 引入的文件。例如： 

```php
<?php
$a = 1;
include 'b.php';
```

这里变量 $a 将会在包含文件 b.php 中生效。但是，在用户自定义函数中，一个局部函数范围将被引入。任何用于函数内部的变量按缺省情况将被限制在局部函数范围内。

编辑 `/home/shiyanlou/variable.php`

```php
<?php

$hi = 'Hi';
$hello = 'Hello';

function sayHi()
{
    echo $hi;
}

function sayHello($hello)
{
    echo $hello;
}

sayHi();
sayHello($hello);
```

执行 php variable.php

从结果中可以看出，函数要使用外部变量可以通过传参实现，此外还可以使用下节实验中的全局变量。

##        全局变量    



全局变量通常使用关键字 `global` 来声明

```php
<?php

$a = 1;
$b = 2;

function sum()
{
    global $a, $b;
    $b = $a + $b;
}

sum();
echo $b;
```

结果输出 3。在函数中声明了全局变量 a和a 和 a和b 之后，对任一变量的所有引用都会指向其全局版本。对于一个函数能够声明的全局变量的最大个数，PHP 没有限制。 

在全局范围内访问变量的第二个办法，是用特殊的 PHP 自定义 $GLOBALS 数组。前面的例子可以写成： 

```php
<?php

$a = 1;
$b = 2;

function sum()
{
    $GLOBALS['b'] = $GLOBALS['a'] + $GLOBALS['b'];
}

sum();
echo $b;
```

上节实验中，通过传参实现了函数调用外部变量，接下来使用 global 关键字

编辑 `/home/shiyanlou/variable.php`

```php
<?php

$hi = 'Hi';
$hello = 'Hello';

function sayHi()
{
    global $hi;
    echo $hi;
}

function sayHello($hello)
{
    echo $hello;
}

sayHi();
sayHello($hello);
```

执行 php variable.php



##        静态变量    



变量范围的另一个重要特性是静态变量。静态变量仅在局部函数域中存在，但当程序执行离开此作用域时，其值并不丢失。看看下面的例子： 

```php
<?php

function test()
{
    $a = 0;
    echo $a;
    $a++;
}
```

每次调用时都会将 a的值设为0并输出0。将变量加一的a 的值设为 0 并输出 0。将变量加一的 a的值设为0并输出0。将变量加一的a++ 没有作用，因为一旦退出本函数则变量 $a 就不存在了。

要写一个不会丢失本次计数值的计数函数，要将变量 $a 定义为静态的： 

编辑 `/home/shiyanlou/test.php`

```php
<?php

function test()
{
    static $a = 0;
    echo $a.PHP_EOL;
    $a++;
}

test();
test();
```

执行 php test.php

##        可变变量    



一个变量的变量名可以动态的设置和使用，例如： 

```
<?php

$a = 'hello';
$$a = 'world';

var_dump($a,$hello);
```

结果输出

```
string(5) "hello"
string(5) "world"
```

上例中动态设置了一个变量 $hello，通常多个 `$` 会依次从最后边开始解析，最后生成 `$` 前一个值为名称的变量。

编辑 `/home/shiyanlou/test.php`

```php
<?php

$a = 'b';
$b = 'c';
$c = 'd';

$$$$a = 'bcd';

var_dump($d);
```

执行 php test.php

# 六、常量

##        常量定义    



#### 命名规范

- 通常常量用大写字母表示，并且遵循和变量一样的命名规范，即以字母或下划线开头，后面跟任何字母，数字或下划线。
- 避免使用 `__` 两个下划线开头，被预留为 PHP 内置魔术常量使用。

#### 定义方式

- 使用 `define()` 函数定义

```php
<?php

define('HELLO', 'Hello');

//使用 defined() 来判断一个常量是否被定义
defined('SHIYANLOU') or define('SHIYANLOU', 'shiyanlou');
```

- 使用 `const` 关键字定义类之外的常量

```php
<?php

const HELLO = 'Hello';
const SHIYANLOU = 'shiyanlou';

class Test
{
}
```

注意使用 `const` 只能在类外部定义，且必须处于最顶端的作用区域，因为用此方法是在编译时定义的。这就意味着不能在函数内，循环内以及 if 语句之内用 const 来定义常量。

一个常量一旦被定义，就不能再改变或者取消定义。

编辑 `/home/shiyanlou/const.php`

```php
<?php

const HELLO = 'Hello';
const SHIYANLOU = 'shiyanlou';

class Test
{
    public function sayHi()
    {
        define(HELLO, 'Hi');
        echo HELLO;
    }
}

$t = new Test();
$t->sayHi();
```

执行 php const.php

从结果可以看出，常量 HELLO 被定义后，无法重新赋值。

##       相比变量    



#### 相同点

命名规范都必须以字母或下划线开头，后面跟字母，数字或下划线

#### 不同点

- 常量前面没有美元符号 `$`
- 常量只能通过 `define()` 或 `const` 定义，而不能通过赋值语句
- 常量可以不用理会变量的作用域而在任何地方定义和访问
- 常量一旦定义就不能被重新定义或者取消定义
- 常量的值只能是标量

##       魔术常量    



PHP 向它运行的任何脚本提供了大量的预定义常量。不过很多常量都是由不同的扩展库定义的，只有在加载了这些扩展库时才会出现，或者动态加载后，或者在编译时已经包括进去了。 

有八个魔术常量它们的值随着它们在代码中的位置改变而改变。例如 `__LINE__` 的值就依赖于它在脚本中所处的行来决定，这些特殊的常量不区分大小写。

- `__LINE__`，文件中的当前行号。
- `__FILE__`，文件的完整路径和文件名。如果用在被包含文件中，则返回被包含的文件名。
- `__DIR__`，文件所在的目录。如果用在被包括文件中，则返回被包括的文件所在的目录。
- `__FUNCTION__`，函数名称，返回该函数被定义时的名字。
- `__CLASS__`，类的名称，返回该类被定义时的名字。
- `__TRAIT__`，Trait 的名字（PHP 5.4.0 新加）。自 PHP 5.4 起此常量返回 trait 被定义时的名字（区分大小写）。Trait 名包括其被声明的作用区域（例如 Foo\Bar）。
- `__METHOD__`，类的方法名，返回该方法被定义时的名字（区分大小写）。
- `__NAMESPACE__`，当前命名空间的名称（区分大小写）。

# 七、运算符

##        算术运算符    



常见算术运算符包括

- `-$a`，取反
- `$a + $b`，加法，a和a 和 a和b 的和
- `$a - $b`，减法，a和a 和 a和b 的差
- `$a * $b`，乘法，a和a 和 a和b 的积
- `$a / $b`，除法，a和a 和 a和b 的商
- `$a % $b`，取余，a除以a 除以 a除以b 的余数
- `$a ** $b`，乘方，a的a 的 a的b 次方

*Practice*

编辑 `/home/shiyanlou/arithmetic.php`

```php
<?php

$a = 9 / 3;
$b = 9 / 4;
$c = -5 % 3;
$d = 5 % -3;
$e = 2 ** -2;

echo <<<EOT
9 / 3 = $a
9 / 4 = $b
-5 % 3 = $c
5 % -3 = $d
2 ** -2 = $e
EOT;
```

执行

```
$ php arithmetic.php
```

从结果可以看出

- 除法运算符总是返回浮点数。只有在下列情况例外：两个操作数都是整数（或字符串转换成的整数）并且正好能整除，这时它返回一个整数。
- 取余运算符的操作数在运算之前都会转换成整数（除去小数部分）。 并且结果和被除数的符号（正负号）相同。即 aa % ab 的结果和 $a 的符号相同。

##        赋值运算符    



基本的赋值运算符是 `=`，意味着把右边表达式的值赋给左边的运算数。 

赋值运算表达式的值也就是所赋的值。也就是说，$a = 3 的值是 3。这样就可以做一些小技巧： 

```php
<?php

$a = ($b = 4) + 5; // $a 现在成了 9，而 $b 成了 4。
```

对于数组 array，对有名字的键赋值是用 `=>` 运算符。此运算符的优先级和其它赋值运算符相同。

```php
<?php

$a = ['a' => 1, 'b' => 3 * 4];
```

在基本赋值运算符之外，还有适合于所有二元算术，数组集合和字符串运算符的`组合运算符`，这样可以在一个表达式中使用它的值并把表达式的结果赋给它，例如： 

```php
<?php

$a = 3;
$a += 5; //相当于 $a = $a + 5;
$b = "Hello ";
$b .= "There!"; //相当于 $b = $b. "There" ;
```

注意赋值运算将原变量的值拷贝到新变量中（传值赋值），所以改变其中一个并不影响另一个。这也适合于在密集循环中拷贝一些值例如大数组。 

**引用赋值**

PHP 支持引用赋值，引用赋值意味着两个变量指向了同一个数据，没有拷贝任何东西。 

*Practice*

编辑 `/home/shiyanlou/assign.php`

```php
<?php

$arr1 = $arr2 = [1,2,3];

foreach($arr1 as &$a) {
    $a++;
}
foreach($arr2 as $a) {
    $a++;
}
print_r($arr1);
print_r($arr2);
```

执行

```
$ php assign.php
```

从结果中可以看出，引用赋值会改变原值，传值赋值则不会。

##       位运算符    



位运算符允许对整型数中指定的位进行求值和操作。

- `$a & $b`，And（按位与），将把 a和a 和 a和b 中都为 1 的位设为 1。
- `$a | $b`，Or（按位或），将把 a和a 和 a和b 中任何一个为 1 的位设为 1。
- `$a ^ $b`，Xor（按位异或），将把 a和a 和 a和b 中一个为 1 另一个为 0 的位设为 1。
- `~$a`，Not（按位取反），将 $a 中为 0 的位设为 1，反之亦然。
- `$a << $b`，Shift left（左移），将 a中的位向左移动a 中的位向左移动 a中的位向左移动b 次（每一次移动都表示乘以 2）。
- `$a >> $b`，Shift right（右移），将 a中的位向右移动a 中的位向右移动 a中的位向右移动b 次（每一次移动都表示除以 2）。

**Example 1 整数的 AND，OR 和 XOR 位运算符**

```
<?php
/*
 * Ignore the top section,
 * it is just formatting to make output clearer.
 */

$format = '(%1$2d = %1$04b) = (%2$2d = %2$04b)'
        . ' %3$s (%4$2d = %4$04b)' . "\n";

echo <<<EOH
 ---------     ---------  -- ---------
 result        value      op test
 ---------     ---------  -- ---------
EOH;


/*
 * Here are the examples.
 */

$values = array(0, 1, 2, 4, 8);
$test = 1 + 4;

echo "\n Bitwise AND \n";
foreach ($values as $value) {
    $result = $value & $test;
    printf($format, $result, $value, '&', $test);
}

echo "\n Bitwise Inclusive OR \n";
foreach ($values as $value) {
    $result = $value | $test;
    printf($format, $result, $value, '|', $test);
}

echo "\n Bitwise Exclusive OR (XOR) \n";
foreach ($values as $value) {
    $result = $value ^ $test;
    printf($format, $result, $value, '^', $test);
}
?> 
```

以上例程会输出：

```
---------     ---------  -- ---------
 result        value      op test
 ---------     ---------  -- ---------
 Bitwise AND
( 0 = 0000) = ( 0 = 0000) & ( 5 = 0101)
( 1 = 0001) = ( 1 = 0001) & ( 5 = 0101)
( 0 = 0000) = ( 2 = 0010) & ( 5 = 0101)
( 4 = 0100) = ( 4 = 0100) & ( 5 = 0101)
( 0 = 0000) = ( 8 = 1000) & ( 5 = 0101)

 Bitwise Inclusive OR
( 5 = 0101) = ( 0 = 0000) | ( 5 = 0101)
( 5 = 0101) = ( 1 = 0001) | ( 5 = 0101)
( 7 = 0111) = ( 2 = 0010) | ( 5 = 0101)
( 5 = 0101) = ( 4 = 0100) | ( 5 = 0101)
(13 = 1101) = ( 8 = 1000) | ( 5 = 0101)

 Bitwise Exclusive OR (XOR)
( 5 = 0101) = ( 0 = 0000) ^ ( 5 = 0101)
( 4 = 0100) = ( 1 = 0001) ^ ( 5 = 0101)
( 7 = 0111) = ( 2 = 0010) ^ ( 5 = 0101)
( 1 = 0001) = ( 4 = 0100) ^ ( 5 = 0101)
(13 = 1101) = ( 8 = 1000) ^ ( 5 = 0101)
```

##        比较运算符    



比较运算符，如同它们名称所暗示的，允许对两个值进行比较。

- `$a == $b`，如果类型转换后 a等于a 等于 a等于b，返回 TRUE。
- `$a === $b`，如果 a等于a 等于 a等于b，并且它们的类型也相同，返回 TRUE。
- `$a != $b`，如果类型转换后 a不等于a 不等于 a不等于b，返回 TRUE。
- `$a <> $b`，等同于 `!=`
- `$a !== $b`，如果 a和a 和 a和b 的值或类型不同，返回 TRUE。
- `$a < $b`    ，如果 a严格小于a 严格小于 a严格小于b，返回 TRUE。
- `$a > $b`，如果 a严格大于a 严格大于 a严格大于b，返回 TRUE。
- `$a <= $b`，如果 a小于或者等于a 小于或者等于 a小于或者等于b，返回 TRUE。
- `$a >= $b`，如果 a大于或者等于a 大于或者等于 a大于或者等于b，返回 TRUE。

如果比较一个数字和字符串或者比较涉及到数字内容的字符串，则字符串会被转换为数值并且比较按照数值来进行。此规则也适用于 switch 语句。当用 === 或 !== 进行比较时则不进行类型转换，因为此时类型和数值都要比对。 

*Practice*

编辑 `/home/shiyanlou/compare.php`

```php
<?php

var_dump(null == "");
var_dump(null == false);
var_dump(true > false);
var_dump(0 == "a");
var_dump("1" == "01");
var_dump("10" == "1e1");
var_dump(100 == "1e2");
var_dump([4,5] < [1,2,3]);
var_dump((object)"Test" > "Test");
var_dump((object)"Test" > [2,3]);

switch ("a") {
case 0:
    echo "0";
    break;
case "a": 
    echo "a";
    break;
}
```

执行 

```
$ php compare.php
```

从结果可以看出

- null 或 String 和 string 比较时，将 null 转换为 ""，进行数字或词汇比较
- bool 或 null 和其他类型比较时，转换为 bool，FALSE < TRUE
- string，resource 或 number 相互比较时，将字符串或资源转换为数字，按普通数字比较
- array 之间比较时，具有较少成员的数组较小
- object 和其他类型比较时，object 总是更大
- array 和其他类型比较时，array 总是更大，但是比对象小
- switch 中第一个条件满足时，不会执行后面满足条件的语句

##       错误控制运算符    



PHP 支持一个错误控制运算符：`\@`。当将其放置在一个 PHP 表达式之前，该表达式可能产生的任何错误信息都被忽略掉。

```php
<?php

$my_file = @file ('non_existent_file') or
    die ("Failed opening file: error was '$php_errormsg'");

$value = @$cache[$key];
```

错误控制运算符只对表达式有效。对新手来说一个简单的规则就是：

- 如果能从某处得到值，就能在它前面加上 `\@` 运算符。例如，可以把它放在变量，函数和 include 调用，常量，等等之前。
- 不能把它放在函数或类的定义之前，也不能用于条件结构例如 if 和 foreach 等。

注意：目前的 `\@` 错误控制运算符前缀甚至使导致脚本终止的严重错误的错误报告也失效。这意味着如果在某个不存在或者敲错了字母的函数调用前用了 `\@` 来抑制错误信息，那脚本会没有任何迹象显示原因而死在那里。

##       执行运算符    



PHP 支持一个执行运算符：反引号（``）。注意这不是单引号！PHP 将尝试将反引号中的内容作为外壳命令来执行，并将其输出信息返回（例如，可以赋给一个变量而不是简单地丢弃到标准输出）。

```php
<?php
$output = `ls -al`;
echo "<pre>$output</pre>";
```

注意，反引号运算符在激活了安全模式或者关闭了 shell_exec() 时是无效的。

##        递增（减）运算符    



常见递增（减）运算符

- `++$a`，a的值加一返回a 的值加一返回 a的值加一返回a。
- `$a++`，返回 a，然后将a，然后将 a，然后将a 的值加一。
- `--$a`，a的值减一返回a 的值减一返回 a的值减一返回a。
- `$a--`，返回 a，然后将a，然后将 a，然后将a 的值减一。

递增（减）运算符对布尔和 NULL 类型的影响

```php
<?php

$a = null;
$b = true;

var_dump(++$a, --$a, ++$b, --$b);
```

结果输出

```
int(1)
int(0)
bool(true)
bool(true)
```

布尔值不受影响，NULL 递增为 1，递减为 0

*Practice*

编辑 `/home/shiyanlou/xcre.php`

```php
<?php

$a = 0;
$i = 'W';
while($a < 6) {
    echo "$a : ".++$i . PHP_EOL;
    $a++;
}
```

执行

```
$ php xcre.php
```

从结果可知

在处理字符变量的算数运算时，PHP 沿袭了 Perl 的习惯，而非 C 的。例如

```php
$a = 'Z';

// Perl 中
$a++;//将把 $a 变成'AA'

//C 中
$a++;//将把 $a 变成 '['（'Z' 的 ASCII 值是 90，'[' 的 ASCII 值是 91）
```

注意字符变量只能递增，不能递减，并且只支持纯字母（a-z 和 A-Z）。递增（减）其他字符变量则无效，原字符串没有变化。

##        逻辑运算符    



常见逻辑运算符

- `$a and $b`，逻辑与，如果 a和a 和 a和b 都为 TRUE
- `$a && $b`，逻辑与，如果 a和a 和 a和b 都为 TRUE，其中 `&&` 优先级高于 `and`
- `$a or $b`，逻辑或，如果 a或a 或 a或b 任一为 TRUE
- `$a || $b`，逻辑或，如果 a或a 或 a或b 任一为 TRUE，`||` 优先级高于 `or`
- `$a xor $b`，逻辑异或，如果 a或a 或 a或b 任一为 TRUE，但不同时是，则返回 TRUE
- `! $a`，逻辑非，如果 $a 不为 TRUE

*Practice*

编辑 `/home/shiyanlou/logical.php`

```php
<?php

$a = (false && foo());
$b = (true  || foo());
$c = (false and foo());
$d = (true  or  foo());

var_dump($a, $b, $c, $d);

$e = false || true;
$f = false or true;

$g = true && false;
$h = true and false;

var_dump($e, $f, $g, $h);
```

执行

```
$ php logical.php
```

从结果可知

- `foo()` 虽然没有定义，但是并没有机会执行，因为之前的表达式已经确定结果，`foo()` 被短路。
- `&&`，`||` 的优先级高于 `=`，`=` 的优先级高于 `and`，`or`。

##       字符串运算符    



有两个字符串运算符。

- 第一个是连接运算符 `.`，它返回其左右参数连接后的字符串
- 第二个是连接赋值运算符 `.=`，它将右边参数附加到左边的参数后。

```php
<?php
$a = "Hello ";
$b = $a . "World!"; // now $b contains "Hello World!"

$a = "Hello ";
$a .= "World!";     // now $a contains "Hello World!"
```

##        数组运算符    



常见数组运算符

- `$a + $b`，a和a 和 a和b 的联合
- `$a == $b`，a和a 和 a和b 键和值都相同则为 TRUE
- `$a === $b`，a和a 和 a和b 键和值且顺序和类型都相同返回 TRUE
- `$a != $b`，a和a 和 a和b 中键或值不同返回 TRUE
- `$a <> $b`，等同于 !=
- `$a !== $b`，a和a 和 a和b 中键，值，顺序或类型，其中一个不相同则返回 TRUE

*Practice*

编辑 `/home/shiyanlou/array.php`

```php
<?php

$a = ["a" => "apple", "b" => "banana"];
$b = ["a" => "pear", "b" => "strawberry", "c" => "cherry"];
$c = ["b" => "banana", "a" => "apple"];


var_dump($a + $b, $b + $a);
var_dump($a == $c, $a === $c);
```

执行

```
$ php array.php
```

从结果可以看出

- `+` 运算符把右边的数组元素（除去键值与左边的数组元素相同的那些元素）附加到左边的数组后面，但是重复的键值不会被覆盖。
- `===`，需要数组的键，值，类型和顺序都相同，才返回 TRUE。

##       PHP7 新增操作符    



#### 组合比较符

太空船操作符使用 `<=>` 表示，用于比较两个表达式。当 a小于、等于或大于a 小于、等于或大于 a小于、等于或大于b 时它分别返回-1、0或1。 比较的原则是沿用 PHP 的常规比较规则进行的。

```php
<?php
// 整数
echo 1 <=> 1; // 0
echo 1 <=> 2; // -1
echo 2 <=> 1; // 1

// 浮点数
echo 1.5 <=> 1.5; // 0
echo 1.5 <=> 2.5; // -1
echo 2.5 <=> 1.5; // 1

// 字符串
echo "a" <=> "a"; // 0
echo "a" <=> "b"; // -1
echo "b" <=> "a"; // 1
?>
```

#### NULL合并运算符

NULL 合并运算符使用 `??` 表示，意味着如果 `??` 之前的变量存在且值不为 NULL，它就会返回自身的值，否则返回 `??` 后的操作数。

```php
<?php

$username = $_GET['user'] ?? 'nobody';
$username = isset($_GET['user']) ? $_GET['user'] : 'nobody';

$username = $_GET['user'] ?? $_POST['user'] ?? 'nobody';
```

合并运算符通常可用三元运算符作为替换，多个合并运算符的优先级从左到右一次执行。

# 八、控制结构（一）

##       if 语句    



if 结构是很多语言包括 PHP 在内最重要的特性之一，它允许按照条件执行代码片段。PHP 的 if 结构和 C 语言相似：

```php
<?php
if ($a > $b)
  echo "a is bigger than b";
```

#### else 语句

经常需要在满足某个条件时执行一条语句，而在不满足该条件时执行其它语句，这正是 else 的功能。else 延伸了 if 语句，可以在 if 语句中的表达式的值为 FALSE 时执行语句。例如以下代码在 a大于a 大于 a大于b 时显示 a is bigger than b，反之则显示 a is NOT bigger than b： 

```php
<?php
if ($a > $b) {
  echo "a is greater than b";
} else {
  echo "a is NOT greater than b";
}
```

#### else if 语句

也可以写作 elseif，和此名称暗示的一样，是 if 和 else 的组合。和 else 一样，它延伸了 if 语句，可以在原来的 if 表达式值为 FALSE 时执行不同语句。但是和 else 不一样的是，它仅在 elseif 的条件表达式值为 TRUE  时执行语句。例如以下代码将根据条件分别显示 a is bigger than b，a equal to b 或者 a is smaller  than b： 

```php
<?php
if ($a > $b) {
    echo "a is bigger than b";
} elseif ($a == $b) {
    echo "a is equal to b";
} else {
    echo "a is smaller than b";
}
```

在同一个 if 语句中可以有多个 elseif 部分，其中第一个表达式值为 TRUE（如果有的话）的 elseif 部分将会执行。

elseif 的语句仅在之前的 if 和所有之前 elseif 的表达式值为 FALSE，并且当前的 elseif 表达式值为 TRUE 时执行。 

注意: 必须要注意的是 elseif 与 else if 只有在类似上例中使用花括号的情况下才认为是完全相同。如果用冒号来定义 if/elseif 条件，那就不能用两个单词的 else if，否则 PHP 会产生解析错误。

```php
<?php

/* 不正确的使用方法： */
if($a > $b):
    echo $a." is greater than ".$b;
else if($a == $b): // 将无法编译
    echo "The above line causes a parse error.";
endif;


/* 正确的使用方法： */
if($a > $b):
    echo $a." is greater than ".$b;
elseif($a == $b): // 注意使用了一个单词的 elseif
    echo $a." equals ".$b;
else:
    echo $a." is neither greater than or equal to ".$b;
endif;
```

#### 流程控制的替代语法

PHP 提供了一些流程控制的替代语法，包括 if，while，for，foreach 和  switch。替代语法的基本形式是把左花括号（{）换成冒号（:），把右花括号（}）分别换成  endif;，endwhile;，endfor;，endforeach; 以及 endswitch;

```php
<?php if ($a == 5): ?>
A is equal to 5
<?php endif; ?> 
```

替代语法同样可以用在 else 和 elseif 中。下面是一个包括 elseif 和 else 的 if 结构用替代语法格式写的例子：

```php
<?php
if ($a == 5):
    echo "a equals 5";
    echo "...";
elseif ($a == 6):
    echo "a equals 6";
    echo "!!!";
else:
    echo "a is neither 5 nor 6";
endif;
?> 
```

不要在同一个控制块内混合使用两种语法。

##       while 语句    



while 循环是 PHP 中最简单的循环类型。它和 C 语言中的 while 表现地一样。while 语句的基本格式是(该代码为语法格式，不是代码案例，无需敲打该代码)：

```
while (expr)
    {statement}
```

while 语句的含意很简单，它告诉 PHP 只要 while 表达式的值为 TRUE  就重复执行嵌套中的循环语句。表达式的值在每次开始循环时检查，所以即使这个值在循环语句中改变了，语句也不会停止执行，直到本次循环结束。有时候如果  while 表达式的值一开始就是 FALSE，则循环语句一次都不会执行。 

和 if 语句一样，可以在 while 循环中用花括号括起一个语句组，或者用替代语法： 

```
while (expr):
    statement
    ...
endwhile;
```

下面两个例子完全一样，都显示数字 1 到 10： 

```
<?php
/* example 1 */

$i = 1;
while ($i <= 10) {
    echo $i++;  /* the printed value would be
                    $i before the increment
                    (post-increment) */
}

/* example 2 */

$i = 1;
while ($i <= 10):
    print $i;
    $i++;
endwhile;
?> 
```

#### do-while 语句

do-while 循环和 while 循环非常相似，区别在于表达式的值是在每次循环结束时检查而不是开始时。和一般的 while  循环主要的区别是 do-while 的循环语句保证会执行一次（表达式的真值在每次循环结束后检查），然而在一般的 while  循环中就不一定了（表达式真值在循环开始时检查，如果一开始就为 FALSE 则整个循环立即终止）。 

do-while 循环只有一种语法：

```
<?php
$i = 0;
do {
   echo $i;
} while ($i > 0);
?> 
```

以上循环将正好运行一次，因为经过第一次循环后，当检查表达式的真值时，其值为 FALSE（$i 不大于 0）而导致循环终止。 资深的 C 语言用户可能熟悉另一种不同的 do-while 循环用法，把语句放在 do-while(0) 之中，在循环内部用 break 语句来结束执行循环。以下代码片段示范了此方法： 

```
<?php
do {
    if ($i < 5) {
        echo "i is not big enough";
        break;
    }
    $i *= $factor;
    if ($i < $minimum_limit) {
        break;
    }
    echo "i is ok";

    /* process i */

} while(0);
?> 
```

如果还不能立刻理解也不用担心。即使不用此“特性”也照样可以写出强大的代码来。自 PHP 5.3.0 起，还可以使用 goto 来跳出循环。

##       for 语句    



for 循环是 PHP 中最复杂的循环结构。它的行为和 C 语言的相似。 for 循环的语法是(该代码为语法格式，不是代码案例，无需敲打该代码)： 

```
for (expr1; expr2; expr3)
    {statement}
```

第一个表达式（expr1）在循环开始前无条件求值（并执行）一次。 

expr2 在每次循环开始前求值。如果值为 TRUE，则继续循环，执行嵌套的循环语句。如果值为 FALSE，则终止循环。 

expr3 在每次循环之后被求值（并执行）。 

每个表达式都可以为空或包括逗号分隔的多个表达式。表达式 expr2 中，所有用逗号分隔的表达式都会计算，但只取最后一个结果。expr2  为空意味着将无限循环下去（和 C 一样，PHP 暗中认为其值为 TRUE）。这可能不像想象中那样没有用，因为经常会希望用有条件的 break  语句来结束循环而不是用 for 的表达式真值判断。 

考虑以下的例子，它们都显示数字 1 到 10： 

```
<?php
/* example 1 */

for ($i = 1; $i <= 10; $i++) {
    echo $i;
}

/* example 2 */

for ($i = 1; ; $i++) {
    if ($i > 10) {
        break;
    }
    echo $i;
}

/* example 3 */

$i = 1;
for (;;) {
    if ($i > 10) {
        break;
    }
    echo $i;
    $i++;
}

/* example 4 */

for ($i = 1, $j = 0; $i <= 10; $j += $i, print $i, $i++);
?> 
```

当然，第一个例子看上去最简洁（或者有人认为是第四个），但用户可能会发现在 for 循环中用空的表达式在很多场合下会很方便。  PHP 也支持用冒号的 for 循环的替代语法(该代码为语法格式，不是代码案例，无需敲打该代码)。

```
for (expr1; expr2; expr3):
    statement;
    ...
endfor;
```

有时经常需要像下面这样例子一样对数组进行遍历： 

```
<?php
/*
 * 此数组将在遍历的过程中改变其中某些单元的值
 */
$people = Array(
        Array('name' => 'Kalle', 'salt' => 856412), 
        Array('name' => 'Pierre', 'salt' => 215863)
        );

for($i = 0; $i < sizeof($people); ++$i)
{
    $people[$i]['salt'] = rand(000000, 999999);
}
?> 
```

以上代码可能执行很慢，因为每次循环时都要计算一遍数组的长度。由于数组的长度始终不变，可以用一个中间变量来储存数组长度以优化而不是不停调用 count()：

```
<?php
$people = Array(
        Array('name' => 'Kalle', 'salt' => 856412), 
        Array('name' => 'Pierre', 'salt' => 215863)
        );

for($i = 0, $size = sizeof($people); $i < $size; ++$i)
{
    $people[$i]['salt'] = rand(000000, 999999);
}
?> 
```

##         foreach 语句    



foreach 语法结构提供了遍历数组的简单方式。foreach 仅能够应用于数组和对象，如果尝试应用于其他数据类型的变量，或者未初始化的变量将发出错误信息。有两种语法

```
foreach (array as $value)
    statement
foreach (array as $key => $value)
    statement
```

- 第一种格式遍历给定的 array 数组。每次循环中，当前单元的值被赋给 $value 并且数组内部的指针向前移一步。 
- 第二种格式做同样的事，只除了当前单元的键名也会在每次循环中被赋给变量 $key。 

#### 改变元素的值

在 foreach 中有两种方式改变元素的值

- 在 $value 之前加上 `&`来修改数组的元素。
- 通过 $key 重新赋值。

*Practice*

编辑 `/home/shiyanlou/foreach.php`

```php
<?php
$arr1 = $arr2 = [1, 2, 3, 4];

foreach ($arr1 as &$value) {
    $value = $value * 2;
}

var_dump($arr1, $value);

foreach ($arr2 as $key => $value) {
    $arr2[$key] = $value * 2;
}
var_dump($arr2, $value);
```

执行 

```
$ php foreach.php
```

从结果可以看出，数组最后一个元素的 $value 引用在 foreach 循环之后仍会保留。建议使用 unset() 来将其销毁。

##        break 语句    



break 结束当前 for，foreach，while，do-while 或者 switch 结构的执行。 break 可以接受一个可选的数字参数来决定跳出几重循环。 

*Practice*

编辑 `/home/shiyanlou/break.php`

```php
<?php
$i = 0;
while (++$i) {
    for ($j = 0;$j < 10;$j++) {
        switch ($i) {
        case 1:
            echo "At 1".PHP_EOL;
            break 2;
        case 5:
            echo "At 5".PHP_EOL;
            break 2;
        case 10:
            echo "At 10; quitting".PHP_EOL;
            break 3;
        default:
            break;
        }
    }
}
```

执行

```
$ php break.php
```

从结果可以看到，break 带参数可以指定跳出几重循环。

# 九、控制结构（二）

##        continue 语句    



continue 在循环结构用来跳过本次循环中剩余的代码并在条件求值为真时开始执行下一次循环。 

**Note: 注意在 PHP 中 switch 语句被认为是可以使用 continue 的一种循环结构。**

continue 接受一个可选的数字参数来决定跳过几重循环到循环结尾。默认值是 1，即跳到当前循环末尾。 

*Practice*

编辑 `/home/shiyanlou/continue.php`

```php
<?php

$i = 0;
while ($i++ < 3) {
    echo "Outer".PHP_EOL;
    while (1) {
        echo "Middle".PHP_EOL;
        while (1) {
            echo "Inner".PHP_EOL;
            continue 3;
        }
        echo "This never gets output.".PHP_EOL;
    }
    echo "Neither does this.".PHP_EOL;
}
```

执行

```
$ php continue.php
```

##       switch 语句    



当有多个 if elseif 条件时，可以使用 switch 来替换。

```php
<?php

if ($a == 'apple') {
    //do something
} elseif ($a == 'bnanan') {
    //do something
} elseif ($a == 'oringe') {
    //do something
} else {
    //do something
}
```

使用 switch 替换为

```php
<?php

switch ($a) {
    case 'apple':
        //do something
    break;    
    case 'bnanan':
        //do something
    break;    
    case 'oringe':
        //do something
    break;    
    default:
        //do something
}
```

- switch 中每个条件需要一对 `case` 和 `break`。
- `default` 匹配任何条件。如果匹配了某个条件，但是该语句中没有使用 `break`，则 `default` 中的语句将会被执行。

**Note: 注意和其它语言不同，continue 语句作用到 switch 上的作用类似于 break。如果在循环中有一个 switch 并希望 continue 到外层循环中的下一轮循环，用 continue 2。**

在 switch 语句中条件只求值一次并用来和每个 case 语句比较。在 elseif 语句中条件会再次求值。如果条件比一个简单的比较要复杂得多或者在一个很多次的循环中，那么用 switch 语句可能会快一些。 

在一个 case 中的语句也可以为空，这样只不过将控制转移到了下一个 case 中的语句。 

```
<?php
switch ($i) {
    case 0:
    case 1:
    case 2:
        echo "i is less than 3 but not negative";
        break;
    case 3:
        echo "i is 3";
}
?> 
```

一个 case 的特例是 default。它匹配了任何和其它 case 都不匹配的情况。例如：

```
<?php
switch ($i) {
    case 0:
        echo "i equals 0";
        break;
    case 1:
        echo "i equals 1";
        break;
    case 2:
        echo "i equals 2";
        break;
    default:
        echo "i is not equal to 0, 1 or 2";
}
?> 
```

case 表达式可以是任何求值为简单类型的表达式，即整型或浮点数以及字符串。不能用数组或对象，除非它们被解除引用成为简单类型。 

switch 支持替代语法的流程控制。更多信息见控制结构(一)的替代语法一节。 

```
<?php
switch ($i):
    case 0:
        echo "i equals 0";
        break;
    case 1:
        echo "i equals 1";
        break;
    case 2:
        echo "i equals 2";
        break;
    default:
        echo "i is not equal to 0, 1 or 2";
endswitch;
?> 
```



##        declare 语句    



declare 结构用来设定一段代码的执行指令。declare 的语法和其它流程控制结构相似(该代码为语法格式，不是代码案例，无需敲打该代码)： 

```
declare (directive)
    statement
```

directive 部分允许设定 declare 代码段的行为。目前只认识两个指令：ticks（更多信息见下面 ticks 指令）以及 encoding（更多信息见下面 encoding 指令）。 **Note:  encoding 是 PHP 5.3.0 新增指令。** declare 代码段中的 statement 部分将被执行——怎样执行以及执行中有什么副作用出现取决于 directive 中设定的指令。 

declare 结构也可用于全局范围，影响到其后的所有代码（但如果有 declare 结构的文件被其它文件包含，则对包含它的父文件不起作用）。

```
<?php
// these are the same:

// you can use this:
declare(ticks=1) {
    // entire script here
}

// or you can use this:
declare(ticks=1);
// entire script here
?> 
```

#### Ticks

Tick（时钟周期）是一个在 declare 代码段中解释器每执行 N 条可计时的低级语句就会发生的事件。N 的值是在 declare 中的 directive 部分用 ticks=N 来指定的。 

不是所有语句都可计时。通常条件表达式和参数表达式都不可计时。 

在每个 tick 中出现的事件是由 register_tick_function() 来指定的。更多细节见下面的例子。注意每个 tick 中可以出现多个事件。  **Example 1 Tick 的用法示例**

```
<?php

declare(ticks=1);

// A function called on each tick event
function tick_handler()
{
    echo "tick_handler() called\n";
}

register_tick_function('tick_handler');

$a = 1;

if ($a > 0) {
    $a += 2;
    print($a);
}

?> 
```

**Example 2 Ticks 的用法示例**

```
<?php

function tick_handler()
{
  echo "tick_handler() called\n";
}

$a = 1;
tick_handler();

if ($a > 0) {
    $a += 2;
    tick_handler();
    print($a);
    tick_handler();
}
tick_handler();

?> 
```

#### Encoding

可以用 encoding 指令来对每段脚本指定其编码方式。 **Example3 对脚本指定编码方式**

```
<?php
declare(encoding='ISO-8859-1');
// code here
?> 
```

##       return 语句    



如果在一个函数中调用 `return` 语句，将立即结束此函数的执行并将它的参数作为函数的值返回。

```php
<?php

function sayHello()
{
    return "Hello";
    echo "World";//不会被执行
}
echo sayHello();//Hello
```

`return` 可以不接任何参数。

```php
<?php

function sayHello()
{
    return;
}
```

##       include 语句    



#### PHP 中 4 种包含语句

- include
- include_once
- require
- require_once

include 和 require 都可以加载文件，不同点在于如果加载的文件包含错误，include 发出警告，继续执行后面的语句，而 require 则发出致命错误，终止程序执行。

include_once 和 require_once 的区别同 include 和 require，都是遇到错误时是否继续执行。

include 和 include_once，以及 require 和 require_once 的区别在于是否进行重复检测。

```php
<?php

include 'a.php';
require 'app/b.php';
```

#### 包含文件执行顺序

1. 参数是绝对路径（以 `/` 开头的路径），则直接包含该文件

```php
<?php

include '/usr/local/share/a.php';
```

1. 参数是相对路径或文件名，按照 `include_path` （可以通过 `phpinfo()` 查看当前包含的路径）指定的目录寻找。
2. 如果在 include_path 下没找到该文件则在调用脚本文件所在的目录和当前工作目录下寻找。
3. 如果最后仍未找到文件则 include 结构会发出一条警告；这一点和 require 不同，后者会发出一个致命错误。 

#### 变量范围

当一个文件被包含时，其中所包含的代码继承了 include 所在行的变量范围。从该处开始，调用文件在该行处可用的任何变量在被调用的文件中也都可用。不过所有在包含文件中定义的函数和类都具有全局作用域。 

## 十、函数

##        用户自定义函数    



#### 定义

一个函数可由以下的语法来定义：

```php
<?php
function foo($arg1, ..., $argn)
{
    //do something
    return $retval;
}
```

#### 命名规范

函数名和 PHP 中的其它标识符命名规则相同。有效的函数名以字母或下划线打头，后面跟字母，数字或下划线。

#### 函数无需在调用之前被定义

```php
<?php

function actionA()
{
    echo "A";
}

actionA();//可以调用
actionB();//可以调用

function actionB()
{
    echo "B";
}
```

除非函数是有条件被定义或者在函数中调用函数，一般都无须在调用函数之前定义。

#### 函数中调用函数

```php
<?php

function actionA()
{
  function actionB()
  {
      echo "B";
  }
}

actionB();//无法调用
actionA();//定义函数 actionB()
actionB();//可以调用
```

PHP 不支持函数重载，也不可能取消定义或者重定义已声明的函数。

```php
<?php

function sayHi()
{
    echo 'Hi';
}
function sayHi()
{
    echo 'Hello';
}
sayHi();//报错，不能重定义函数 sayHi()
```

函数名是大小写无关的，不过在调用函数的时候，通常使用其在定义时相同的形式。

#### 递归函数

递归函数的本质是函数调用函数本身，但是要避免递归函数／方法，调用超过 100-200 层，因为可能会使堆栈崩溃从而使当前脚本终止。 无限递归可视为编程错误。

```php
<?php
function recursion($a)
{
    if ($a < 20) {
        echo "$a\n";
        recursion($a + 1);
    }
}
```



##       函数的参数    



通过参数列表可以传递信息到函数，即以逗号作为分隔符的表达式列表。

PHP 支持按值传递参数（默认），通过引用传递参数以及默认参数。也支持可变数量的参数；

#### 通过引用传递参数

缺省情况下，函数参数通过值传递（因而即使在函数内部改变参数的值，它并不会改变函数外部的值）。如果希望允许函数修改它的参数值，必须通过引用传递参数。  如果想要函数的一个参数总是通过引用传递，可以在函数定义中该参数的前面预先加上符号 &： 

```php
<?php
function actionB(&$string)
{
    $string .= 'and something extra.';
}
$str = 'This is a string, ';
actionB($str);
echo $str;    // outputs 'This is a string, and something extra.'
?> 
```

#### 默认参数的值

函数可以定义 C++ 风格的标量参数默认值，如下：

```php
<?php
function makecoffee($type = "cappuccino")
{
    return "Making a cup of $type.\n";
}
echo makecoffee();
echo makecoffee(null);
echo makecoffee("espresso");
?> 
```

以上例程会输出：

```
Making a cup of cappuccino.
Making a cup of .
Making a cup of espresso.
```

##       返回值    



值通过使用可选的返回语句返回。可以返回包括数组和对象的任意类型。返回语句会立即中止函数的运行，并且将控制权交回调用该函数的代码行。

#### return

```php
<?php
function square($num)
{
    return $num * $num;
}
echo square(4);   // outputs '16'.
```

函数不能返回多个值，但可以通过返回一个数组来得到类似的效果。 

#### 返回一个数组以得到多个返回值

```php
<?php
function small_numbers()
{
    return array (0, 1, 2);
}
list ($zero, $one, $two) = small_numbers();
```

从函数返回一个引用，必须在函数声明和指派返回值给一个变量时都使用引用操作符 & ： 

#### 从函数返回一个引用

```
<?php
function &returns_reference()
{
    return $someref;
}

$newref =& returns_reference();
?> 
```

#### 返回值类型声明

PHP 7 增加了对返回类型声明的支持。 类似于参数类型声明，返回类型声明指明了函数返回值的类型。可用的类型与参数声明中可用的类型相同。

```php
<?php

function arraysSum(array ...$arrays): array
{
    return array_map(function(array $array): int {
        return array_sum($array);
    }, $arrays);
}

print_r(arraysSum([1,2,3], [4,5,6], [7,8,9]));
```

以上例程会输出：

```
Array
(
    [0] => 6
    [1] => 15
    [2] => 24
)
```

##        可变函数    



PHP 支持可变函数的概念。这意味着如果一个变量名后有圆括号，PHP 将寻找与变量的值同名的函数，并且尝试执行它。可变函数可以用来实现包括回调函数，函数表在内的一些用途。 

变量函数不能用于语言结构，例如 echo， print， unset()， isset()， empty()， include， require 以及类似的语句。需要使用自己的包装函数来将这些结构用作变量函数。 

*Practice*

编辑 `/home/shiyanlou/variable.php`

```php
<?php

class Test
{
    public static $actionB = "property B";

    public function actionA()
    {
        echo "method A";
    }
    public static function actionB()
    {
        echo "method B";
    }
}

function sayHi() {
    echo "Hi".PHP_EOL;
}

function sayHello($word = '') {
    echo "Hello $word";
}


$func = 'sayHi';
$func();

$func = 'sayHello';
$func('World');

$func = 'actionA';
(new Test())->$func();

echo Test::$actionB;
$actionB = 'actionB';
Test::$actionB();
```

执行

```
$ php variable.php 
```

从结果可以看出

- 可以用可变函数的语法来调用一个对象方法和静态方法。
- 静态方法调用优先级高于属性调用

##       内部函数    



也称为内置函数，PHP 有很多标准的函数和结构。还有一些函数需要和特定地 PHP 扩展模块一起编译，否则在使用它们的时候就会得到一个致命的未定义函数错误。

例如，要使用 image 函数中的 `imagecreatetruecolor()`，需要在编译 PHP 的时候加上 GD 的支持。或者，要使用 `mysql_connect()` 函数，就需要在编译 PHP 的时候加上 MySQL 支持。

调用 `phpinfo()` 或者 `get_loaded_extensions()` 可以得知 PHP 加载了那些扩展库。同时还应该注意，很多扩展库默认就是有效的。PHP 手册按照不同的扩展库组织了它们的文档。

Note: 如果传递给函数的参数类型与实际的类型不一致，例如将一个 array 传递给一个 string 类型的变量，那么函数的返回值是不确定的。在这种情况下，通常函数会返回 NULL。但这仅仅是一个惯例，并不一定如此。

参见 function_exists()，函数参考，get_extension_funcs() 和 dl()。

##        匿名函数    



匿名函数（Anonymous functions），也叫闭包函数（closures），允许 临时创建一个没有指定名称的函数。最经常用作回调函数（callback）参数的值。当然，也有其它应用的情况。

匿名函数目前是通过 Closure 类来实现的。

```php
<?php
echo preg_replace_callback('~-([a-z])~', function ($match) {
    return strtoupper($match[1]);
}, 'hello-world');
// 输出 helloWorld
```

闭包函数也可以作为变量的值来使用。PHP 会自动把此种表达式转换成内置类 Closure 的对象实例。把一个 closure 对象赋值给一个变量的方式与普通变量赋值的语法是一样的，最后也要加上分号：

#### 匿名函数变量赋值

```php
<?php
$greet = function($name)
{
    printf("Hello %s\r\n", $name);
};

$greet('World');
$greet('PHP');
```

#### 从父作用域继承变量

闭包可以从父作用域中继承变量。 任何此类变量都应该用 use 语言结构传递进去。 PHP 7.1 起，不能传入此类变量： superglobals、 $this 或者和参数重名。

*Practice*

编辑 `/home/shiyanlou/anonymous.php`

```php
<?php
$msg = 'hello';

$a = function () {
    var_dump($msg);
};
$a();

$b = function () use ($msg) {
    var_dump($msg);
};
$b();

$msg = 'hi';
$b();

$c = function () use (&$msg) {
    var_dump($msg);
};
$c();

$d = function ($arg) use ($msg) {
    var_dump($arg . ' ' . $msg);
};
$d("hello");
```

执行

```
$ php anonymous.php
```

从结果可以看出

- 使用 use 可以从父作用域继承变量
- 匿名函数是在定义的时候继承父作用域变量，而不是在调用的时候继承
- 变量使用引用赋值，则原变量发生改变，则引用该变量的变量也会发生变化



## 十一、类与对象（一）

本节介绍 PHP 的类和对象。PHP 中用 `class` 来定义类，用 `new`  来实例化对象，用 `extends` 继承类，不过只能单继承。属性和方法有 `public`、`private` 和 `protected` 做访问控制，默认为 public，在类里定义常量不需要 `$`，用（`::`）范围解析符可以调用父类的方法，访问类的静态变量、静态方法和常量。

##        基本概念    



#### 定义

- 每个类的定义都以关键字 `class` 开头，后面跟着类名（非保留字）。
- 类名后跟着一对花括号，里面包含有类属性和方法的定义。

```php
<?php
class A
{
    //属性
    public $a;
    private $b;

    //方法
    public function actionA()
    {

    }
}
```

#### 类成员默认值

在定义类属性的时候，可以使用默认值

```php
<?php
class A
{
    //默认值
    public $a = 'Hi';
    private $b = 'Hello';

    //方法
    public function actionA()
    {

    }
}
```

#### 创建实例

要创建一个对象的实例，使用关键字 `new` 

```php
<?php
$a = new A();//创建类 A 的实例
```

#### 对象赋值

当把一个对象已经创建的实例赋给一个新变量时，新变量会访问同一个实例，就和用该对象赋值一样。此行为和给函数传递入实例时一样。可以用克隆给一个已创建的对象建立一个新实例。

```php
<?php

class A
{
}

$a = new A();
$b = $a;
$c = &$a;
$d = clone $a;

$a = null;

var_dump($a,$b,$c,$d);
```

上述结果为

```
NULL
object(A)#1 (0) {
}
NULL
object(A)#2 (0) {
}
```

#### $this

伪变量 `$this` 可以在当一个方法在对象内部调用时使用。$this 是一个到调用对象（通常是方法所属于的对象，但也可以是另一个对象，如果该方法是从第二个对象内静态调用的话）的引用。

*Practice*

编辑 `/home/shiyanlou/this.php`

```php
<?php
class A
{
    function actionA()
    {
        if (isset($this)) {
            echo '$this is defined (';
            echo get_class($this);
            echo ")\n";
        } else {
            echo '$this is not defined.'.PHP_EOL;
        }
    }
}

class B
{
    function actionB()
    {
        A::actionA();
    }
}

$a = new A();
$a->actionA();
A::actionA();
$b = new B();
$b->actionB();
B::actionB();
```

执行

```
$ php this.php 
```

从结果可以看出，`$this` 只能在对象中使用，不能在静态方法中调用。但是如果在另一个对象（类 B）中调用静态方法，则 `$this` 指向该类（ B ）。



##        对象继承    



一个类可以在声明中用 `extends` 关键字继承另一个类的方法和成员。PHP 不支持多重继承，一个类只能继承一个类。 

```php
class A
{

}
class B extends A
{

}
```

被继承的方法和成员可以通过用同样的名字重新声明被覆盖，除非父类定义方法时使用了 `final` 关键字。可以通过 `parent::` 来访问被覆盖的方法或成员。

*Practice* 编辑 `/home/shiyanlou/extends.php`

```php
<?php
class A
{
    public function sayHi()
    {
        echo "Hi".PHP_EOL;
    }
    final public function sayBye()
    {
        echo "Bye".PHP_EOL;
    }
}

class B extends A
{
    public function sayHi()
    {
        parent::sayHi();
        echo "Hello".PHP_EOL;
        parent::sayBye();
    }
    //不能被覆盖，报错，练习的时候注意删除该方法
    public function sayBye()
    {
        echo "See you";
    }
}

$b = new B();
$b->sayHi();
```

执行

```
$ php extends.php
```

从结果可以看到

- 使用 `final` 修饰的方法不能被覆盖
- 使用 `parent::` 可以调用父类方法或属性

##        属性    



#### 定义

类的变量成员叫做`属性`。属性声明是由访问控制关键字 `public`，`protected`  或 `private` 和一个变量来组成，同时可以加上默认值。

```php
<?php

class A
{
    //只能在类本身使用
    private $a = "Hello";

    //可以在子类和类本身使用
    protected $b = <<<EOT
This is variable b;
EOT;

    //除了子类，类本身，外部也可以访问
    public $c;
}
```

#### 访问属性

在类的成员方法里面，可以通过 `$this->` 加变量名来访问类的属性和方法，但是要访问类的静态属性或者在静态方法要使用 `self::` 加变量名。

注意 `self::` 这种方式后的变量名需要加 `$` 符号，而 `$this->` 后的变量名不需要加

*Practice*

编辑 `/home/shiyanlou/property.php`

```php
<?php 

class A
{
    private $a = "Hello";

    protected $b = <<<EOT
This is property b
EOT;

    public static $c = 'This is a'.' static property';

    public function talk()
    {
        echo $this->a.PHP_EOL;
        echo $this->b.PHP_EOL;
        echo self::$c;
    }
}

(new A())->talk();
```

执行

```
php property.php
```

##       类常量    



我们可以在类中定义常量。常量的值将始终保持不变。在定义和使用常量的时候不需要使用 `$` 符号。 

```php
<?php

class A
{
    const ENV = 'env';
    const HELLO = 'Hello';
}
```

接口（interface）中也可以定义常量

```php
<?php

interface B
{
    const ENV = 'ENV';
    public function sayHi();
}
```

##       自动加载对象    



要进行一个类操作时，需要先将该类加载进来，例如 `include`，`require` 等。

如果要执行的类很多，则需要大量 `include` 操作，会导致重复加载，管理苦难等一系列问题。

在 PHP 5 中，不用这样做了，可以使用 `spl_autoload_register()` 函数来注册任意数量的自动加载器。

```php
<?php

spl_autoload_register(function ($class_name) {
    include $class_name . '.php';
});

new A();
new B();
```

本例尝试分别从 A.php 和 B.php 文件中加载 A 和 B 类，相当于 

```php
<?php

include 'A.php';
include 'B.php';

new A();
new B();
```

##       构造和析构函数    



#### 构造函数

void __constuct()

创建一个对象时（ `new` 操作），构造函数会自动调用

```php
class A
{
    public function __construct()
    {
        echo 'init...'.PHP_EOL;
    }

    public function sayHi()
    {
        echo "hi";
    }
}

(new A())->sayHi();
```

输出结果为

```
init...
hi
```

实例化 A 的时候执行构造函数。

注意: 如果子类中定义了构造函数则不会隐式调用其父类的构造函数。要执行父类的构造函数，需要在子类的构造函数中调用parent::__construct()。

```php
<?php
class A 
{
   public function __construct() 
   {
       echo "A";
   }
}

class B extends A 
{
   public function __construct() 
   {
       parent::__construct();
       echo "B";
   }
}

new A(); // A
new B(); // AB
```

#### 析构函数

void __destruct( void )

析构函数会在到某个对象的所有引用都被删除或者当对象被显式销毁时执行。

```
<?php
class A {
   public function __construct() {
       echo 'Start...';
   } 

   public function sayHi()
   {
       echo "Hi...";    
   }

   public function __destruct() {
       echo "Finish";
   }
}

(new A())->sayHi(); // Start...Hi...Finish
```

和构造函数一样，父类的析构函数不会被引擎暗中调用。要执行父类的析构函数，必须在子类的析构函数体中显式调用 parent::__destruct()。 

析构函数即使在使用 `exit()` 终止脚本运行时也会被调用。在析构函数中 调用 `exit()` 将会中止其余关闭操作的运行。 

##        访问控制    



对属性或方法的访问控制，是通过在前面添加关键字 public、protected 或 private 来实现的。如果未添加，则默认为 public。

- `public` 所定义的类成员可以在任何地方被访问
- `protected` 所定义的类成员则可以被其所在类的子类和父类访问（当然，该成员所在的类也可以访问）
- `private` 定义的类成员则只能被其所在类访问

*Practice*

编辑 `/home/shiyanlou/access.php`

```php
<?php

class A
{
    private $hi = 'Hi'.PHP_EOL;
    protected $hello = 'Hello'.PHP_EOL;
    public $bye = 'Bye'.PHP_EOL;

    private function sayHi()
    {
        echo $this->hi;
    }

    protected function sayHello()
    {
        echo $this->hello;
    }

    public function sayBye()
    {
        echo $this->bye;
    }

}

class B extends A
{
    public function talk()
    {
        parent::sayHello();
    }
}

$a = new A();
$a->sayHi();//报错，无法调用


$b = new B();
$b->sayHello();//报错，无法调用
$b->talk();
$b->sayBye();
```

执行

```
$ php access.php
```

从结果可知，声明为 `private` 的方法或属性无法在类外部调用，同时子类也无法调用该方法。

##        范围解析操作符（::）    



范围解析操作符，可以简单地说是一对冒号，可以用于访问静态成员、方法和常量，还可以用于覆盖类中的成员和方法。 

当在类的外部访问这些静态成员、方法和常量时，必须使用类的名字。 

*Practice*

编辑 `/home/shiyanlou/paamayim.php`

```php
<?php

class A 
{
    const CONST_A = 'A constant value';

    public static function sayHello()
    {
        echo 'Hello';
    }
}

class B extends A
{
    public static $b = 'static var b';

    /**
     * 覆盖父类方法
     *
     */
    public static function sayHello()
    {
        echo parent::sayHello().' World'.PHP_EOL;

    }

    public static function actionB() 
    {
        self::sayHello();
        echo parent::CONST_A.PHP_EOL;
        echo self::$b;
    }
}

B::actionB();
```

执行

```
$ php paamayim.php
```

从结果可知

- 使用 `parent`，`self` 可以调用父类和自身的方法属性
- `::` 可以调用静态方法，静态属性和常量

# 十二、类与对象（二）

##        Static 关键字    



声明类成员或方法为 static，就可以不实例化类而直接访问。不能通过一个对象来访问其中的静态成员（静态方法除外）。 

由于静态方法不需要通过对象即可调用，所以伪变量 `$this` 在静态方法中不可用。静态属性不可以由对象通过 `->` 操作符来访问。 

> 注意：在 PHP7 中通过（::）调用非静态方法会产生一个 E_DEPRECATED 级别的警告，不赞成这样使用，在以后可能会取消对这种用法的支持。 

*Practice*

编辑 `/home/shiyanlou/static.php`

```php
<?php 
class Test
{
    public $hi = 'Hi';
    public static $hello = 'Hello';

    public function sayHi()
    {
        echo $this->hi;
    }
    public static function sayHello()
    {
        echo self::$hello;
    }
    public function sayWorld()
    {
        echo " World".PHP_EOL;
    }
}

$obj = new Test();
$obj->sayHi();
$obj->sayWorld();

Test::sayHello();
Test::sayWorld();
```

执行 

```
$ php static.php
```

从结果可以看出

- 通过 `::` 可以执行静态和非静态方法，但是不赞成通过这种方式调用非静态方法，此方式有可能被官方移除，因此上面 `sayWorld()`，应该通过 `(new Test())->sayWorld()` 这种方式调用
- 静态属性和方法可以通过 `self` 关键字调用

就像其它所有的PHP静态变量一样，静态属性只能被初始化为一个字符值或一个常量，不能使用表达式。 所以你可以把静态属性初始化为整型或数组，但不能指向另一个变量或函数返回值，也不能指向一个对象。 

##        抽象类    



定义为抽象的类可能无法直接被实例化，任何一个类， 如果它里面至少有一个方法是被声明为抽象的，那么这个类就必须被声明为抽象的。如果类方法被声明为抽象的， 那么其中就不能包括具体的功能实现。

*Practice*

编辑 `/home/shiyanlou/abstract.php`

```php
<?php

abstract class Say
{
    abstract public function sayHello($word);
    abstract public function sayHi();
}

class Speak extends Say
{
    public function sayHello($word)
    {
        echo "Hello $word";
    }

    public function sayHi()
    {
        echo "Hi".PHP_EOL;
    }
}

$s = new Speak();
$s->sayHi();
$s->sayHello("World");
```

执行

```
$ php abstract.php
```

从结果可以看出

- 继承一个抽象类的时候，子类必须定义父类中的所有抽象方法。例如，在类 Speak 中移除方法 sayHi()，结果为

```
Fatal error: Class Speak contains 1 abstract method and must therefore be declared abstract or implement the remaining methods (Say::sayHi)...
```

- 这些方法的访问控制必须和父类中一样（或者更为宽松）。例如，在类 Speak 中 sayHi() 声明为 protected，则报错

```
Fatal error: Access level to Speak::sayHi() must be public (as in class Say)...
```

- 此外方法的调用方式必须匹配，即类型和所需参数数量必须一致。例如，移除抽象方法 sayHello() 中的参数，则

```
Fatal error: Declaration of Speak::sayHello($word) must be compatible with Say::sayHello()...
```



##        接口    



使用接口（interface），你可以指定某个类必须实现哪些方法，但不需要定义这些方法的具体内容。我们可以通过 interface 来定义一个接口，就像定义一个标准的类一样，但其中定义所有的方法都是空的。  接口中定义的所有方法都必须是public，这是接口的特性。

#### 实现

要实现一个接口，可以使用 `implements` 操作符。类中必须实现接口中定义的所有方法，否则会报一个 fatal 错误。如果要实现多个接口，可以用逗号来分隔多个接口的名称。

```php
<?php

interface A
{
    public function actionA();
}

interface B
{
    public function actionB();
}

//实现多个接口
class C implements A, B
{
    public function actionA()
    {
        //do something
    }
    public function actionB()
    {
        //do something
    }
}
```

注意：

- 实现多个接口时，接口中的方法不能有重名。
- 接口也可以继承，通过使用 `extends` 操作符。

```php
<?php

interface A
{
    public function actionA();
}

interface B extends A
{
    public function actionB();
}

class C implements A
{
    public function actionA()
    {
        //do something
    }
    public function actionB()
    {
        //do something
    }
}
```

#### 常量

接口中也可以定义常量。接口常量和类常量的使用完全相同。 它们都是定值，不能被子类或子接口修改。 

```
<?php
interface A
{
    const B = 'Interface constant';
}

// 输出接口常量
echo A::B;

// 错误写法，因为常量的值不能被修改。接口常量的概念和类常量是一样的。
class C implements A
{
    const B = 'Class constant';
}
```

##       匿名类    



php7支持通过new class 来实例化一个匿名类，这可以用来替代一些“用后即焚”的完整类定义。

```php
<?php
interface Logger {
    public function log(string $msg);
}

class Application {
    private $logger;

    public function getLogger(): Logger {
         return $this->logger;
    }

    public function setLogger(Logger $logger) {
         $this->logger = $logger;
    }
}

$app = new Application;
$app->setLogger(new class implements Logger {
    public function log(string $msg) {
        echo $msg;
    }
});

var_dump($app->getLogger());
?>
```

以上例程会输出：

```
object(class@anonymous)#2 (0) {
}
```