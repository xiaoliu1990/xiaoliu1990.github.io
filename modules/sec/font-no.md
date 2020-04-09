###corodva Refused to load the font——拒绝加载字体

>Refused to load the font 'data:application/x-font-ttf;base64,AAEAAAAPAIAAAwBwRkZUTXMrDTgAAAD8AAAAHE9T…oNBgQrsQYBRLEkAYhRWLBAiFixBgNEsSYBiFFYuAQAiFixBgFEWVlZWbgB/4WwBI2xBQBEAAAA' because it violates the following Content Security Policy directive: "default-src *". Note that 'font-src' was not explicitly set, so 'default-src' is used as a fallback.
>这个错误是说：拒绝加载字体，需要在CSP进行配置

解决办法：
找到html中的<metahttp-equiv="Content-Security-Policy"> 标签；
在content中加入<font color=red>font-src * data:;</font>。
问题解决

