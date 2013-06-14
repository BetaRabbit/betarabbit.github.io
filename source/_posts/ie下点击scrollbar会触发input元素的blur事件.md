title: IE下点击scrollbar会导致焦点移动到body
date: 2013-06-10 23:09:45
tags: JavaScript
---

## 现象
IE这货果然与众不同，当光标焦点在input时，点击同页面内其他区域的scrollbar，会导致焦点移动到body，从而触发绑定在input上的blur事件，如果input中的值与之前不同，甚至还会触发change事件...
Chrome曾经也有类似的问题，但在最新版中已经修正了，而Firefox则完全没有这样的问题。

## 影响
这个问题看起来微不足道，实际上影响还是非常大的，主要表现在下面2个方面

- 多数的suggest控件会出错
<br>
suggest往往是通过input(输入部分)和div(下拉框部分)组成。有时，下拉框内容过多，用户需要移动滚动条才能看全选项，但因为点击滚动条会让input失去焦点，导致控件误认为用户结束输入，从而关闭suggest的下拉部分，导致用户实际上无法正确的进行滚动条操作。

- form
<br>
这个更容易理解了，一般来说form的验证都是绑定在blur或者change事件上，如果form太长，需要移动滚动条才能看全的情况下，一旦鼠标点击滚动条就会错误的触发form验证操作，将无用的错误信息显示给用户。
 
## 解决方案
我们来看看jQueryUI的Autocomplete是怎么解决这个问题的。
{% code lang:js %}
// input's blur event
blur: function( event ) {
    if ( this.cancelBlur ) {
        delete this.cancelBlur;
        return;
    }

    clearTimeout( this.searching );
    this.close( event );
    this._change( event );
}

// dropdown's mousedown event
mousedown: function( event ) {
    // prevent moving focus out of the text field
    event.preventDefault();

    // IE doesn't prevent moving focus even with event.preventDefault()
    // so we set a flag to know when we should ignore the blur event
    this.cancelBlur = true;
    this._delay(function() {
        delete this.cancelBlur;
    });

    // clicking on the scrollbar causes focus to shift to the body
    // but we can't detect a mouseup or a click immediately afterward
    // so we have to track the next mousedown and close the menu if
    // the user clicks somewhere outside of the autocomplete
    var menuElement = this.menu.element[ 0 ];
    if ( !$( event.target ).closest( ".ui-menu-item" ).length ) {
        this._delay(function() {
            var that = this;
            this.document.one( "mousedown", function( event ) {
                if ( event.target !== that.element[ 0 ] &&
                        event.target !== menuElement &&
                        !$.contains( menuElement, event.target ) ) {
                    that.close();
                }
            });
        });
    }
}
{% endcode %}

这下就很清楚了，要处理这个问题，要点有两个：

- 通过自定义的flag判断是否需要跳过(直接return)input的blur事件
- 全局(document)监视下一次mousedown事件，如果不是特定区域才执行blur相关操作