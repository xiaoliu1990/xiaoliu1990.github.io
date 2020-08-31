# UEditor第二次加载总是不渲染的解决方案。
>使用UEditor-1.4.3中遇到第一次跳转到使用UEditor的界面后，编辑器加载正常，返回后第二次再跳转到这个界面就出现UEditor无法正常加载，也没百度到答案，看UEditor源码，发现这样一段：
```javascript
  /**
     * @file
     * @name UE
     * @short UE
     * @desc UEditor的顶部命名空间
     */
    /**
     * @name getEditor
     * @since 1.2.4+
     * @grammar UE.getEditor(id,[opt])  =>  Editor实例
     * @desc 提供一个全局的方法得到编辑器实例
     *
     * * ''id''  放置编辑器的容器id, 如果容器下的编辑器已经存在，就直接返回
     * * ''opt'' 编辑器的可选参数
     * @example
     *  UE.getEditor('containerId',{onready:function(){//创建一个编辑器实例
     *      this.setContent('hello')
     *  }});
     *  UE.getEditor('containerId'); //返回刚创建的实例
     *
     */
    UE.getEditor = function (id, opt) {
        var editor = instances[id];
        if (!editor) {
            editor = instances[id] = new UE.ui.Editor(opt);
            editor.render(id);
        }
        return editor;
    };


    UE.delEditor = function (id) {
        var editor;
        if (editor = instances[id]) {
            editor.key && editor.destroy();
            delete instances[id]
        }
    };
```
>这段可以看到，在调用UE.getEditor(‘_editor’)初始化UEditor时，先从放置编辑器的容器instances中获取，没有实例才实例化一个Editor，这就是引起问题的原因。
在第一次跳转到编辑器界面时，正常的实例化了一个新的编辑器对象，并放入instances，调用editor.render(id)渲染编辑器的DOM；
第二次初始化时却仅从容器中取到实例：var editor = instances[id]; 直接返回了editor对象，而编辑器的DOM并没有渲染。

#### 用下面的方式调用：
```javascript
  jQuery(function($) {
    UE.getEditor('_editor').render('_editor')
  )}
  //或者
  jQuery(function($) {
    UE.delEditor('_editor');
    var ue = UE.getEditor('_editor');
  )}
```