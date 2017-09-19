---
title: jshint
date: 2017-08-31 18:43:24
categories:
- javascript
tags:
- js
- jshint
---
jshint  静态代码检测工具

>参考地址：[http://jshint.com/docs/](http://jshint.com/docs/)

安装

```
npm install jshint -g
```
项目中有可能用到的文件：

* .jshintrc jshint配置文件／或者用--config命令指定一个.json文件
* .jshintignore jshint不忽略检测的文件 ／ 或者用 --exclude-path命令指定一个.gitignore
* reporter.js 自定义的jshint的输出方法，具体可参考本文的 [附件:reporter](#reporter)

使用
##ide拓展
主流的ide都有自己的拓展插件，这里用vscode简单的说明。
在vscode左侧的应用商店搜索jshint排第一个就是。配合项目中的.jshintrc文件可以在ide中报错。

####jshint CLI
jshint自带命令行

| 例子 |  解释 |
| --- | --- |
| jshint myfile.js | 执行jshint |
| jshint --reporter=myreporter.js myfile.js | 定义reporter |
| jshint --verbose myfile.js  | 输出中加入错误码 |
| jshint --show-non-errors myfile.js | 输出中加入错误信息 |
| jshint --exclude path | 加入不想被linted的目录 |
| jshint --exclude-path | 指定.jshintignore |
| --prereq | 指定全局变量 |
| jshint --help | 帮助 |
| jshint --version | 版本 |
| ... ||

#####屏蔽某些错误提示

jshint提供了option选项去根据不同的项目需求和程序员习惯开启或屏蔽一些错误或警告提示。但是还是有些提示并不能通过配置去掉。（这有点烦人。。）这时可以用：
    
```
/* jshint -W034 */
```
去除对应的警告。这个W034就是错误码。具体怎么得到：

```
jshint --verbose myfile.js
myfile.js: line 6, col 3, Unnecessary directive "use strict". (W034)
```
或者有些ide的提示也会直接提示你错误码。（note： 一般警告的错误码会以W开头，错误的错误码会以E开头）
如果你想重新启动被屏蔽的错误那就用

```
/* jshint +W034 */
```



##配置方法
按照顺序依次查找以下的config配置：

* 通过jshint --config  ..path/myconfig.json指定config
* 在package.json里增加一个jshintConfig属性，在属性里配置参数。
* 在项目根目录添加一个.jshintrc的json配置文件，jshint运行是会从代码文件目录往上寻找直到找到.jshintrc文件。都找不到会在全局的默认配置里找.jshintrc文件。

##Inline configuration
除了以上三种方法jshint还支持行内配置。
类似这样，在文件或函数开头加入：

```
/* jshint undef: true, unused: true */
/* globals MY_GLOBAL */
```
行内配置有以下几种方式：

**jshint开头，多个选项以逗号分开**

```
/* jshint strict: true */
```
**globals开头，标识全局变量避免启用undef选项时应用未定义变量报错**

```
/* globals MY_LIB: false */
```
**exported，标识全局变量避免启用unused选项时变量未使用报错**


```
/* exported EXPORTED_LIB */
```
**ignore，ignore中的代码不做jshint检测**

```
// Code here will be linted with JSHint.
/* jshint ignore:start */
// Code here will be ignored by JSHint.
/* jshint ignore:end */
```
**falls through, 避免Switch statements 没有break报错**

```
switch (cond) {
    case "one":
        doSomething(); // JSHint will warn about missing 'break' here.
        /* falls through */
    case "two":
        doSomethingElse();
}
```

##jshintrc参数
>参考[http://jshint.com/docs/options/](http://jshint.com/docs/options/)

具体的参数可以参考上面的官方文档。这里不做多介绍。

一个例子：

```
{
    //加强选项：

    //use es6. 3/5/6
    "esversion": 6,

    //循环必须用大括号包起来
    "curly": true,

    //设置为true,禁止使用这个选项 ==和 !=，强制使用 ===和 !==。
    "eqeqeq": false,

    //允许警告js未来版本中定义的标识符。
    "futurehostile": true,

    //检查无效 typeof操作符的值
    "notypeof": true,

    //检查变量重复定义
    // 他接受4个值："inner" 只检查是否在相同的作用域重复定义;"outer" 检查外部作用域;
    // false 与inne一样; true 允许变量覆盖
    "shadow": "inner",

    //ECMAScript 5严格模式
    // "global" - 全局层面的严格模式"use strict";
    // "implied" - 文件里面使用"use strict";
    // false - 禁止使用严格模式
    // true - 函数上面必须使用一个"use strict"; 
    "strict": "implied",

    //变量未定义
    "undef": true,

    //变量定义未使用
    "unused": true,

    // 设置为true时，禁止使用var声明变量
    // "varstmt": true,

    // "globals": {
    //     "require": true
    // },

    //宽松选项：

    // 禁止缺少分号警告
    "asi": true,

    //环境选项：
    //暴露浏览器属性的全局变量，列如 window,document;
    //注意:这个选项不暴露变量 alert或 console。
    "browser": true,

    //这个选项定义了全局变量,通常用于日志调试: console, alert等等
    "devel": true,

    //这个选项定义全局变量可以当你的代码运行在node的运行时环境
    "node": true,

    //这个选项告诉JSHint,输入代码描述了一个ECMAScript 6模块。所有模块的代码解释为严格模式代码。
    "module": true,

    //这个选项定义全局暴露的jQuery库。
    "jquery": true

}
```
这里要说的是option里除了加强选项，宽松选项，环境选项还有一些特殊选项：
**entend**

```
{
  "extends": "../.jshintrc",
  "globals": {
    "test": false,
    "assert": false
  }
}
//在一些情况下你可能想要在项目配置之外加载一些已有的配置
```
**overrides**

```
{
  "shadow": false,
  "overrides": {
    "lib/*-test.js": {
      "expr": true
    }
  }
}
//在不同的文件中应用不同的配置
```

##附件
<h3 id="reporter">reporters</h3>
JSHint Reporter.js是一个你自定义的代替jshint默认输出方法的文件。
用jshint命令可以执行你的reporter

```
jshint --reporter=myreporter.js myfile.js
```
下面是一个例子：

```
"use strict";

module.exports = {
  reporter: function (res) {
    var len = res.length;
    var str = "";

    res.forEach(function (r) {
      var file = r.file;
      var err = r.error;

      str += file + ": line " + err.line + ", col " +
        err.character + ", " + err.reason + "\n";
    });

    if (str) {
      process.stdout.write(str + "\n" + len + " error" +
        ((len === 1) ? "" : "s") + "\n");
    }
  }
};
```

上面的例子中res中存的error的格式为：

```
[
  {
    file: 'demo.js',
    error:  {
      id: '(error)',
      code: 'W117',
      reason: '\'module\' is not defined.'
      evidence: 'module.exports = {',
      line: 3,
      character: 1,
      scope: '(main)',

      // [...]
    }
  },

  // [...]
]
```

如果你想用exporter禁止报某些错误的话可以写个判断，error.code === 'Wxxx' 时 return。










