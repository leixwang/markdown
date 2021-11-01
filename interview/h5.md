# No1 面试题

## １. 清除浮动的方法

```
1. 直接给父盒子设置一个高度(严格的说不算清除浮动)。这种方式简单方便，但 是当父盒子没有办法直接设置高度就必须用其他方法清除浮动。
```

```
2. 在发生浮动的父盒子中的最后添加一个空元素，然后给该空元素设置clear属性值。

    clear属性值    含义
    left    清除左浮动
    right    清除右浮动
    both    清除所有浮动
    但是当页面中发生浮动的元素很多时就需要添加很多空元素，不推荐使用。
```

```
3. 直接给父盒子设置overflow属性
  overflow：hidden;
  这种方式简单方便，但是如果页面一旦出现了定位，那么定位可能会受到影响。

overflow应用：

overflow值    含义
hidden    超出部分隐藏
scroll    超出部分显示滚动条
auto    超过部分显示滚动条，否则不显示
```

```
4.使用伪元素清除浮动(不理解先背下来会用)
css .clearfix::after, .clearfix::before { content: ""; height: 0; display: block; visibility: hidden; line-height: 0; clear: both; } .clearfix { zoom: 1;//兼容ie } 可以查看新浪等网站查看
```

## 2.什么是闭包? 创建闭包的方法? 闭包的优点，缺点?

```
  闭包是有权访问另一个函数作用域内部变量的函数
在一个函数A中,返回一个函数,并且返回函数中引用了A中的变量.那么这个返回函数就是一个闭包
闭包的优点:
1.减少全局变量的定义,实现变量私有化
2.可以根据不同函数,动态生成一些值

缺点:
1.私有变量会常驻内存,直到该变量使用完毕,会增加内存消耗
```

## ３.什么是事件循环机制?

```
 进程调用之后,会把回调函数放到事件循环中,事件完成之后,会到事件队列中,事件循环会通过事件名匹配事件其相对于的回调函数,当匹配到回调函数的时候,会触发并执行回调函数,如何继续等待下一个事件的到来
```

## 4.说说函数声明提升与变量声明提升?

```
变量的声明会提升到当前作用域顶部;
函数声明提升,会把函数声明提升到当前作用域顶部.
变量声明会比函数声明提升靠前,若变量有赋值,则该值是变量,若变量没有赋值,则是函数!!!!!!
```

## 5.说说使用原生js与jQuery 如何获取元素高度?

```
jQuery获取
$(".f").css("height")  获取内容高度
$(".f").height()    内容高度
$(".f").innerHeight()    padding+content
$(".f").outerHeight(false)    padding+border+content+可选margin(默认为false)
$(window).height()    $(document).height();    获取浏览器可视窗口的高度
原生js
 //只读属性，不能修改
    console.log("f.offsetWidth = ",f.offsetWidth);//content + padding + border
    console.log("f.offsetHeight = ",f.offsetHeight);

    //只读属性，不能修改(经过定位)
    console.log(s.offsetLeft);//获得left
    console.log(s.offsetTop);//获得top
```

## 6.什么是原型链继承?

```
JavaScrip可以采用构造器(constructor)生成一个新的对象,每个构造器都拥有一个prototype属性,而每个通过此构造器生成的对象都有一个指向该构造器原型(prototype)的内部私有的链接(proto),而这个prototype因为是个对象,它也拥有自己的原型,这么一级一级直到原型为null,这就构成了原型链.
这里我们涉及到了一个隐匿属性__proto__,那么__proto__和prototype究竟有什么区别嘞? 注: proto 是一个不应在你代码中出现的非正规的用法，这里仅仅用它来解释JavaScript原型继承的工作原理。
https://github.com/norfish/blog/wiki/%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3JavaScrip%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E5%92%8C%E5%8E%9F%E5%9E%8B%E7%BB%A7%E6%89%BF
```

## 7.简说一下css元素分类

```
块级元素：div,p,h1,form,ul,li;

行内元素 : span>,a,label,input,img,strong,em;
```

## 8.css盒子模型有几种?

```
有两种， IE 盒子模型、标准 W3C 盒子模型；IE的content部分包含了 border 和 pading;

盒模型： 内容(content)、填充(padding)、边界(margin)、 边框(border).
```

## 9.什么是栅格系统

