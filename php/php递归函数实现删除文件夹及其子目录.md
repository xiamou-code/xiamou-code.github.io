## PHP递归函数实现删除文件夹及其子目录

**递归函数： recursion  函数自身调用自身， 但必须调用自身之前有满足特定条件，否则会无线调用下去。**

```php
<?php
// 封装一个递归函数，目的是删除所有缓存目录（及其子目录，文件）
$dir = __DIR__ . DIRECTORY_SEPARATOR . 'runtime';
// echo $dir;
function delete_dir_file($dir)
{
    $flag = false; //默认没删除成功runtime目录
    //判断是不是文件夹
    if (is_dir($dir)) {
        // 打开目录流 成功返回一个资源类型 目录句柄  否则false
        if ($handle = opendir($dir)) {
            while (($file = readdir($handle)) !== false) {
                // 在php中删除一个文件夹的前提是该文件夹为空
                if ($file != '.'  &&  $file != '..') {
                    if (is_dir($dir . DIRECTORY_SEPARATOR . $file)) {
                        // 子内容是目录
                        delete_dir_file($dir . DIRECTORY_SEPARATOR . $file);
                    } else {
                        // 子内容是文件
                        unlink($dir . DIRECTORY_SEPARATOR . $file);
                    }
                }
            }
            closedir($handle);
            if (rmdir($dir)) {
                $flag = true;
            }
        }
    } else {
        echo "没有找到文件夹...";
    }
    return $flag;
}
$res = delete_dir_file($dir);
if ($res) {
    echo json_encode(['msg' => '清除成功', 'satatus' => 1], 320);
}
```

### 数据库初步

#### 常用SQL语句

SELECT USER()

得到登陆的用户

SELECT VERSION()

得到MySQL的版本信息

SELECT NOW()

得到当前的日期时间

SELECT DATABASE()

得到当前打开的数据库

![WechatIMG39](/Users/xja/Downloads/PHP中文网学习/notes/homework/images/WechatIMG39.png)

![WechatIMG40](/Users/xja/Downloads/PHP中文网学习/notes/homework/images/WechatIMG40.png)![WechatIMG41](/Users/xja/Downloads/PHP中文网学习/notes/homework/images/WechatIMG41.png)操作环境：MacOs11.4(m1芯片) + Navicat Premium 15 + MAMP

