> **目录**

> [TOC]

# Javascript语言编程规范

> **说明**

> * 本规范翻译自Douglas Crockford关于Javascript编码规范的文章 [Code Conventions for the JavaScript Programming Language](http://javascript.crockford.com/code.html)
> * 更多资料请参考 [Google公司的Javascript编码规范](http://alloyteam.github.io/JX/doc/specification/google-javascript.xml)

本文阐述了Javascript编程过程中可以参考的一组约定及规范，它受到Sun公司 [Java语言编码规范](http://javascript.crockford.com/javacodeconventions.pdf) 的启发。当然，Javascript不是Java，因此我做了大量的修改；
软件产品对于一家公司的长期价值，很大程度上依赖于其代码质量。在软件产品的整个生命周期内，一段程序将会被很多人修改。因此，如果程序能够清晰明了的表达其架构设计及业务逻辑，那么，那么在不远的将来当它被修改的时候，就不那么容易腐烂掉。

编码规范有助于提高程序的健壮性。

所有我们交付的Javascript代码都是直接对外发布的，因此它应该总是达到对外发布的质量标准。

保持简洁。

## Javascript源文件

应该将Javascript程序作为`.js`文件进行存储和交付。

除非和单个会话关联，否则Javascript不应该被嵌入HTML源文件。嵌入HTML中的Javascript代码会徒增页面份量，却无法灵活的对其进行缓存和压缩。

在HTML源文件中，`<script src=filename.js>`标签应该被定义在页面主体（`注: body标签`）的最后位置，这样可以缓解由脚本加载带来的页面渲染延迟。该标签没有必要提供`language`和`type`属性，因为[MIME类型](http://www.w3school.com.cn/media/media_mimeref.asp)是由服务器决定的，而不是标签本身。

## 缩进

缩进的单位是4个空格。应该避免使用缩进，因为对于缩进的停靠位置，至今（本文写作于21世纪）也没有统一标准。使用空格取代缩进，虽然会使文件更大，但是在本地网络传输时，其影响并不大，而且，还可以通过[源码压缩技术](http://yuiblog.com/blog/2006/03/06/minification-v-obfuscation/)降低文件大小，消除空格符的影响。

## 代码行长度

避免代码行的长度大于80个字符。当一条语句无法单行显示时，有必要将其换行。***将换行后的代码放置于一个操作符之后，理想情况下放置于逗号之后。采用操作符将代码换行，可以降低复制粘贴过程中分号间接隐藏的错误***。下一行应该缩进8个空格；

## 注释

在注释这件事情上要大度一点。通过注释提供的信息，对于后续走读你代码的人（有可能是你自己）来说是很有用的，因为他可以更好的理解你做了些什么。注释应该清晰明了，排版规整，就像它所注解的代码一般。注释中偶尔的幽默是很受欢迎的，但抱怨并不是如此。

注释要保持同步更新，这很重要，因为错误的注释往往使程序更难理解。

编写有意义的注释。关注代码背后那些并不显而易见的信息，而不是将代码阅读者的时间浪费在诸如`i = 0; // Set i to zero.`这样无意义的信息上。

通常情况下请使用行注释。块注释一般用在正式文档中。

## 变量声明

所有变量都应该先声明后使用。虽然Javascript并不做强制要求，但这样做会让程序更易理解。同时，由于未经声明而被隐式转换为全局的变量则更容易被检查出来。应该杜绝使用隐式的全局变量。要尽可能少的使用全局变量。

将`var`声明语句作为函数体的首行。

推荐将每个变量的定义及其注释置于同一行，如果可能的话，变量名应该按照首字母顺序先后排列。例如：
```javascript
    var currentEntry, // currently selected table entry
        level,        // indentation level
        size;         // size of table
```

Javascript并没有代码块级的作用域，因此对于熟悉C语言家族的开发者来讲，在代码块内定义的变量会让他们感到困惑。要将所有变量定义在函数体的靠前位置。

## 函数声明

所有的函数都应该先声明后调用，内部函数应采用`var`语句来定义。这样可以区分清楚哪些变量在该函数作用域内。

函数名和参数列表的起始符 （ `（左括号）`之间不应该保留空格。右括号和函数体的起始符 { `（左大括号）`之间应该保留一个空格。函数体整体要有4个空格的缩进。将函数体结束符 } `（右大括号）` 和函数声明的初始行对齐。例如：
```javascript
    function outer(c, d) {
        var e = c * d;

        function inner(a, b) {
            return (e * a) + b;
        }

        return inner(0, 1);
    }
```
这条规则很适合Javascript语言，因为在Javascript中，任何表达式可以被调用的地方，都可以调用函数和对象字面量。这就使得内联函数和复杂数据结构具有最好的可读性。
```javascript
    function getElementsByClassName(className) {
        var results = [];
        walkTheDOM(document.body, function (node) {
            var array,                // array of class names
                ncn = node.className; // the node's classname

    // If the node has a class name, then split it into a list of simple names.
    // If any of them match the requested name, then append the node to the list of results.

            if (ncn && ncn.split(' ').indexOf(className) >= 0) {
			    results.push(node);
            }
        });
        return results;
}
```

对于匿名函数，关键字`function`和`（`（左括号）之间应该保留一个空格。如果没有空格，那么`function`就是该函数的名称，这会导致误读。
```javascript
    div.onclick = function (e) {
        return false;
    };

    that = {
        method: function () {
            return this.datum;
        },
        datum: 0
    };
```

应该尽量少使用全局函数。

如果一个函数会被立即调用，那么应该用圆括号包含整个函数调用表达式，以便于清晰的表明该函数的执行结果即为表达式的返回值，而不是函数本身。
```javascript
    var collection = (function () {
	    var keys = [], values = [];
	
	    return {
	        get: function (key) {
	            var at = keys.indexOf(key);
	            if (at >= 0) {
	                return values[at];
	            }
	        },
	        set: function (key, value) {
	            var at = keys.indexOf(key);
	            if (at < 0) {
	                at = keys.length;
	            }
	            keys[at] = key;
	            values[at] = value;
	        },
	        remove: function (key) {
	            var at = keys.indexOf(key);
	            if (at >= 0) {
	                keys.splice(at, 1);
	                values.splice(at, 1);
	            }
	        }
	    };
	}());
```

## 命名

变量名应该由26个大小写字母（A..Z，a..z）、10个数字（0..9）以及 `_`（下划线）组成。避免使用国际化字符，因为在很多地方，它们并不容易被阅读和理解。命名中不要包含`$`（美元符号）或 `\` （反斜杠）。

命名中不要将`_`（下划线）作为首尾字母，有时它被用于标识私有变量，但实际上这并不能保证变量的私有性。如果对你来讲成员变量的私有性很重要，那么请使用下划线来标识。应避免使用这种不具备约束力的命名方式。

大部分变量及函数的名称都应该首字母小写。

对于采用`new`关键字调用的构造函数，其函数名应该首字母大写。如果`new`关键字缺失，Javascript并不会报编译时及运行时告警。但是这样会导致意想不到的错误，因此构造函数名首字母大写是我们唯一可用的预防措施。

全局变量名称应该全大写。（Javascript不支持宏和静态变量，因此关于全大写的变量命名方式，Javascript并没有太多特性与之相关）

## 语句

### 简单语句

每行代码应该至少包含一条语句。每条简单语句以 `;`（分号）结尾。请注意，针对函数或对象字面量的赋值依然是一条赋值语句，因此也必须以分号结尾。

Javascript允许将任何表达式作为语句使用，这会隐藏一些错误，特别是当语句中包含分号时。唯一可当作语句使用的表达式就是赋值语句和调用语句。

### 复合语句

复合语句指的是包含在`{}`（大括号）内的一系列语句列表。

* 大括号内包含的语句应该整体缩进4个空格。
* `{`（左括号）应该作为复合语句首行的行尾。
* `}`（右括号）应该另起一行，并和包含`{`（左括号）的复合语句首行保持左对齐。
* 应该以括号包含所有的语句，即使是`if`或`for`这种控制流中的单条语句。这样可以在不引入问题的情况下很方便的添加语句。

### 语句标签

语句标签是可选的。只有诸如`while`、`do`、`for`、`switch`的语句有必要使用标签。

### `return`语句

在用于返回值的`return`语句中，不应该用`()`（括号）包含该值。被返回的语句必须和`return`关键字在同一行，以避免Javascript引擎自动插入分号而引入问题。

### `if`语句

`if`语句应该遵循如下格式：
```javascript
    if (condition) {
        statements
    }
    
    if (condition) {
        statements
    } else {
        statements
    }
    
    if (condition) {
        statements
    } else if (condition) {
        statements
    } else {
        statements
    }
```

### `for`语句 

`for`语句应该遵循如下格式： 
```javascript
    for (initialization; condition; update) {
        statements
    }

    for (variable in object) {
        if (filter) {
            statements
        } 
    }
``` 

第一种格式适用于数组和已知长度的迭代器的遍历。 
The first form should be used with arrays and with loops of a predeterminable number of iterations. 

第二种格式适用于对象成员的遍历。请注意，这种格式下，添加至对象原型的成员变量也会被循环遍历出来。明智的做法是，调用对象的`hasOwnProperty`方法将对象真正的成员区分出来。 
```javascript
   for (variable in object) {
        if (object.hasOwnProperty(variable)) {
            statements
        } 
    }
```

### `while`语句 

`while`语句应该遵循如下格式： 
```javascript
    while (condition) {
        statements
    }
``` 

### `do`语句 

`do`语句应该遵循如下格式： 
```javascript
    do {
        statements
    } while (condition);
``` 
跟其他的复合语句有所不同，`do`语句应该总是以`;`（分号）结尾。 

### `try`语句 

`try`语句应该遵循如下格式： 
```javascript
    try {
        statements
    } catch (variable) {
        statements
    }

    try {
        statements
    } catch (variable) {
        statements
   } finally {
        statements
    }
``` 

### `continue`语句 

避免使用`continue`语句，它会扰乱函数的控制流。 

### `with`语句 

不应该使用`with`语句。 

### 空格 

空白行可以通过隔离逻辑上关联的区段来提高可读性。 

应该在如下场景中使用空格： 

* 关键字和其后的`(`（左括号）应该以空格分割。如： ` while (true) {  `    
* 函数值和`(`（左括号）之间不应该有空格。这样有助于区分函数关键字和函数调用。 
* 除了`.`（句点）、`(`（左括号）、`[`（左中括号）之外，所有的二元操作符都应该和它们的操作数以空格分割。 
* 不应该以空格分割一元操作符及其操作数，除非该操作符不是标点符号，而是类似`typeof`这样的单词。 
* 在`for`语句的控制流部分，每个`;`（分号）之后都应跟随一个空格。 
* 每个`,`（逗号）之后都应跟随一个空格。 

## 更多建议 

### `{}`和`[]` 

用`{}`代替`new Object()`。用`[]`代替`new Array();`。 
当成员名为连续的整数时，请使用数组。当成员名为字符串时，请使用对象。 

### `,`（逗号）操作符 

避免使用逗号操作符。（这里不包含逗号分割符，它通常被用在对象字面量、数组字面量、`var`语句及参数列表中） 

### 块作用域 

Javascript中，代码块并没有作用域。只有函数有作用域。除了复合语句之外，其他地方不要使用代码块。 

### 赋值表达式 

不要在`if`和`while`语句的条件判断中使用赋值表达式。 
``` 
 if (a = b) {
``` 
它是正确的吗？或许正确的应该是 
``` 
 if (a == b) {
``` 
避免难以判断正确性的赋值表达式。 

### `===` 和 `!==`操作符 

使用`===` 和 `!==`操作符。由于会做强制类型转换，因此不应该使用`==` 和 `!=`。 

### 加法和减法运算符的混淆使用

请注意，不要在`+`后跟随`+`或`++`，这会难以理解。为了清晰表达你的意图，请用括号区分这两部分。 
```javascript
total = subtotal + +myInput.value;
``` 
应该写为 
```javascript
total = subtotal + (+myInput.value);
``` 
这样`+ +`才不会被误读为`++`。 

### 邪恶的`eval` 

`eval`函数是Javascript中最容易被错误使用的。请避免使用它。 

`eval`有别名。不要使用`Function`构造器。不要将字符串作为参数传递给`setTimeout`或`setInterval`函数。 
