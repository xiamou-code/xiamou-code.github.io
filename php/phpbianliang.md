## PHP的变量类型及函数

###  变量是用于存储信息的"容器"

####PHP 变量规则：

 \* 变量以 $ 符号开始，后面跟着变量的名称

 \* 变量名必须以字母或者下划线字符开始

 \* 变量名只能包含字母、数字以及下划线（A-z、0-9 和 _ ）

 \* 变量名不能包含空格

 \* 变量名是区分大小写的（$y 和 $Y 是两个不同的变量）

#### 字符串（String）

一个字符串是一串字符的序列，就像 "Hello world!"。

```Php
$astr = "Hello world!";
echo $astr;
echo "<hr>";
```

#### 整型（Integer）

* 整数是一个没有小数的数字。
* 整数规则:
* 整数必须至少有一个数字 (0-9)
* 整数不能包含逗号或空格
* 整数是没有小数点的
* 整数可以是正数或负数
* 整型可以用三种格式来指定：十进制， 十六进制（ 以 0x 为前缀）或八进制（前缀为 0）。

```Php
$x = 1234;
var_dump($x);
echo "<br>"; 
$X = -1234; // 负数 
var_dump($X);
echo "<br>"; 
$a = 0x8; // 十六进制数
var_dump($a);
echo "<br>";
$b = 011; // 八进制数
var_dump($b);
/**
 * 浮点型：
 * 浮点数是带小数部分的数字，或是指数形式。
**/
$afloat = 0.1;
var_dump($afloat);
echo "<br>"; 
$xe = 1.1e3;
var_dump($xe);
echo "<br>"; 
$xee = 3E-5;
var_dump($xee);
echo "<hr>";
echo $xee . "^^^^" . $xe;
```

#### 布尔型（Boolean）

```php
/**
 * 布尔型可以是 TRUE 或 FALSE
**/
$btrue = true;
$bfalse = false;
$bbtrue = TRUE;
$BBFALSE = FALSE;
echo "<hr>";
echo "$btrue---$bfalse---$bbtrue---$BBFALSE";
echo "<hr>";
var_dump($bfalse);
echo "<hr>";
echo $bfalse . "------" . $btrue . "------";//false 在浏览器上没有输出，true在浏览器上输出1
```

#### 数组（Array）

 \* 数组是一个能在单个变量中存储多个值的特殊变量。

 \* 在 PHP 中，有三种类型的数组：

 \* 数值数组 - 带有数字 ID 键的数组

 \* 关联数组 - 带有指定的键的数组，每个键关联一个值

 \* 多维数组 - 包含一个或多个数组的数组

```php
echo "<hr>";
$myarray = ['php','中文网','学习'];
$myarray1 = array('php','中文网','天下第一');
echo "我在{$myarray[0]}{$myarray[1]}{$myarray[2]}";
echo "<br>";
var_dump($myarray1);
echo PHP_EOL;
$stu=array("php学员"=>"21448","age"=>"99","weight"=>"100kg");
echo "<br>";
echo "学号：". $stu['php学员'] . "<br>" . "年龄：" . $stu['age'] . "<br>" . "体重：" . $stu['weight'];
$stus = [
    ['name' => '学员一', 'stuNo' => 001, 'mail' => '1234@php.cn'],
    ['name' => '学员二', 'stuNo' => 002, 'mail' => '5678@php.cn']
];
echo "<hr>";
for ($i = 0; $i < count($stus); $i++){
    echo $stus[$i]['name'] . $stus[$i]['mail'] . "<br>";
}
```

#### 对象（Object）

> 如果将一个对象转换成对象，它将不会有任何变化。如果其它任何类型的值被转换成对象，将会创建一个内置类 `stdClass` 的实例。如果该值为 **null**，则新的实例为空。 array 转换成 object 将使键名成为属性名并具有相对应的值。注意：在这个例子里， 使用 PHP 7.2.0 之前的版本，数字键只能通过迭代访问。

```php
<?php
$obj = (object) array('1' => 'foo');
var_dump(isset($obj->{'1'})); // PHP 7.2.0 后输出 'bool(true)'，之前版本会输出 'bool(false)' 
var_dump(key($obj)); // PHP 7.2.0 后输出 'string(1) "1"'，之前版本输出  'int(1)' 
?>
```

对于其他值，会包含进成员变量名 `scalar`。

```php
<?php
$obj = (object) 'ciao';
echo $obj->scalar;  // 输出 'ciao'
?>
```

**对象数据类型也可以用于存储数据。在 PHP 中，对象必须声明。**

```Php
class Cat
{
    var $color;
    function __construct($color="green") {
      $this->color = $color;
    }
    function what_color() {
      return $this->color;
    }
}
function print_vars($obj) {
   foreach (get_object_vars($obj) as $prop => $val) {
     echo "\t$prop = $val\n";
   }
}
// 实例一个对象
$herbie = new Cat("white");
echo "\therbie: Properties\n";
print_vars($herbie);
$newobj = new Cat('blue');
echo "<hr>";
print_vars($newobj);
```

#### null

\* NULL 值表示变量没有值。NULL 是数据类型为 NULL 的值。NULL 值指明一个变量是否为空值。 同样可用于数据空值和NULL值的区别。可以通过设置变量值为 NULL 来清空变量数据。

 \* 特殊的 null 值表示一个变量没有值。NULL 类型唯一可能的值就是 null。在下列情况下一个变量被认为是 null：被赋值为 null。尚未被赋值。被 unset()。

 \* null 类型只有一个值，就是不区分大小写的常量 null。

 \* null 不代表变量值为0，null就是null。

```php
$var = NULL;
$snull = null;
var_dump($var);
echo $snull;
var_dump($snull);
```

#### Resource 资源类型

资源 resource 是一种特殊变量，保存了到外部资源的一个引用。资源是通过专门的函数来建立和使用的。
```Php
$fo = fopen('demo.txt','r');
file_put_contents('demo.txt','hello php!');
if (is_resource($fo)) 
{
    echo "文件打开成功...";
}
else 
{
    echo "打开文件错误";
}
```

### php函数

```Php
/**
 * 函数 完成特定功能的代码块
 * 
 * function 函数名称([参数列表,])
 * {
 *      函数体
 *      return 返回值
 * }
 */
```

```php+HTML
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        td {
            width: 100px;
            background: lightblue;
        }
    </style>
</head>

<body>
    <table>
        <tr>
            <?php
            $price = [10, 10, 10, 10, 10];
            function add()
            {
                $total = 0;
                for ($j = 0; $j < count($GLOBALS['price']); $j++) {
                    $total += $GLOBALS['price'][$j];
                }
                return $total;
            };
            $totalp = add();
            foreach ($price as $item) :
                echo "<td>{$item}</td>";
            endforeach;
            echo "<td>总价{$totalp}</td>";
            ?>
        </tr>
    </table>
    <?php
    $price = [12, 45, 65, 213, 231,];

    foreach ($price as $item) {
        echo $item . "<br>";
    }
    ?>
    <ul>
        <?php
        foreach ($price as $item) {
            echo "<li>{$item}</li>";
        }
        ?>
    </ul>
</body>

</html>
```

