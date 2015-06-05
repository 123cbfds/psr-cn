编码风格指引
==================

本指引在基础编码标准[PSR-1]上进行了延伸和扩展.

本指引通过列举一系列格式化PHP代码的通用规则和期望,来帮助开发者减少阅读其他作者代码时的不便和观念上的冲突.

各种样式规则均来自各成员项目中的共性.当不同的作者跨项目协作时,能有一套应用于所有项目的准则.所以,本指引的好处不在这些规则本身,而是在这些公则的共享.

本文中的关键字"MUST(必须)", "MUST NOT(严禁)", "REQUIRED(需要)", "SHALL(应当)", "SHALL NOT(不得)", "SHOULD(应该)",
"SHOULD NOT(不应该)", "RECOMMENDED(建议)", "MAY(可以)", and "OPTIONAL(可选)"遵循[RFC 2119]中的描述.

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt
[PSR-0]: PSR-0.md
[PSR-1]: PSR-1.md


1. 概述
-----------

- 代码MUST(必须)遵循基础编码标准[[PSR-1]].

- 代码缩进MUST(必须)使用4个空格,而非tabs.

- MUST NOT(严禁)强制限制每一行的长度;软限制MUST(必须)是120字符;每行的长度(应该)不超过80个字符.

- 命名空间声明后MUST(必须)有一行空白行,在`use`声明后也MUST(必须)有一行空白行.

- 类开始的左大括号MUST(必须)从第二行开始,结束的右大括号MUST(必须)在代码后换行结束.

- 方法开始的左大括号MUST(必须)从第二行开始,结束的右大括号MUST(必须)在代码后换行结束.

- 所有属性和方法的可见性都MUST(必须)声明;`abstract`和`final` MUST(必须)在可见性之前声明;`static` MUST(必须)在可见性之后声明.
  
- 控制结构的关键字后MUST(必须)有一个空格;请求方法和函数时MUST NOT(严禁)包含空格.

- 控制结构开始的左大括号MUST(必须)在同一行,结束的右大括号MUST(必须)在代码后换行结束.

- 控制结构开始的圆括号MUST NOT(严禁)在后面跟一个空格,结束的圆括号MUST NOT(严禁)在前面出现空格.

### 1.1. 例如

这个例子概述了上面的一些规则:

```php
<?php
namespace Vendor\Package;

use FooInterface;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class Foo extends Bar implements FooInterface
{
    public function sampleFunction($a, $b = null)
    {
        if ($a === $b) {
            bar();
        } elseif ($a > $b) {
            $foo->bar($arg1);
        } else {
            BazClass::bar($arg2, $arg3);
        }
    }

    final public static function bar()
    {
        // method body
    }
}
```

2. 总则
----------

### 2.1 基本编码标准

代码MUST(必须)遵守[PSR-1]里面所有的规则.

### 2.2 文件

所有PHP文件MUST(必须)使用Unix LF做为换行符.

所有PHP文件MUST(必须)以一个空白行结束.

如果文件中只包含PHP代码,则`?>`关闭标记MUST(必须)省略.

### 2.3. 行

行的长度MUST NOT(严禁)有硬限制.

每行长度的软限制MUST(必须)是120个字符;当碰到软限制时,自动样式检查功能MUST(必须)是警告但是MUST NOT(严禁)标识为错误.

每行的长度SHOULD NOT(不应该)超过80个字符;如果一行的长度超过了80个字符,那么SHOULD(应该)拆分成每行不超过80个字的若干行.

非空白行MUST NOT(严禁)以空格结尾.

为了提高代码可读性,MAY(可以)通过插入空行来分割相关的代码块.

每一行MUST NOT(严禁)声明一个以上内容.

### 2.4. 缩进

代码MUST(必须)使用4个空格做为缩进,而MUST NOT(严禁)使用tab做为缩进.

> 注意:仅使用空格而非tab或者两者的混合,能够避免进行对比差异,打补丁,查看历史和写注释时的问题.同时,使用空格能更好的在跨行的代码中进行对齐.

### 2.5. 关键字和True/False/Null

PHP[关键字]MUST(必须)使用小写.

PHP常量`true`, `false`, 和 `null` MUST(必须)使用小写.

[关键字]: http://php.net/manual/en/reserved.keywords.php



3. 命名空间及声明
---------------------------------

在命名空间声明后MUST(必须)插入一个空行.

所有的`use`声明MUST(必须)在`namespace`声明之后.

