# What is event.originalEvent?

It's also important to note that the event object contains a property called originalEvent, which is the event object that the browser itself created. jQuery wraps this native event object with some useful methods and properties, but in some instances, you'll need to access the original event via event.originalEvent for instance. This is especially useful for touch events on mobile devices and tablets.

[event.originalEvent jQuery](http://stackoverflow.com/questions/16674963/event-originalevent-jquery)

# 如何识别鼠标滚动事件 Mousewheel/detail
* “mousewheel” 事件中的 “event.wheelDelta” 属性值：返回的值，如果是正值说明滚轮是向上滚动，如果是负值说明滚轮是向下滚动；返回的值，均为 120 的倍数，即：幅度大小 = 返回的值 / 120。
* “DOMMouseScroll” 事件中的 “event.detail” 属性值：返回的值，如果是负值说明滚轮是向上滚动（与 “event.wheelDelta” 正好相反），如果是正值说明滚轮是向下滚动；返回的值，均为 3 的倍数，即：幅度大小 = 返回的值 / 3。
* “mousewheel” 事件在 Opera 10+ 中却是个特例，既有 “event.wheelDelta” 属性，也有 “event.detail” 属性。

```javascript
$('#abs').bind('mousewheel DOMMouseScroll', (e)=> {
    var scrollTo = null;

    if (e.type == 'mousewheel') {
        scrollTo = (e.originalEvent.wheelDelta * -1); //因为 mousewheel和DOMMouseScroll里取到的wheelDelta和detail方向的正负是相反的，所以 * -1
    }
    else if (e.type == 'DOMMouseScroll') {
        scrollTo = 40 * e.originalEvent.detail;//因为 wheelDelta 的值滚动一次增加120，detail的值滚动一次增加3，相差40倍，所以 *40
    }

    if (scrollTo) {
        e.preventDefault();
        e.currentTarget.scrollTop += scrollTo;//当前位置 = 滚动的位置+当前位置
    }
});
```
[如果内层div滚动到底了，不滚动外部结构](https://ruby-china.org/topics/10249)

[浅谈 Mousewheel 事件](http://www.planabc.net/2010/08/12/mousewheel_event_in_javascript/)

# 移动端滚动穿透问题
[最强解释](https://github.com/pod4g/tool/wiki/%E7%A7%BB%E5%8A%A8%E7%AB%AF%E6%BB%9A%E5%8A%A8%E7%A9%BF%E9%80%8F%E9%97%AE%E9%A2%98)

比上面介绍的阻止弹出框底部继续滚动的方式要更好一些
