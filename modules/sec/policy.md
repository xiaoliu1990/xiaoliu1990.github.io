该Content-Security-Policy元标记可以让你减少风险XSS允许你定义在资源可以被加载，从而防止数据加载浏览器从任何其它位置的攻击。
这使攻击者更难将恶意代码注入您的网站。我撞到了一堵砖墙，试图弄清楚为什么我一个接一个地得到CSP错误，似乎没有任何简明扼要的指示，说明它是如何工作的。
所以这是我尝试简要解释CSP的一些要点，主要集中在我发现很难解决的问题上。为简洁起见，我不会在每个样本中写出完整的标签。
相反，我只会显示content属性，所以说明的样本content="default-src 'self'"
意味着：<meta http-equiv="Content-Security-Policy" content="default-src 'self'">
1.如何允许多个来源？您可以在指令之后将源列出为空格分隔列表：content="default-src 'self' https://example.com/js/"
请注意，除了特殊参数之外，参数周围没有引号，例如'self'。此外，:指令后没有冒号（）。
只是指令，然后是空格分隔的参数列表。隐式允许低于指定参数的所有内容。这意味着在上面的示例中，这些将是有效的来源：https://example.com/js/file.js https://example.com/js/subdir/anotherfile.js
但是，这些无效：http://example.com/js/file.js^^^^ wrong protocol https://example.com/file.js                    ^^ above the specified path
2.如何使用不同的指令，它们各自做什么？
最常见的指令是：default-src 加载javascript，图像，CSS，字体，AJAX请求等的默认策略script-src 定义javascript文件的有效来源style-src 定义css文件的有效源img-src 定义图像的有效来源connect-src为XMLHttpRequest（AJAX），WebSockets或EventSource定义有效目标。如果尝试连接到此处不允许的主机，浏览器将模拟400错误还有其他人，但这些是你最需要的。
3.如何使用多个指令？您可以通过使用分号（;）终止它们来定义一个元标记内的所有指令：content="default-src 'self' https://example.com/js/; style-src 'self'"
4.如何处理端口？除了默认端口之外的所有内容都需要通过在允许的域之后添加端口号或星号来明确允许：content="default-src 'self' https://ajax.googleapis.com http://example.com:123/free/stuff/"以上将导致：https://ajax.googleapis.com:123                            ^^^^ Not ok, wrong port https://ajax.googleapis.com - OKhttp://example.com/free/stuff/file.js                  ^^ Not ok, only the port 123 is allowed http://example.com:123/free/stuff/file.js - OK正如我所提到的，您还可以使用星号来明确允许所有端口：content="default-src example.com:*"
5.如何处理不同的协议？默认情况下，只允许使用标准协议。例如，要允许WebSockets，ws://您必须明确允许它：content="default-src 'self'; connect-src ws:; style-src 'self'"                                          ^^^ web sockets are now allowed on all domains and ports
6.如何允许文件协议file://？如果您尝试将其定义为此类，则无效。相反，你将使用filesystem参数允许它：content="default-src filesystem"
7.如何使用内联脚本和样式定义？除非明确允许，否则不能使用内联样式定义，代码内部<script>标记或标记属性等onclick。你允许他们这样：content="script-src 'unsafe-inline'; style-src 'unsafe-inline'"您还必须明确允许内联，base64编码的图像：content="img-src data:"
8.如何允许eval()？我相信很多人会说你没有，因为'评估是邪恶的'，并且最有可能导致世界即将结束。那些人会错的。当然，你可以使用eval将主要漏洞打入你网站的安全性，但它具有完全有效的用例。你只需要聪明地使用它。你允许这样：content="script-src 'unsafe-eval'"
9.究竟是什么'self'意思？您可能需要'self'表示localhost，本地文件系统或同一主机上的任何内容。它并不意味着任何这些。它意味着具有与定义内容策略的文件相同的方案（协议），相同主机和相同端口的源。通过HTTP服务您的站点？除非您明确定义，否则暂无https。我已经'self'在大多数例子中使用过，因为包含它通常是有意义的，但它绝不是强制性的。如果你不需要它，请把它拿出来。但是等一下！我不能只使用它content="default-src *"并完成它吗？不会。除了明显的安全漏洞之外，这还会使它无法正常运行。即使有些文档声称它允许任何内容，但事实并非如此。它不允许内联或遗漏，所以真的，真的，使你的网站更容易受到攻击，你会使用这个：content="default-src * 'unsafe-inline' 'unsafe-eval'"......但我相信你不会。进一步阅读：http://content-security-policy.comhttp://en.wikipedia.org/wiki/Content_Security_Policy