每个声明都MUST(必须)使用一个`use`关键字.

`use`代码块后MUST(必须)插入一个空行.

例如:

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

// ... additional PHP code ...

```


4. 类,属性和方法
-----------------------------------

"类"泛指所有的类,接口及特征.

### 4.1. 扩展和实现

`extends`和`implements`关键字的声明MUST(必须)和类的名称放在同一行.

类开始的左大括号MUST(必须)在类名的同一行,结束的右大括号MUST(必须)在代码后换行结束.

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements \ArrayAccess, \Countable
{
    // constants, properties, methods
}
```

`implements`列表MAY(可以)拆分成为多行,后续每一行都需要缩进一次.如果这样做时,列表中的第一个项目MUST(必须)换行,每一行MUST(必须)只有一个接口.

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements
    \ArrayAccess,
    \Countable,
    \Serializable
{
    // constants, properties, methods
}
```

### 4.2. 属性

每个属性的可见性都MUST(必须)声明.

MUST NOT(严禁)通过`var`关键字来声明一个属性.

每行语句MUST NOT(严禁)声明超过一个属性,即每行语句只能声明一个属性.

属性名称SHOULD NOT(不应该)通过加一个下划线来表示其保护或者私有的可见性.

属性的声明看起来像下面这样.

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public $foo = null;
}
```

### 4.3. 方法

每个方法的可见性都MUST(必须)声明.

方法名SHOULD NOT(不应该)通过加一个下划线来表示其保护或者私有的可见性.

方法名的声明后MUST NOT(严禁)有空格.方法开始的左大括号MUST(必须)在方法名的同一行,结束的右大括号MUST(必须)在代码后换行结束.开始的左圆括号之后和结束的右圆括号之前MUST NOT(严禁)有空格.

方法的声明看起来像下面这样.注意圆括号,逗号,空格和大括号的位置:

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function fooBarBaz($arg1, &$arg2, $arg3 = [])
    {
        // method body
    }
}
```    

### 4.4. 方法的参数

在参数列表中,每个逗号的前面MUST NOT(严禁)有空格,每个逗号的后面MUST(必须)有一个空格.

有默认值的方法参数MUST(必须)在参数列表的末尾.

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function foo($arg1, &$arg2, $arg3 = [])
    {
        // method body
    }
}
```

参数列表MAY(可以)拆分成若干行,后续每一行都需要缩进一次.如果这样做时,列表中的第一个项目MUST(必须)换行,每一行MUST(必须)只有一个参数.

当参数列表被拆分为多行时,右边的圆括号和左边的大括号MUST(必须)写在一行中,用一个空格分开.

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function aVeryLongMethodName(
        ClassTypeHint $arg1,
        &$arg2,
        array $arg3 = []
    ) {
        // method body
    }
}
```

### 4.5. `abstract`, `final`, 和 `static`

当前, `abstract` 和 `final`声明都MUST(必须)在可见性前声明.

当前, `static`声明MUST(必须)在可见性后声明.

```php
<?php
namespace Vendor\Package;

abstract class ClassName
{
    protected static $foo;

    abstract protected function zim();

    final public static function bar()
    {
        // method body
    }
}
```

### 4.6. 请求方法或函数

当调用函数或方法时,函数或方法名与左边圆括号中间MUST NOT(严禁)有空格,左边圆括号之后MUST NOT(严禁)有空格,右边圆括号之前MUST NOT(严禁)有空格.在参数列表中,每个逗号的前面MUST NOT(严禁)有空格,每个逗号的后面MUST(必须)有一个空格.

```php
<?php
bar();
$foo->bar($arg1);
Foo::bar($arg2, $arg3);
```
参数列表MAY(可以)拆分成若干行,后续每一行都需要缩进一次.如果这样做时,列表中的第一个项目MUST(必须)换行,每一行MUST(必须)只有一个参数.

```php
<?php
$foo->bar(
    $longArgument,
    $longerArgument,
    $muchLongerArgument
);
```

5. 控制结构
---------------------

控制结构的通用格式规则如下:

- 控制结构关键字后面MUST(必须)有一个空格
- 开始的左圆括号后MUST NOT(严禁)有空格
- 结束的右圆括号前MUST NOT(严禁)有空格
- 结束的右圆括号和开始的左大括号中间MUST(必须)有一个空格
- 控制结构体内容MUST(必须)缩进一次
- 结束的右大括号MUST(必须)在代码后换行结束

每个结构都MUST(必须)用大括号括起来.通过规范代码结构的样式,减少了将新代码行添加到结构体从而产生错误的可能性.


### 5.1. `if`, `elseif`, `else`

`if`结构就像下面这样.注意圆括号,空格和大括号的位置;`else`和`elseif`都在上一个结构体结束的右大括号的同一行.

```php
<?php
if ($expr1) {
    // if body
} elseif ($expr2) {
    // elseif body
} else {
    // else body;
}
```

为了使所有的控制关键字都是一个单词,关键字`elseif` SHOULD(应该)替换`else if`.


### 5.2. `switch`, `case`

`switch`结构就像下面这样.注意圆括号,空格和大括号的位置.`case`语句MUST(必须)比`switch`再缩进一次,`break`关键字(或者其他表示终止的关键字)MUST(必须)和`case`代码结构同样的缩进.故意穿透`case`代码结构的代码MUST(必须)有一个注释,例如`// no break`.

