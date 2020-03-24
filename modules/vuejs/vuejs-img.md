##vue webpack打包后图片、js、css路径错误的完美解决方法

项目用run dev build 打包后，发现很多图片都不显示，在本地是没有问题的啊！找原因发现通过webpack+vuecli默认打包的css、js等资源，路径都是绝对的。

1.css、js路径不对

解决方法：打开config/index.js，将其中的assetsPublicPath值改为’./’

![avatar](https://img-blog.csdnimg.cn/2019062714421629.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FhcHBsZWg=,size_16,color_FFFFFF,t_70)

改了之后打包发现虽然css,js文件路径对了，但是css中引用的图片路径和element-ui的字体图标路径还是不对，多了两层目录/static/css,把/static/css这两层目录去掉后能正常访问图片和字体图标资源，如图：

![avatar](https://img-blog.csdnimg.cn/20190627150446500.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FhcHBsZWg=,size_16,color_FFFFFF,t_70)

![avatar](https://img-blog.csdnimg.cn/20190627145325386.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FhcHBsZWg=,size_16,color_FFFFFF,t_70)



之所以打包后会多出两层路径，是因为我们的图片路径都是经历过文件夹的，在本地引用图片是绝对路径，但打包后因为把配置的static文件夹当成了根路径，所以很多图片找不到都不显示。要想在页面正常显示css中引用的图片和字体图标资源，还需要修改另外一个文件，即本文第二点。

2.css中引用的图片资源路径不对找不到

解决方法：打开webpack.prod.conf.js，在output：增加 publicPath: './'
![avatar](https://img-blog.csdnimg.cn/20190627150748918.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FhcHBsZWg=,size_16,color_FFFFFF,t_70)


虽然解决了资源路径的引用问题，但是资源里面的背景图片还是不显示， background: url("../../assets/images/bg.jpg") no-repeat;被相对打包后变成了url(static/img/bg.2fbf2.png) no-repeat所以我们要保留css引用图片的正常路径，即：url(../../static/img/bg.2fbf2.png) no-repeat

那么就需要修改build文件夹下的utils.js代码，如图所示：

![avatar](https://img-blog.csdnimg.cn/20190627151420408.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FhcHBsZWg=,size_16,color_FFFFFF,t_70)


添加publicPath:'../../'这一行代码，这样不论是字体还是图片的引用问题都能解决。
