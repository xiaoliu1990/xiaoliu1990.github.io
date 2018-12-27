# react-router按需加载问题解决

1、需要在entry中配置需要拆分的插件
```javascript
  const about = (location, cb) => {
        require.ensure([], require => {
            cb(null, require('../Component/about').default)
        },'about')
    }
```
>需要注意的是。require('../Component/about').default 必须后面跟上 default，否则this.props.children会是undefined。