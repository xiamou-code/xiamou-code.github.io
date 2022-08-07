## php运行机制与原理及变量

![1642597306718](/Users/xja/Downloads/PHP中文网学习/notes/homework/images/1642597306718.jpg)

> #### 扫描（scanning）：进行语法分析和词法分析，然后将`index.php`内容变成一个个语言片段（token）

> #### 解析（parsing）：词法分析后，就需要一个个token去组成有意义的表达式
>
> > **Parsing 首先会丢弃tokens array 中的多余的空格，然后将剩余的tokens转换成一个个简单的表达式**

> #### 编译（complication）：将表达式编译成中间码（opcode）

> #### 执行（execution）：将中间码一条一条执行

> #### 输出缓存（output buffer）：将要输出的内容输出到缓冲区，然后输出到浏览器/客户端

![1642599326581](/Users/xja/Downloads/PHP中文网学习/notes/homework/images/1642599326581.jpg)

**分析代码结果发现，源码中的字符串，字符，空格，都会原样返回，并且出现在相应顺序处。**

token id (在zend内部该token的对应码)

具体的定义可以在`zend_language_parser.h`中找到

------
### PHP变量类型

```php
<?php
// php一共有八种变量类型
/**
 * 4种标量类型：整型 字符串 布尔型 浮点型
 * 2种复合类型：数组 对象
 * 2种特殊类型：resource null
 **/
$username = 'PHP中文网学员';
$password = 123456;
$boolean = true;
$float = 1.1;
// 数组:一维数组，多维数组
// 下标为整型的索引数组
$arr = [147, 258, 369, 253];
var_dump($arr);
// 下标为字符串的关联数组
$user = ['id' => 1, 'name' => 'PHP中文网学员一号', 'email' => '123456@php.cn'];
var_dump($user);
// 多维数组 
$users = [
    ['id' => 1, 'name' => '学员一号', 'email' => '123456@php.cn'],
    ['id' => 2, 'name' => '学员二号', 'email' => '567891@php.cn'],
    ['id' => 3, 'name' => '学员三号', 'email' => '987652@php.cn']
];
var_dump($users);
// 对象 
$obj = new stdClass;
var_dump($obj);
//Resource 资源类型
//资源 resource 是一种特殊变量，保存了到外部资源的一个引用。由于资源类型变量保存有为打开文件、数据库连接、图形画布区域等的特殊句柄，因此将其它类型的值转换为资源没有意义。
/**
* 特殊的 null 值表示一个变量没有值。NULL 类型唯一可能的值就是 null。
* 在下列情况下一个变量被认为是 null：
* 被赋值为 null。
* 尚未被赋值。
* 被 unset()。
**/
$strchar = "Hello world!";
$strchar = null;
var_dump($x);
```

**var_dump()** 函数用于输出变量的相关信息。

>**函数显示关于一个或多个表达式的结构信息，包括表达式的类型与值。数组将递归展开值，通过缩进显示其结构。**

