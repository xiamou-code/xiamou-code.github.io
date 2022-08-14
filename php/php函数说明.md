## PHP函数说明

 \* 函数是全局成员，不受作用域限制。

 \* php函数的作用：完成特定功能的代码块，封装成函数可以实现复用性，提高代码的可维护性。

 \* php内置了上千种函数可供我们直接调用，函数库文件已经编译到我们所使用的发行版中了，可以直接指定函数名称来调用，当然我们也可以自定义函数来完成特定功能。

 \* 函数的命名规则基本和变量的命名规则一直，可以以字母或下划线开头，后跟字母数字或下划线，但不能以数字开头，函数名不区分大小写。

 \* 函数有三大要素：参数，返回值，函数体。

> function 函数名 (参数1, 参数2, ..., 参数n){
> ​    函数体;
> ​    return 返回值;
> }
>
> 1) 在声明函数时可以没有参数列表：
>
> function 函数名(){
> ​    函数体;
> ​    return 返回值;
> }
>
> 2) 在声明函数时可以没有返回值：
>
> function 函数名(参数1, 参数2, ..., 参数n){
> ​    函数体;
> }
>
> 3) 在声明函数时可以没有参数列表和返回值：
>
> function 函数名(){
> ​    函数体;
> }

>  \* 参数：可选的，对外提供一个接口，供函数调用者按照自己的意愿改变函数体内的执行行为
>
>  \* 参数 形参 实参
>
>  \* 默认参数：有默认值的参数，如果不传参或者少传参数，就会默认参数的值
>
>  \* 参数是从左往右求值，所以默认参数放到最右边
>
> 同一个脚本不能存在同名函数,但是可以使用命名空间

```Php
namespace ns1 {
    function learnphp($name, $webadr, $langu)
    {
        return $name . "在" . $webadr . $langu;
    }
    echo learnphp('白居易', 'php中文网', '学习php');
}

namespace ns2 {
    function learnphp($name, $webadr, $langu)
    {
        return $name . "在" . $webadr . $langu;
    }
    echo "<hr>";
    echo learnphp('李白', 'php中文网', '学习JavaScript');
}

namespace ns3 {
    // 按值传递参数 不会改变全局变量的值,导入到函数中的只是$roomprice的副本,不会更改原来的值
    $num = 10;
    $price = 100;
    function totalbookprice($num, $price, $discount = 1)
    {
        $price *= $discount;
        return $price * $num;
    }
    echo "<hr>";
    echo totalbookprice(2, 54, 0.8);
    echo "<hr>";
    echo totalbookprice($num, $price, 0.5);
    echo "<hr>";
    echo $price;
}

namespace ns4 {
    // 按变量引用传参 会改变父程序里变量的值  $roomprice的变量内容所处的内存地址会被导入到函数中
    $num = 10;
    $price = 100;
    echo "<br>" . $price . "---现在这个值还没有被更改。。。" . "<br>";
    function totalbookprice($num, &$price, $discount = 1)
    {
        $price *= $discount;
        return $price * $num;
    }
    //echo totalbookprice(2,54,0.8);//参数中有引用参数，调用时就不能直接传入参数了
    echo "<hr>";
    echo totalbookprice($num, $price, 0.5);
    echo "<hr>";
    echo $price . "---这个值已经被更改了。。。"; //50
    echo "<hr>";
    echo totalbookprice($num, $price, 0.5);
    echo "<hr>";
    echo $price . "---这个值再次被更改了。。。"; //50
}
```

### 函数返回值

**接口开发 函数返回值会转为通用的json格式的数据返回，这样以来就可以和其他的编程语言进行数据交互，例如js与java，php，python。**

**json_encode()第二个参数是一个常量，JSON_UNESCAPED_UNICODE（中文不转为unicode ，对应的数字 256），JSON_UNESCAPED_SLASHES （不转义反斜杠，对应的数字 64）。**

```php
function test()
{
    return md5('123456');
}
echo test();
echo "<hr>";
$laohuangli = file_get_contents('http://v.juhe.cn/laohuangli/d?date=2022-01-21&key=4f0b4c5**********649b98a');
echo $laohuangli;
echo "<hr>";
$name = "%E5%8F%8C%E9%B1%BC%E5%BA%A7";
$time = "month";
$xingzuo = file_get_contents("http://web.juhe.cn/constellation/getAll?consName={$name}&type={$time}&key=2156088*******806b0fd32f20b");
echo $xingzuo;
```

 * 函数属于全局成员

 \* 特殊形式的函数

 \* 1.匿名函数（闭包函数）

 \* 2.回调函数

 * 3.递归函数

