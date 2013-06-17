title: "如何将一个String和多个String值进行比较"
date: 2013-04-20 22:34:47
tags: JavaScript
---

开发中我们经常需要将一个String和多个String值进行比较。直觉反应是使用`||`符号连接多个`===`完成，比如：
{% code lang:js %}
if (string === 'banana' || string === 'pineapple') {
   fruitColor = 'yellow';
}
{% endcode %}
这样能够很好的完成需求，但总觉得有点笨，并且对扩展不友好，当我们的水果种类变多时：
{% code lang:js %}
if (string === 'banana' || string === 'pineapple' || string === 'mongo' || string === 'lemon') {
   fruitColor = 'yellow';
}
{% endcode %}
上面的代码看起来就不那么好看了，让我们看看有什么其他方式能够处理这种需求。

##Switch
{% code lang:js %}
switch(string) {
    case 'banana':
    case 'pineapple':
    case 'mongo':
    case 'lemon':
      fruitColor = 'yellow';
}
{% endcode %}
这看起来不错，但是总是要多打些字，对于不喜欢多打字的人来说不是个好方法。

##Array
{% code lang:js %}
if (['banana', 'pineapple', 'mongo', 'lemon'].indexOf(string) >= 0) {
    fruitColor = 'yellow';
}
{% endcode %}
这下好多了，但还有个问题，IE9以下的IE浏览器并不支持`indexOf`方法，如果你要在IE<=8的环境中使用Array方式比较多个string值，要么自己写一个`indexOf`方法，要么就得引入一些库来做浏览器兼容。

##jQuery
jQuery提供了一个`inArray`方法
{% code lang:js %}
if ($.inArray(['banana', 'pineapple', 'mongo', 'lemon'], string) >= 0) {
    fruitColor = 'yellow';
}
{% endcode %}

##Underscore
Underscore提供了一个`contains`方法
{% code lang:js %}
if (_.contains(['banana', 'pineapple', 'mongo', 'lemon'], string)) {
    fruitColor = 'yellow';
}
{% endcode %}

##正则表达式
当然，我们还有终极武器——正则表达式
{% code lang:js %}
if (/^(banana|pineapple|mongo|lemon)$/.test(string)) {
    fruitColor = 'yellow';
}
{% endcode %}
