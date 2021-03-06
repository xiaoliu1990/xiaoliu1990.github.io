# 彻底理解js中的&&和||
阅读代码时对一行代码很困惑
>step > max_step && (step = min_step);
>查阅资料后发现它等价于
>if (step > max_step) {
>   step = min_step;
>}
```
js中的&&和||本质是什么呢？
&& 和 || 的作用只有一个(定义)：
进行布尔值的且和或的运算。当运算到某一个变量就得出最终结果之后，就返回哪个变量。

在javascript中：
以下内容会被当成false处理："" , false , 0 , null , undefined , NaN
其他都是true。注意：字符串"false"也会被当做true处理，在未转型的情况下他是字符串，属于一个对象，所以是true。

所以：
a || b：如果a是true，那么b不管是true还是false，都返回true。因此不用判断b了，这个时候刚好判断到a，因此返回a。
如果a是false，那么就要判断b，如果b是true，那么返回true，如果b是false，返回false，其实不就是返回b了吗。
a && b：如果a是false，那么b不管是true还是false，都返回false，因此不用判断b了，这个时候刚好判断到a，因此返回a。
如果a是true，那么就要在判断b，和刚刚一样，不管b是true是false，都返回b。

来个复杂的例子（注意一点：在js中&&运算符优先级大于||）
假设：
```
```javascript
  var a=new Object(),b=0,c=Number.NaN,d=1,e="Hello";
  alert(a || b && c || d && e);  
  /*表达式从左往右执行，先&&后||
    1、(b && c)：b是false，此时不需要判断c，因为不管c是true是false，最终结果一定是false，因此返回当前判断对象b，也就是0；
    2、(d && e)：d是true，这个时候判断e，此时不管e是true，是false，返回结果一定是e，e为true，因此返回"Hello"；
    3、(a || b)：a是true，此时不管b是true是false，结果都是true，所以不判断b，所以返回当前判断对象a，因此返回new Object()；
    4、(a || e)：同上，因此返回a。
    这个表达式最终结果为a，也就是new Object()*/
```
```
结论：
a&& b :如果执行a后返回true，则执行b并返回b的值；如果执行a后返回false，则整个表达式返回a的值，b不执行；
a || b :如果执行a后返回true，则整个表达式返回a的值，b不执行；如果执行a后返回false，则执行b并返回b的值；
&& 优先级高于 ||;
```