```php
<?php
switch ($expr) {
    case 0:
        echo 'First case, with a break';
        break;
    case 1:
        echo 'Second case, which falls through';
        // no break
    case 2:
    case 3:
    case 4:
        echo 'Third case, return instead of break';
        return;
    default:
        echo 'Default case';
        break;
}
```


### 5.3. `while`, `do while`

`while`结构就像下面这样.注意圆括号,空格和大括号的位置.

```php
<?php
while ($expr) {
    // structure body
}
```

类似的,`do while`结构就像下面这样.注意圆括号,空格和大括号的位置.

```php
<?php
do {
    // structure body;
} while ($expr);
```

### 5.4. `for`

`for`结构就像下面这样.注意圆括号,空格和大括号的位置.

```php
<?php
for ($i = 0; $i < 10; $i++) {
    // for body
}
```

### 5.5. `foreach`
    
`foreach`结构就像下面这样.注意圆括号,空格和大括号的位置.

```php
<?php
foreach ($iterable as $key => $value) {
    // foreach body
}
```

### 5.6. `try`, `catch`

`try catch`代码块就像下面这样.注意圆括号,空格和大括号的位置.

```php
<?php
try {
    // try body
} catch (FirstExceptionType $e) {
    // catch body
} catch (OtherExceptionType $e) {
    // catch body
}
```

6. 闭包
-----------

闭包的声明语句中,`function`关键字后MUST(必须)有一个空格,`use`关键字前后都MUST(必须)有一个空格.

开始的左大括号MUST(必须)从第二行开始,结束的右大括号MUST(必须)在代码后换行结束.

参数列表或变量列表开始的圆括号后MUST NOT(严禁)有空格,结束的圆括号后MUST NOT(严禁)有空格.

在参数列表或变量列表中,每个逗号的前面MUST NOT(严禁)有空格,每个逗号的后面MUST(必须)有一个空格.

闭包中有默认值的方法参数MUST(必须)在参数列表的末尾.

闭包的声明就像下面这样.注意圆括号,逗号,空格和大括号的位置:

```php
<?php
$closureWithArgs = function ($arg1, $arg2) {
    // body
};

$closureWithArgsAndVars = function ($arg1, $arg2) use ($var1, $var2) {
    // body
};
```

参数列表和变量列表MAY(可以)拆分成若干行,后续每一行都需要缩进一次.如果这样做时,列表中的第一个项目MUST(必须)换行,每一行MUST(必须)只有一个参数.

当列表(参数列表或者变量列表)拆分成若干行后,关闭的圆括号和开始的左大括号MUST(必须)放在一行,中间用空格隔开.

下面的例子展示了没有参数列表的闭包和变量列表被拆分为多行的闭包.

```php
<?php
$longArgs_noVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) {
   // body
};

$noArgs_longVars = function () use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgs_longVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgs_shortVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use ($var1) {
   // body
};

$shortArgs_longVars = function ($arg) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};
```

注意:格式化的规则在闭包做为函数或者方法的参数使用时同样适用.

```php
<?php
$foo->bar(
    $arg1,
    function ($arg2) use ($var1) {
        // body
    },
    $arg3
);
```


7. 结论
--------------

本指引中有很多风格和实践中的元素都被可以的省略了.这些包括但不限于:

- 全局变量和全局常量的声明

- 函数的声明

- 运算符和赋值

- 跨行对齐

- 注释和文档块

- 类名的前后缀

- 最佳实践

未来的建议MAY(可能)修改和扩展本指引,解决这些问题或者其他风格和实践元素.