```
1692年，新登基的法国国王路易十四感到法国的印刷水平不尽人意，因此命令成立一个管理印刷的皇家特别委员会。他们的首要任务是设计出科学的、合理的， 重视功能性的新字体。委员会由数学家尼古拉斯加宗（Nicolas Jaugeon）担任领导，他们以罗马体为基础，采用方格为设计依据，每个字体方格分为64个基本方格单位，每个方格单位再分成36个小格，这样，一个印刷版面就有2304个小格组成，在这个严谨的几何网格网络中设计字体的形状，版面的编排，试验传达功能的效能，这是世界上最早对字体和版面进行科学实验的活动，也是栅格系统最早的雏形。
以规则的网格阵列来指导和规范网页中的版面布局以及信息分布
```

## 10.IE中出现的兼容问题

```
  (1)不同浏览器的标签默认的外补丁和内补丁不同        解决方案：CSS里    *{margin:0;padding:0;}
(2)设置较小高度标签（一般小于10px），在IE6，IE7，遨游中高度超出自己设置高度    给超出高度的标签设置overflow:hidden;或者设置行高line-height 小于你设置的高度。
(3)图片默认间距   用float解决
(4)低版本IE不支持min-height
(5)IE8以下都不支持opacity    使用filter: alpha(opacity=50)
```

# No2 面试题

##１. 什么是javascript面向对象继承?
```
继承是指一个对象直接使用另一对象的属性和方法。
```
##２. call 方法和apply方法的区别?
```
共同功能：可以用来代替另一个对象调用一个方法，将一个函数的对象上下文从初始的上下文改变为由thisObj指定的新对象
区别：apply和call的功能是一样的，只是传入的参数列表形式不同。

/*apply()方法*/
function.apply(thisObj[, argArray])

/*call()方法*/
function.call(thisObj[, arg1[, arg2[, [,...argN]]]]);
　　它们各自的定义：

　　　   apply：应用某一对象的一个方法，用另一个对象替换当前对象。例如：B.apply(A, arguments);即A对象应用B对象的方法。

　　　　call：调用一个对象的一个方法，以另一个对象替换当前对象。例如：B.call(A, args1,args2);即A对象调用B对象的方法。

　　它们的共同之处：

　　　　都“可以用来代替另一个对象调用一个方法，将一个函数的对象上下文从初始的上下文改变为由thisObj指定的新对象”。

　　它们的不同之处：

　　　　apply：最多只能有两个参数——新this对象和一个数组argArray。如果给该方法传递多个参数，则把参数都写进这个数组里面，当然，即使只有一个参数，也要写进数组里。如果argArray不是一个有效的数组或arguments对象，那么将导致一个TypeError。如果没有提供argArray和thisObj任何一个参数，那么Global对象将被用作thisObj，并且无法被传递任何参数。

　　　　call：它可以接受多个参数，第一个参数与apply一样，后面则是一串参数列表。这个方法主要用在js对象各方法相互调用的时候，使当前this实例指针保持一致，或者在特殊情况下需要改变this指针。如果没有提供thisObj参数，那么 Global 对象被用作thisObj。

　　　　实际上，apply和call的功能是一样的，只是传入的参数列表形式不同。
~~~
```
 3.为什么字符串不是对象也能对方法的调用?
```
 因为字付串调用原型链的new String()方法生成一个对象，然后对方法进行调用，调用之后就把生成的对象删除，字符串是利用new String()间接对方法调用，所以字符串不是对象也能对方法的调用
 ```

 4.JSON　和　XML的区别?
```
XML和JSON的优缺点对比  
(1).可读性方面。   
JSON和XML的数据可读性基本相同，JSON和XML的可读性可谓不相上下，一边是建议的语法，一边是规范的标签形式，XML可读性较好些。

(2).可扩展性方面。  
XML天生有很好的扩展性，JSON当然也有，没有什么是XML能扩展，JSON不能的。

(3).编码难度方面。  
XML有丰富的编码工具，比如Dom4j、JDom等，JSON也有json.org提供的工具，但是JSON的编码明显比XML容易许多，即使不借助工具也能写出JSON的代码，可是要写好XML就不太容易了。

(4).解码难度方面。  
XML的解析得考虑子节点父节点，让人头昏眼花，而JSON的解析难度几乎为0。这一点XML输的真是没话说。

(5).流行度方面。  
XML已经被业界广泛的使用，而JSON才刚刚开始，但是在Ajax这个特定的领域，未来的发展一定是XML让位于JSON。到时Ajax应该变成Ajaj(Asynchronous Javascript and JSON)了。

(6).解析手段方面。  
JSON和XML同样拥有丰富的解析手段。  

(7).数据体积方面。  
JSON相对于XML来讲，数据的体积小，传递的速度更快些。  

(8).数据交互方面。  
JSON与JavaScript的交互更加方便，更容易解析处理，更好的数据交互。

(9).数据描述方面。  
JSON对数据的描述性比XML较差。  

(10).传输速度方面。  
JSON的速度要远远快于XML。
```

