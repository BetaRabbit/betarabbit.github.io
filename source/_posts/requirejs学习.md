title: "RequireJS学习"
date: 2012-11-19 16:38:20
tags: JavaScript
---

RequireJS是一个JavaScript文件和模块加载器。除了可以在浏览器中使用外，还可以用Node或Rhino等Server端环境。

最新版可以在[这里](http://requirejs.org/docs/download.html)下载。

## 基本用法
假设你的工程目录结构如下：

- project
    - index.html
    - js
        - lib
            - jquery.js
        - app
            - sub_app.js
            - app.js

首先，将requirejs.js放入js/lib目录。

- project
    - index.html
    - js
        - lib
            - jquery.js
            - require.js
        - app
            - sub_app.js
            - app.js

然后，在index.html中引入`<script>`用来加载require.js。

{% code lang:html %}
<script data-main="js/app" src="js/lib/require.js"></script>
{% endcode %}

在app.js中，使用`require`方法加载其他脚本

{% code lang:js %}
requirejs.config({
    // 默认从js/lib目录加载
    baseUrl: 'js/lib',
    // 如果模块ID以app开头，则从js/app目录加载
    // paths相对于baseUrl设定
    // 不要指定".js"后缀，因为paths可以是一个目录
    paths: {
        app: '../app',
        jquery: 'jquery.min',
    }
});
 
// app入口
require(['app/sub_app'], function (sub) {
    sub.hello();
});
{% endcode %}

在sub_app.js中定义一个module
{% code lang:js %}
// define相对于baseUrl设定
define(['jquery'], function ($) {
    return {
        log: function (msg) {
            if (window.console && console.log) {
                console.log(msg);
            } else {
                alert(msg);
            }
        },
        hello: function () {
            this.log("Hello, I'm powered by jQuery " + $().jquery + "!");
        }
    };
});
{% endcode %}

现在，打开浏览器的控制台，应该能看到我们自定义的module成功使用jQuery输出了下面这句话：
{% code lang:html %}
Hello, I'm powered by jQuery 1.8.3! 
{% endcode %}