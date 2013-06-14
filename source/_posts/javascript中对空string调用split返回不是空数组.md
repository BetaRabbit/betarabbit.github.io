title: Javascript中对空string调用split返回不是空数组
date: 2012-08-02 02:57:06
tags: JavaScript
---

今天在工作中发现一个诡异的问题，理论上应该是没有元素的数组，长度居然是1。查了半天，原来是Javascript中的split和其他语言中不同，即对空string使用split会返回含有一个空string的数组，而不是一个空数组。

{% code lang:js %}
var str = "",
    arr = str.split("_");
 
console.log(arr.length === 1); //true
console.log(arr === []); //false
console.log(arr === [""]); //true
{% endcode %}

参考MDN，也有类似的说明。
{% blockquote %}
Note: When the string is empty, split returns an array containing one empty string, rather than an empty array.
{% endblockquote %}