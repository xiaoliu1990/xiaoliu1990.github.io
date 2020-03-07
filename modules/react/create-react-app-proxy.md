# create-react-app proxy代理配置

```javascript
/** 1. src 目录下新建 setupProxy.js ,必须是这个名字，否则无法识别
    2. 导入http-proxy-middleware
    3. js代码
    4. axios 引用直接在Url上面写 /api或者/abc
*/
const { createProxyMiddleware } = require('http-proxy-middleware');
module.exports = function(app) {
  app.use(
    createProxyMiddleware('/api', {
      target: 'http://api.xxx.cn/api',
      changeOrigin: true,
      pathRewrite: {
        '^/api': ''
      }
  }));
  app.use(
    createProxyMiddleware('/abc', {
      target: 'http://api.abc.cn/abc',
      changeOrigin: true,
      pathRewrite: {
        '^/abc': ''
      }
  }));
}

```