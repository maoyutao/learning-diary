##基础语法
* PHP 脚本可放置于文档中的任何位置。
* PHP 脚本以 <?php 开头，以 ?> 结尾：

```
<?php
// 此处是 PHP 代码
?>
```
* PHP 文件的默认文件扩展名是 ".php"。
* PHP 文件通常包含 HTML 标签以及一些 PHP 脚本代码。
* PHP 语句以分号结尾（;）。PHP 代码块的关闭标签也会自动表明分号（因此在 PHP 代码块的最后一行不必使用分号）。
* 在 PHP 中，所有**用户定义的函数、类和关键词（例如 if、else、echo 等等）**都对大小写不敏感 所有**变量**都对大小写敏感

###注释
```
<?php
// 这是单行注释

# 这也是单行注释

/*
这是多行注释块
它横跨了
多行
*/
?>
```

###变量
* PHP 没有创建变量的命令。
变量会在首次为其赋值时被创建 （没有 var 什么什么）
* 变量以 $ 符号开头
* static关键词  跟c一样
* 函数之外声明的变量拥有 Global 作用域，**只能在函数以外**进行访问。（跟c的global不一样！！）
函数内部声明的变量拥有 LOCAL 作用域，只能在函数内部进行访问。
* 要在函数内访问全局变量：

	1.在函数里加一个类似声明的语句
	
	```
	<?php
	$x=5;
	$y=10;

	function myTest() {
  	global $x,$y;
   $y=$x+$y;
	}

	myTest();
	echo $y; // 输出 15
	?>
	```
	2.用GLOBALS[]数组<br/>
	PHP 同时在名为 $GLOBALS[index] 的数组中存储了所有的全局变量。下标存有变量名。这个数组在函数内也可以访问，并能够用于直接更新全局变量。
	
	```
	<?php
	$x=5;
	$y=10;

	function myTest() {
	  $GLOBALS['y']=$GLOBALS['x']+$GLOBALS['y'];
	} 

	myTest();
	echo $y; // 输出 15
	?>
	```
	
###输出
* echo - 能够输出一个以上的字符串
* print - 只能输出一个字符串，并始终返回 1
* echo 和print 是一个语言结构，有无括号均可使用：echo 或 echo()。
* echo后面：
	1. echo "里面是字符串 可以有<标签> 会解析"
	2. echo $变量名（没有引号）
	3. echo "balabala","bakabala","逗号用来连起来“<br/>
	 （后面运算符那里会学到用.也能连，这两种稍微有点区别）
	4. echo "balabla$变量" //这样也行 
	5. echo "My car is a $cars[0]"; //数组
* print除了不能用逗号，跟echo一样(可以用.)
*  echo比较快
*   print_r();
   功能：用于输出数组。

###数据类型
* 字符串、整数、浮点数、逻辑、数组、对象、NULL。
* 字符串可以是引号内的任何文本，可以使用单引号或双引号
* 可以用三种格式规定整数：十进制、十六进制（前缀是 0x）或八进制（前缀是 0）
* var\_dump() 会返回变量的数据类型和值<br/>
var\_dump($x) 结果是这样：float(10.365) <br/>
 var\_dump($数组名)  结果是这样：array(3) { [0]=> string(5) "Volvo" [1]=> string(3) "BMW" [2]=> string(4) "SAAB" }  
* **对象**
 1. 声明类 要用var声明变量！！跟外面不一样
 
 ```
  class Car
{
    var $color;
    function Car($color="green") {
      $this->color = $color;
    }
 }
 ```
 
 2. 声明对象的时候 只能用`$herbie = new Car("white");`不能用`Car $herbie("white");`这种
 3. 都不用.了  用->
* **数组**  
还有多维数组 `$cars[0][1]`  
索引数组 自动分配或手动分配  遍历可用count()  
关联数组 `$age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");`或

 ```
$age['Peter']="35";
$age['Ben']="37";
$age['Joe']="43";
```
使用：`echo $age['Peter'];`

 遍历只能用foreach了
 
###常量
* 有效的常量名以字符或下划线开头（常量名称前面没有 $ 符号）。
* 与变量不同，常量贯穿整个脚本是自动全局的。
* 如需设置常量，请使用 define() 函数 - 它使用三个参数：  
首个参数定义常量的名称  
第二个参数定义常量的值  
可选的第三个参数规定常量名是否对大小写不敏感。默认是 false。  

```
<?php
define("GREETING", "Welcome to W3School.com.cn!");
echo GREETING;
?>
```
###运算符
+-*／% ++ -- 赋值= += 

字符串拼接 .   .=  
`$txt1 = "Hello" $txt2 = $txt1 . " world!"`

比较：  
== != === !== > < >=

逻辑：  
and or xor(异或) && || !

数组运算符：  
$x + $y 联合 （但不覆盖重复的键）  
$x == $y	如果 $x 和 $y 拥有相同的键/值对，则返回 true。  
$x === $y	如果 $x 和 $y 拥有相同的键/值对，且**顺序相同**类型相同，则返回 true。  

