# ES6 学习
[What is “export default” in javascript?](http://stackoverflow.com/questions/21117160/what-is-export-default-in-javascript)

##方式一：
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
##方式二：
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

[harmony:modules](http://wiki.ecmascript.org/doku.php?id=harmony:modules)
