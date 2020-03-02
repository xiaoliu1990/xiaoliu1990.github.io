# 微信内置浏览器浏览H5页面弹出的键盘遮盖文本框的解决办法

```javascript
  /*scrollIntoViewIfNeeded和scrollIntoView
    scrolltoView：尽可能（向上或向下）滚动浏览器窗口或容器元素，以便在当前视窗的可见范围看见当前元素。存在三个重载参数：
      element.scrollIntoView();不显示声明任何参数，则相当于是element.scrollIntoView(true)
      element.scrollIntoView(alignToTop);Boolean类型参数
      如果为 true，则元素的顶部将尽可能滚动到与父元素可见区域顶部对齐的位置，这是默认值。
      如果为 false，则元素的底部将尽可能滚动到与父元素可见区域底部对齐的位置
      element.scrollIntoView(scrollIntoViewOptions);
      Object类型参数
      {
      behavior: 'auto' | 'instant' | 'smooth',
      block: 'start' | 'end'
      }
      behavior：定义了元素滚动的行为，instant代表是立即滚动元素，smooth代表动画性的平滑滚动，但大部分浏览器并不支持smooth这个属性值，一般都是 instant。
      block：定义了元素滚动的方向，对应Boolean类型参数，如果设置了start值，则相当于是设置了element.scrollIntoView(true)，如果设置了end值，则相当于是设置了element.scrollIntoView(false)，
      需要注意的是，无论是滚动到父元素的顶部还是底部，并不代表子元素会完全滚动到那个位置，浏览器只是尽可能让子元素完全与父元素顶部或者底部对齐，但也有可能滚动到的位置距离父元素顶部或者底部还差一些距离，这取决于页面中其他元素的布局。几乎所有浏览器都支持这个 API。

      scrollIntoViewIfNeeded：只在当前元素在视窗的可见范围内不可见的情况下，才滚动浏览器窗口或容器元素，最终让当前元素可见。如果当前元素在视窗中已经可见了，那么这个方法将不做任何处理，此方法是对Element.scrolltoView()的进一步补充。
      存在一个Boolean类型参数：
      Element.scrollIntoViewIfNeeded(opt_center)；
      如果设置为 true,并且当前元素在视窗的可见范围内不可见，元素将尽量滚动到父元素可视区域的中部位置（垂直方向）
      如果设置为 false,并且当前元素在视窗的可见范围内不可见，元素将尽量滚动到父元素可视区域距离最近的一边，至于到底滚动到父元素的顶部还是底部，取决于滚动的子元素距离父元素的哪一边比较近。
  */
  var androidInputBugFix = function () {
    if (/Android/gi.test(navigator.userAgent)) {
      window.addEventListener('resize', function () {
        if (document.activeElement.tagName == 'INPUT' || document.activeElement.tagName == 'TEXTAREA') {
          window.setTimeout(function () {
            document.activeElement.scrollIntoViewIfNeeded();
          }, 0);
        }
      })
    }
  };

  var iptblur = function () {
    var u = navigator.userAgent;
    var isiOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/);
    if (isiOS) { // 判断是否为IOS系统
      setTimeout(() => {
        const scrollHeight = document.documentElement.scrollTop || document.body.scrollTop || 0;
        window.scrollTo(0, Math.max(scrollHeight - 1, 0));
      }, 100);
    }
  }


  $(function () {

    if (document.activeElement.tagName == "INPUT" || document.activeElement.tagName == "TEXTAREA") {
      window.setTimeout(function () {
        document.activeElement.scrollIntoViewIfNeeded();
      }, 1);
    }
    //微信内置浏览器浏览H5页面弹出的键盘遮盖文本框的解决办法
    window.addEventListener("resize", function () {
      if (document.activeElement.tagName == "INPUT" || document.activeElement.tagName == "TEXTAREA") {
        window.setTimeout(function () {
          document.activeElement.scrollIntoViewIfNeeded();
        }, 1);
      }
    })
  });
```
