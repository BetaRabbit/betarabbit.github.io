title: "String Primitive和String Object"
date: 2012-11-04 00:26:45
tags: JavaScript
---

网上看到下面这段代码，很有意思。
{% code lang:js %}
String.prototype.cut = function (len) {
  return this.length > len ? this.substring(0, len) + '...' : this;
};

var obj = ["Superman", "Batman", "Iron Man"];
console.log(typeof obj[1].cut(6));
console.log(obj[1].cut(6));
{% endcode %}
 
这段代码很简单，判断字符串的长度，如果大于给定长度（L）输出，输出字符串前L位加上“…”，否则输出字符串本身。

那么，上面的代码是不是和我们期待的一样输出下面的内容呢?
{% code lang:js %}
"String"
"Batman"
{% endcode %}

答案是否定的，实际的输出其实是:
{% code lang:js %}
"object"
<String Object>
{% endcode %}

这其实是因为String对象(String Object)和String基本类型(String Primitive)的不同导致的。字符串在JavaScript中有两种存在形式:
{% code lang:js %}
new String('object');// String Object
'primitive';// String Primitive
{% endcode %}

String的所有实用方法其实都是在String对象的prototype上，String基本类型是没有这些方法的。

因此当执行`'primitive'.slice(0)`时，JavaScript会自动将基本类型包装成对应的对象，调用对象上的方法，完成之后自动将对象销毁。因此，最上面的cut函数中this其实已经不是String Primitive而是String Object，当我们直接返回`this`时，返回值其实是”Batman”的String Object，所以`typeof`返回`object`。

所以，这个cut函数正确写法应该用String转换函数将Object转为Primitive。
{% code lang:js %}
String.prototype.cut = function (len) {
  return this.length > len ? this.substring(0, len) + '...' : String(this);
};
{% endcode %}