### 语句
if...elseif....else    
switch  
while 循环  
for - 循环  
**foreach - 遍历数组中的每个元素并循环代码块**
foreach 仅能够应用于数组和对象，如果尝试应用于其他数据类型的变量，或者未初始化的变量将发出错误信息。有两种语法：

```
foreach (array_expression as $value)
    statement
foreach (array_expression as $key => $value)
    statement
```
 
第一种格式遍历给定的 array_expression 数组。每次循环中，当前单元的值被赋给 $value 并且数组内部的指针向前移一步（因此下一次循环中将会得到下一个单元）。

第二种格式做同样的事，只除了当前单元的键名也会在每次循环中被赋给变量 $key。

函数的默认值

```
function setHeight($minheight=50) {
  echo "The height is : $minheight <br>";
}
```
###超全局变量
```
$GLOBALS
$_SERVER
$_REQUEST
$_POST
$_GET
$_FILES
$_ENV
$_COOKIE
$_SESSION
```
`$_SERVER` 这种超全局变量保存关于报头、路径和脚本位置的信息。  `$_SERVER["PHP_SELF"] `返回当前脚本的文件名 在表单action那可能有用  
`$_SERVER["REQUEST_METHOD"] ` post或get    
`$_REQUEST` 用于收集 HTML 表单提交的数据。  

```
<?php 
$name = $_REQUEST['fname']; 
echo $name; 
?>
```
post和get类似request 只是对应两种方式  
此外`$_GET` 也可以收集 URL 中的发送的数据
  
```
<html>
<body>

<a href="test_get.php?subject=PHP&web=W3school.com.cn">测试 $GET</a>

</body>
</html>
```
test_get.php：

```
<html>
<body>

<?php 
echo "Study " . $_GET['subject'] . " at " . $_GET['web'];
?>

</body>
</html>
```
###表单
注意体会这种和ajax的差别  
直接到anction那个页面去了
  
```
<html>
<body>

<form action="welcome_get.php" method="get">
Name: <input type="text" name="name"><br>
E-mail: <input type="text" name="email"><br>
<input type="submit">
</form>

</body>
</html>
```
welcome_get.php:

```
<html>
<body>

Welcome <?php echo $_GET["name"]; ?><br>
Your email address is: <?php echo $_GET["email"]; ?>

</body>
</html>
```
提交之后就会显示  

```
Welcome John
Your email address is john.doe@example.com
```

* 表单验证

```
function test_input($data) {
  $data = trim($data);//去除用户输入数据中不必要的字符（多余的空格、制表符、换行）
  $data = stripslashes($data);//删除用户输入数据中的反斜杠（\）
  $data = htmlspecialchars($data);
  return $data;
}
```
* 错误显示
* 表单必填
* 表单防清空 `Name: <input type="text" name="name" value="<?php echo $name;?>">`

##高级语法
###命名空间
命名空间通过关键字namespace 来声明。如果一个文件中包含命名空间，它必须在其它所有代码之前声明命名空间，除了一个以外：declare关键字。
跟全局一起出现或者有多个命名空间的时候用大括号括起来

```
<?php
declare(encoding='UTF-8');
namespace MyProject {

const CONNECT_OK = 1;
class Connection { /* ... */ }
function connect() { /* ... */  }
}

namespace { // 全局代码
session_start();
$a = MyProject\connect();
echo MyProject\Connection::start();
}
?>
```
与目录和文件的关系很象，PHP 命名空间也允许指定层次化的命名空间的名称。因此，命名空间的名字可以使用分层次的方式定义：

```
<?php
namespace MyProject\Sub\Level;

const CONNECT_OK = 1;
class Connection { /* ... */ }
function connect() { /* ... */  }

?>
```
你能在同一个文件中定义多个命名空间化的代码，比较合适的做法是每个文件定义一个命名空间（可以是相同命名空间）。

```
use userCenter\register; //引用空间  
use userCenter\register as reg; //引用空间并加别名
```

