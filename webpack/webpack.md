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
