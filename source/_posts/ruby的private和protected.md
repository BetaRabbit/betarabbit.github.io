title: "Ruby的private和protected"
date: 2012-11-08 23:41:34
tags: Ruby
---

今天，下面这段程序让我纠结了很久，Ruby中private的概念真的很奇怪。。。

{% code lang:ruby %}
class Test private
  def test_print
    puts 'test'
  end
end

class Test2 < Test
  def test_print2
    # self.test_print #=> 这里加上self就不能调用，private method `test_print' called for # (NoMethodError)
    test_print #=> 不加self就能调用
  end
end

Test2.new.test_print2 
{% endcode %}

为什么不加self的话，private也可以调用父类的方法呢？

原来在Ruby中，private和Java或者其他语言不一样，子类也可以调用，只是不能指定调用者。

翻了下《The Ruby Way》，书上说：
{% blockquote %}
private：类和子类都能调用，但是private方法不能指定调用者，默认为self。

protected：类和子类都能调用，可以指定调用者。
{% endblockquote %}

这就解释了为什么上面的代码中，用self调用会出错，而不加self就能正确执行。