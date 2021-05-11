### 第一条：了解你使用的 JavaScript版本

总结：ES5 中引入了 ———— 严格模式(strice mode)
1. 什么是严格模式？使用严格模式的原因？
    + 严格模式：一种版本控制的考量
    + 在程序中启用的方式是在程序的最开始增加一个特定的字符串字面量(literal)
    “use strict”
    + 也可以在函数体的开始处加入这句指令来启用该函数的严格模式
    + 使用严格模式是为了在受限制的JavaScript版本中，禁止使用一些JavaScript语言中问题较多或易于出错的特性。
2. 在模式下严格模式的使用
    + 在严格模式下，不允许重定义 arguments 变量
    ```
    function f(x) {
        "use strict";
        var arguments = []; // error: redefinition of arguments...
    }
    ```
    + 严格模式下如何正确连接文件？
    ```
    //file1.js
    "use strict";
    function f() {
        //...
    }
    //...
    
    //file2.js
    // no strict-mode directive
    function g() {
        var arguments = [];
        // ...
    }
    // ...
    ```
    + 如果我们以 file1.js 文件开始，那么连接后的代码运行于严格模式下
    + 如果以 file2.js 开头, 那么连接后的代码就运行于非严格模式下
    (“use strict” 只在代码开头或者函数体开头在会生效)
    
    + 本书中提供的两种解决方案：
        + 方案一：不要将进行严格模式检查的文件和不进行严格模式检查的文件连接起来。
            + 这种方案最简单，但是会限制你对应用程序或文件结构的控制力
        + 方案二：通过将其自身包裹在立即调用的函数表达式（IIFE）中的方式连接多个文件。
            + 这种方案每个文件都放在一个单独的作用域中，里面的内容都是私有的不会影响全局的。
        ```
        //file1.js
        (function() {
            "use strict"
            function f() {}
        })();
        //file2.js
        (function() {
            function f() {
                var arguments = [];
            }
        })()
        ```   
    + 基于本书中的方案二, 我首先想到的是利用单例模式和闭包的方式进行解决。
    ```
    //file1
    let file1Module = (function () {
        "use strict"
        function f() {

        }
        return init {
            f
        }
    })()
    file1Module.init();
    //file2
    let file2Module = (function() {
        function f() {
            var arguments = [];
        }
        return init {
            f
        }
    })()
    file2Module.init()
    ```
**注意**
+ 决定你的应用程序支持JavaScript的那些版本。
+ 确保你使用的任何JavaScript的特性对于应用程序将要运行的所有环境都是支持的。
+ 总是在执行严格模式检查的环境中测试严格代码。
+ 当心连接那些在不同严格模式下有不同预期的脚本。