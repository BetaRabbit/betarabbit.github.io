title: "[翻译] 比较不同Observer模式的实现"
date: 2014-01-27 13:32:25
tags: JavaScript
---

原文链接：[Comparison between different Observer Pattern implementations](https://github.com/millermedeiros/js-signals/wiki/Comparison-between-different-Observer-Pattern-implementations)

下面的比较只是关于订阅、发布事件以及删除事件监听器的一些基本特性。主要是基于各种基本概念实现上的不同和它们使用上的优缺点，而不是可用的特性。有些被标记为缺点的部分可以通过“好的实现”或则“hack”来避免，但是通常情况下，这些缺点都是存在的。

所有的实现都基于一种设计模式（[Observer](http://en.wikipedia.org/wiki/Observer_pattern)）完成了相同的任务，他们虽然有很多共同点，但运行方式上却有些许不同。本文主要是为了帮助你选择哪种实现最适合你的工作流以及你要解决的问题种类。

## Event Emitter/Target/Dispatcher ##

 - 所有派发自定义事件的对象都需要继承自EventEmitter/EventTarget/EventDspatcher或者实现特定的接口。
 - 使用字符串定义事件类型。
 - DOM2/DOM3 Events就是基于这样的模式。

### 代码示例 ###

```javascript
  myObject.addEventListener('myCustomEventTypeString', handler);
  myObject.dispatchEvent(new Event('myCustomEventTypeString'));
  myObject.removeEventListener('myCustomEventTypeString', handler);
```
### 优点 ###

 - 对*target object*具有完全的控制，确保只监听特定target派发的事件。
 - 能够派发任意的事件类型而不用修改target object。
 - 每一种target/object/event都使用相同的方法。
 - 代码容易理解。
 - 事件的target通常都是object本身，这使得事件冒泡_更有逻辑性_。
 - 流行。

### 缺点 ###

 - 倾向于使用继承而不是组合。
 - 使用字符串定义事件类型，容易产生拼写错误并且IDE自动完成不能很好的工作。
 - Event handler通常只接受一个参数（Event Object）。
   - 如果想传递额外的数据，必须创建一个实现了特定接口的自定义Event对象或者扩展一个基本的Event对象，这个过程通常是繁琐不便的。

## Publish / Subscribe *(pub/sub)* ##

 - 使用同一个对象向多个*订阅者**广播*消息。
   - 并不是必要条件，但大多数实现都使用一个静态的集中的对象作为广播者。
 - 使用字符串定义事件类型。
 - *消息*和事件的目标之间并没有关系。

### 代码示例 ###

```js
  globalBroadcaster.subscribe('myCustomEventTypeString', handler);
  globalBroadcaster.publish('myCustomEventTypeString', paramsArray);
  globalBroadcaster.unsubscribe('myCustomEventTypeString', handler);
```

### 优点 ###

 - 任意对象都能订阅/发布任何事件类型。
 - 轻量级。
 - 容易使用/实现。

### 缺点 ###

 - 任意对象都能订阅/发布任何事件类型（是的，这既是优点也是缺点）。
 - 使用字符串定义事件类型。
   - 容易出错。
   - 没有自动完成（除非将value保存为变量/常量）。
   - 通过命名规范来避免*消息*被错误的订阅者拦截。

## Signals ##

 - 每种事件类型都有自己的*控制器*。
 - 事件类型不依赖于字符串。

### 代码示例 ###

```js
  myObject.myCustomEventType.add(handler);
  myObject.myCustomEventType.dispatch(param1, param2, ...);
  myObject.myCustomEventType.remove(handler);
```
### 优点 ###

 - 不依赖于字符串。
   - 自动完成能正常工作。
   - 派发或监听一个不存在的事件类型会抛出错误（能够更早的发现错误）。
   - 不需要创建常量来存储字符串值。
 - 细粒度的控制每个监听器和事件类型。
   - 每个signal都是一个特定的目标/容器。
 - 容易定义对象派发的*signal*。
 - 倾向于使用组合而不是继承。
   - 别和原型链混淆了。

### 缺点 ###

 - 不能派发任意类型的事件。*（这在大多数情况下也是个优点）*
 - 每个事件类型都是个对象成员。*（这在大多数情况下也是个优点）*
   - 如果有很多事件类型的话，易导致*命名空间混乱*。
 - 不会将事件类型和目标对象传递给callback使得很难使用通用的handler（工作于多个事件类型和目标）。
 - 与大多数人使用的不同。

## 结论 ##

就像生活中的大多数东西一样，每种解决方案都有它的优点和缺点。决定哪种方式最适合取决于你。我希望本文能在你做决定时帮助到你。再一次，没有解决方案是万能的。
