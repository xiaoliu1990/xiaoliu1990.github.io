# jquery data方法
在前端我们经常会做的操作就是**做数据状态的判断和数据处理、提交**，经常会操作dom，也会保存一个**全局的数据处理**。这样做是可以实现很多功能，但是缺点就是过多**操作dom会浪费性能**，全局数据保存多了有时候真的会搞混淆。所以jq提供了一个**data()缓存机制**，有两种使用方式，**一种是绑定在元素上面的data()，一种是存储在一个链式的对象上面**。下面我们就来介绍这两种使用方式。

####第一种：绑定在dom上
```javascript
  var dom = document.body;
  $.data(dom, 'name', '张三');
  console.log($('body').data('name'))
```
这里，我们就想，它们存储在哪里呢？我们用找到body发现没有存储在标签里面，输出这个body也找不到。

**其实啊，这个data绑定在它的一个缓存里面，我们使用jQuery.cache，发现输出了一个对象出来，里面就有保存我们刚刚设置的数据，这个对象我们可以看作一个缓存池或者是缓存链表。我们还发现它这个输出的对象都是数字开头的，所以我们得想办法找到他的这个节点**

**从jq的文档中得知这个data不会直接保存在元素上面，而是保存在这个cache全局对象上面，先给这个元素添加一个jQuery.expando的属性，这里面有一个uuid或者是uid（这里是根据不同的jq版本来的），这个uuid是一个连续的数字，跟我们开始看到的cache对象对应的一模一样。**

所以我们这里把jQuery.expando输出，得到了一个串"jQuery110200914271316066011"这样类似的，得到它以后怎么找到cache对象的对应数字呢，我们用刚刚定义的dom来操作，可以得到一个数字，那么这个数字就对应cache里面的，代码如下：
```javascript
  jQuery.expando;
  dom[jQuery.expando]
```
接下来我们去jq里面找到这个expando
```javascript
  expando: "jQuery" + ( jQuery.fn.jquery + Math.random() ).replace( /\D/g, "" ),
```
下面我们尝试一下用这个cache获取一下我们刚刚存储的数据
```javascript
  jQuery.cache[dom[jQuery.expando]].data.name
```
####第二种：不绑定在元素上面，绑定在对象上面
```javascript
  var obj = {};
  jQuery.data(obj, 'name', '王麻子');
```
这里的obj直接返回的是一个expando对象，所以我们调用的时候可以：
```javascript
  obj.jQuery110208157397060133658.data.name;
  obj[jQuery.expando].data.name;
```

# .attr()与.data()的区别
####本质区别
**attr和data**本质上属于DOM属性和Jquery对象属性的区别。
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Jquery中.attr和.data的区别</title>
    </head>
    <body>
        <p id="app" data-foo="hello"></p>
    </body>
    <script type="text/javascript" src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <script type="text/javascript">
        var getAttr1 = $('#app').attr('data-foo');
        var getData1 = $('#app').data('foo');
        console.log('getAttr1: ' + getAttr1); //hello
        console.log('getData1: ' + getData1); //hello

        $('#app').attr('data-foo', 'world'); //$.attr 设置DOM元素属性值
        var getAttr2 = $('#app').attr('data-foo');
        var getData2 = $('#app').data('foo');
        console.log('getAttr2: ' + getAttr2); //world
        console.log('getData2: ' + getData2); //*** hello ***

        $('#app').data('foo', 'WORLD'); //$.data 设置DOM node属性值
        var getAttr3 = $('#app').attr('data-foo');
        var getData3 = $('#app').data('foo');
        console.log('getAttr3: ' + getAttr3); //world
        console.log('getData3: ' + getData3); //*** WORLD ***

    </script>
</html>
```
1. attr()每次都从DOM元素中取属性的值，即和视图中标签内的属性值保持一致。 
  attr('data-foo')会从标签内获得data-foo属性值； 
  attr('data-foo', 'world')会将字符串'world'塞到标签的data-foo属性中；
2. data()是从Jquery对象中取值，由于对象属性值保存在内存中，因此可能和视图里的属性值不一致的情况。 
  data('foo')会从Jquery对象内获得foo的属性值，不是从标签内获得data-foo属性值； 
  data('foo', 'world')会将字符串'world'塞到Jquery对象的foo属性中，而不是塞到视图标签的data-foo属性中。
####建议
  1. 从性能的角度来说，建议使用$.data()来进行set和get操作，因为它仅仅修改的Jquey对象的属性值，不会引起额外的DOM操作。
  2. attr()和data()应避免混合用。