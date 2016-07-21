# php7 新特性

### 标量类型声明
```
    有两种模式 : 强制 ( 默认 ) 和 严格模式。 现在可以使用下列类型参数（无论用强制模式还是严格模式）： 字符串 (string), 整数 (int), 浮点数 (float), 以及布尔值 (bool) 。它们扩充了 PHP5 中引入的其他类型：类名，接口，数组和 回调类型。在旧版中，函数的参数申明只能是 (Array $arr) 、 (CLassName $obj) 等，基本类型比如 Int ， String 等是不能够被申明的

    <?php
    function check(int $bool)
    {
        var_dump($bool);
    }
    check(1);
    check(true);
    ?>
    若无强制类型转换，会输入 int(1)bool(true) 。转换后会输出 bool(true) bool(true)
```

### 返回值类型声明
```
PHP 7 增加了对返回类型声明的支持。返回类型声明指明了函数返回值的类型。可用的类型与参数声明中可用的类型相同。

<?php
    function arraysSum(array ...$arrays): array
    {
        return array_map(function(array $array): int 
        {
            return array_sum($array);
        }, $arrays);
    }
    print_r(arraysSum([1,2,3], [4,5,6], [7,8,9]));
    以上例程会输出：
    Array
    (
        [0] => 6
        [1] => 15
        [2] => 24
    )
?>
```

### null 合并运算符
```
项目中存在大量同时使用三元表达式和 isset() 的情况，新增了 null 合并运算符 (??) 这个语法糖。如果变量存在且值不为 NULL ， 它就会返回自身的值，否则返回它的第二个操作数。

旧版： isset($_GET[‘id']) ? $_GET[id] : err;

新版： $_GET['id'] ?? 'err';
```

### 太空船操作符（组合比较符）
```
太空船操作符用于比较两个表达式。当 $a 大于、等于或小于 $b 时它分别返回 -1 、 0 或 1 。 比较的原则是沿用 PHP 的常规比较规则进行的。

<?php

    // Integers
    echo 1 <=> 1; // 0
    echo 1 <=> 2; // -1
    echo 2 <=> 1; // 1
    // Floats
    echo 1.5 <=> 1.5; // 0
    echo 1.5 <=> 2.5; // -1
    echo 2.5 <=> 1.5; // 1
    // Strings
    echo "a" <=> "a"; // 0
    echo "a" <=> "b"; // -1
    echo "b" <=> "a"; // 1
?>
```

### 通过 define() 定义常量数组
```
<?php

    define('ANIMALS', ['dog', 'cat', 'bird']);

    echo ANIMALS[1]; // outputs "cat"
?>
```

### 匿名类
```
现在支持通过 new class 来实例化一个匿名类，这可以用来替代一些“用后即焚”的完整类定义.

<?php

    interface Logger 
    {
        public function log(string $msg);
    }

    class Application 
    {
        private $logger;
        public function getLogger(): Logger
        {
            return $this->logger;
        }
        public function setLogger(Logger $logger)
        {
            $this->logger = $logger;
        }

    }
    $app = new Application;
    $app->setLogger(new class implements Logger {
        public function log(string $msg)
        {
            echo $msg;
        }
    });
    var_dump($app->getLogger());
?>
```

###  Unicode codepoint 转译语法
```
这接受一个以 16 进制形式的 Unicode codepoint ，并打印出一个双引号或 heredoc 包围的 UTF-8 编码格式的字符串。 可以接受任何有效的 codepoint ，并且开头的 0 是可以省略的。

<?php
    echo “\u{9876}”
?>

旧版输出： \u{9876}

新版输入：顶
```

### Closure::call()
```
Closure::call() 现在有着更好的性能，简短干练的暂时绑定一个方法到对象上闭包并调用它。

<?php
    class Test{public $name = "lixuan";}
    //PHP7 和 PHP5.6 都可以
    $getNameFunc = function(){return $this->name;};
    $name = $getNameFunc->bindTo(new Test, 'Test');
    echo $name();
    //PHP7 可以 ,PHP5.6 报错
    $getX = function() {return $this->name;};
    echo $getX->call(new Test);
?>
```

### 为 unserialize() 提供过滤
```
这个特性旨在提供更安全的方式解包不可靠的数据。它通过白名单的方式来防止潜在的代码注入。

<?php

    // 将所有对象分为 __PHP_Incomplete_Class 对象
    $data = unserialize($foo, ["allowed_classes" => false]);
    // 将所有对象分为 __PHP_Incomplete_Class 对象 除了 ClassName1 和 ClassName2
    $data = unserialize($foo, ["allowed_classes" => ["ClassName1", "ClassName2"]);
    // 默认行为，和 unserialize($foo) 相同
    $data = unserialize($foo, ["allowed_classes" => true]);

?>
```
### IntlChar
```
新增加的 IntlChar 类旨在暴露出更多的 ICU功能。这个类自身定义了许多静态方法用于操作多字符集的 unicode 字符。 Intl 是 Pecl 扩展，使用前需要编译进 PHP 中，也可 apt-get/yum/port install php5-intl

<?php

    printf('%x', IntlChar::CODEPOINT_MAX);
    echo IntlChar::charName('@');
    var_dump(IntlChar::ispunct('!'));

?>

以上例程会输出：

10ffff

COMMERCIAL AT

bool(true)
```

