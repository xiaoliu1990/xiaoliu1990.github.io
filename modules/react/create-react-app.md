# create-react-app + react-router 5.1.2 + antd-mobile 脚手架

## create-react-app如何更改打包文件目录

```javascript
  /*在 github issue 里找到解决办法了
  在 config-overrides.js ，修改 create-react-app 中的 paths.appBuild 变量就可以了。*/
  const paths = require('react-scripts/config/paths');
  paths.appBuild = path.join(path.dirname(paths.appBuild), 'dist'); // 修改打包目录
```
## create-react-app如何更改打包文件路径
>在package.json 加入
>"homepage": "需要的路径",
```json
{
  "name":"liu",
  "version":"1.0",
  "homepage": "./"
}
```
## create-react-app打包取消map文件
>打开package.json文件，找到"scripts"标签处的"build"那行；
>修改打包的命令为cross-env GENERATE_SOURCEMAP=false
>注意：必须先下载cross-env包
```json
"scripts": {
  "start": "react-app-rewired start",
  "build": "cross-env GENERATE_SOURCEMAP=false react-app-rewired build",
  "test": "react-app-rewired test --env=jsdom",
  "eject": "react-scripts eject"
}
```

###
```javascript

/*通过process.env.npm_lifecycle_event获取package.json中的scripts
启动类型（start，test，build等），分别定义不同环境的接口地址*/
const ENVIRONMENT = process.env.npm_lifecycle_event;
let serverUrl='';
if ( ENVIRONMENT === "start") {
  serverUrl='//www.api.com/api/start';
}
if ( ENVIRONMENT === "test") {
  serverUrl='//www.api.com/api/test';
}
if (ENVIRONMENT === "build") {
  serverUrl='//www.api.com/api/build';
}

```

### override 基本配置

```javascript
module.exports = override(
  //设置按需加载 babel-plugin-import
  fixBabelImports('import', {
    libraryName: 'antd-mobile',
    style: 'css',
  }),
  //设置绝对路径别名
  addWebpackAlias({
    "modules": path.resolve(__dirname, "src/modules"),
    "common": path.resolve(__dirname, "src/common"),
    "components": path.resolve(__dirname, "src/components")
  }),
  //less配置函数
  addLessLoader({
    javascriptEnabled: true,
    modifyVars: { '@primary-color': '#1DA57A' }
  }),
  //适配通常采用 rem 布局
  addPostcssPlugins([require('postcss-pxtorem')({
    rootValue: 16,
    propList: ['*']
    // propList: ['*', '!border*', '!font-size*', '!letter-spacing'],
    // propWhiteList: []
  }),]),
  //定义使用环境变量
  addWebpackPlugin(new webpack.DefinePlugin({
    'process.env':{
      'SERVER_URL_START':JSON.stringify(serverUrl),
      'SERVER_URL_TEST':JSON.stringify(serverUrl),
      'SERVER_URL_PRD':JSON.stringify(serverUrl),
    }
  }))
);
```
