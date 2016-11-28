# ES6
##  立即执行匿名函数
## 变量提升
## 块级作用域与函数声明
考虑到环境导致的行为差异太大，应该避免在块级作用域内声明函数。如果确实需要，也应该写成函数表达式，而不是函数声明语句。
```javascript
// 函数声明语句
{
  let a = 'secret';
  function f() {
    return a;
  }
}

// 函数表达式
{
  let a = 'secret';
  let f = function () {
    return a;
  };
}
```
另外，还有一个需要注意的地方。ES6的块级作用域允许声明函数的规则，只在使用大括号的情况下成立，如果没有使用大括号，就会报错。
```javascript
// 不报错
'use strict';
if (true) {
  function f() {}
}

// 报错
'use strict';
if (true)
  function f() {}
```

## 解构
### 数组的解构赋值
ES6允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。
```javascript
/*本质上，这种写法属于“模式匹配”，只要等号两边的模式相同，左边的变量就会被赋予对应的值*/
var [a,b,c] = [1,2,3]; // a=1,b=2,c=3
//下面是一些使用嵌套数组进行解构的例子
let [foo, [[bar],baz]] = [1,[[2],3]]; // foo=1,bar=2,baz=3
let [,,third] = [1,2,3]; //third = 3
let [head, ...tail] = [1, 2, 3, 4]; // head =1, tail = [2,3,4]
//如果解构不成功，变量的值就等于undefined。
let [x, y, ...z] = ['a']; // x = "a",y = undefined, z = []
var [foo] = []; //foo = undefined
var [cba,foo] = [1]; //foo = undefined

/*另一种情况是不完全解构，即等号左边的模式，只匹配一部分的等号右边的数组。*/
let [x,y] = [1,2,3]; //x = 1, y = 2
let [a,[b],d] = [1,[2,3],4] //a = 1, b = 2, d = 4

//默认值
//解构赋值允许指定默认值。
var [foo = true] = []; //foo = true;
[x, y = 'b'] = ['a']; // x='a', y='b'
[x, y = 'b'] = ['a', undefined]; // x='a', y='b'
//ES6内部使用严格相等运算符（===），判断一个位置是否有值。所以，只有当赋值 === undefined时，才会使用默认值
var [x = 1] = [undefined]; //x = 1
//实际是预先判断 新的赋值 undefined === undefined 为true，所以采用默认值 1

var [x = 1] = [null]; // x = null
//因为 null === undefined 为false，所以赋值成功 x = null

//如果默认值是一个表达式，那么这个表达式是惰性求值的，即只有在用到的时候，才会求值。在这里因为x的赋值不为undefined所以f()不会被执行
function f() {
  console.log('aaa');
}
let [x = f()] = [1];
```
默认值可以引用解构赋值的其他变量，但该变量必须已经声明。
```javascript
let [x = 1, y = x] = [];     // x=1; y=1
let [x = 1, y = x] = [2];    // x=2; y=2
let [x = 1, y = x] = [，2];   // x=1; y=2
let [x = 1, y = x] = [1, 2]; // x=1; y=2
let [x = y, y = 1] = [];     // 报错因为x用到y时，y还没声明。
```
### 对象的解构赋值 
```javascript
/*只要某种数据结构具有 `Iterator` 接口，都可以采用数组形式的解构赋值*/ 
//变量名会根据属性名去获取，与顺序无关
var {bar,foo,zzzba} = {a:'a',foo:'foo',bar:'bar'} // foo = "foo", bar = "bar", zzzba = undefined 
//如果变量名与属性名不一致，必须写成下面这样。
var { foo: baz } = { foo: 'aaa', bar: 'bbb' }; //baz = "aaa"
let obj = { first: 'hello', last: 'world' };
let { first: f, last: l } = obj; //f = 'hello', l = 'world'
//这实际上说明，对象的解构赋值是下面形式的简写
var { foo: value1, bar: value2 } = { foo: "aaa", bar: "bbb" };
//也就是说，对象的解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。 `:` 左边的是key，右边的才是被赋值的value
var { foo: baz } = { foo: "aaa", bar: "bbb" }; //baz = "aaa", foo 为error: foo is not defined
//因为let 和 const都不能重新声明，所以这种写法会报错
let foo;
let {foo} = {foo: 1}; // SyntaxError: Duplicate declaration "foo"
//正确写法如下，并且let命令下面一行的圆括号是必须的，否则会报错。因为解析器会将起首的大括号，理解成一个代码块，而不是赋值语句。
let foo;
({foo} = {foo: 1}); // 成功
//和数组一样，解构也可以用于嵌套结构的对象
var obj = {
  p: [
    'Hello',
    { y: 'World' }
  ]
};
var { p: [x, { y }] } = obj;
// x = "Hello", y = "World", p 为 error:p is undefined，因为p是key，并不是value，所以没有值。

//再来个例子
var node = {
  loc: {
    start: {
      line: 1,
      column: 5
    }
  }
};
var { loc: { start: { line }} } = node;
//line = 1, loc 为 error: loc is undefined, start 为 error: start is undefined，因为loc和start都是key
```
嵌套赋值
```javascript
let obj = {};
let arr = [];

({ foo: obj.prop, bar: arr[0] } = { foo: 123, bar: true });

obj // {prop:123}
arr // [true]
```
对象的解构也可以指定默认值。默认值生效的条件是，对象的属性值严格等于undefined(和上面的条件一致 都为 ===undefined 为true时取默认值，所以不包括赋值为null的情况)
```javascript
var {x = 3} = {};
x // 3

var {x, y = 5} = {};
x // undefined
y // 5

var {x, y = 5} = {x: 1};
x // 1
y // 5

var {x, y = 5} = {x: 1,y:23423423423};
x // 1
y // 23423423423

var {x:y = 3} = {};
y // 3

var {x:y = 3} = {x: 5};
y // 5

var { message: msg = 'Something went wrong' } = {};
msg // "Something went wrong"
```
如果解构模式是嵌套的对象，而且子对象所在的父属性不存在，那么将会报错。
```javascript
// 因为foo这时等于undefined，再取子属性就会报错。
var {foo: {bar}} = {baz: 'baz'};
```
如果要将一个已经声明的变量用于解构赋值，必须非常小心。因为JavaScript引擎会将{x}理解成一个代码块，从而发生语法错误。只有不将大括号写在行首，避免JavaScript将其解释为代码块，才能解决这个问题。如：
```javascript
// 错误的写法
var x;
{x} = {x: 1}; // SyntaxError: syntax error

// 正确的写法
({x} = {x: 1});
```
由于数组本质是特殊的对象，因此可以对数组进行对象属性的解构。
```javascript
var arr = [1, 2, 3];
var {0 : first, [arr.length - 1] : last} = arr;
first // 1
last // 3
上面代码对数组进行对象解构。数组arr的0键对应的值是1，[arr.length - 1]就是2键，对应的值是3。方括号这种写法，属于“属性名表达式”。
```
还可以将对象的方法赋值给某个变量
```javascript
let { log, sin, cos } = Math;
```
### 字符串的解构赋值
字符串也可以解构赋值。这是因为此时，字符串被转换成了一个类似数组的对象。
```javascript
const [a, b, c, d, e, f] = 'hello'; // a = "h", b = "e", c = "l", d = "l", e = "o", f  = undefined
```
类似数组的对象都有一个length属性，因此还可以对这个属性解构赋值。
```javascript
let {length : len} = 'hello'; // len = 5
```
### 数值和布尔值的解构赋值 
```javascript
let {toString: s} = 123;
s === Number.prototype.toString // true
let {toString: s} = true;
s === Boolean.prototype.toString // true
//上面代码中，数值和布尔值的包装对象都有toString属性，因此变量s都能取到值。
//解构赋值的规则是，只要等号右边的值不是对象，就先将其转为对象。由于undefined和null无法转为对象，所以对它们进行解构赋值，都会报错。
```
### 函数参数的解构赋值
```javascript
//函数add的参数表面上是一个数组，但在传入参数的那一刻，数组参数就被解构成变量x和y，相当于 function add(x,y)
function add([x, y]){
  return x + y;
}
add([1, 2]); // 3

[[1, 2], [3, 4]].map(([a, b]) => a + b); // [3,7]
```
接下来要对比两个例子
```javascript
//例子1，这里 ({x=0,y=0} = {}) ，因为赋值为undefined所以x,y的默认值生效为0
function move({x = 0, y = 0} = {}) {
  return [x, y];
}
move({x: 3, y: 8}); // [3, 8]
move({x: 3}); // [3, 0]
move({}); // [0, 0]
move(); // [0, 0]

//例子2，这里({x,y} = {x:0,y:0}，默认先给x,y赋值为0，所以move()直接调用可以得到[0,0]，但是其他的只要赋值，就会按照赋值的来，并且没有默认值。)
function move({x, y} = { x: 0, y: 0 }) {
  return [x, y];
}
move({x: 3, y: 8}); // [3, 8]
move({x: 3}); // [3, undefined]
move({}); // [undefined, undefined]
move(); // [0, 0]
```
map函数
```javascript
//undefined就会触发函数参数x的默认值。
//map遍历中三个参数分别为 map(currentValue,index,array)
//currentValue callback 的第一个参数，数组中当前被传递的元素。
//index callback 的第二个参数，数组中当前被传递的元素的索引。
//array callback 的第三个参数，调用 map 方法的数组。
[1, undefined, 3].map((x = 'yes',y,z) => x);
// [ 1, 'yes', 3 ]
```