### 预期
```
预期是向后兼用并增强之前的 assert() 的方法。 它使得在生产环境中启用断言为零成本，并且提供当断言失败时抛出特定异常的能力。老版本的 API 出于兼容目的将继续被维护， assert()现在是一个语言结构，它允许第一个参数是一个表达式，而不仅仅是一个待计算的 string或一个待测试的 boolean 。

<?php

    ini_set('assert.exception', 1);
    class CustomError extends AssertionError {}
    assert(false, new CustomError('Some error message'));
?>
以上例程会输出：

Fatal error: Uncaught CustomError: Some error message

```

### Group use declarations (命名空间的变化)
```
从同一 namespace 导入的类、函数和常量现在可以通过单个 use 语句 一次性导入了。
<?php

    //PHP7 之前
    use some\namespace\ClassA;
    use some\namespace\ClassB;
    use some\namespace\ClassC as C;
    use function some\namespace\fn_a;
    use function some\namespace\fn_b;
    use function some\namespace\fn_c;
    use const some\namespace\ConstA;
    use const some\namespace\ConstB;
    use const some\namespace\ConstC;
    // PHP7 之后
    use some\namespace\{ClassA, ClassB, ClassC as C};
    use function some\namespace\{fn_a, fn_b, fn_c};
    use const some\namespace\{ConstA, ConstB, ConstC};
?>

```

### intdiv()
```
    接收两个参数作为被除数和除数，返回他们相除结果的整数部分。

    <?php

        var_dump(intdiv(7, 2));
    ?>
    输出 int(3)
```

### CSPRNG
```
    新增两个函数 : random_bytes() and random_int(). 可以加密的生产被保护的整数和字符串。(就是随机数变得安全了)

    andom_bytes — 加密生存被保护的伪随机字符串

    random_int — 加密生存被保护的伪随机整数
```

### preg_replace_callback_array()
```
    新增了一个函数 preg_replace_callback_array() ，使用该函数可以使得在使用 preg_replace_callback() 函数时代码变得更加优雅。在 PHP7之前，回调函数会调用每一个正则表达式，回调函数在部分分支上是被污染了。

```

### Session options
```
    现在， session_start() 函数可以接收一个数组作为参数，可以覆盖 php.ini 中 session 的配置项。

    比如，把 cache_limiter 设置为私有的，同时在阅读完 session 后立即关闭。
    <?php
        session_start([
        'cache_limiter' => 'private',
        'read_and_close' => true,
        ]);
    ?>
```

### 生成器的返回值
```
    在 PHP5.5 引入生成器的概念。生成器函数每执行一次就得到一个 yield 标识的值。在 PHP7 中，当生成器迭代完成后，可以获取该生成器函数的返回值。通过 Generator::getReturn() 得到。
    <?php

        function generator()
        {
            yield 1;
            yield 2;
            yield 3;
            return "a";
        }
        $generatorClass = ("generator")();
        foreach ($generatorClass as $val)
        {
            echo $val." ";
        }
        echo $generatorClass->getReturn();
    ?>
    输出为： 1 2 3 a
```

### 生成器中引入其他生成器
```
    在生成器中可以引入另一个或几个生成器，只需要写 yield from functionName1

<?php

    function generator1()
    {
        yield 1;
        yield 2;
        yield from generator2();
        yield from generator3();
    }

    function generator2()
    {
        yield 3;
        yield 4;
    }
    function generator3()
    {
        yield 5;
        yield 6;
    }

    foreach (generator1() as $val)
    {
        echo $val, " ";
    }
?>
输出： 1 2 3 4 5 6
```

## 不兼容性

### foreach 不再改变内部数组指针
```
在 PHP7 之前，当数组通过 foreach 迭代时，数组指针会移动。现在开始，不再如此，见下面代码。

<?php

    $array = [0, 1, 2];
    foreach ($array as &$val)
    {
        var_dump(current($array));
    }
?>
    PHP5 输出：
    int(1)
    int(2)
    bool(false)
    
    PHP7 输出：
    int(0)
    int(0)
    int(0)
```

### foreach 通过引用遍历时，有更好的迭代特性
```
当使用引用遍历数组时，现在 foreach在迭代中能更好的跟踪变化。例如，在迭代中添加一个迭代值到数组中，参考下面的代码：

<?php

    $array = [0];
    foreach ($array as &$val)
    {
        var_dump($val);
        $array[1] = 1;
    }

?>

PHP5 输出：
int(0)

PHP7 输出：
int(0)
int(1)
```

### 十六进制字符串不再被认为是数字
```
<?php

    var_dump("0x123" == "291");
    var_dump(is_numeric("0x123"));
    var_dump("0xe" + "0x1");
    var_dump(substr("foo", "0x1"));

?>

PHP5 输出：
bool(true)
bool(true)
int(15)
string(2) "oo"

PHP7 输出：
bool(false)
bool(false)
int(0)

Notice: A non well formed numeric value encountered in /tmp/test.php on line 5
string(3) "foo"
```

