title: "innerText和textContent"
date: 2013-02-26 20:11:32
tags: JavaScript
---

今天在使用`innerText`时遇到一个兼容性问题，FireFox不支持`innerText`方法，查了MDN，发现FireFox下有个类似的方法，叫`textContent`，它和IE的`innerText`类似， 都是用来获取（设置）元素中text的方法。


## 语法
- 设置
{% code lang:js %}
element.textContent = “text”;
{% endcode %}
- 获取
{% code lang:js %}
var text = element.textContent;
{% endcode %}

**Note**: textContent和innerText类似，也会同时获取子元素的text content，比如
{% code lang:js %}
<div>this is <span>a</span> text!</div>
// div.textContent == "this is a text!"
{% endcode %}

## 与`innerText`的区别
- `textContent`会获取所有元素的content，包括`<script>`和`<style>`元素
- `innerText`不会获取hidden元素的content，而`textContent`会
- `innerText`会触发reflow，而`textContent`不会
- `innerText`返回值会被格式化，而`textContent`不会

## 主流浏览器支持情况
- IE 9+
- Chrome 1+
- FireFox(Gecko)