###错误处理
1. `die()`
2.  高级
 * 创建自定义错误处理器  `error_function(error_level,error_message,
error_file,error_line,error_context)`
例：  
 
 ```
function customError($errno, $errstr)
 { 
 echo "<b>Error:</b> [$errno] $errstr<br />";
 echo "Ending Script";
 die();
 }
```
 * 把上面的函数改造为脚本运行期间的默认错误处理程序
  `set_error_handler("customError");`
 * 如果要设置还有哪些情况自己触发错误
   
  ```
<?php
$test=2;
if ($test>1)
{
trigger_error("Value must be 1 or below");
}
?>
```
 * 还可以通过 E-Mail 发送错误消息
  error_log(）
  
###异常处理
 throw try catch(类名 变量名）
 
```
 <?php
class customException extends Exception
 {
 public function errorMessage()
  {
  //error message
  $errorMsg = $this->getMessage().' is not a valid E-Mail address.';
  return $errorMsg;
  }
 }
$email = "someone@example.com";
try
 {
 try
  {
  //check for "example" in mail address
  if(strpos($email, "example") !== FALSE)
   {
   //throw exception if email is not valid
   throw new Exception($email);
   }
  }
 catch(Exception $e)
  {
  //re-throw exception
  throw new customException($email);
  }
 }
catch (customException $e)
 {
 //display custom message
 echo $e->errorMessage();
 }
?>
```


怎样抛出异常 类的扩展 重新抛出异常   
顶层异常处理器  
`set_exception_handler('function_name');` 函数可设置处理所有未捕获异常的用户定义函数。

###过滤器
to do……

###mysql
* 连接：`mysql_connect(servername,username,password);`
* 关闭：`mysql_close($con);`

```
<?php
$con = mysql_connect("localhost","peter","abc123");
if (!$con)
  {
  die('Could not connect: ' . mysql_error());
  }

// some code

mysql_close($con);
?>
```
* mysql_query("指令")

###xml解析器
to do……

###AJAX
html:  

```
<html>
<head>
<script src="selectuser.js"></script>
</head>
<body>

<form> 
Select a User:
<select name="users" onchange="showUser(this.value)">
<option value="1">Peter Griffin</option>
<option value="2">Lois Griffin</option>
<option value="3">Glenn Quagmire</option>
<option value="4">Joseph Swanson</option>
</select>
</form>

<p>
<div id="txtHint"><b>User info will be listed here.</b></div>
</p>

</body>
</html>
```
js：  

```
var xmlHttp

function showUser(str)
{ 
xmlHttp=GetXmlHttpObject()
if (xmlHttp==null)
 {
 alert ("Browser does not support HTTP Request")
 return
 }
var url="getuser.php"
url=url+"?q="+str
url=url+"&sid="+Math.random()
xmlHttp.onreadystatechange=stateChanged 
xmlHttp.open("GET",url,true)
xmlHttp.send(null)
}

function stateChanged() 
{ 
if (xmlHttp.readyState==4 || xmlHttp.readyState=="complete")
 { 
 document.getElementById("txtHint").innerHTML=xmlHttp.responseText 
 } 
}

function GetXmlHttpObject()
{
var xmlHttp=null;
try
 {
 // Firefox, Opera 8.0+, Safari
 xmlHttp=new XMLHttpRequest();
 }
catch (e)
 {
 //Internet Explorer
 try
  {
  xmlHttp=new ActiveXObject("Msxml2.XMLHTTP");
  }
 catch (e)
  {
  xmlHttp=new ActiveXObject("Microsoft.XMLHTTP");
  }
 }
return xmlHttp;
}
```
php:

```
<?php
$q=$_GET["q"];

$con = mysql_connect('localhost', 'peter', 'abc123');
if (!$con)
 {
 die('Could not connect: ' . mysql_error());
 }

mysql_select_db("ajax_demo", $con);

$sql="SELECT * FROM user WHERE id = '".$q."'";

$result = mysql_query($sql);

echo "<table border='1'>
<tr>
<th>Firstname</th>
<th>Lastname</th>
<th>Age</th>
<th>Hometown</th>
<th>Job</th>
</tr>";

while($row = mysql_fetch_array($result))
 {
 echo "<tr>";
 echo "<td>" . $row['FirstName'] . "</td>";
 echo "<td>" . $row['LastName'] . "</td>";
 echo "<td>" . $row['Age'] . "</td>";
 echo "<td>" . $row['Hometown'] . "</td>";
 echo "<td>" . $row['Job'] . "</td>";
 echo "</tr>";
 }
echo "</table>";

mysql_close($con);
?>
```


###其他
1. 创建标准页头、页脚或者菜单文件等  
include "filename"  
请在此时使用 require：当文件被应用程序请求时。（ require 语句返回严重错误之后脚本就会终止执行）  
请在此时使用 include：当文件不是必需的，且应用程序在文件未找到时应该继续运行时。 （如果用 include 语句引用某个文件并且 PHP 无法找到它，脚本会继续执行）
2. cookie和session（？？？）   
setcookie() 函数用于设置 cookie。  
setcookie() 函数必须位于 <html> 标签之前。
`setcookie(name, value, expire, path, domain);`
`setcookie("user", "Alex Porter", time()+3600);`//到期时间

```
<html>
<body>

 <?php
if (isset($_COOKIE["user"])) //isset()确认是否已设置了 cookie
  echo "Welcome " . $_COOKIE["user"] . "!<br />";//取回cookie
else
  echo "Welcome guest!<br />";
?>

</body>
</html>
```
 

 删除 cookie :使过期日期变更为过去的时间点。

```
<?php 
// set the expiration date to one hour ago
setcookie("user", "", time()-3600);
?>
```