## 5.什么是MVC?
```
1、模型（Model）  
模型是应用程序的主体部分。模型表示业务数据，或者业务逻辑.

2、视图（View）  
视图是应用程序中用户界面相关的部分，是用户看到并与之交互的界面。

3、控制器（controller)  
控制器工作就是根据用户的输入，控制用户界面数据显示和更新model对象状态。
```


6.前端有那些框架(至少说５个)?
```
### 1. Bootstrap
### 2.react
### 3.angular
### 4.vue
### 5.jQuery
### 6.zpto
### 7.foundation
### 8.jQuery UI
```

7.构建动态网站用到什么技术(说出两套)?
```
### LAMP (Linux ,Apach,MySql,PHP)
### LNM  (Linux ,Node , MongoDB)
### WHMP  (WindowServer ,Http服务　,MySql ,PHP)
```

8.怎么用ｊｓ实现对一个div的拖动?
```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>js实现拖动div块</title>
</head>
<body>
<div style="width:500px;height:300px;border:2px solid #ccc; position:absolute; left: 500px;top:100px;" id="div"></div>
<script type="text/javascript">
var px;
var py;
var div = document.getElementById("div");
div.onmousedown = function(e){
 if(!e) e = window.event;//if不是firefox等浏览器，那么e为IE浏览器
 px = e.clientX - parseInt(div.style.left);
 py = e.clientY - parseInt(div.style.top);
 div.onmousemove = mousemove;
}
div.onmouseup = function()
{
    div.onmousemove = null;
}
function mousemove(em){
 if(!em) em = window.event;//if不是firefox等浏览器，那么e为IE浏览器
 div.style.left = (em.clientX-px)+'px';
 div.style.top = (em.clientY-py)+'px';
}
</script>
</body>
</html>
```

9.怎么实现对一个div的编辑?
```
contentEditable="true"
```

10.什么是同源?
```
同源是指，域名，协议，端口均相同
```


# No3 面试题

１. Doctype 的作用？ 标准模式与兼容模式的区别 ?
```
!DOCTYPE声明位于位于HTML文档中的第一行，处于html标签之前。告知浏览器的解析器用什么文档标准解析这个文档。DOCTYPE不存在或格式不正确会导致文档以兼容模式呈现。

标准模式的排版 和JS运作模式都是以该浏览器支持的最高标准运行。在兼容模式中，页面以宽松的向后兼容的方式显示,模拟老式浏览器的行为以防止站点无法工作。
```

２. jQuery 对象和DOM对象的转换．
```
  DOM对象转jQuery对象:在DOM对象加$();
  jQuery对象转DOM对象:在jQuery对象加上[0]或者.get(0);
```

３. 什么是对象?
```
所有操作都是以对象为中心,围绕对象展开,对象拥有的属性和方法都是对象的附属.
```
４. 链式调用的原理
```
每次调用方法都会返回之前选择器选中的元素
```

５. jQuery能干什么?
```
1:简化js任务（DOM操作、AJAX调用和事件处理）;
2:兼容浏览器
3:创建AJAX无刷新网页
4:提供对javascript语言的增强
5:增强事件处理
6:修改页面内容
7:供页面动态效果

```

６. 数组方法至少答出四种
```
1、push()方法将一个或多个元素添加到数组的末尾，并返回删除后的长度
2、pop()方法从数组中删除最后一个元素，并返回删除的值
3、unshift()方法将一个或多个元素添加到数组的开头，并返回添加元素后的长度
4、shift（）方法从数组中删除第一个元素，并返回删除元素的值
5、join()方法将数组(或一个类数组对象)的所有元素连接到一个字符串中
6、reverse()方法将数组中元素的位置颠倒
7、sort()方法会默认按照字符编码进行排序，并返会数组
```

７. 解决闭包的方法
```
  1.使用jQuery方法直接把值传进去
  2.使用立即函数
  3.使用jQuery的DOM0级方法
```

