---
title: '【前端_js】JavaScript知识点总结'
date: 2019-05-22 08:24:00
categories:
  - 前端_js
------

# 1.JavaScript的定义及特性

## 1.1.定义

javascript是运行在客户端的一种直译式脚本语言(程序在运行过程中逐行进行解释)，它的解释器被称为JavaScript引擎，为浏览器的一部分。

## 1.2.特性

(1)[脚本语言](https://baike.baidu.com/item/%E8%84%9A%E6%9C%AC%E8%AF%AD%E8%A8%80)。JavaScript是一种解释型的脚本语言,C、[C++](https://baike.baidu.com/item/C%2B%2B)等语言先[编译](https://baike.baidu.com/item/%E7%BC%96%E8%AF%91)后执行,而JavaScript是在程序的运行过程中逐行进行解释。

(2)基于对象。JavaScript是一种基于对象的脚本语言,它不仅可以创建对象,也能使用现有的对象。

(3)简单。JavaScript语言中采用的是弱类型的变量类型,对使用的数据类型未做出严格的要求,是基于Java基本语句和控制的脚本语言,其设计简单紧凑。

(4)动态性。JavaScript是一种采用事件驱动的脚本语言,它不需要经过Web服务器就可以对用户的输入做出响应。在访问一个网页时,鼠标在网页中进行鼠标点击或上下移、窗口移动等操作JavaScript都可直接对这些事件给出相应的响应。

(5)运行在客户端。不同于服务器端脚本语言，例如[PHP](https://baike.baidu.com/item/PHP/9337)、[ASP](https://baike.baidu.com/item/ASP/128906)和JSP，JavaScript主要被作为客户端脚本语言在用户的浏览器上运行，不需要服务器的支持。

## 1.3.用途

1.嵌入[动态文本](https://baike.baidu.com/item/%E5%8A%A8%E6%80%81%E6%96%87%E6%9C%AC)于HTML页面。

2.对浏览器事件做出响应。

3.读写[HTML元素](https://baike.baidu.com/item/HTML%E5%85%83%E7%B4%A0)。

4.在数据被提交到服务器之前验证数据。

5.检测访客的浏览器信息。

6.控制[cookies](https://baike.baidu.com/item/cookies/187064)，包括创建和修改等。

7.基于Node.js技术进行服务器端编程。

![](/images/前端_js/JavaScript知识点总结_004.png)

【注】:虽然javascript与java都是基于对象的语言，但二则有着本质的区别。js是一种弱类型语言，故js函数可以返回任意类型的数据

# 2.DOM（文档对象模型）

## 2.1.整个html页面上的元素节点构成一棵树

![](/images/前端_js/JavaScript知识点总结_001.gif)

## 2.2.创建新的元素节点

如需向 HTML DOM 添加新元素，您必须首先创建该元素（元素节点），然后向一个已存在的元素追加该元素。

```
<div id="div1">
<p id="p1">这是一个段落</p>
<p id="p2">这是另一个段落</p>
</div>
<script>
var para=document.createElement("p");
var node=document.createTextNode("这是新段落。");
para.appendChild(node);
var element=document.getElementById("div1");
element.appendChild(para);
</script>
```

## 2.3.修改已有的元素节点

### 2.3.1.修改节点的内容

借助节点的innerHTML属性

```
语法：
document.getElementById(id).innerHTML=new HTML
实例：
<html>
<body>
<p id="p1">Hello World!</p>
<script>
document.getElementById("p1").innerHTML="New text!";
</script>
</body>
</html>
```

### 2.3.2.修改节点的属性

语法：

document.getElementById(id).attribute=new value

实例:

<!DOCTYPE html>

<html>

<body>

<img id="image" src="smiley.gif">

<script>

document.getElementById("image").src="landscape.jpg";

</script>

</body>

</html>

### 2.3.3.修改节点的样式

语法：

document.getElementById(id).style.property=new style

实例：

<p id="p2">Hello World!</p>

<script>

document.getElementById("p2").style.color="blue";

</script>

## 2.4.通过DOM为元素添加事件处理

向元素button添加click 事件处理

<script>

document.getElementById("myBtn").onclick=function(){displayDate()};

</script>

## 2.5.为动态创建的元素添加事件处理

### 2.5.1.用元素的事件属性来绑定

function show(arg){

console.log(arg);

}

var show=document.createElement("input");

div.appendChild(show);

show.type="button";

show.value="查看";

方式一

show.onclick=function(){

show(this)

};

方式二

show.onclick= show;注意此处不能加参数

### 2.5.2.jQuery为动态创建的元素绑定事件

对于动态创建并append到页面上的元素来说，不能直接用下面的方法来绑定事件

$(".class a").click(function(){   })  

常用on()方法并借助父选择器为动态创建的元素绑定事件

语法：$(ParentSelector).on(event,DynamicElement,data,function,map)

event：必需。规定添加到元素的一个或多个事件。事件的类型包含： blur, focus, focusin, focusout, load, resize, scroll, unload, click, dblclick, mousedown, mouseup, mousemove, mouseover, mouseout, mouseenter, mouseleave, change, select, submit, keydown, keypress, keyup, error等，由空格分隔多个事件值。必须是有效的事件。

childSelector：需要绑定事件的标签的ID、类、属性选择器。如果没有childSelector项的话，只能给被选元素（已存在的元素）绑定事件，不能给未来元素（后续动态生成的元素）绑定事件

data：触发事件需要传入的数据，可选。规定传递到函数的额外数据。

function：必需。规定当事件发生时运行的函数。

map：可选，规定事件映射 ({event:function, event:function, ...})，包含要添加到元素的一个或多个事件，以及当事件发生时运行的函数。

实例：

$("div").on("click","p",function(){  
    alert($(this).text());  
})

$("div").off( "click", "p", handler );

<script>

function handlerName(event)

{

alert(event.data.msg);

}

$(document).ready(function(){

$("p").on("click", {msg: "You just clicked me!"}, handlerName)

});

</script>

### 2.5.3.循环的为元素绑定事件的几个常用方法

<div class="wrap">  

        <a href="javascript:;" class="link">第一个链接</a>  

        <a href="javascript:;" class="link">第二个链接</a>  

        <a href="javascript:;" class="link">第三个链接</a>  

</div> 

window.onload = function()   

{  

     var aLi = document.getElementsByTagName("a");  

     for(var i = 0; i < aLi.length; i++)   

     {  

         aLi[i].onclick = function()   

         {  

                alert(i);   //3  

         }     

      }  

}  

发现不管点哪个链接都是弹出"3"，其实在我们调用onclick函数时候，外层循环结束已经结束了，i值结束为3跳出。那其实换句话说，只要我们能把这个i值动态传到onclick函数里不就可以了。

解决方案一：给要进行事件绑定的元素一个内置属性count用于记录位置

window.onload = function()   

{  

     var aLi = document.getElementsByTagName("a");  

     for(var i = 0; i < aLi.length; i++)   

     {  

      aLi[i].count = i;  

      aLi[i].onclick = function()   

      {  

           alert(this.count);     

      }     

      }   

}  

解决方案二：可以循环给绑定事件的函数动态传入参数，闭包的经典例子

window.onload = function()   

{  

    var aLi = document.getElementsByTagName("a");  

    for(var i = 0; i < aLi.length; i++)   

    {  

    var a = function(k)  

        {  

         aLi[k].onclick = function()   

         {  

        alert(k);  

          }  

}

//事件的绑定是在循环结束后才进行的，但方法的调用却是在每次循环中进行的

a(i);

  }        

}  

注：按照常规的循环给元素绑定事件，由于事件绑定是在整个循环结束后才进行的，所以导致传给所有元素的事件处理方法的变量都是循环结束后的最终值。

# 3.BOM (浏览器对象模型)

浏览器对象模型 (BOM) 使 JavaScript 有能力与浏览器"对话"，由于现代浏览器已经实现了 JavaScript 交互性方面的相同方法和属性，因此常被认为是 BOM 的方法和属性。

## 3.1.Window 对象

所有浏览器都支持 **window** 对象，它表示浏览器窗口，所有 JavaScript 全局对象、函数以及变量均自动成为 window 对象的成员。其中全局变量是 window 对象的属性，全局函数是 window 对象的方法，甚至 HTML DOM 的 document 也是 window 对象的属性之一。

### 3.1.1.window对象的常用的方法

1. open方法 语法格式：window.open(URL,窗口名称,窗口风格)

功能：打开一个新的窗口，并在窗口中装载指定URL地址的网页

2. close方法 语法格式：window.close()

功能：close方法用于关闭当前浏览器窗口

3. alert方法 语法格式： window.alert(提示字符串)

功能：弹出一个警告框，在警告框内显示提示字符串文本

4. confirm方法 语法格式：window.confirm(提示字符串)

功能：显示一个确认框，在确认框内显示提示字符串，当用户单击"确定"按钮时该方法返回true，单击"取消"时返回false

5. prompt方法 语法格式：window.prompt(提示字符串，缺省文本)

功能：显示一个输入框，在输入框内显示提示字符串，在输入文本框显示缺省文本，并等待用户输入，当用户单击"确定"按钮时，返回用户输入的字符串，当单击"取消"按钮时，返回null值

6. back方法 语法格式：window.back()

功能：模拟用户点击浏览器上的"后退"按钮，将页面转到浏览器的上一页。

说明：仅当当前页面存在上一页时才能进行该操作。

7. forward方法

语法格式：window.forward()

功能：模拟用户点击浏览器上的"前进"按钮，将页面转到浏览器的下一页。

说明：仅当当前页面存在下一页时才能进行该操作

【注】:方法6,7也可以借助history对象来实现。

## 3.2.Location对象

### 3.2.1.location对象的属性

|  |  |  |
| --- | --- | --- |
| **属性名** | **例子** | **说明** |
| host | "[www.wrox.com:80](https://link.jianshu.com?t=http%3A%2F%2Fwww.wrox.com%3A80)" | 返回服务器名称和端口 |
| hostname | "[www.wrox.com](https://link.jianshu.com?t=http%3A%2F%2Fwww.wrox.com)" | 返回服务器名称 |
| herf | "[http://www.wrox.com](https://link.jianshu.com?t=http%3A%2F%2Fwww.wrox.com)" | 返回当前页面完整的URL |
| pathname | "/WileyCDA" | 返回url中的目录和文件名 |
| port | "8080" | 返回端口 |
| protocol | "http:" | 返回页面使用协议（http、https） |
| search | "?q=javascript" | 返回url查询字符串。以问号开头 |

【注】针对location对象的属性有两种操作，若不赋值则表示获取某属性的值，若给某属性赋值则表示根据新的URL加载新的页面

### 3.2.2.Location对象的方法

location.assign('url');

<!-- 跳转到新的页面，并将来之前的页面保存在历史记录history中-->

<button onclick="location.assign('https://www.jd.com')">跳转到京东</button>

location.replace('url');

<!--跳转到新的页面，但不会把之前的页面保存在历史记录中-->

<button onclick="location.replace('https://www.jd.com')">跳转到京东</button>

location.reload();

<!--刷新当前页面-->

<button onclick="location.reload()">刷新</button>

【注】1.location.href是最常用的属性，用于获取或设置窗口的URL，改变该属性，就可以跳转到新的页面：

<script type="text/javascript">    

    location.href = "http://www.sunchis.com";    

</script>   

上面代码的作用是：打开页面后将会跳转到http://www.sunchis.com的页面。  
assign()方法也可实现上述操作：

[html] [view plain](http://blog.csdn.net/xuxiaoping1989/article/details/53486219 "view plain") [copy](http://blog.csdn.net/xuxiaoping1989/article/details/53486219 "copy")

<script type="text/javascript">    

   location.assign("http://www.sunchis.com");    

</script>  

2.如果不想让包含脚本的页面能从浏览器的历史记录中访问，replace()方法可以做到这一点。replace()方法所做的操作与assign()方法一样，但它多了一步操作，即从浏览器的历史记录中删除了包含脚本的页面，这样就不能通过浏览器的后退按钮和前进按钮来访问它了，assign()方法却可以通过后退按钮来访问上个页面。你可以自己测试一下这段关于replace()方法的代码：

<html>  

    <head>  

     <title>不能访问此页面的历史页面</title>  

    </head>  

    <body>  

        <p>测试一下效果，请等待一秒钟……</p>  

        <p>然后点击浏览器的"后退按钮"，你会发现什么？</p>  

        <script type="text/javascript">  

            setTimeout(function(){    

                location.replace("http://www.sunchis.com");    

            },1000);    

        </script>  

    </body>  

</html>   

3. reload()方法有两种模式，即从浏览器的缓存中重载，或从服务器端重载。究竟采用哪种模式由该方法的参数决定。  
false：从缓存中重新载入页面，也是replace()方法的默认形式  
true：从服务器重新载入页面。

## 3.3.History 对象

history对象保存着用户的上网记录，使用go()方法可以在上网记录中前进后退

history.go(0);//刷新

history.go(1);//前进

history.forward();//同上

history.go(-1);//后退

history.back();//同上

    history.go('http://baidu.com');//跳转到最近的http://baidu.com页面，不定是前进还是后退，取最近

# 4.js中函数的两种定义方式

定义js函数的两种最常见形式：function语句式与函数直接量方式。

## 4.1.function语句式

调用可以在定义之前。当调用在定义之前时必须保证二者在同一个script标签里

例如：

function语句式，调用和定义在同一个script标签，此时调用可以在定义之前

<html>

    <head>

        <script>

            test();

        </script>

    </head>

    <body>

        <div>HelloWorld</div>

    </body>

    <script>

        function test() {

            alert('a');

        }

    </script>

</html>

此时虽然是function语句式，但函数的调用与定义不在同一个script中，所以会报错。

## 4.2.直接量的方式

函数直接量用来定义函数所使用的。  
var square1=function(x){return x\*x};

var square2=function funMethod(x){return x\*x};

var square3=new function("x","return x\*x")

var square4=function(){ alert('直接量的形式');}  
函数直接量是用关键字function 后加可选的函数名、用括号括起来的参数列表和用花括号括起来的函数体定义。  
直接量也称为字面量，是JavaScript中一种对象的表示(或者说创建)方式，是指直接给变量赋值JS中原生对象值（对象、函数）的方式从而转换为一个相应的对象。

注：1、对于函数直接量的方式，定义必须在调用之前

1. 函数直接量分为有参函数直接量和无参函数直接量，其调用形式分别为：square1(X)、square4()

# 5.JS中六种数据类型

js中有六种数据类型：Number, Boolean String, 引用 ,Undefined,Null简记为数字、布尔、字符串、引用、未定、空指针。其中引用类型是一种复杂数据类型。

**typeof 操作符**

由于js中的变量是松散类型的，所以它提供了一种检测当前变量的数据类型的方法，也就是typeof关键字.

type of   123　　 //Number

type of    true       //Boolean

type of   'abc'　　//String

type of    { }           //Object

type of    [ ]           //Object

type of    undefined   //Undefined

type of    null        //Object

type of    console.log()       //Function

null类型进行typeof操作符后，结果是object，原因在于，null类型被当做一个空对象引用。综上所述JS的六种数据类型中：四种基本类型两种引用类型

## 5.1.Number类型

Number类型包含整数和浮点数（浮点数数值必须包含一个小数点，且小数点后面至少有一位数字）两种值。

NaN:非数字类型。

特点：①涉及到的任何关于NaN的操作，都会返回NaN ②NaN不等于自身。

isNaN() 函数用于检查其参数是否是非数字值。

isNaN(123)  //false   isNaN("hello")  //true

## 5.2.Boolean类型

该类型只有两个值，true和false

## 5.3.String类型

字符串有length属性。

字符串转换：转型函数String(),适用于任何数据类型（null,undefined 转换后为null和undefined）；toString()方法（null,defined没有toString()方法）。

## 5.4.Undefined类型

只有一个值，即undefined值。使用var声明了变量，但未给变量初始化值，那么这个变量的值就是undefined。

## 5.5.Null类型

null类型被看做空对象指针，前文说到null类型也是空的对象引用。

## 5.6.引用类型

js中对象是一组属性与方法的集合。这里就要说到引用类型了，引用类型是一种数据结构，用于将数据和功能组织在一起。引用类型有时候也被称为对象定义，因为它们描述的是一类对象所具有的属性和方法。

### 5.6.1.布尔对象Boolean

逻辑对象只能取值为true或false，如果逻辑对象无初始值或者其值为 0、-0、null、""、false、undefined 或者 NaN，那么对象的值为 false。否则，其值为 true（即使当自变量为字符串 "false" 时）！

### 5.6.2.算数对象Math

Math对象中有8个数值常量及几个常用方法

### 5.6.3.日期对象Date

1.利用new Date(time)返回指定时间的日期对象，具体的时间由time指定。无参数时默认返回当前日期对象。例如

![](/images/前端_js/JavaScript知识点总结_006.png)![](/images/前端_js/JavaScript知识点总结_009.png)

【注】:利用new Date("month dd yyyy hh:mm:ss")可以指定日期的格式，后面的hh:mm:ss可选（不选的话就是默认的开始时间），而且前三项的顺序可以随意，甚至各字段后面可以加逗号。

2.时间对象常用的方法：

var mydate = new Date();

mydate.getYear(); //获取当前年份(2位)

mydate.getFullYear(); //获取完整的年份(4位)

mydate.getMonth(); //获取当前月份(0-11,0代表1月)

mydate.getDate(); //获取当前日(1-31)

mydate.getDay(); //获取当前时间的星期X(0-6,0代表星期天)

mydate.getHours(); //获取当前小时数(0-23)

mydate.getMinutes(); //获取当前分钟数(0-59)

mydate.getSeconds(); //获取当前秒数(0-59)

Note:若用new Date(time)指定了某个时间，则以上方法返回的均是所指定时间的信息

【技巧】可以把前三种引用类型简记为"是不是到了学数学的时间了"

### 5.6.4.JS对象

var person = new Object();

person.name = "Micheal";

person.age = 24;

//JS对象的字面量表示法

var person = {

　　name : "Micheal",

　　age : 24

};

特殊的JS对象JSON

var jsonObject={  

        "name":"Nicholas",  

        "age":29  

} 

总结：

1.JSON是JS对象的特殊描述形式，就是用花括号括起来的键值对，每一组键值对之间用逗号分隔，属性名用双引号引起来。

2.JSON只是一种格式，没有对象一说，平时说的JSON对象是JSON格式的JS对象

### 5.6.5.数组Array

数组的每一项可以用来保存任何类型的数据，也就是说，可以用数组的第一个位置来保存字符串，第二个位置保存数值，第三个位置保存对象....另外，数组的大小是可以动态调整的。创建数组的基本方式有两种

第一种是使用Array构造函数，如下所示

var colors = new Array("red","blue","yellow");

第二种是使用数组字面量表示法，如下所示

var colors = ["red","blue","yellow"];

1.使用 for...in 来遍历数组。例如

![](/images/前端_js/JavaScript知识点总结_007.png)

Note:此处与Java中的foreach不同，输出的是mycars[x]而不是x

2.使用 concat() 方法来合并两个数组

![](/images/前端_js/JavaScript知识点总结_003.png)

3.使用join(str)方法将数组用str连接成一个字符串

![](/images/前端_js/JavaScript知识点总结_002.png)

4.使用 sort() 方法对数值或文本数组排序

![](/images/前端_js/JavaScript知识点总结_008.png)

![](/images/前端_js/JavaScript知识点总结_005.png)

**【总结】JS对象实际上就是"关联数组"（associative array），数组是下标索引，JS对象是键索引**

比如在javascript中，可以这样定义一个对象： 

var a={"城市":"北京","面积":16800,"人口":1600};

但是，也可以定义成一个关联数组：

a["城市"]="北京";

a["面积"]=16800;

a["人口"]=1600;

### 5.6.6.Function类型

每个函数都是Function类型的实例，而且都与其他引用类型一样具有属性和方法。函数通常是使用函数声明语法定义的，如下所示

function sum(num1,num2){

　　return num1 + num2;

};

这和使用函数表达式定义函数的方式相差无几。

var sun = function (){

　　return sum1 + sum2;

};

**总结：**

**1. JS中的数据类型按照存储方式分为值类型和引用类型两种**

题目1： var a = 100;

　　　　var b = a;

　　　   a = 200;

　　　　console.log (b);

题目2： var a = {age : 20};

　　　　var b = a;

　　　　b.age = 21;

　　　　console.log (a.age);

题目1的答案是 100，题目2的答案是21，

非引用类型（值类型）之间赋值是传值，引用类型之间赋值是传址。引用型的变量之间赋值时两个变量都保存了同一个对象地址，则这两个变量指向了同一个对象。因此，改变其中任何一个变量，都会相互影响。因此，引用类型的赋值其实是对象保存在栈区地址指针的赋值，因此两个变量指向同一个对象，任何的操作都会相互影响。

**2. typeof (obj)返回变量的类型**

其返回值为字符串，有六种可能："number"、"string"、"boolean"、"object"、"function"、"undefined"

1.判断变量是否为undefined:

var tmp = undefined;

if (typeof(tmp) == "undefined"){

alert("undefined");

}

2.判断变量是否为null:

var tmp = null;

if (!tmp && typeof(tmp)!="undefined" && tmp!=0){

alert("null");

}

# 6.js中变量的作用域

js中的变量同样分为全局变量、局部变量和块变量三种。对应的作用域为全局作用域（Global Scope）、函数作用域（局部作用域Local Scope）及（块变量Block Scope）。

## 6.1.全局变量

　　全局变量拥有全局作用域，在代码的任何地方都有定义，一般来说以下几种情形拥有全局作用域：

　　（1）不在任何函数内的变量拥有全局作用域，例如：

var scope="global"; //声明一个全局变量

function checksope(){

function showglobal(){

alert(scope); //弹窗全局变量

}

showglobal();

}

checksope() // global 内部函数可以访问全局变量

（2）所有末定义直接赋值的变量自动声明为拥有全局作用域，例如：

function checksope(){

var scope="local";

scopeglobal="global";

alert(scope);

}

checksope(); // local

//未经var声明的变量，成为window对象的内置属性，即为全局变量

alert(scopeglobal);

alert(scope); //脚本错误

 【注】：变量scopeglobal未经var定义直接赋值，其拥有全局作用域，而scope在函数外部无法访问到。

（3）所有window对象的属性拥有全局作用域

　　一般情况下，window对象的内置属性都拥有全局作用域，例如window.name、window.location、window.top等等。

## 6.2.局部变量

局部作用域一般只在固定的代码片段内可访问到，最常见的例如函数内部，所以这种作用域也称为函数作用域，例如下列代码中的bolgName和函数innerSay都只拥有局部作用域。

function checksocpe(){

var socpe="local";

function inner(){

alert(socpe);

}

inner();

}

alert(socpe); //脚本错误

inner(); //脚本错误 函数外部无法访问内部定义的函数

### 6.2.1.局部变量声明前置的特性

  javascript的函数作用域是指在函数内声明的所有变量在函数体内始终是可见的，有意思的是，这意味着变量在声明之前甚至已经可用。JavaScript的这个特性被非正式地称为声明提前，即JavaScript函数里声明的所有变量（但不涉及赋值）都被"提前"至函数体的顶部。

var scope = "glocal";

function f(){

console.log(scope);//输出"undefined",而不是"global"

var scope = "local";//变量在这里赋初始值，但变量本身在函数体内任何地方均是有定义的

console.log(scope);//输出"local"

}

也许您会误认为第一行会输出"global"，因为代码还没有执行到var语句声明局部变量的地方。其实不然，由于函数作用域提前声明的特性，局部变量在整个函数体始终是有定义的，也就是说，在函数体内局部变量覆盖了同名全局变量。尽管如此，只有在程序执行到var语句的时候，局部变量才会被真正赋值。因此，上述过程等价于：将函数内的变量声明"提前"至函数顶部，同时变量初始化留在原来的位置：

【注】："声明提前"这步操作是在JavaScript引擎的"预编译"时进行的，是在代码开始运行之前。

## 6.3.块变量

  在一些类似c语言中，花括号内定义的变量都具有各自的作用域，而且变量在花括号之外是不可见的，我们称之为块级作用域。而JavaScript中不能视花括号内定义的变量为块变量,块变量必须要用let关键字来定义。

let关键字定义块变量

let c = 3;

console.log('函数外let定义c：' + c);//输出c=3

function change(){

let c = 6;

console.log('函数内let定义c：' + c);//输出c=6

}

change();

console.log('函数调用后let定义c不受函数内部定义影响：' + c);//输出c=3

总结：

1.不在任何函数内的变量、未经var关键字申明直接赋值的变量以及window对象的内置属性都是全局变量

2.函数体内任何位置经var定义的变量都是局部变量，在本函数内均可见

3.经let关键字申明的变量都是块级变量，在一个块内不能重复定义同一个块变量。

4.同名的局部变量会覆盖全局变量，而且局部变量都有申明提前但赋值在原位置的特性

5.用const关键字申明的变量为常量，常量在申明时必须赋初值且不可更改。

# 7.H5页面跳转和参数传递

## 7.1.页面跳转

方法一：用location对象来实现页面的跳转

<script language="javascript">  

    window.location = "http://www.baidu.com";  

</script>  

<script language="javascript">  

    document.location = "http://www.baidu.com";  

</script>  

方法二：用history对象来实现页面的前进或后退

<input type=button value=刷新 onclick="window.location.reload()">  

<input type=button value=前进 onclick="window.history.go(1)">  

<input type=button value=后退 onclick="window.history.go(-1)">  

<input type=button value=前进 onclick="window.history.forward()">  

<input type=button value=后退 onclick="window.history.back()">  

方法三:用头部辅助功能标签meta来刷新页面

<head>  

<!-- 以下方式只是刷新不跳转到其他页面 -->  

<meta http-equiv="refresh" content="10">  

<!-- 以下方式定时转到其他页面 -->  

<meta http-equiv="refresh" content="5;url=hello.html">   

</head>  

Javascript刷新当前页面的几种方法：

1 history.go(0) 

2 location=location  

3 location.reload()  

4 location.replace(location)  

5 location.assign(location)  

6 document.execCommand('Refresh')  

7 window.navigate(location)  

1. document.URL=location.href

## 7.2.参数的传递

### 7.2.1.URL传参

location.href = "https://www.google.com/search?q=hello&oq=hello"

function parseURL(url){

    var url = url.split("?")[1];//或者url.search获取参数字符串

    var para = url.split("&");

    var len = para.length;

    var res = {};//json对象

    var arr = [];//数组对象

    for(var i=0;i<len;i++){

        arr = para.split("=");

        res[arr[0]] = arr[1];

    }

    return res;//将参数以键值对集合的形式返回

}

### 7.2.2.Cookie传参

a页面保存Cookie，b页面读取

<script type="text/javascript">  

/\*\*\*  

\* @param {string} cookieName Cookie名称  

\* @param {string} cookieValue Cookie值  

\* @param {number} nDays Cookie过期天数  

\*/  

function SetCookie(cookieName,cookieValue,nDays) {  

    /\*当前日期\*/  

    var today = new Date();  

    /\*Cookie过期时间\*/  

    var expire = new Date();  

    /\*如果未设置nDays参数或者nDays为0，取默认值1\*/  

    if(nDays == null || nDays == 0)

     nDays = 1;  

    /\*计算Cookie过期时间，以毫秒为单位。1秒(s)=1000毫秒(ms)\*/  

    expire.setTime(today.getTime() + 3600000 \* 24 \* nDays);  

    /\*设置Cookie值\*/  

    document.cookie = cookieName + "=" + escape(cookieValue)  

        + ";expires=" + expire.toGMTString();  

}  

function login() {  

    var username = $("user").value;  

    var password = $("pass").value;  

    /\*是否选中7天内无需登录\*/  

    var save = $("save").checked;  

    if(username=="abc" && password=="abc") {  

        if(save) SetCookie("username",username,7);  

        else SetCookie("username",username,1);  

        /\*跳转到ex8.html页面\*/  

        document.location = "b.htm";  //跳转到b页面

    } else {  

        alert("用户名或密码错误！");  

    }  

}  

</script>  

<script type="text/javascript">  

/\*\*\*  

\*读取指定的Cookie值  

\*@param {string} cookieName Cookie名称  

\*/  

function ReadCookie(cookieName) {  

    var theCookie = "" + document.cookie;  

    var ind = theCookie.indexOf(cookieName);  

    if(ind==-1 || cookieName=="") return "";  

    var ind1 = theCookie.indexOf(';',ind);  

    if(ind1==-1) ind1 = theCookie.length;  

    /\*读取Cookie值\*/  

    return unescape(theCookie.substring(ind+cookieName.length+1,ind1));  

}  

function $(id) {  

    return document.getElementById(id);  

}  

function init() {  

    var username = ReadCookie("username");  

    if(username && username.length>0) {  

        $("msg").innerHTML = "<h1>欢迎光临," + username + "!</h1>";  

    } else {  

        $("msg").innerHTML = "<a href='a.htm'>请登录</a>";  

    }  

}  

</script>  

### 7.2.3.localStorage对象传值

H5中的localStorage对象，可以将待传递的参数作为localStorage对象的内置属性传递给下个页面

a页面存储参数

localStorage.setItem("lastname","lu");

或者localStorage.lastname = "lu";

b页面获取参数

localStorage.getItem("lastname");

或者localStorage.lastname;

# 8.js中的定时器

1、setTimeout创建只执行一次的定时器,在指定的毫秒数后执行指定的代码

语法：setTimeout(expression,milliseconds)；

参数说明：expression可以是匿名函数，也可以是函数的调用

　　　　　 time: 指延迟要执行的时间（单位为毫秒）

实例：

setTimeout(function(){},1000);

setTimeout(test,1000);

setTimeout(test(str),1000);

function test (str){

console.log(str);

}

2、setInterval创建反复执行的定时器（每隔指定的毫秒数不停地执行指定的代码），用法同setTimeout(expression,milliseconds)。二者区别在于setTimeout只执行一次，但setInterval会反复执行。

3、定时器的清除

由于在创建定时器时会返回一个整形的数字表示该定时器的序号，所以定时器的清除要借助于这个返回的数字。

语法：clearTimeout(timer)和clearInterval(timer)。

实例：

var timer = setInterval(function(){

if(left>700){

clearInterval(timer);

}

left++;

oBox.style.left = left+'px';

},30);

clearInterval(timer);//清除定时器
