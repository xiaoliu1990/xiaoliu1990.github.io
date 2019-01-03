# vue-cli 3.0 + element 框架搭建

1、启动 vue-cli；
```javascript
  vue ui
```
2、选择“创建”项目=>新建项目；
> 项目名称--包管理器（npm）--git（有的话可以填上）--下一步

3、新手从没有建过框架选择手动配置，进入功能模块；
>选择的功能有：
> Babel
> Router
> Vuex
> Linter/Formatter（建议选上，对于自己写代码的优美程度有很大约束，加了这个代码如果不规范会提示错误信息，有利于统一代码风格）
> 使用配置文件。
>

4、进入配置界面选择第三个 ESLint + Standard config ，选择后创建系统，创建过程中会要一定时间下载相关依赖和插件；

5、进入项目仪表盘，选择左侧导航第二个插件，选择顶部添加插件，搜索 vue-cli-plugin-element，选中第一个插件，点击安装插件，会自动安装element-ui;

6、需要手动安装axios
```javascript
  npm install axios
```

7、引入elementh和axios，在main.js文件夹里面引入 element.js是element插件的配置文件，httpIntercept.js是axios拦截器，element.js查看官网说明 http://element-cn.eleme.io/#/zh-CN/component/quickstart
```javascript
  import Vue from 'vue'
  import App from './App.vue'
  import router from './router'
  import store from './store'
  import './plugins/element.js'
  import axios from './plugins/httpIntercept.js'

  Vue.config.productionTip = false
  Vue.prototype.$axios = axios

  new Vue({
    router,
    store,
    render: h => h(App)
  }).$mount('#app')
```
```javascript
  //httpIntercept.js
  import { Loading, Message } from 'element-ui'
  import axios from 'axios'
  //由于需要解决跨域问题，不能在拦截器这里定义URL
  // const BASE_URL = '/api'
  // axios.defaults.baseURL = BASE_URL
  axios.defaults.timeout = 30000
  axios.defaults.headers.post['Content-Type'] = 'application/json;charset=UTF-8'
  axios.defaults.withCredentials = true
  // http request 拦截器
  axios.interceptors.request.use((config) => {
    Loading.service()
    return config
  }, (error) => {
    Loading.service().close()
    Message({
      showClose: true,
      message: '接口地址错误',
      type: 'warning'
    })
    return Promise.reject(error)
  })

  // http response 拦截器
  axios.interceptors.response.use((res) => {
    Loading.service().close()
    return res
  }, (error) => {
    if (error.response) {
      Loading.service().close()
      switch (error.response.status) {
        case 500:
          Message({
            showClose: true,
            message: '服务器宕机',
            type: 'warning'
          })
          break
        case 404:
          Message({
            showClose: true,
            message: '找不到接口地址',
            type: 'warning'
          })
          break
      }
    } else {
      Message({
        showClose: true,
        message: '网络异常，请稍后重试！',
        type: 'warning'
      })
      return false
    }
    return Promise.reject(error)
  })
  export default axios
```

8、解决接口调用跨域问题
1. 需要在vue-cli 3.0 中创建 vue.config.js，运用proxy解决跨域问题
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
>注意：target必须要加http:// ，单独//不行，必须强制 http:// 否则就会报错

2. 运用axios调用接口
```javascript
  created: function () {
    this.$axios({
      url: '/api/abc',
      data: {
        'name': '小刘',
        'sex': '男'
      },
      method: 'post'
    }).then((res) => {
      console.log(res)
    })
  }
```