８. 创建ajax的过程
```
(1)创建XMLHttpRequest对象,也就是创建一个异步调用对象.
(2)创建一个新的HTTP请求,并指定该HTTP请求的方法、URL及验证信息.
(3)设置响应HTTP请求状态变化的函数.
(4)发送HTTP请求.
(5)获取异步调用返回的数据.
(6)使用JavaScript和DOM实现局部刷新.
```

### 9.cookie和session区别　　　　　　　　　
```
  1、cookie数据存放在客户的浏览器上，session数据放在服务器上。
  2、cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗
    考虑到安全应当使用session。
  3、session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能
     考虑到减轻服务器性能方面，应当使用COOKIE。
  4、单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie。
```

### 10.jQuery 库中的 $() 是什么？
```
  $():一个选择器， jQuery() 函数的别称

```

1.HTML有几种选择器

```
10种 分别为:标签选择器,类选择器,ID选择器,,并集选择器,交集选择器,通配选择器,后代选择器,子选择器,相邻选择器,伪类选择器
```

2.同时列入两个jQuery文件该怎么解决

```
var $$ = $.noConflict();//将最后一个赋值变量上
    console.log($.fn.jquery);//3.0.0
    //$$ === jQuery仍然是1.12.4版本
    console.log($$.fn.jquery);//1.12.4
    console.log(jQuery.fn.jquery);//1.12.4
```

3.XML优缺点

```
优点:
A.格式统一，符合标准；
B.容易与其他系统进行远程交互，数据共享比较方便。

缺点:
A.XML文件庞大，文件格式复杂，传输占带宽；
B.服务器端和客户端都需要花费大量代码来解析XML，导致服务器端和客户端代码变得异常复杂且不易维护；
C.客户端不同浏览器之间解析XML的方式不一致，需要重复编写很多代码；
D.服务器端和客户端解析XML花费较多的资源和时间。
```

4.JSON优缺点
```
优点:
A.数据格式比较简单，易于读写，格式都是压缩的，占用带宽小；
B.易于解析，客户端JavaScript可以简单的通过eval()进行JSON数据的读取；
C.支持多种语言，包括ActionScript, C, C#, ColdFusion, Java, JavaScript, Perl, PHP, Python, Ruby等服务器端语言，便于服务器端的解析；
D.在PHP世界，已经有PHP-JSON和JSON-PHP出现了，偏于PHP序列化后的程序直接调用，PHP服务器端的对象、数组等能直接生成JSON格式，便于客户端的访问提取；
E.因为JSON格式能直接为服务器端代码使用，大大简化了服务器端和客户端的代码开发量，且完成任务不变，并且易于维护。
缺点:
JSON的缺点
A.没有XML格式这么推广的深入人心和喜用广泛，没有XML那么通用性；
B.JSON格式目前在Web Service中推广还属于初级阶段。

```
5.JSON 与XML的异同点
```
可读性　　JSON和XML的可读性可谓不相上下，一边是建议的语法，一边是规范的标签形式，很难分出胜负。
可扩展性　　XML天生有很好的扩展性，JSON当然也有，没有什么是XML能扩展，JSON不能的。
编码难度　　XML有丰富的编码工具，比如Dom4j、JDom等，JSON也有json.org提供的工具，但是JSON的编码明显比XML容易许多，即使不借助工具也能写出JSON的代码，可是要写好XML就不太容易了。
解码难度　　XML的解析得考虑子节点父节点，让人头昏眼花，而JSON的解析难度几乎为0。这一点XML输的真是没话说。
```
6.输入url发生什么
```
进行DNS域名解析,找到相对应的服务器,在客户端和服务器进行连接,然后客户端给服务器发送HTTP请求,服务器接收请求兵根据请求查找相关资源并返回给客户端,客户端接收服务器的响应,根据响应内容进行渲染在网页显示
```

7.什么是语义化
```
在没有CSS样式的情况下,依然能做到结构清晰组织有序,有利于搜索引擎优化和其他设备进行解析
```
8.ajax能干什么?
```
ajax是一种想服务器发送异步请求,实现web的局部刷新
```

9.H5, CSS有那些新特性?
```
主要是媒体查询,其他还有伸缩盒子,动画,渐变,过渡,2D和3D,边框圆角,阴影,透明有(opacity,rgba),选择器(关系选择器,属性选择器,伪类选择器)
```

