## PHP登录与防止数据注入实例演示

#### 首先准备一张简单的用户数据表

![1643029545374](/Users/xja/Downloads/PHP中文网学习/notes/homework/images/1643029545374.jpg)

#### 编写database.php

```Php
<?php
return [
    'type' => $type ?? 'mysql',
    'username' => $username ?? 'root',
    'password' => $password ?? 'root',
    'host' => $host ?? 'localhost',
    'port' => $port ?? '3308',
    'charset' => $charset ?? 'utf8',
    'dbname' => 'mydatabase'
];
```

#### 编写connect.php

```Php
<?php
// 1. 把数据库连接配置文件引过来
$config = require_once 'database.php';
extract($config);
// 2. 数据库的连接
$dsn = sprintf('%s:host=%s;port=%s;charset=%s;dbname=%s', $type, $host, $port, $charset, $dbname);
try {
    $pdo = new PDO($dsn, $username, $password);
    //var_dump($pdo);
} catch (PDOException $e) {
    die('Connection error : ' . $e->getMessage());
}
```

#### 编写login.php

```php+HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>系统登录</title>
</head>
<style type="text/css">
    html {
        overflow-y: scroll;
        vertical-align: baseline;
    }
    body {
        font-family: Microsoft YaHei, Segoe UI, Tahoma, Arial, Verdana, sans-serif;
        font-size: 12px;
        color: #fff;
        height: 100%;
        line-height: 1;
        background: #999
    }
    * {
        margin: 0;
        padding: 0
    }
    ul,
    li {
        list-style: none
    }
    h1 {
        font-size: 30px;
        font-weight: 700;
        text-shadow: 0 1px 4px rgba(0, 0, 0, .2)
    }
    .login-box {
        width: 410px;
        margin: 120px auto 0 auto;
        text-align: center;
        padding: 30px;
        background: url('http://demo.mxyhn.xyz:8020/cssthemes6/11.20ZF8/images/mask.png');
        border-radius: 10px;
    }
    .login-box .name,
    .login-box .password,
    .login-box .code,
    .login-box .remember {
        font-size: 16px;
        text-shadow: 0 1px 2px rgba(0, 0, 0, .1)
    }
    .login-box .remember input {
        box-shadow: none;
        width: 15px;
        height: 15px;
        margin-top: 25px
    }
    .login-box .remember {
        padding-left: 40px
    }
    .login-box .remember label {
        display: inline-block;
        height: 42px;
        width: 70px;
        line-height: 34px;
        text-align: left
    }
    .login-box label {
        display: inline-block;
        width: 100px;
        text-align: right;
        vertical-align: middle
    }
    .login-box #code {
        width: 120px
    }
    .login-box .codeImg {
        float: right;
        margin-top: 26px;
    }
    .login-box img {
        width: 148px;
        height: 42px;
        border: none
    }
    input[type=text],
    input[type=password] {
        width: 270px;
        height: 42px;
        margin-top: 25px;
        padding: 0px 15px;
        border: 1px solid rgba(255, 255, 255, .15);
        border-radius: 6px;
        color: #fff;
        letter-spacing: 2px;
        font-size: 16px;
        background: transparent;
    }
    button {
        cursor: pointer;
        width: 100%;
        height: 44px;
        padding: 0;
        background: #ef4300;
        border: 1px solid #ff730e;
        border-radius: 6px;
        font-weight: 700;
        color: #fff;
        font-size: 24px;
        letter-spacing: 15px;
        text-shadow: 0 1px 2px rgba(0, 0, 0, .1)
    }
    input:focus {
        outline: none;
        box-shadow: 0 2px 3px 0 rgba(0, 0, 0, .1) inset, 0 2px 7px 0 rgba(0, 0, 0, .2)
    }
    button:hover {
        box-shadow: 0 15px 30px 0 rgba(255, 255, 255, .15) inset, 0 2px 7px 0 rgba(0, 0, 0, .2)
    }
    .screenbg {
        position: fixed;
        bottom: 0;
        left: 0;
        z-index: -999;
        overflow: hidden;
        width: 100%;
        height: 100%;
        min-height: 100%;
    }
    .screenbg ul li {
        display: block;
        list-style: none;
        position: fixed;
        overflow: hidden;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: -1000;
        float: right;
    }
    .screenbg ul a {
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
        display: inline-block;
        margin: 0;
        padding: 0;
        cursor: default;
    }
    .screenbg a img {
        vertical-align: middle;
        display: inline;
        border: none;
        display: block;
        list-style: none;
        position: fixed;
        overflow: hidden;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: -1000;
        float: right;
    }
    .bottom {
        margin: 8px auto 0 auto;
        width: 100%;
        position: fixed;
        text-align: center;
        bottom: 0;
        left: 0;
        overflow: hidden;
        padding-bottom: 8px;
        color: white;
        word-spacing: 3px;
        zoom: 1;
    }
    .bottom a {
        color: #FFF;
        text-decoration: none;
    }
    .bottom a:hover {
        text-decoration: underline;
    }
</style>
<script type="text/javascript" src="http://demo.mxyhn.xyz:8020/cssthemes6/11.20ZF8/js/jquery-1.8.2.min.js"></script>
<script type="text/javascript">
    $(function() {
        $(".screenbg ul li").each(function() {
            $(this).css("opacity", "0");
        });
        $(".screenbg ul li:first").css("opacity", "1");
        var index = 0;
        var t;
        var li = $(".screenbg ul li");
        var number = li.size();
        function change(index) {
            li.css("visibility", "visible");
            li.eq(index).siblings().animate({
                opacity: 0
            }, 3000);
            li.eq(index).animate({
                opacity: 1
            }, 3000);
        }
        function show() {
            index = index + 1;
            if (index <= number - 1) {
                change(index);
            } else {
                index = 0;
                change(index);
            }
        }
        t = setInterval(show, 8000);
        //根据窗口宽度生成图片宽度
        var width = $(window).width();
        $(".screenbg ul img").css("width", width + "px");
    });
</script>
<body>
    <div class="login-box">
        <h1>瓜子二手车系统后台登录</h1>
        <form>
            <div class="name">
                <label>管理员账号：</label>
                <input type="text" required name="username" id="" tabindex="1" autocomplete="off" />
            </div>
            <div class="password">
                <label>密 码：</label>
                <input type="password" required name="password" maxlength="16" id="" tabindex="2" />
            </div>
            <!-- <div class="code">
                <label>验证码：</label>
                <input type="text" name="" maxlength="4" id="code" tabindex="3" />
                <div class="codeImg">
                    <img src="images/captcha.jpeg.jpg" />
                </div>
            </div> -->
            <div class="remember">
                <input type="checkbox" id="remember" tabindex="4">
                <label>记住密码</label>
            </div>
            <div class="login">
                <input type="button" name="btn" value="登录">
            </div>
        </form>
    </div>
    <div class="bottom">©2022php中文网 </div>
    <div class="screenbg">
        <ul>
            <li><a href="javascript:;"><img src="http://demo.mxyhn.xyz:8020/cssthemes6/11.20ZF8/images/0.jpg"></a></li>
            <li><a href="javascript:;"><img src="http://demo.mxyhn.xyz:8020/cssthemes6/11.20ZF8/images/1.jpg"></a></li>
            <li><a href="javascript:;"><img src="http://demo.mxyhn.xyz:8020/cssthemes6/11.20ZF8/images/2.jpg"></a></li>
        </ul>
    </div>
    <div style="text-align:center;">

    </div>
    <script type="text/javascript">
        // 登录 
        $('input[name="btn"]').click(function() {
            var data = {};
            data.username = $.trim($('input[name="username"]').val());
            data.password = $.trim($('input[name="password"]').val());

            $.post('submit.php', data, function(res) {
                console.log(res);
            }, "json")
        })
    </script>
</body>
</html>
```