### PHP7 中被移除的函数
```
被移除的函数列表如下：

1: call_user_func() 和 call_user_func_array() 从 PHP 4.1.0 开始被废弃。

2: 已废弃的 mcrypt_generic_end() 函数已被移除，请使用 mcrypt_generic_deinit() 代替。

3: 已废弃的 mcrypt_ecb(), mcrypt_cbc(), mcrypt_cfb() 和 mcrypt_ofb() 函数已被移除。

4: set_magic_quotes_runtime(), 和它的别名 magic_quotes_runtime() 已被移除 . 它们在 PHP 5.3.0 中已经被废弃 , 并且 在 in PHP 5.4.0 也由于魔术引号的废弃而失去功能。

5: 已废弃的 set_socket_blocking() 函数已被移除，请使用 stream_set_blocking() 代替。

6: dl() 在 PHP-FPM 不再可用，在 CLI 和 embed SAPIs 中仍可用。

7: GD 库中下列函数被移除： imagepsbbox() 、 imagepsencodefont() 、 imagepsextendfont() 、 imagepsfreefont() 、 imagepsloadfont() 、 imagepsslantfont() 、 imagepstext()

8: 在配置文件 php.ini 中， always_populate_raw_post_data 、 asp_tags 、 xsl.security_prefs 被移除了。

```

### new 操作符创建的对象不能以引用方式赋值给变量
```
new 操作符创建的对象不能以引用方式赋值给变量

<?php

    class C {}
    $c =& new C;
?>

PHP5 输出：
Deprecated: Assigning the return value of new by reference is deprecated in /tmp/test.php on line 3

PHP7 输出：
Parse error: syntax error, unexpected 'new' (T_NEW) in /tmp/test.php on line 3
```

### 移除了 ASP 和 script PHP 标签
```
使用类似 ASP 的标签，以及 script 标签来区分 PHP 代码的方式被移除。 受到影响的标签有： <% %> 、 <%= %> 、 <script language="php"> </script>
```

### 从不匹配的上下文发起调用
```
在不匹配的上下文中以静态方式调用非静态方法， 在 PHP 5.6 中已经废弃， 但是在 PHP 7.0 中， 会导致被调用方法中未定义 $this 变量，以及此行为已经废弃的警告。

<?php

class A
{
    public function test() { var_dump($this); }
}
// 注意：并没有从类 A 继承
class B 
{
    public function callNonStaticMethodOfA() { A::test(); }
}
(new B)->callNonStaticMethodOfA();
?>

PHP5 输出：

Deprecated: Non-static method A::test() should not be called statically, assuming $this from incompatible context in /tmp/test.php on line 8

object(B)#1 (0) {

}

PHP7 输出：

Deprecated: Non-static method A::test() should not be called statically in /tmp/test.php on line 8

Notice: Undefined variable: this in /tmp/test.php on line 3

NULL
```

### 在数值溢出的时候，内部函数将会失败
```
浮点数转换为整数的时候，如果浮点数值太大，导致无法以整数表达的情况下， 在之前的版本中，内部函数会直接将整数截断，并不会引发错误。 在 PHP 7.0 中，如果发生这种情况，会引发 E_WARNING 错误，并且返回 NULL 。
```

### JSON 扩展已经被 JSOND 取代
```
JSON 扩展已经被 JSOND 扩展取代。 对于数值的处理，有以下两点需要注意的： 第一，数值不能以点号（ . ）结束 （例如，数值 34. 必须写作 34.0 或 34 ）。 第二，如果使用科学计数法表示数值， e 前面必须不是点号（ . ） （例如， 3.e3 必须写作 3.0e3 或 3e3 ）。
```

###  INI 文件中 # 注释格式被移除
```
在配置文件 INI 文件中，不再支持以 # 开始的注释行， 请使用 ; （分号）来表示注释。 此变更适用于 php.ini 以及用 parse_ini_file() 和 parse_ini_string() 函数来处理的文件。
```

### $HTTP_RAW_POST_DATA 被移除
```
不再提供 $HTTP_RAW_POST_DATA 变量。 请使用 php://input 作为替代。
```

###  yield 变更为右联接运算符
```
在使用 yield 关键字的时候，不再需要括号， 并且它变更为右联接操作符，其运算符优先级介于 print 和 => 之间。 这可能导致现有代码的行为发生改变。可以通过使用括号来消除歧义。

<?php

    echo yield -1;
    // 在之前版本中会被解释为：
    echo (yield) - 1;
    // 现在，它将被解释为：
    echo yield (-1);
    yield $foo or die;
    // 在之前版本中会被解释为：
    yield ($foo or die);
    // 现在，它将被解释为：
    (yield $foo) or die;

?>
```


PHP 官方网站文档： http://php.net/manual/zh/migration70.php

可以浏览 PHP5.6 到 PHP7 时，新特性、新增函数、已经被移除的函数、不兼容性、新增的类和接口等内容。
