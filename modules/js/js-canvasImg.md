# HTML5 file API加canvas实现图片前端JS压缩并上传
> 要想使用JS实现图片的压缩效果，原理其实很简单，核心API就是使用canvas的drawImage()方法。
> canvas的drawImage()方法API如下：
```
context.drawImage(img, dx, dy);
context.drawImage(img, dx, dy, dWidth, dHeight);
context.drawImage(img, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight);
img
  就是图片对象，可以是页面上获取的DOM对象，也可以是虚拟DOM中的图片对象。
dx, dy, dWidth, dHeight
  表示在canvas画布上规划处一片区域用来放置图片，dx, dy为canvas元素的左上角坐标，dWidth, dHeight指canvas元素上用在显示图片的区域大小。如果没有指定sx,sy,sWidth,sHeight这4个参数，则图片会被拉伸或缩放在这片区域内。
sx,sy,swidth,sheight
  这4个坐标是针对图片元素的，表示图片在canvas画布上显示的大小和位置。sx,sy表示图片上sx,sy这个坐标作为左上角，然后往右下角的swidth,sheight尺寸范围图片作为最终在canvas上显示的图片内容。

  注意：drawImage()方法有一个非常怪异的地方，大家一定要注意，那就是5参数和9参数里面参数位置是不一样的，这个和一般的API有所不同。一般API可选参数是放在后面。但是，这里的drawImage()9个参数时候，可选参数sx,sy,swidth,sheight是在前面的。如果不注意这一点，有些表现会让你无法理解。
```

```html
  <p><input id="file" type="file" accept="image/gif, image/png, image/jpg, image/jpeg"></p>
  <p id="log"></p>
```

```javascript
    // 写log方法，演示辅助，与主逻辑无关
  var log = function (info) {
    document.getElementById('log').innerHTML += (info + '<br>');
  };

  var eleFile = document.querySelector('#file');

  if (window.FormData) {
    // 压缩图片需要的一些元素和对象
    var reader = new FileReader(), img = new Image();

    // 选择的文件对象
    var file = null;

    // 缩放图片需要的canvas
    var canvas = document.createElement('canvas');
    var context = canvas.getContext('2d');

    // base64地址图片加载完毕后
    img.onload = function () {
      // 图片原始尺寸
      var originWidth = this.width;
      var originHeight = this.height;

      log('图片原尺寸是：' + [originWidth, originHeight].join('x'));

      // 计算出目标压缩尺寸
      var maxWidth = 400, maxHeight = 400;

      // 目标尺寸
      var targetWidth = originWidth, targetHeight = originHeight;

      if (originWidth > maxWidth || originHeight > maxHeight) {
        // 图片尺寸超过400x400的限制
        if (originWidth / originHeight > maxWidth / maxHeight) {
          // 更宽，按照宽度限定尺寸
          targetWidth = maxWidth;
          targetHeight = Math.round(maxWidth * (originHeight / originWidth));
        } else {
          targetHeight = maxHeight;
          targetWidth = Math.round(maxHeight * (originWidth / originHeight));
        }

        log('超过400x400的限制，图片大小限制为' + [targetWidth, targetHeight].join('x'));
      } else {
        log('图片尺寸较小，不压缩');
      }

      canvas.width = targetWidth;
      canvas.height = targetHeight;

      // 清除画布
      context.clearRect(0, 0, targetWidth, targetHeight);

      // 图片压缩
      context.drawImage(img, 0, 0, targetWidth, targetHeight);

      log('图片blob形式ajax上传，当前进度<span id="percent"></span>');
      // 转为blob并上传
      canvas.toBlob(function (blob) {
        // 图片ajax上传
        var xhr = new XMLHttpRequest();
        // 显示进度的元素
        var elePercent = document.getElementById('percent');
        // 上传文件名
        var filename = encodeURIComponent(file.name).replace(/%/g, '');

        // 上传中
        xhr.upload.addEventListener("progress", function (e) {
          elePercent.innerHTML = Math.round(100 * e.loaded / e.total) / 100 + '%';
        }, false);

        // 文件上传成功或是失败
        xhr.onreadystatechange = function (e) {
          if (xhr.readyState == 4) {
            if (xhr.status == 200) {
              // 100%进度
              elePercent.innerHTML = '100%';

              // 显示上传成功后的图片地址
              var response = xhr.responseText;

              if (/^\/\//.test(response)) {
                response = response.split(filename)[0] + filename;
                log('图片上传成功，地址是：<a href="' + response + '" target="_blank">' + response + '</a>');
              } else {
                log(response);
              }
            }
          }
        };

        // 开始上传
        xhr.open("POST", '../201109/upload.php', true);
        xhr.setRequestHeader("FILENAME", encodeURIComponent(filename));
        xhr.send(blob);
      }, file.type || 'image/png');
    };

    // 文件base64化，以便获知图片原始尺寸
    reader.onload = function (e) {
      // 图片尺寸
      img.src = e.target.result;
    };
    eleFile.addEventListener('change', function (event) {
      file = event.target.files[0];

      if (file.type.indexOf("image") == 0) {
        log('已选择图片' + file.name + '，大小为' + Math.round(1000 * file.size / (1024 * 1024)) / 1000 + 'M。');

        reader.readAsDataURL(file);
      } else {
        log('选择的文件非图片，到此为止。');
      }
    });
  }
```