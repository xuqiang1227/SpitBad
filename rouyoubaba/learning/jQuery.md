#学习jQuery使用技巧
以前总觉得jQuery简单，现在看看觉得功能很强大，而且太多东西没有用到了。现在开始总结一下用到的东西。
* jQuery 遍历 - is() 方法，返回 false，因为 input 元素的父元素是 p 元素，如果is("p")，返回就为true：
```javascript
  var isFormParent = $("input[type='checkbox']").parent().is("form");
  $("div").text("isFormParent = " + isFormParent);
```

* jQuery 遍历 - has() 方法，检测某个元素是否在另一个元素中：
```javascript
  $("ul").append("<li>" + ($("ul").has("li").length ? "Yes" : "No") + "</li>"); //输出YES
  $("ul").has("li").addClass("full"); //ul增加一个class="full"
```

* jQuery 遍历 - siblings() 方法，查找每个 p 元素的所有类名为 "selected" 的所有同级元素，并将背景色置为yellow：
```javascript
$("p").siblings(".selected").css("background", "yellow")
```
* 切换元素的隐藏或者显示状态 .toggle()

* jQuery 遍历 - closest() 方法：closest() 方法获得离点击元素最近的那个li元素：
```javascript
$( e.target ).closest("li")
```

* addBack()：可以在遍历之后将自己也加入
```javascript
const linkDom = $(e.currentTarget).find('.toggle-icon').addBack('.toggle-icon');
```
