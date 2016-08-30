#webpack的external和npm package.json 又install的关系
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
