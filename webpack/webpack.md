# webpack app.js文件拆分，插件拆分

1、需要在entry中配置需要拆分的插件
```javascript
  entry: {
        vendor: ["react", "react-dom","jquery"],
        "babel-polyfill":"babel-polyfill",
        app: path.resolve(__dirname, "../src/app.js")
      },
```
2、在plugins里面配置CommonsChunkPlugin 用来定义拆分的文件名
```javascript
plugins: [
    new webpack.optimize.CommonsChunkPlugin({
      filename: "vendor.js",
      name: ["jquery"],
    })
  ]
```
3、UglifyJsPlugin 压缩代码的插件(这个可以很显著的缩小包的体积，但是我用上后感觉没啥用，js文件并没有减少，或者用的不对)
```javascript
new webpack.optimize.UglifyJsPlugin({ 
  compress: { warnings: false } 
}),
```
