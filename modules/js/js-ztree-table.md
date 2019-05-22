# ztree 表格树
### 原理很简单：利用zTree的addDiyDom方法，自定义每个DOM节点，在原来的节点后面加一些div，再利用css样式使它看起来像个表格。
需要引入的文件
> https://cdn.bootcss.com/zTree.v3/3.5.29/css/zTreeStyle/zTreeStyle.min.css
https://cdn.bootcss.com/jquery/3.2.1/jquery.min.js
https://cdn.bootcss.com/zTree.v3/3.5.29/js/jquery.ztree.all.min.js

```css
/*css样式代码*/
    .ztree {
        padding: 0;
        border: 2px solid #CDD6D5;
    }

    .ztree li a {
        vertical-align: middle;
        height: 30px;
    }

    .ztree li > a {
        width: 100%;
    }

    .ztree li > a,
    .ztree li a.curSelectedNode {
        padding-top: 0px;
        background: none;
        height: auto;
        border: none;
        cursor: default;
        opacity: 1;
    }

    .ztree li ul {
        padding-left: 0px
    }

    .ztree div.diy span {
        line-height: 30px;
        vertical-align: middle;
    }

    .ztree div.diy {
        height: 100%;
        width: 20%;
        line-height: 30px;
        border-top: 1px dotted #ccc;
        border-left: 1px solid #eeeeee;
        text-align: center;
        display: inline-block;
        box-sizing: border-box;
        color: #6c6c6c;
        font-family: "SimSun";
        font-size: 12px;
        overflow: hidden;
    }

    .ztree div.diy:first-child {
        text-align: left;
        text-indent: 10px;
        border-left: none;
    }

    .ztree .head {
        background: #5787EB;
    }

    .ztree .head div.diy {
        border-top: none;
        border-right: 1px solid #CDD2D4;
        color: #fff;
        font-family: "Microsoft YaHei";
        font-size: 14px;
    }

```

```javascript
  var zTreeNodes;
    var setting = {
        view: {
            showLine: false,
            showIcon: false,
            addDiyDom: addDiyDom
        },
        data: {
            simpleData: {
                enable: true
            }
        }
    };
    /***自定义DOM节点 */
    function addDiyDom(treeId, treeNode) {
        var spaceWidth = 15;
        var liObj = $("#" + treeNode.tId);
        var aObj = $("#" + treeNode.tId + "_a");
        var switchObj = $("#" + treeNode.tId + "_switch");
        var icoObj = $("#" + treeNode.tId + "_ico");
        var spanObj = $("#" + treeNode.tId + "_span");
        aObj.attr('title', '');
        aObj.append('<div class="diy swich"></div>');
        var div = $(liObj).find('div').eq(0);
        switchObj.remove();
        spanObj.remove();
        icoObj.remove();
        div.append(switchObj);
        div.append(spanObj);
        var spaceStr = "<span style='height:1px;display: inline-block;width:" + (spaceWidth * treeNode.level) + "px'></span>";
        switchObj.before(spaceStr);
        var editStr = '';
        editStr += '<div class="diy">';
        editStr += '<a href="active.view.html?actId=">增加子部门</a>';
        editStr += '<span class="vertical-line">|</span><a href="active.view.html?actId=">编辑部门</a>';
        editStr += '<span class="vertical-line">|</span><a href="active.view.html?actId=">删除部门</a>';
        editStr += '</div>';
        aObj.append(editStr);
    }
    /***查询数据*/
    function query() {
        var data = [{"CONTACT_USER":"张三","CONTACT_PHONE":"18888888888","addFlag":true,"ORG_ID":1,"id":"o1","pId":"onull","open":true,"name":"单位1","modFlag":true,"CORP_CAT":"港口-天然液化气,港口-液化石油气","TYPE":"org","delFlag":true},{"CONTACT_USER":null,"SECTOR_NAME":"部门1","addFlag":true,"CONTACT_PHONE":null,"SECTOR_ID":1,"ORG_ID":1,"id":"s1","pId":"o1","name":"部门1","modFlag":true,"PARENT_ID":null,"CORP_CAT":"港口-天然液化气","TYPE":"sector","delFlag":true}];
        //初始化列表
        zTreeNodes = data;
        //初始化树
        $.fn.zTree.init($("#dataTree"), setting, zTreeNodes);
        //添加表头
        var li_head = ' <li class="head"><a><div class="diy">部门名称</div><div class="diy">操作</div></a></li>';
        var rows = $("#dataTree").find('li');
        if (rows.length > 0) {
            rows.eq(0).before(li_head)
        } else {
            $("#dataTree").append(li_head);
            $("#dataTree").append('<li ><div style="text-align: center;line-height: 30px;" >无符合条件数据</div></li>')
        }
    }

    $(function () {
        query();
    });
```
```html
<ul id="dataTree" class="ztree"></ul>
```