10.　浏览器内核有哪些?
```
IE(ie浏览器)  Webkit(google浏览器)  Gecko(火狐浏览器)　Presto(Opera浏览器)
```

11.普通事件和绑定事件区别
```
普通事件只有一个事件如果有多个事件就会被覆盖，
绑定事件是针对DOM元素，可以有一个或者多个事件
```


# No5 面试题

1. 弹性布局的思想？
```
	让容器有能力来改变项目的宽度和高度,子元素根据父元素调整宽高
```
2. bootstrap优点
```
简洁、直观、强悍的前端开发框架，让web开发更迅速、简单；
用于开发响应式布局、移动设备优先的 WEB 项目。
```
3. bootstrap常见组件？
```
字体图标，下拉菜单，按钮组，导航条，巨幕，缩略图，媒体对象，列表组，面板
```
4. bootstrap使用前的准备？
```
加两个meta标签
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1">
加Bootstrap.css文件
加Jquery.js文件
加Bootstrap.js文件
```
5. setInterval 和 setTimeout 区别？
```
setTimeout指定的时间过后运行函数，并且只运行一次,setInterval 每隔一定时间就重复执行一次那个函数;
```
6. CSS优先级算法如何计算？
```
公式  important > style >id > class > 标签 > 继承 > 浏览器的默认属性
混合使用：该元素的css的优先级 默认为0
出现一次 id, 那么 优先级 加 100
出现一次 class, 那么 优先级 加 10
出现一次 标签, 那么 优先级 加 1
当优先级相同时 ：最后的设置有效
```
7. 什么是媒体查询？
```
媒体查询可以让我们根据设备显示器的特性（如视口宽度、屏幕比例、设备方向：横向或纵向）为其设定CSS样式
```
8. 页面导入样式时，使用link和@import有什么区别？
```
 link的作用
  引入样式表(浏览器是单独加载样式表)
  开启dns预解析
 引入table小图标的icon

 @import
 @import 引入样式表只能在样式中填写
 @import 引入样式表是阻塞加载的（如果该样式表加载不完，那么后续的其他内容都不能解析）
```

9. 同源是什么？跨域是什么？
```
同源: 协议，域名，端口，三个相同
跨域: 不同域名之间相互访问
```


10. 事件三要素?
```
事件源、 事件、事件处理程序三部分组成
```


# No 6 面试题

1. CSS里的渐进增强和优雅降级分别是什么？
```
渐进增强 ：针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验。
优雅降级 ：一开始就构建完整的功能，然后再针对低版本浏览器进行兼容。
```

2. get 和post 的区别

```
GET：一般用于信息获取，使用URL传递参数，对所发送信息的数量也有限制，一般在2000个字符
POST：一般用于修改服务器上的资源，对所发送的信息没有限制。
GET方式需要使用Request.QueryString来取得变量的值，而POST方式通过Request.Form来获取变量的值，
也就是说Get是通过地址栏来传值，而Post是通过提交表单来传值。
然而，在以下情况中，请使用 POST 请求：
无法使用缓存文件（更新服务器上的文件或数据库）
向服务器发送大量数据（POST 没有数据量限制）
发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠
```

3. http状态码

```
1XX代表请求已经被接收；
2xx代表请求已成功被服务器接收、理解、并接受。常用的200表示请求已成功，请求所希望的响应头或数据体将随此响应返回；
3xx代表重定向。
4xx代表客户端错误。404表示网页不存在。
5xx代表服务器错误。500表示服务器内部错误，503表示服务器暂时不可用

```

4. css属性是否区分大小写，网页多个h1标签会不利于seo吗？
```
否,不影响
```
5. 说说对作用域链的理解
```
作用域链的作用是保证执行环境里有权访问的变量和函数是有序的，作用域链的变量只能向上访问，变量访问到window对象即被终止，作用域链向下访问变量是不被允许的。
```

6. null 和undefined的区别
```
null是一个表示"无"的对象，转为数值时为0；undefined是一个表示"无"的原始值，转为数值时为NaN。
当声明的变量还未被初始化时，变量的默认值为undefined。
null用来表示尚未存在的对象，常用来表示函数企图返回一个不存在的对象。
undefined表示"缺少值"，就是此处应该有一个值，但是还没有定义。典型用法是：（1）变量被声明了，但没有赋值时，就等于undefined。
（2) 调用函数时，应该提供的参数没有提供，该参数等于undefined。
（3）对象没有赋值的属性，该属性的值为undefined。
（4）函数没有返回值时，默认返回undefined。
null表示"没有对象"，即该处不应该有值。典型用法是：
（1） 作为函数的参数，表示该函数的参数不是对象。
（2） 作为对象原型链的终点。
```

