# react-router 3按需加载问题解决

1、需要在entry中配置需要拆分的插件
```javascript
  //1、定义接收this.props.children的组件
  class Layout extends Component {
    constructor(props) {
      super(props);
    }
    render() {
      return (
        <div>
          {this.props.children}
        </div>
      )
    }
  }
  //2、定义getComponent
  export default {
    path: "test",
    childRoutes: [{
        name: 'test',
        path: 'test',
        getComponent(location, cb) {
            require.ensure([], (require) => {
                cb(null, require('../Component/test').default)
            }, 'test');
        }
    }
  }
  //3、引用getComponent所在的文件，并且放到childRoutes里
  import test from "../Component/getComponent";
  export default [{
    path: "/",
    component: Layout,
    childRoutes: [
      test
    ]
  }]
  //4、将第三步放入到Router里面{Routes}
  export default class Main extends Component {
    render(){
      return (
        <Router routes={Routes} history={hashHistory} ></Router>
      )
    }
  }
```
>需要注意的是。require('../Component/about').default 必须后面跟上 default，否则this.props.children会是undefined。