# webpack

### SourceMap配置
编译代码与源代码形成一个映射关系，便于查看代码错误
```
devtool: 'source-map'
```
###### 最佳实践  
线上环境(prod)  ``` devtool: 'cheap-module-source-map' ```  
开发环境(dev)  ``` devtool: 'cheap-module-eval-source-map' ```

### webpack-dev-server
###### 安装
```
npm install webpack-dev-server -D
```
###### 配置
```
devServer:{
  contentBase: './dist',//启动目录
  open: true, // 打开浏览器
  port: 8080, // 打开端口
}
```
#### 手写类 webpack-dev-server (在node中直接使用webpack)
###### 所需模块
node模块 express  
webpack开发中间件 webpack-dev-middleware  
安装 ```npm install express webpack-dev-middleware -D ```
###### 基本配置
```
  const express = require('express') //express 模块
  const webpack = required('webpack') //webpack模块
  const webpackDevMiddleware =  require('webpack-dev-middleware') //webpack 中间件
  const config = require('./webpack.config.js') //webpack配置文件
  const compiler = webpack(config) //编译器

  const app = express() //创建http服务器

  app.use(webpackDevMiddleware(compiler,{
    // publicPath: config.output.publicPath //可不写，需要在output配置publicPath
  }))

  app.listen(3000, () => {
    console.log('server is running')
  })
```

### webpack HMR(Hot Module Replacement) 热模块更新
###### 配置
1.devServer增加相关配置
```
devServer{
  hot: true, //开启HMR
  hotOnly: true
}
```
2.使用webpack自带插件 HotModuleReplacementPlugin

```
const webpack = require(webpack)
...
plugins: [
  new webpack.HotModuleReplacementPlugin()
],
...
```
### 使用Babel 处理 ES6
###### 安装
```
npm install babel-loader @bable/core -D  //babel/core 语法核心库
npm install @babel/presets-env -D  // 语法转换规则
npm install @bable/polyfill -D // 补充低版本浏览器不存在的ES6语法内容
```
###### 配置
业务代码 Babel使用配置(polyfill会产生全局污染)
```
...
module: {
  rules: [
    {
      test: /\.js$/,
      loader: 'babel-loader',
      exclude: /node-modules/,
      options:{
        presets: [['@babel/presets-env',{
          targets:{
            chorme: '67' //打包运行浏览器版本
          },
          useBuiltIns: 'usage' //polyfill语法实现按需加载
        }]]
      }
    },
    ...
  ]
},
...
```
类库/组件库 Babel配置  
1.安装
```
npm install --save-dev @babel/plugin-transform-runtime
npm install --save @babel/runtime
```
2.创建 .babelrc 文件

webpack.congfig.js文件配置:
```
...
module:{
  rules:[
    {
      test: /\.js$/,
      loader: 'babel-loader',
      exclude: /node-modules/
    },
    ...
  ]
},
...
```
.babelrc文件配置(不能写注释)
```
{
  "plugins": [
    [
      "@babel/plugin-transform-runtime",
      {
        "corejs": 2,
        "helpers": true,
        "regenerator": true,
        "useESMModules": false
      }
    ]
  ]
}
```
