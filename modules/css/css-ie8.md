# css兼容ie8
1、css透明背景（运用滤镜实现透明背景效果）
```css
  filter:progid:DXImageTransform.Microsoft.gradient(startColorstr=#99ededed,endColorstr=#99ededed);
```
> 注意--#99ededed 其中 99是透明度（透明度代码：0.1--19，0.2--33，0.3--4c，0.4--66，0.5--7f，0.6--99，0.7--b2，0.8--c8，0.9--e5）

2、背景图片不支持background-size
> 最简单的方法是固定背景大小的情况，图片直接切成div块的大小。

3、video不支持ie8播放
> 需要运用falsh插件进行播放，推荐播放器组件html5media

4、transform转换工具
>http://www.useragentman.com/IETransformsTranslator/

5、只在ie浏览器有效的css写法
```css
  color:#FFF\0;                    /* IE8 */
  color:#FFF\9;                    /* 所有IE浏览器(ie6+) */
```
6、盒阴影(box-shadow)效果
```css
  /*Internet Explorer 8 */
-ms-filter:"progid:DXImageTransform.Microsoft.Shadow(color=#CCCCCC,direction=0,strength=6)
　　　　　　  progid:DXImageTransform.Microsoft.Shadow(color=#CCCCCC,direction=90,strength=6)
　　　　　　　progid:DXImageTransform.Microsoft.Shadow(color=#CCCCCC,direction=180,strength=6)
　　　　　　　progid:DXImageTransform.Microsoft.Shadow(color=#CCCCCC,direction=270,strength=6)";

/*低于Internet Explorer 版本8*/

*filter: progid:DXImageTransform.Microsoft.Shadow(color=#CCCCCC, direction=0, strength=6)
　　　　　　progid:DXImageTransform.Microsoft.Shadow(color=#CCCCCC, direction=90, strength=6)
　　　　　　progid:DXImageTransform.Microsoft.Shadow(color=#CCCCCC direction=180, strength=6)
　　　　　　progid:DXImageTransform.Microsoft.Shadow(color=#CCCCCC, direction=270, strength=6);
```
7、放大1.2倍
```css
-ms-filter: "progid:DXImageTransform.Microsoft.Matrix(M11=1.2,M12=0,M21=0,M22=1.2,SizingMethod='auto expand')";
```
8、阴影+放大+并且多个div并排hover放大后放大div在所有div之上
```css
  -ms-filter: "progid:DXImageTransform.Microsoft.Matrix(M11=1.2,M12=0,M21=0,M22=1.2,SizingMethod='auto expand')progid:DXImageTransform.Microsoft.Shadow(color=#CCCCCC,direction=0,strength=10)progid:DXImageTransform.Microsoft.Shadow(color=#CCCCCC,direction=90,strength=10)progid:DXImageTransform.Microsoft.Shadow(color=#CCCCCC,direction=180,strength=10)progid:DXImageTransform.Microsoft.Shadow(color=#CCCCCC,direction=270,strength=10)";
	margin-top: -45px\0;position:relative\0;z-index: 20\0;
```css