### 命名/匿名函数

```php
echo learn('PHP开发'); //这里也可以调用
echo "<hr>";
function learn(string $language): string  //规定函数的返回值类型
{
    return "我在学习" . $language;
}
echo learn('PHP开发');
// 匿名函数 :通常会被当做回调函数的参数来使用。
$nameless = function (string $lev): string {
    return "PHP中文网" . $lev;
};
echo "<br>" . $nameless('天下第一') . "<br>";
// 闭包引来（变量）作用域的问题
// 全局变量是指声明在函数外部的变量，在函数内部访问不到。
// 局部变量是指声明在函数内部的变量，只能在函数内部被访问到。
// 1. global
// 2. 超全局数组$GLOBALS   $_GET $_POST 
// 3. 闭包函数借助关键字use
// 闭包改变变量上下文的值 需要引用传递
$myname = '李白';
$email = 'libai@php.cn';
function libai()
{
    // global $myname,$email;
    $honor = '诗仙';
    return "恭喜" . $GLOBALS['myname'] . "注册邮箱---" . $GLOBALS['email'] . "成功";
}
echo libai();
echo "<hr>";
$anothername = '杜甫';
$anoemail = 'dufu@php.cn';
$dufu = function ($honor) use ($anothername, $anoemail) {
    return "恭喜{$honor}{$anothername}注册邮箱----{$anoemail}成功";
};
echo $dufu('诗圣') ;
echo "<br>" . $anothername . "----此时，全局变量未被改变";
$changename = function($newname) use (&$anothername){
    $anothername = $newname;
    return $newname;
};
echo "<hr>";
echo $changename('苏轼');
echo "<br>" . $anothername . "----此时，全局变量已经被改变";//此时，全局变量已经被改变

```

### 回调函数

> \* 回调函数：php回调是指在主线程函数执行的过程中,突然跳去执行设置的回调函数,回调函数执行结束后, 再回到主线程处理接下来的流程
>
> \* 匿名函数最通常作为回调函数的参数使用
>
>php脚本是单线程，脚本是同步执行的，如果遇到耗时函数将会发生线程阻塞的问题，应该将它改为异步回调的方式执行
>
>call_user_func()把第一个参数作为回调函数调用
>
>call_user_func_array()
>
>回调的是全局函数 

```Php
//示例：// 给到一个任意数组，把数组中的偶数筛选出来，组成一个新数组 返回 然后计算新数组所有偶数的和
$manynum = [12, 432, 432, 54, 52, 65, 22, 456, 893];
//匿名函数
$odd = function (array $arrs) {
    for ($i = 0; $i < count($arrs); $i++) {
        if ($arrs[$i] % 2 == 0) {
            $newarrs[] = $arrs[$i];
        }
    }
    return $newarrs;
};
var_dump($odd($manynum));
// 求和 params 第一个参数是一个匿名函数，第二个参数是一个任意数组
function sum(Closure $func, $manynum)
{
    $addodd = $func($manynum);
    return array_sum($addodd); //内置求和函数
}
function sums(array $arrplus){
    return array_sum($arrplus);
}
echo "<hr>";
echo sum($odd, $manynum);// 输出调用，传入匿名函数跟数组
echo "<hr>";
$sumplus = call_user_func($odd, $manynum);
var_dump($sumplus);
echo "<hr>";
echo sums($sumplus);
```

### 递归函数

**递归函数即自调用函数，也就是函数在函数体内部直接或间接地自己调用自己。需要注意的是使用递归函数时通常会在函数体中附加一个判断条件，以判断是否需要继续执行递归调用，当条件满足时会终止函数的递归调用。**

```php
<?php
//递归函数
function factorial($num)
{
    //确定递归函数的出口
    if ($num == 1) {
        return 1;
    } else {
        return $num * factorial($num - 1);
    }
}
echo '15 的阶乘是：' . factorial(15);
echo '<hr>';
//计算斐波那契数列
function demo($num)
{
    if ($num == 1 || $num == 2) {
        return 1;
    } else {
        return demo($num - 1) + demo($num - 2);
    }
}
echo '数列第 10 位是：' . demo(10);
```

