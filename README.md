# 自动打包上传脚本

echo "拉取本地"

git pull

echo "开始打包"

ttm d

cd /Users/jinghanyu/rsuperboss-jd-message/

git pull

rm -rf /Users/jinghanyu/rsuperboss-jd-message/*

cp -R /Users/jinghanyu/superboss-jd-message/dist/* /Users/jinghanyu/rsuperboss-jd-message/

git add .

git commit -m "自动提交"

echo "开始提交..."

git push

echo "提交成功"

# Ajax原理

一、什么是Ajax
Ajax（Asynchronous JavaScript and XML的缩写）是一种异步请求数据的web开发技术，对于改善用户的体验和页面性能很有帮助。简单地说，在不需要重新刷新页面的情况下，Ajax 通过异步请求加载后台数据，并在网页上呈现出来。常见运用场景有表单验证是否登入成功、百度搜索下拉框提示和快递单号查询等等。
Ajax目的：提高用户体验，较少网络数据的传输量

![](https://user-gold-cdn.xitu.io/2018/6/10/163e8f98a78dc412?w=928&h=731&f=png&s=83195)

二、Ajax原理是什么

Ajax请求数据流程,其中最核心的依赖是浏览器提供的XMLHttpRequest对象，它使得浏览器可以发出HTTP请求与接收HTTP响应。浏览器接着做其他事情，等收到XHR返回来的数据再渲染页面。理解了Ajax的工作原理后，接下来我们探讨下如何使用Ajax。
![](https://user-gold-cdn.xitu.io/2018/6/10/163e8f98a81cf781?w=760&h=327&f=png&s=85386)


三、Ajax的使用
1.创建Ajax核心对象XMLHttpRequest(记得考虑兼容性)

    1. var xhr=null;  
    2. if (window.XMLHttpRequest)  
    3.   {// 兼容 IE7+, Firefox, Chrome, Opera, Safari  
    4.   xhr=new XMLHttpRequest();  
    5.   } else{// 兼容 IE6, IE5 
    6.     xhr=new ActiveXObject("Microsoft.XMLHTTP");  
    7.   } 

2.向服务器发送请求

    1. xhr.open(method,url,async);  
    2. send(string);//post请求时才使用字符串参数，否则不用带参数。
method：请求的类型；GET 或 POST

url：文件在服务器上的位置

async：true（异步）或 false（同步）
注意：post请求一定要设置请求头的格式内容

xhr.open("POST","test.html",true);  
xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded");  
xhr.send("fname=Henry&lname=Ford");  //post请求参数放在send里面，即请求体
3.服务器响应处理（区分同步跟异步两种情况）

responseText 获得字符串形式的响应数据。

responseXML 获得XML 形式的响应数据。

①同步处理

    1. xhr.open("GET","info.txt",false);  
    2. xhr.send();  
    3. document.getElementById("myDiv").innerHTML=xhr.responseText; //获取数据直接显示在页面上
②异步处理

相对来说比较复杂，要在请求状态改变事件中处理。

    1. xhr.onreadystatechange=function()  { 
    2.    if (xhr.readyState==4 &&xhr.status==200)  { 
    3.       document.getElementById("myDiv").innerHTML=xhr.responseText;  
    4.      }
    5.    } 
readyState

0-（未初始化）还没有调用send()方法

1-（载入）已调用send()方法，正在发送请求

2-（载入完成）send()方法执行完成，已经接收到全部响应内容

3-（交互）正在解析响应内容

4-（完成）响应内容解析完成，可以在客户端调用了

status

![](https://user-gold-cdn.xitu.io/2018/6/10/163e8fee8d04d0f9?w=575&h=239&f=png&s=43584)

③GET和POST请求数据区别

请求数据时的区别，详情见下面两张图：

![](https://user-gold-cdn.xitu.io/2018/6/10/163e8f98a82895fe?w=516&h=308&f=png&s=96921)

总而言之：

GET请求的差数直接拼接在url上面
POST请求的参数就不是放在url了，而是放在send里面，即请求体


# Ajax上传文件

上传文件通用的方法有:
    form表单上传、
    AjaxFileUpload、
    FormData


1、ajax上传文件怎么实现的

    文件上传的基本原理：
    使用html 的<input type=”file” name=”xxx”> 标签，提交form 的几个属性必须为： method=post  encType=multipart/form-data;

    method 属性必须设为post的原因是：值不是放在URL之后传递到服务器的；

    encType属性：这个属性管理的是表单的MIME编码

    几个属性详解：

            application/x-www-form-urlencoded   在发送前编码所有字符（默认）

        multipart/form-data  不对字符编码，在使用包含文件上传控件的表单时，必须使用该值；对于“multipart/form-data”类型的form表单，浏览器上传的实体内 容中的每个表单字段元素的数据之间用字段分隔界线进行分割，两个分隔界线间的内容称为一个分区，每个分区中的内容可以被看作两部分，一部分是对表单字段元 素进行描述的描述头，另外一部是表单字段元素的主体内容

        text/plain 空格转换为“+”，不对特殊字符编码

  表单文件上传，会刷新页面，很多时候，上传文件时不需要刷新页面，这就需要使用Ajax形式的文件上传

2、 AjaxFileUpload、通过(form + hidden iframe方式)
    通过IFrame技术模拟实现，异步提交。原理实际上就是利用了一个隐藏的Iframe子窗口，把提交的表单的target指向这个隐藏的窗口。在提交时，浏览器的头部还会出现载入信息，可是页面却没有不论什么刷新
    原理就是构建一个form,然后用POST的方式提交整个Form。在后台中。通过传递来的Form。得到HttpPostedFile。在获取当中的图片信息，这样就实现后台上传图片了。





表单如下：
<form id="excelForm">					
  <input type="file" name="file">					
  <input type="button" value="上传" onclick="excel()">
</form>

上传文件函数如下：


function excel(){
    var data = new FormData($('#excelForm')[0]);
    $.ajax({
	    type: "post", 
        url: '/uploadfile',
	    data: data,
        processData: false, //不序列化数据 
        contentType: false, //multipart request
        success: function (data) {
	        alert(data.status+"::"+data.info);
        },
	    error: function (data) {
	        alert(data.status+"::"+data.info);
	    }
    });
}
、


2、文件是怎么从前端到后端的，

![](https://img-blog.csdn.net/20170906100033528?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGlwaW5nYW5x/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

1、首部中Content-Type定义了分隔符boundary，以4个破折号开头
2、实体中分隔符 = "\r\n--" + 首部中分隔符boundary
3、实体首部Content_Disposition中定义了filename表示上传文件名（name=“fileName”表示input域属性），由于这里是一个input框中上传两个Excel文件，所以这里的2个实体首部中的Content_Disposition的name属性都为fileName
4、实体首部Content-Type描述了上传文件的类型，这里表示Sheet工作表，即Excel
5、实体首部中多行是以CRLF分隔的，即\r\n
6、实体中可能存在多部分，每一部分之间以分隔符boundary分隔，每个部分的实体首部与主体之间以CRLFCRLF分隔，即\r\n\r\n
7、符号 - 标志 - 十六进制 - 字节码 ， 回车： \r - CR - 0d - 13, 换行：\n - LF - oa - 10

![](https://img-blog.csdn.net/20170906111136804?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGlwaW5nYW5x/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)


http 请求其实是通过Tcp的传输管道

1.TCP为HTTP提供了一条可靠的传输管道。从TCP一端填入的字节会从另一端以原有的顺序、正确的传送出来，TCP提供了：

无差错的数据传输
按序传输 - 数据总是按照发送的顺序到达
未分段的数据流 - 可以在任何时刻以任意尺寸将数据发送出去
2.HTTP要传送一条报文时，会以流的形式将报文数据的内容通过一条打开的TCP连接按序传输，TCP收到数据流后，会将数据流砍成被称作段的小数据块，并将段封装在IP分组中，通过因特网传输。所有这些工作都是由TCP/IP软件完成的，HTTP程序员什么也看不见

3.只要建立了TCP连接，客户端与服务器之间的报文交换就不会丢失，不会被破坏，也不会在接收时出现错序了。



# 关于跨域
        jsonp解决跨域
        CORS
    
1、 JSONP
    解决方案: JSONP JSON with Padding(填充式JSON)是一种使用JSON数据的方法，用于解决浏览器xhr跨域请求限制

    jsonp思路:

    发起异步请求时不使用'ajax对象xhr'，而是使用一个动态创建的 script 标签代替xhr:  

    <script async src="跨域访问x.php"></script>        //async:异步

    步骤:
        ① 客户端:方法1:  创建标签→设置属性→追加在head中
        
        btn.onclick = function(){  

            var script=document.createElement("script");  

            script.src="http://127.0.0.1/kuayu/async.php"; 

            script.async=true;                    //异步请求属性 

            document.head.appendChild(script);    //创建一个script标签，并追加到head中
            
            }
            
            或者:
            
            <script>
                function doResponse(data){  
                    console.log(jsondata);    //处理获得的json数据
                }
            </script>
            
            <script src="http://127.0.0.1:8090?doResponse=?"></script>
            
            方法2:  
            
            使用dataType:"jsonp"//jsonp只能是 'GET' 形式，不要写POST，可能会报错        
            
            jQuery封装好的方法1，会在head开头插入script标签,响应成功后移除
            
            $.ajax({
                url:'http://127.0.0.1/web1703/day08/06_jsonp.php',
                dataType:"jsonp",//使用JSONP形式调用函数，标志跨域请求
                jsonp:'jsonp_callback',//跨域函数名的键值(服务器提取函数名的钥匙，默认为callback)
                jsonpCallback:'doRespons',//客户端与服务端约定的函数名，取代jQuery自动生成的随机函数名     
                                         //若服务器已设置jsonp属性，则不需要再设置此属性(此值用来替代GET或POST请求中URL参数里的"callback=?"部分，并传给服务器)  
                success:function(res){},    //若设置jsonpCallback属性，则执行success函数，不执行error函数  
                                            //否则相反  
                error:function(err){}});
                
                ② 创建函数，接收参数(设置jsonpCallback后可省略)
                function doResponse(res){     
                    //创建函数，用来接收服务器响应数据data  
                    console.log(res);
                }
                
                ③ 服务器:
                
                <?phpheader("Content-Type:application/javascript;charset=utf-8");    //向客户端发送js程序
                
                $json='{"ename":"tom"}';
                
                echo 'doResponse('.$json.')';        //拼接json字符串，js函数收到的参数就是这个json字符串
                ?>                                      //客户端接收:doResponse(json),会执行这个函数node中res.end("callback("+str+")");    //发送的函数名要与ajax的jsonpCallback的值一致                                 //这个函数callback可不写，会自动接着执行ajax的success函数jQuery中的jsonp类型，会创建一个查询字符串参数callback=?，这个参数会加在请求的URL后面，服务器端应当在JSON数据前加上回调函数名，以便完成一个有效的JSONP请求如果要指定回调函数的参数名来取代默认的callback，可以通过设置$.ajax()的jsonp参数当从服务器接收到数据时，实际上是用了&lt;script&gt;标签而不是XMLHttpRequest对象，$.ajax()不再返回一个XMLHttpRequest对象，并且也不会传递事件处理函数，比如beforeSend


2、 CORS

    

扩展、fetch

### End
