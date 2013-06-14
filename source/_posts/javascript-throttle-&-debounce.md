title: JavaScript Throttle &amp; Debounce
date: 2013-03-19 00:23:47
tags: JavaScript
---

## Throttle
无视一定时间内所有的调用，适合在发生频度比较高的，处理比较重的时候使用。

{% code lang:js %}
var throttle = function (func, threshold, alt) {
    var last = Date.now();
    threshold = threshold || 100;

    return function () {
        var now = Date.now();

        if (now - last < threshold) {
            if (alt) {
                alt.apply(this, arguments);
            }
            return;
        }

        last = now;
        func.apply(this, arguments);
    };
};
{% endcode %}

## Debounce
一定间隔内没有调用时，才开始执行被调用方法

{% code lang:js %}
var debounce = function (func, threshold, execASAP) {
    var timeout = null;
    threshold = threshold || 100;

    return function () {
        var self = this;
        var args = arguments;
        var delayed = function () {
            if (!execASAP) {
                func.apply(self, args);
            }
            timeout = null;
        };

        if (timeout) {
            clearTimeout(timeout);
        } else if (execASAP) {
            func.apply(self, args);
        }

        timeout = setTimeout(delayed, threshold);
    };
};
{% endcode %}

## Test
{% code lang:js %}
var test = function (wrapper, threshold) {
    var log = function () {
        console.log(Date.now() - start);
    };
    var wrapperedFunc = wrapper(log, threshold);
    var start = Date.now();
    var arr = [];

    for (var i = 0; i < 10; i++) {
        arr.push(wrapperedFunc);
    }

    while(i > 0) {
        var random = Math.random() * 1000;
        console.log('index: ' + i);
        console.log('random: ' + random);
        setTimeout(arr[--i], random);
    }
};

test(debounce, 1000);
test(throttle, 1000);
{% endcode %}