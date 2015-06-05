基础编码标准
=====================

为了保证共享的PHP代码能够在技术上更好的协作,标准的这个部分讨论了哪些编码元素应该被加入到标准中.

本文中的关键词"MUST(必须)", "MUST NOT(严禁)", "REQUIRED(需要)", "SHALL(应当)", "SHALL NOT(不得)", "SHOULD(应该)",
"SHOULD NOT(不应该)", "RECOMMENDED(建议)", "MAY(可能)", and "OPTIONAL(可选)"遵循[RFC 2119]中的描述.

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt
[PSR-0]: psr-0.md
[PSR-4]: psr-4.md


1. 概述
-----------

- 文件MUST(必须)使用`<?php` 和 `<?=` 标记.

- PHP代码MUST(必须)使用没有BOM签名的UTF-8编码进行保存.

- 文件SHOULD(应该)*要么*声明各种符号(类,函数,常量,等)*或者*执行一些业务操作(例如输出内容,改变设置,等)但是SHOULD NOT(不应该)两者都包含.

- 命名空间和类MUST(必须)遵循"自动加载"标准: [[PSR-0], [PSR-4]].

- 类名MUST(必须)遵循驼峰式命名法.

- 类常量MUST(必须)使用下划线分割的大写字母.

- 方法名MUST(必须)遵循驼峰式命名法.


2. 文件
--------

### 2.1. PHP 标记

PHP代码MUST(必须)使用长标记 `<?php ?>`或者短的输出标记`<?= ?>`;MUST NOT(严谨)使用其他类型的标记.

### 2.2. 字符编码

PHP代码MUST(必须)使用没有BOM签名的UTF-8编码进行保存.

### 2.3. 副作用

单一文件SHOULD(应该)只声明一个新的符号(类,函数,常量,等)而不要有其他功能,或者SHOULD(应该)执行业务逻辑,但是SHOULD NOT(不应该)两者都做.

"副作用"这个词指和声明类,函数,常量等逻辑不相关的操作.*这些只能通过包含文件完成*.

"副作用"包括但不限于:输出内容,显式的使用`require`或者`include`,请求外部服务,修改设置,抛出异常或者错误,修改全局或静态变量,读写文件,等等.

下面这段代码既包含声明又包含副作用,应到避免写出这样的代码:

```php
<?php
// side effect: change ini settings
ini_set('error_reporting', E_ALL);

// side effect: loads a file
include "file.php";

// side effect: generates output
echo "<html>\n";

// declaration
function foo()
{
    // function body
}
```

下面这段代码只包含声明并不包含副作用,是一个可以效仿的例子:

```php
<?php
// declaration
function foo()
{
    // function body
}

// conditional declaration is *not* a side effect
if (! function_exists('bar')) {
    function bar()
    {
        // function body
    }
}
```


3. 命名空间和类名
----------------------------

命名空间和类名MUST(必须)遵循"自动加载"标准: [[PSR-0], [PSR-4]].

这就意味着每个文件只能编写一个类,而且至少有一个级别的命名空间:一个顶级的空间名.

类名MUST(必须)遵循驼峰式命名法.

PHP5.3及以后版本的代码MUST(必须)使用正式的命名空间.

例如:

```php
<?php
// PHP 5.3 and later:
namespace Vendor\Model;

class Foo
{
}
```

PHP5.2.x及以前版本的代码SHOULD(应该)使用伪命名空间约定,在类名前加`Vendor_`前缀.

```php
<?php
// PHP 5.2.x and earlier:
class Vendor_Model_Foo
{
}
```

4. 类常量,属性和方法
-------------------------------------------

这里的类指所有的类,接口及特征.

### 4.1. 常量

类常量MUST(必须)使用下划线分割的大写字母.
例如:

```php
<?php
namespace Vendor\Model;

class Foo
{
    const VERSION = '1.0';
    const DATE_APPROVED = '2012-06-01';
}
```

### 4.2. 属性

本指引意在避免任何关于在属性名中使用`$StudlyCaps`, `$camelCase`,或`$under_score`的建议.

无论怎样,在一个合理的范围内命名约定SHOULD(应该)坚持使用.这个范围可能是顶级空间名,包级别,类级别或者方法级别.

### 4.3. 方法

方法名MUST(必须)遵循驼峰式命名法.