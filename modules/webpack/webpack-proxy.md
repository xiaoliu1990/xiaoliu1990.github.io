# vue-cli 3.0 基于webpack4解决跨域问题。

1、需要在vue-cli 3.0 中创建 vue.config.js，运用proxy解决跨域问题
```javascript
  module.exports = {
    // 基本路径
    baseUrl: './',
    // 生产环境是否生成 sourceMap 文件
    productionSourceMap: false,
    //在devServer中配置proxy代理，解决跨域
    devServer: {
      proxy: {
        '/api': {
          target: 'http://www.baidu.com', //接口地址
          changeOrigin: true, //是否跨域
          pathRewrite: {
            '^/api': '' //重写接口地址
          }
        }
      }
    }
  }
```
>注意：target必须要加http:// ，单独//不行，必须强制 http://否则就会报错