7. 什么事EventEmitter?
```
EventEmitter是node中一个实现观察者模式的类，
主要功能是监听和发射消息，用于处理多模块交互问题.
```

8. 事件委托是什么?
```
利用事件冒泡的原理，让自己的所触发的事件，让他的父元素代替执行！
```

9. 如何阻止事件冒泡和默认事件
```
function stopBubble(e)
{
   if (e && e.stopPropagation)
       e.stopPropagation()
   else
       window.event.cancelBubble=true
}
return false
```

### 10. new 操作符具体做了什么？
```
1、创建一个空对象，并且 this 变量引用该对象，同时还继承了该函数的原型。

2、属性和方法被加入到 this 引用的对象中。

3、新创建的对象由 this 所引用，并且最后隐式的返回 this 。
var obj  = {};

obj.__proto__ = Base.prototype;

Base.call(obj);
```


### 1. 为什么要用Nodejs？
```
1. 采用事件驱动、异步编程，为网络服务而设计。其实Javascript的匿名函数和闭包特性非常适合事件驱动、异步编程。而且JavaScript也简单易学，很多前端设计人员可以很快上手做后端设计。
2. Node.js非阻塞模式的IO处理给Node.js带来在相对低系统资源耗用下的高性能与出众的负载能力，非常适合用作依赖其它IO资源的中间层服务。
3. Node.js轻量高效，可以认为是数据密集型分布式部署环境下的实时应用系统的完美解决方案。Node非常适合如下情况：在响应客户端之前，您预计可能有很高的流量，但所需的服务器端逻辑和处理不一定很多。
```
### 2. 什么是阻塞和非阻塞？
```
阻塞I/O的一个特点是调用之后一定要等到系统内核层面完成所有事件操作后, 调用才结束.
非阻塞 I/O 和阻塞 I/O 的差别为调用之后会立即返回.
```
### 3. 什么是异步？
```
当一个调用发出后,调用者线程不需要等待调用结果的返回就可以继续执行后续的操作,实际处理调用是操作会以状态或者消息来通知调用者,或者通过已定的回调函数来处理调用结果.
```
### 4. css隐藏元素的几种方法？
```
将 opacity 设为 0、将 visibility 设为 hidden、将 display 设为 none
```

### 5. window.onload 和$(function(){})的区别
```
1.执行时间
  window.onload必须等到页面内包括图片的所有元素加载完毕后才能执行.
  $(document).ready()是DOM结构绘制完毕后就执行，不必等到加载完毕.
2.编写个数不同
  window.onload不能同时编写多个，如果有多个window.onload方法，只会执行一个.
  $(document).ready()可以同时编写多个，并且都可以得到执行.
3.简化写法
  window.onload没有简化写法.
  $(document).ready(function(){})可以简写成$(function(){}).
```

### 6. 解决闭包导致的副作用的方法
```
第一种解决方法	(jquery)
   lists.eq(i).click(i,function(event){
   	console.log(event.data);
   });
第二种解决方法	(立即函数)
    (function(i){
        lists[i].onclick = function(){
           console.log(i);
      	}
      })(i);
第三种解决方法	(面向对象)
      lists[i].index = i;
      lists[i].onclick = function(){
        console.log(this.index);
      }
```

### 7. 函数与变量声明提升问题
```
变量的声明会提升到当前作用域顶部.
函数声明提升,会把函数声明提升到当前作用域顶部.

变量没有赋值： 变量在最顶部
变量赋值： 函数在最顶部
```

### 8. jquery 与DOM　的转换
```
dom ====> jQuery    用 $()
jQuery =====> dom   用 jqBox[0]
```

### 9.  如何创建一个闭包
```
在一个函数A中创建一个返回函数,并且该函数中引用了A中的变量.那么这个返回函数就是一个闭包.
```

### 10. 什么是函数绑定
```
定义一个函数然后将其绑定到特定DOM元素或集合的某个事件触发程序上，绑定函数经常和回调函数及事件处理程序一起使用，以便把函数作为变量传递的同时保留代码执行环境。
```
