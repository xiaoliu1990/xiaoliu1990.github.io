# create-react-app如何更改打包文件目录

```javascript
  /*在 github issue 里找到解决办法了
  在 config-overrides.js ，修改 create-react-app 中的 paths.appBuild 变量就可以了。*/
  const paths = require('react-scripts/config/paths');
  paths.appBuild = path.join(path.dirname(paths.appBuild), 'dist'); // 修改打包目录
```
# create-react-app如何更改打包文件路径
>在package.json 加入
>"homepage": "需要的路径",
```json
{
  "name":"liu",
  "version":"1.0",
  "homepage": "./"
}
```