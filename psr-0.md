自动加载标准
====================

为了使自动加载器能够通用,下面描述的强制性要求,开发者必须遵守.

强制性要求
---------

* 一个完全合格的命名空间和类必须遵循如下结构 `\<Vendor Name>\(<Namespace>\)*<Class Name>`
* 每个命名空间都必须有一个顶级命名空间 ("Vendor Name").
* 每个命名空间可以有任意多个子命名空间.
* 读取文件时,每个命名空间的分隔符都会被转换成为`DIRECTORY_SEPARATOR`.
* 每个类名中的`_`字符都会被转换成为`DIRECTORY_SEPARATOR`.字符`_`在命名空间中没有特殊含义.
* 一个完全合格的命名空间和类的文件必须以`.php`结尾.
* 命名空间和类名可以是大小写英文字母的组合.

例如
--------

* `\Doctrine\Common\IsolatedClassLoader` => `/path/to/project/lib/vendor/Doctrine/Common/IsolatedClassLoader.php`
* `\Symfony\Core\Request` => `/path/to/project/lib/vendor/Symfony/Core/Request.php`
* `\Zend\Acl` => `/path/to/project/lib/vendor/Zend/Acl.php`
* `\Zend\Mail\Message` => `/path/to/project/lib/vendor/Zend/Mail/Message.php`

命名空间和类名中需要强调的地方
-----------------------------------------

* `\namespace\package\Class_Name` => `/path/to/project/lib/vendor/namespace/package/Class/Name.php`
* `\namespace\package_name\Class_Name` => `/path/to/project/lib/vendor/namespace/package_name/Class/Name.php`

我们设置的这些标准,是能够让自动加载器能够无损通用化的最低标准.如果你遵循了这些标准,你就可以使用下面这个SplClassLoader加载PHP 5.3版本的类.

实例
----------------------

下面我们用一个简单的函数来演示一下如何自动加载根据上述建议的标准编写的代码.

```php
<?php

function autoload($className)
{
    $className = ltrim($className, '\\');
    $fileName  = '';
    $namespace = '';
    if ($lastNsPos = strrpos($className, '\\')) {
        $namespace = substr($className, 0, $lastNsPos);
        $className = substr($className, $lastNsPos + 1);
        $fileName  = str_replace('\\', DIRECTORY_SEPARATOR, $namespace) . DIRECTORY_SEPARATOR;
    }
    $fileName .= str_replace('_', DIRECTORY_SEPARATOR, $className) . '.php';

    require $fileName;
}
```

SplClassLoader实现
-----------------------------

如果你按照上面推荐的通用自动加载器标准编写代码,下面这个简单的SplClassLoader实例就能加载你的类. 这是目前加载PHP 5.3 符合上述标准类的推荐方法.

* [http://gist.github.com/221634](http://gist.github.com/221634)