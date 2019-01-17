# 现象：项目中用requireJS，在用到UEditor的时候会报这种错误（ZeroClipboard undefined）。
>原因：在有requireJS时ZeroClipboard不会把自己暴露为全局变量
解决问题：
1.在require模块加载配置中修改配置，把ZeroClipboard写入配置中
2.在使时先调用ZeroClipboard模块，把模块定义到全局变量


```javascript
  //1.在require的配置中加入ZeroClipboard 
  require.config({
      paths: {
          ZeroClipboard: "./UEditor.../ZeroClipboard"//主要是加这句话
      }
  });
  //2.在使用前先调用模块，定义为全局变量
  require(['ZeroClipboard'], function (ZeroClipboard) {
    window['ZeroClipboard'] = ZeroClipboard;
  });
```
>这种模式在使用的时候我在用的时候报Cannot read property 'config' of undefined,所以我用了第二种方法，直接修改ZeroClipboard 源码
```javascript
  //找到

  if (typeof define === "function" && define.amd) {
    define(function() {
      return ZeroClipboard;
    });
  } else if (typeof module === "object" && module && typeof module.exports === "object" && module.exports) {
    module.exports = ZeroClipboard;
  } else {
    window.ZeroClipboard = ZeroClipboard;
  }
  //修改成
  if (typeof define === "function" && define.amd) {
    define(function() {
      return ZeroClipboard;
    });
  } else if (typeof module === "object" && module && typeof module.exports === "object" && module.exports) {
    module.exports = ZeroClipboard;
  }
  window.ZeroClipboard = ZeroClipboard;
```