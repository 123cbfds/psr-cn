日志接口
================

本文档介绍了一个日志类库的公共接口.

主要的目标是允许类库接受一个`Psr\Log\LoggerInterface`对象,然后用一个简单和通用的方法来写日志.框架和模型如果有自定义的需求MAY(可以)扩展接口来实现目的,但是SHOULD(应该)与本文档保持兼容.这样确保了应用使用的第三方类库能够把日志集中写在应用日志中.

本文中的关键字"MUST(必须)", "MUST NOT(严禁)", "REQUIRED(需要)", "SHALL(应当)", "SHALL NOT(不得)", "SHOULD(应该)",
"SHOULD NOT(不应该)", "RECOMMENDED(建议)", "MAY(可以)", and "OPTIONAL(可选)"遵循[RFC 2119]中的描述.

文档中`implementor`(实现者)这个词表示实现了`LoggerInterface`接口的框架或与日志相关的类库.日志的用户被成为`user`(用户).

[RFC 2119]: http://tools.ietf.org/html/rfc2119

1. 规范
-----------------

### 1.1 基础

- `LoggerInterface`暴露出8个方法,用于记录8个[RFC 5424]级别(debug, info, notice, warning, error, critical, alert,  emergency)的日志.

- 第九个方法`log`,第一个参数接受一个日志级别.请求这个方法时,使用任何一个日志级别的常量做为参数MUST(必须)与请求对应确切级别的方法的结果一致.请求这个方法时,如果日志级别在此规范中没有定义,具体实现中无法知道这个级别,就MUST(必须)抛出一个`Psr\Log\InvalidArgumentException`异常.用户在不知道当前实现是否支持的情况下,SHOULD NOT(不应该)使用一个自定义的级别.

[RFC 5424]: http://tools.ietf.org/html/rfc5424

### 1.2 消息

- 每个方法都接受一个字符串,或者是一个带有`__toString()`方法的对象做为消息.实现者MAY(可以)对传入的对象进行特殊的处理.如果不是这样,实现者MUST(必须)将其转换为字符串.

- 消息中MAY(可以)包含一个占位符,实现者MAY(可以)根据上下文中数组的值来替换它.

  占位符的名字MUST(必须)符合上下文中数组的键名.
  
  占位符的名字MUST(必须)通过单个开始的左花括号和单个结束的右花括号包含起来.括号与占位符名字中间MUST NOT(严禁)有空格.

  占位符的名字SHOULD(应该)只包含`A-Z`,`a-z`,`0-9`,下划线`_`,和句号`.`.其他的字符保留给未来占位符规范修改时使用.

  实现者MAY(可以)通过使用占位符来实现各式的加解密策略和将日志转换用于显示.除非用户无法根据显示的上下文理解数据,那用户SHOULD NOT(不应该)提前加解密占位符的值.

  下面这个例子说明了通过替换占位符来实现的一个例子,仅供参考:

  ```php
  /**
   * Interpolates context values into the message placeholders.
   */
  function interpolate($message, array $context = array())
  {
      // build a replacement array with braces around the context keys
      $replace = array();
      foreach ($context as $key => $val) {
          $replace['{' . $key . '}'] = $val;
      }

      // interpolate replacement values into the message and return
      return strtr($message, $replace);
  }

  // a message with brace-delimited placeholder names
  $message = "User {username} created";

  // a context array of placeholder names => replacement values
  $context = array('username' => 'bolivar');

  // echoes "User bolivar created"
  echo interpolate($message, $context);
  ```

### 1.3 上下文

- 每个方法都接受一个数组做为上下文的数据.这样比字符串更好,可以包含任何外来的信息.数组可以包含任何东西.实现者MUST(必须)尽可能仁慈的处理这些上下文数据.上下文中一个给定的值MUST NOT(严禁)抛出异常或者造成任何php错误,警告或者提醒.

