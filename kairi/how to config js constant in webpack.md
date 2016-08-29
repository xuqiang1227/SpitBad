#1、替换html中内容。比如替换js的cdn。
##webpack的配置
```
const cdn = '//cdn.bootcss.com';
new ReplacePlugin({
      entry: './src/assignment-instructor.html',
      //hash: '[hash]',
      output: `${psweb_path}/assignment-instructor.html`,
      data: {react: `<script src="${cdn}/react/0.14.7/react.min.js"></script>`}
    }),
```
##html页面写法
```
  <!-- replace:react -->
  <script src="../js/react.min.js"></script>
  <!-- endreplace -->
```
详细可参考 [replace-webpack-plugin](https://github.com/otelnov/replace-webpack-plugin)

#2、JS中常量的替换

## webpack的配置 在plugins中加入
```
new webpack.DefinePlugin({
            'process.env.NODE_ENV': '"development"',
            'process.env.webSocket': '"192.168.0.193"'
        }),
```
##js中使用：
```
export const webSocketUrl = `ws://${process.env.webSocket}/notice/websocket`;
```
在js 使用｛｝将在webpack中定义的变量引入即可。
详细可参考[webpack DefinePlugin](https://webpack.github.io/docs/list-of-plugins.html#defineplugin)