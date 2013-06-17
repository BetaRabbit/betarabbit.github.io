title: "[理解Underscore和Lo-Dash] Collections _.each"
date: 2013-05-31 00:43:11
tags: JavaScript
---

# _.each

遍历集合，对集合中的每个元素执行回调。

# API

## Lo-Dash
_.forEach(collection [, callback=identity, thisArg])

### Aliases
*each*

### Arguments
1. `collection` *(Array|Object|String)*: 要遍历的集合
2. `[callback=identity]` *(Function)*: 每次迭代中调用的函数
3. `[thisArg]` *(任意)*: 绑定到`callback`的`this`
4. `callback`接受三个参数: `(value, index|key, collection)`

### Returns
*(Array, Object, String)*: 返回`collection`.
 
## Underscore
_.each(list, iterator, [context])

### Aliases
*forEach*

### Arguments
1. `list` *(Array|Object|String)*: 要遍历的集合
2. `iterator` *(Function)*: 每次迭代中调用的函数
3. `[context]` *(任意)*: 绑定到`callback`的`this`
4. `iterator`接受三个参数: `(element|value, index|key, list)`

### Returns
*(Array, Object, String)*: 返回`undefined`.

## Note
- Lo-Dash可以省略回调函数，而Underscore则必须传入
- Lo-Dash可以通过在回调中返回`false`提前结束迭代
- Lo-Dash会返回Collection从而允许链式操作，Underscore的返回值则是undefined

# Example
## Lo-Dash
{% code lang:js %}
_.forEach([1,2,3])
// => 返回[1,2,3]

_([1, 2, 3]).forEach(alert).join(',');
// => alert每个数字并返回'1,2,3'

_.forEach({ 'one': 1, 'two': 2, 'three': 3 }, alert);
// => alert每个数字value(不保证按照定义的顺序执行)
{% endcode %}

## Underscore
{% code lang:js %}
_.each([1, 2, 3], alert);
// => alert每个数字
_.each({one : 1, two : 2, three : 3}, alert);
// => alert每个数字value(不保证按照定义的顺序执行)
{% endcode %}

# Source

## Lo-Dash
{% code lang:js %}
function forEach(collection, callback, thisArg) {
    var index = -1,
    length = collection ? collection.length : 0;

    callback = callback && typeof thisArg == 'undefined' ? callback : lodash.createCallback(callback, thisArg);
    if (typeof length == 'number') {
        while (++index < length) {
            if (callback(collection[index], index, collection) === false) {
                break;
            }
        }
    } else {
        forOwn(collection, callback);
    }
    return collection;
}
{% endcode %}

## Underscore
{% code lang:js %}
var each = _.each = _.forEach = function(obj, iterator, context) {
    if (obj == null) return;
    if (nativeForEach && obj.forEach === nativeForEach) {
      obj.forEach(iterator, context);
    } else if (obj.length === +obj.length) {
      for (var i = 0, l = obj.length; i < l; i++) {
        if (iterator.call(context, obj[i], i, obj) === breaker) return;
      }
    } else {
      for (var key in obj) {
        if (_.has(obj, key)) {
          if (iterator.call(context, obj[key], key, obj) === breaker) return;
        }
      }
    }
  };
{% endcode %}

# Additional
- `obj.length === +obj.length`

`+obj`: 将obj转换成10进制数，否则返回`NaN`。因此，上面的判断等价于`obj.length && typeof obj.length == 'number'`

- `if (iterator.call(context, obj[i], i, obj) === breaker) return;`

`breaker`是预先定义的空对象({})，Underscore内部用于提前结束循环的标志，并没有对外公开。另外，因为对象的`===`比较的是对象地址，所以就算用户在自己的iterator中返回`{}`，上述`if`仍然不成立

- `for in`循环不会遍历non-enumerable属性，因此像`Object`的`toString`等就不会被迭代