title: JavaScript中的“this”
date: 2011-09-08 14:02:23
tags: JavaScript
---

JavaScript有自己的一套this机制，在不同情况下，this的指向也不尽相同。

## 全局范围
{% code lang:js %}
console.log(this); //全局变量
{% endcode %}
全局范围使用this指向的是全局变量，浏览器环境下就是window。

注：ECMAScript5的strict模式不存在全局变量，这里的this是undefined。

## 函数调用中                  
{% code lang:js %}
function foo() {
    console.log(this);
}

foo(); //全局变量
{% endcode %}
函数调用中的this也指向全局变量。

注：ECMAScript5的strict模式不存在全局变量，这里的this是undefined。

## 对象方法调用
{% code lang:js %}
var test = {
    foo: function () {
        console.log(this);
    }
}

test.foo(); //test对象
{% endcode %}
对象方法调用中，this指向调用者。
{% code lang:js %}
var test = {
    foo: function () {
        console.log(this);
    }
}

var test2 = test.foo;
test2();  //全局变量
{% endcode %}
不过由于this的晚绑定特性，在上例的情况中this将指向全局变量，相当于直接调用函数。

这点非常重要，同样的代码段，只有在运行时才能确定this指向

## 构造函数
{% code lang:js %}
function Foo() {
    console.log(this);
}

new Foo(); //新创建的对象
console.log(foo); 
{% endcode %}
在构造函数内部，this指向新创建的对象。

## 显式设置this
{% code lang:js %}
function foo(a, b) {
    console.log(this);
}

var bar = {};

foo.apply(bar, [1, 2]); //bar
foo.call(1, 2); //Number对象
{% endcode %}
使用Function.prototype的call或者apply方法是，函数内部this会被设置为传入的第一个参数。