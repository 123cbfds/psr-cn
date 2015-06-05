# 自动加载器

本文中的关键词"MUST(必须)", "MUST NOT(严禁)", "REQUIRED(需要)", "SHALL(应当)", "SHALL NOT(不得)", "SHOULD(应该)","SHOULD NOT(不应该)", "RECOMMENDED(建议)", "MAY(可能)", and "OPTIONAL(可选)"遵循[RFC 2119](http://tools.ietf.org/html/rfc2119)中的描述.


## 1. 概述

这篇PSR描述了关于从路径中[autoloading][](自动加载)类的规范.这是完全可相互操作的,而且能够用于附加在其他任何一个包括[PSR-0][]自动加载的规范.这篇PSR同样也根据其规范,描述了被希望自动加载的文件应该被放在哪里.


## 2. 规范

1. "类"泛指所有的类,接口,特征和其他类似的结构.

2. 一个完全合格的类名具有如下形式:

        \<NamespaceName>(\<SubNamespaceNames>)*\<ClassName>

    1. 一个完全合的类名MUST(必须)有一个顶级的命名空间名,也叫做"供应商命名空间".

    2. 一个合格的类名MAY(可能)有一个或者更多个子命名空间名.

    3. 合格的类名MUST(必须)有一个最终的类名.

    4. 合格的类名中任何位置的下划线都是没有意义的.

    5. 合格的类名中字母的大小写MAY(可以)随意.

    6. 所有的类名MUST(必须)大小写敏感.

3. 根据合格的类名从一个文件中加载内容时...

    1. 合格的类名里(使用命名空间前缀),除去顶级命名空间的分隔符,一个或多个相邻系列的顶级命名空间或子命名空间的名字对应至少一个"基础目录".

    2. "命名空间前缀"后相邻的子命名空间名对应与一个包含"基础目录"的子目录,命名空间的分隔符代表了目录的分隔符.子目录的名称MUST(必须)和子命名空间名称的大小写一致.
 
    3. 最终类名对应一个以`.php`结尾的文件,文件名MUST(必须)和最终类名的大小写一致.

4. 自动加载器MUST NOT(严禁抛出异常), MUST NOT(严禁)提高任何错误级别,SHOULD NOT(不得)有返回值.


## 3. 例子

下标显示了给定一个合格的类名所对应的命名空间前缀,基础目录和文件路径.

| 合格的类名                    | 命名空间前缀       | 基础目录                 | 最终的文件路径
| ----------------------------- |--------------------|--------------------------|-------------------------------------------
| \Acme\Log\Writer\File_Writer  | Acme\Log\Writer    | ./acme-log-writer/lib/   | ./acme-log-writer/lib/File_Writer.php
| \Aura\Web\Response\Status     | Aura\Web           | /path/to/aura-web/src/   | /path/to/aura-web/src/Response/Status.php
| \Symfony\Core\Request         | Symfony\Core       | ./vendor/Symfony/Core/   | ./vendor/Symfony/Core/Request.php
| \Zend\Acl                     | Zend               | /usr/includes/Zend/      | /usr/includes/Zend/Acl.php

遵守本规范的自动加载器实现的例子请查看[examples file][].该例子中MUST NOT(严禁)被视为本规范的一部分,而且MAY(可能)在任何时间改变.

[autoloading]: http://php.net/autoload
[PSR-0]: PSR-0.md
[examples file]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader-examples.md