- 如果上下文数据中出现了`Exception`对象,那MUST(必须)在`'exception'`键中.记录异常是一个常见的模式,当日志后端支持时,就能允许实现者从异常中提取一个堆栈跟踪.实现者MUST(必须)在使用前确认`'exception'`键的值确实是一个`Exception`,因为他MAY(可以)包含任何东西.

### 1.4 辅助类与接口

- 扩展`Psr\Log\AbstractLogger`类可以让你轻松实现`LoggerInterface`和实现通用的`log`方法.其他的八种方法其上下文和对消息的转发.

- 同样的,只要通过实现通用的`log`方法就能使用`Psr\Log\LoggerTrait`.注意,如果特征无法实现接口,你仍然需要实现`LoggerInterface`.

- 接口也提供了`Psr\Log\NullLogger`.如果没有提供日志记录器给接口的使用者,它MAY(可以)用来提供一个"黑洞"的记录器做为后续的实现.然后如果上下文数据的创建十分昂贵,那有条件的记录日志是一个更好的方法.

- `Psr\Log\LoggerAwareInterface`只提供了一个`setLogger(LoggerInterface $logger)`方法,框架可以用来自动随意的切换日志记录的实例.

- `Psr\Log\LoggerAwareTrait`特征可以在任何类中轻松的实现等效的接口.能够让你访问`$this->logger`.

- `Psr\Log\LogLevel`类包含了8个日志级别的常量.

2. 包
----------

[psr/log](https://packagist.org/packages/psr/log)包提供了一些关于异常的接口和类,还包括一套验证你相关实现的工具.

3. `Psr\Log\LoggerInterface`
----------------------------

```php
<?php

namespace Psr\Log;

/**
 * Describes a logger instance
 *
 * The message MUST be a string or object implementing __toString().
 *
 * The message MAY contain placeholders in the form: {foo} where foo
 * will be replaced by the context data in key "foo".
 *
 * The context array can contain arbitrary data, the only assumption that
 * can be made by implementors is that if an Exception instance is given
 * to produce a stack trace, it MUST be in a key named "exception".
 *
 * See https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-3-logger-interface.md
 * for the full interface specification.
 */
interface LoggerInterface
{
    /**
     * System is unusable.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function emergency($message, array $context = array());

    /**
     * Action must be taken immediately.
     *
     * Example: Entire website down, database unavailable, etc. This should
     * trigger the SMS alerts and wake you up.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function alert($message, array $context = array());

    /**
     * Critical conditions.
     *
     * Example: Application component unavailable, unexpected exception.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function critical($message, array $context = array());

    /**
     * Runtime errors that do not require immediate action but should typically
     * be logged and monitored.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function error($message, array $context = array());

    /**
     * Exceptional occurrences that are not errors.
     *
     * Example: Use of deprecated APIs, poor use of an API, undesirable things
     * that are not necessarily wrong.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function warning($message, array $context = array());

    /**
     * Normal but significant events.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function notice($message, array $context = array());

    /**
     * Interesting events.
     *
     * Example: User logs in, SQL logs.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function info($message, array $context = array());

    /**
     * Detailed debug information.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function debug($message, array $context = array());

    /**
     * Logs with an arbitrary level.
     *
     * @param mixed $level
     * @param string $message
     * @param array $context
     * @return null
     */
    public function log($level, $message, array $context = array());
}
```

4. `Psr\Log\LoggerAwareInterface`
---------------------------------

```php
<?php

namespace Psr\Log;

/**
 * Describes a logger-aware instance
 */
interface LoggerAwareInterface
{
    /**
     * Sets a logger instance on the object
     *
     * @param LoggerInterface $logger
     * @return null
     */
    public function setLogger(LoggerInterface $logger);
}
```

5. `Psr\Log\LogLevel`
---------------------

```php
<?php

namespace Psr\Log;

/**
 * Describes log levels
 */
class LogLevel
{
    const EMERGENCY = 'emergency';
    const ALERT     = 'alert';
    const CRITICAL  = 'critical';
    const ERROR     = 'error';
    const WARNING   = 'warning';
    const NOTICE    = 'notice';
    const INFO      = 'info';
    const DEBUG     = 'debug';
}
```