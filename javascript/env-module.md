### 模块化

#### AMD

##### [强制] 使用 `AMD` 作为模块定义。

解释：

AMD 作为由社区认可的模块定义形式，提供多种重载提供灵活的使用方式，并且绝大多数优秀的 Library 都支持 AMD，适合作为规范。

目前，比较成熟的 AMD Loader 有：

- 官方实现的 [requirejs](http://requirejs.org/)
- 百度自己实现的 [esl](https://github.com/ecomfe/esl)


##### [强制] 模块 `id` 必须符合标准。

解释：

模块 id 必须符合以下约束条件：

1. 类型为 string，并且是由 `/` 分割的一系列 terms 来组成。例如：`this/is/a/module`。
2. term 应该符合 [a-zA-Z0-9_-]+ 规则。
3. 不应该有 .js 后缀。
4. 跟文件的路径保持一致。



#### define

##### [建议] 定义模块时不要指明 `id` 和 `dependencies`。

解释：

在 AMD 的设计思想里，模块名称是和所在路径相关的，匿名的模块更利于封包和迁移。模块依赖应在模块定义内部通过 local require 引用。

所以，推荐使用 define(factory) 的形式进行模块定义。


示例：

```javascript
define(
    function (require) {
    }
);
```


##### [建议] 使用 `return` 来返回模块定义。

解释：

使用 return 可以减少 factory 接收的参数（不需要接收 exports 和 module），在没有 AMD Loader 的场景下也更容易进行简单的处理来伪造一个 Loader。

示例：

```javascript
define(
    function (require) {
        var exports = {};

        // ...

        return exports;
    }
);
```




#### require

##### [强制] 全局运行环境中，`require` 必须以 `async require` 形式调用。

解释：

模块的加载过程是异步的，同步调用并无法保证得到正确的结果。

示例：

```javascript
// good
require([ 'foo' ], function (foo) {
});

// bad
var foo = require('foo');
```

##### [强制] 模块定义中只允许使用 `local require`，不允许使用 `global require`。

解释：

1. 在模块定义中使用 global require，对封装性是一种破坏。
2. 在 AMD 里，global require 是可以被重命名的。并且 Loader 甚至没有全局的 require 变量，而是用 Loader 名称做为 global require。模块定义不应该依赖使用的 Loader。


##### [强制] Package在实现时，内部模块的 `require` 必须使用 `relative id`。

解释：

对于任何可能通过 发布-引入 的形式复用的第三方库、框架、包，开发者所定义的名称不代表使用者使用的名称。因此不要基于任何名称的假设。在实现源码中，require 自身的其它模块时使用 relative id。

示例：

```javascript
define(
    function (require) {
        var util = require('./util');
    }
);
```


##### [建议] 不会被调用的依赖模块，在 `factory` 开始处统一 `require`。

解释：

有些模块是依赖的模块，但不会在模块实现中被直接调用，最为典型的是 css / js / tpl 等 Plugin 所引入的外部内容。此类内容建议放在模块定义最开始处统一引用。

示例：

```javascript
define(
    function (require) {
        require('css!foo.css');
        require('tpl!bar.tpl.html');

        // ...
    }
);
```