#### 编写submit.php

```php
// 后端接受前端穿过来的参数
$name = isset($_POST['username']) ? $_POST['username'] : null;
$pwd = isset($_POST['password']) ? $_POST['password'] : null;
// 1.连接数据库
require_once 'connect.php';
// 检查密码的正确性
$pwd = md5($pwd);
$stmt = $pdo->query("SELECT * FROM `userinfo` WHERE `username`='{$name}' AND `password` = '{$pwd}' ");
//var_dump($stmt);
foreach ($stmt as $row) {
    print $row['username'] . "\t";
    print $row['password'] . "\t";
    print $row['id'] . "\n";
}
```

![1643030501710](/Users/xja/Downloads/PHP中文网学习/notes/homework/images/1643030501710.jpg)

正常登录打印，但是当我们在用户名入`' or 1=1#`

就会发现...

![1643030707624](/Users/xja/Downloads/PHP中文网学习/notes/homework/images/1643030707624.jpg)

数据全给打印出来了。。。

因此，重新编辑submit.php如下

```php
<?php
// 后端接受前端穿过来的参数
$name = isset($_POST['username']) ? $_POST['username'] : null;
$pwd = isset($_POST['password']) ? $_POST['password'] : null;
// 1.连接数据库
require_once 'connect.php';
// 检查密码的正确性
$pwd = md5($pwd);
$stmt = $pdo->query("SELECT * FROM `userinfo` WHERE `username`='{$name}' AND `password` = '{$pwd}' ");
//var_dump($stmt);
// foreach ($stmt as $row) {
//     print $row['username'] . "\t";
//     print $row['password'] . "\t";
//     print $row['id'] . "\n";
// }
// pdo预处理
// 准备一条预处理sql语句
$sql = "SELECT * FROM `user` WHERE `username`= ? AND `password` = ? ";
// 准备要执行的语句，并返回语句对象
$stmt = $pdo->prepare($sql);
// 绑定参数到指定的变量名
$stmt->bindParam(1, $name);
$stmt->bindParam(2, $pwd);
// 执行一条预处理语句
$stmt->execute();
$result = $stmt->fetchAll(PDO::FETCH_ASSOC);
var_dump($result);
```

增加数据库预处理语句，大功告成！
