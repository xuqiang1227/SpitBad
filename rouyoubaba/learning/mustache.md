# mustache模板学习
* {{keyName}} 
`{{}}`就是 Mustache 的标示符，花括号里的 keyName 表示键名，这句的作用是直接输出与键名匹配的键值
* {{#keyName}} {{/keyName}}以`#`开始、以`/`结束表示区块，它会根据当前上下文中的键值来对区块进行一次或多次渲染，如果是null,undefined,false，不渲染任何内容。
* {{^keyName}} {{/keyName}}该语法与{{#keyName}} {{/keyName}}类似，不同在于它是当 keyName 值为 null, undefined, false 时才渲染输出该区块内容。
* {{.}}
* {{<partials}}
* {{{keyName}}}
* {{!comments}}

[Web模板引擎——Mustache](http://www.iinterest.net/2012/09/12/web-template-engine-mustache/)
[Tutorial: HTML Templates with Mustache.js](http://coenraets.org/blog/2011/12/tutorial-html-templates-with-mustache-js/)
