# ES6 学习
[What is “export default” in javascript?](http://stackoverflow.com/questions/21117160/what-is-export-default-in-javascript)
##export default
###方式一：
如果用，这种形式导出 `不export default`
```javascript
module "foo" {
    export let x = 42;
}
```
就需要用这种形式引入
```javascript
import { y } from "foo";
```
###方式二：
如果用，这种形式导出 `export default`
```javascript
module "foo" {
    export default function() { console.log("hello!") }
}
```
就需要用这种形式引入
```javascript
import foo from "foo";
foo(); // hello!
```
###`一句话形容就是，使用了 export default后，在 import 这个模块时，可以不加 {}`

* [React中导出模块为何必须要有export default](http://stackoverflow.com/questions/31852933/why-es6-react-component-works-only-with-export-default)

[harmony:modules](http://wiki.ecmascript.org/doku.php?id=harmony:modules)

##exports 和 module.exports 的区别
* exports 是指向的 module.exports 的引用
* module.exports 初始值为一个空对象 {}，所以 exports 初始值也是 {}
* `require()`也就是`import` 返回的是 module.exports 而不是 exports

```javascript
  var name = 'nswbmw';
  exports.name = name;
  exports.sayName = function() {
    console.log(name);
  }
```
相当于
```javascript
  var name = 'nswbmw';
  module.exports.name = name;
  module.exports.sayName = function() {
    console.log(name);
  }
```

基于此，在这里需要回答一个问题
`Babel6`在编译JS文件时，会将 
```javascript
export default XXX
``` 
编译为
```javascript
export.default = XXX
```
而在 `Babel6`的时候，会将
```javascript
export default XXX
```
编译为
```javascript
/**注释的内容是上一行的另一种写法，更易懂*/
exports['default'] = XXX
// exports.default = XXX
module.exports = exports['default'];
//module.exports = exports.default;

//上面两行内容 也就是等于
exports.default = XXX
module.exports = XXX
```
