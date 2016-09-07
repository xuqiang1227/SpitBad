# webpack的external和npm package.json 又install的关系
webpack的external是在打包的时候，刨除的代码。但是这些代码是在运行时又在npm install中安装的。
总的来说，我发布一个release的代码，我的代码可能基于bootStrap，但是我发布我的代码是不需要把bootStrap打包进去的，这就用到了webpack的external，把bootStrap的代码刨去。
但是我的这个代码在运行的时候又依赖bootStrap。我只需要在package.json中加入 bootStrap的引用，在install我的代码的时候，自然就会在运行环境安装bootStrap了。

如:
```javascript
//webpack.prod.config.js
module.exports = {
  externals: {
    'bootStrap': 'bootStrap/bootStrap'
  }
}

//package.json
{
  "name": "xxx",
  "version": "0.0.1",
  "description": "xxx",
  "main": [
    "dist/index.js",
    "dist/e.css"
  ],
  "files": [
    "dist",
    "bower.json"
  ],
  "scripts": {
    "start": "node examples/server.js",
    "babel": "babel -d lib src",
    "lint": "eslint src/ test/ examples/ config/",
    "build": "NODE_ENV=production webpack --config config/webpack.prod.config.js",
    "test": "NODE_ENV=test karma start config/karma.conf.js",
    "postinstall": "bower install"
  },
  "keywords": [],
  "author": "",
  "license": "XXX",
  "devDependencies": {
   "bootStrap": "^1.3.3"
  },
  "dependencies": {
    "bootStrap": "^1.3.3"
  }
}
```

# webpack 生成图片路径问题

在做项目的时候，因为图片太大所以不能打包进JS。

```javascript
//大于8k的图，在img目录下 以 图片名 进行存储
      {
        test: /\.png/,
        loader: 'url-loader?limit=8192&name=img/[name].[ext]'
      }
```
但是将图片单另打包出来的时候，出现了一个问题就是在 外部想使用我这个component的时候，找不到地址(因为是相对路径)，最后发现可以通过webpack的`output.publicPath`来进行处理

```javascript
  output: {
    path: path.resolve(__dirname, '../dist'),
    publicPath: "/assets/energy-score-ui/dist/"
  },
```

[webpack 生成图片路径问题](https://segmentfault.com/q/1010000003850203)

[output.publicpath](http://webpack.github.io/docs/configuration.html#output-publicpath)
