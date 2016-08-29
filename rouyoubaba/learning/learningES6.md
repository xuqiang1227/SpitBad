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

基于此，在这里需要回答一个问题[为何export default XXX之后，import它的时候，调用它需要 XXX.default](http://stackoverflow.com/questions/34736771/webpack-umd-library-return-object-default/34778391#34778391)

`Babel6`在编译JS文件时，会将 
```javascript
export default XXX
``` 
编译为
```javascript
export.default = XXX
```
而在 `Babel5`的时候，会将
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
`Babel6`在编译的时候，少了最后一行 `module.exports = XXX`
* 如果有这一行，说明指定了 `module.exports.default = module.exports`
* module.exports = somethings 是对 module.exports 进行了覆盖，此时 module.exports 和 exports 的关系断裂，module.exports 指向了新的内存块，而 exports 还是指向原来的内存块，为了让 module.exports 和 exports 还是指向同一块内存或者说指向同一个 “对象”，所以我们就 exports = module.exports 。
* 参考[exports 和 module.exports 的区别](https://cnodejs.org/topic/5231a630101e574521e45ef8)
