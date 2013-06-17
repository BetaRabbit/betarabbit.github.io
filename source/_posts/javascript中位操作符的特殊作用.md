title: "JavaScript中位操作符的特殊作用"
date: 2013-04-29 23:47
tags: JavaScript
---

Javascript主要有以下几种位操作符：

- AND ( `&` )
- OR ( `|` )
- XOR ( `^` )
- NOT ( `~` )
- LEFT SHIFT ( `<<` )
- RIGHT SHIFT ( `>>` )
- ZERO-FILL RIGHT SHIFT ( `>>>` )

一般来说，我们在Javascript中很少能用到这些位操作符，但在某些特殊情况下，这些简单的操作符却能抵得上好几行代码（如果不在乎可读性的话）。

## -(n+1)
对一个数进行`~`运算，等同于`-(n+1)`
{% code lang:js %}
~1 === -2 // => true
{% endcode %}

**Note**: 这只能应用于整数部分，`~1.1 === -2`

这在实际使用中常常配合`indexOf`一起使用，`if (~array.indexOf(string))`等同于`if (array中没有string)`

## 取整(忽略小数部分)
{% code lang:js %}
~~1.1 === 1 // => true
1.1 ^ 0 === 1 // => true
{% endcode %}

这两个在某些JS库或游戏编程中经常使用。

## 总结
总的来说，位操作符毕竟可读性不太好，列出来只是为了以后遇到这样的代码能看的懂，实际项目中还是不要玩这些花的东西比较好。

