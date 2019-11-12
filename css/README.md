# CSS

我写css代码有时候也会觉得很痛苦：这个布局的css怎么这么难实现！我也经常会有这种感觉，一个看似简单的布局总是要琢磨半天才能实现，偶尔还会出现一些怪异的超出理解的现象。这是因为我们对css只是大概知道个形，并没有看透css的本质。


## 基本概念

### 流

* “流”又叫文档流，是css的一种基本定位和布局机制。流是html的一种抽象概念，暗喻这种排列布局方式好像水流一样自然自动。“流体布局”是html默认的布局机制，如你写的html不用css，默认自上而下（块级元素如div）从左到右（内联元素如span）堆砌的布局方式。

### 块级元素和内联元素

这个大家肯定都知道。

块级元素是指单独撑满一行的元素，如div、ul、li、table、p、h1等元素。这些元素的display值默认是block、table、list-item等。

内联元素又叫行内元素，指只占据它对应标签的边框所包含的空间的元素，这些元素如果父元素宽度足够则并排在一行显示的，如span、a、em、i、img、td等。这些元素的display值默认是inline、inline-block、inline-table、table-cell等。

实际开发中，我们经常把display计算值为inline inline-block inline-table table-cell的元素叫做内联元素，而把display计算值为block的元素叫做块级元素。*

### width: auto 和 height: auto

>width、height的默认值都是auto。

>对于块级元素，width: auto的自动撑满一行。

> 对于内联元素，width: auto则呈现出包裹性，即由子元素的宽度决定。

> 无论内联元素还是块级元素，height: auto都是呈现包裹性，即高度由子级元素撑开。但是父元素设置height: auto会导致子元素height: 100%百分比失效。

> 流体布局之下，块级元素的宽度width: auto是默认撑满父级元素的。这里的撑满并不同于width: 100%的固定宽度，而是像水一样能够根据margin不同而自适应的宽度。


### 外在盒子和内在盒子

外在盒子是决定元素排列方式的盒子，即决定盒子具有块级特性还是内联特性的盒子。外在盒子负责结构布局。

内在盒子是决定元素内部一些属性是否生效的盒子。内在盒子负责内容显示。

如 display: inline-block; 外在盒子就是inline，内在盒子就是block。外在盒子决定了元素要像内联元素一样并排在一排显示，内在盒子则决定了元素可以设置宽高、垂直方向的margin等属性。如下图

右侧的block和左侧的文字在一行排列（外在盒子inline的表现特征），同时有拥有自定义宽度111px（内在盒子block可以设置宽高）。

### css权重和超越`!important`

    // 假设下面样式都作用于同一个节点元素`span`，判断下面哪个样式会生效
    body#god div.dad span.son {width: 200px;}
    body#god span#test {width: 250px;}

css选择器权重列表如下：


![markdown](https://mmbiz.qpic.cn/mmbiz_png/C94aicOicyXpJ5GRUicD1e4gjwZTynxcW1cSNzlAuiccmSbdnHQGPoPpeb7QFJbhVvFSk5AVbMNnCfMk50s8icPPUtw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "markdown")

在css中，!important的权重相当的高，但是由于宽高会被max-width/min-width覆盖，所以!important会失效。

    width: 100px!important;
    min-width: 200px;

上面代码计算之后会被引擎解析成：

    width: 200px;

### 盒模型（盒尺寸）

元素的内在盒子是由margin box、border box、padding box、content box组成的，这四个盒子由外到内构成了盒模型。

IE模型：box-sizing: border-box  此模式下，元素的宽度计算为border+padding+content的宽度总和。

w3c标准模型）：box-sizing: content-box 此模式下，元素的宽度计算为content的宽度。

由于content-box在计算宽度的时候不包含border pading很烦人，而且又是默认值，业内一般采用以下代码重置样式：

    :root {
    box-sizing: border-box;    
    }
    * {
    box-sizing: inherit;
    }

*，:root，html 三个有什么区别

*是选择所有的元素

:root是选择文档的根元素

html是选择html文件中的元素

### 内联盒模型

内联元素是指外在盒子默认是内联盒子的元素。从表现来说，内联元素的典型特征就是可以和文字在一行显示。因此文字也是内联元素。图片、按钮、输入框、下拉框等替换元素也是内联元素。内联盒模型是指内联元素包含的几个盒子，理解记忆下面的几个概念对css的深入学习极其重要。

1. 内容区域：本质上是字符盒子。在浏览器中，文字选中状态的背景色就是内容区域。

2. 内联盒子：内联盒子就是指元素的外在盒子是内联的，会和其他内联盒子排成一行。

3. 行框盒子：由内联元素组成的每一行都是一个行框盒子。行框盒子由一个个内联盒子组成，如果换行，那就是两个行框盒子。比如一个不换行的的p标签，就存在一个行框盒子。值得注意的是，如果给元素设置display: inline-block，则创建了一个独立的行框盒子。line-height是作用在行框盒子上的，并最终决定高度。

4. 包含盒子：就是包含块。多行文字组成一个包含块。

5. 幽灵空白节点：内联元素的每个行框盒子前面有一个“空白节点”一样，这个“空白节点”不占据任何宽度，无法选中获取，但是又实实在在存在，表现就如同文本节点一样。


### 替换元素

替换元素是指内容可以替换的元素，实际上就是content box可以被替换的元素。如存在src=""属性的 img audio video iframe元素和可以输入文本的 input select textarea元素等。

所有替换元素都是内联元素，默认display属性是inline或inline-block（除了input[type="hidden"]默认display: none;）。

替换元素有自己默认的样式、尺寸（根据浏览器不同而不同），而且其vertical-align属性默认是bottom（非替换元素默认值是baseline）。

## 盒模型四大金刚

### content
对于非替换元素如div,其content就是div内部的元素。
而对于替换元素，其content就是可替换部分的内容。

CSS中的content属性主要用伪元素:before/:after中，除了做字体库和少写个div，对于一般开发来说并无卵用。

### padding
padding是四大金刚中最稳定的了，少见有什么异常。尽管如此还是有些需要注意的地方：

1. 大部分情况下我们会将元素重置为box-sizing: border-box，宽高的计算是包含了padding的，给人一种padding也是content box一部分的感觉，好像line-height属性也作用于padding上。但实际上，元素真正的内容的宽高只是content box的宽高，而line-height属性是不作用于padding的。

![markdown](https://mmbiz.qpic.cn/mmbiz_png/C94aicOicyXpJ5GRUicD1e4gjwZTynxcW1cxvwibZOYRC2CqkXM7RnskjD01vIST1ggONGbbmWicvcmKpms14ialXXjg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "markdown")

2.padding不可为负值，但是可以为百分比值。为百分比时水平和垂直方向的padding都是相对于父级元素宽度计算的。将一个div设为padding: 100%就能得到一个正方形，padding: 10% 50%可以得到一个宽高比 5:1 的矩形。

    body {
        width: 400px;
    }
    .box {
        padding: 10% 50%;
    }   

3.padding配合background-clip属性，可以制作一些特殊形状：

    /*三道杠*/
    .icon1 {
    box-sizing: border-box;
    display: inline-block;
    width: 12px;
    height: 10px;
    padding: 2px 0;
    border-top: 2px solid currentColor;
    border-bottom: 2px solid currentColor;
    background: currentColor; /*注意如果此处背景颜色属性用缩写的话，需要放到其他背景属性的前面，否则会覆盖前面的属性值（此处为background-clip）为默认值*/
    background-clip: content-box;
    }
    /*双层圆点*/
    .icon2 {
    display: inline-block;
    width: 12px;
    height: 12px;
    padding: 2px;
    border: 2px solid currentColor;
    border-radius: 50%;
    background-color: currentColor;
    background-clip: content-box;
    }

预览如下：（currentColor是css中为数不多的变量，指当前文字的颜色值，非常好用）

![markdown](https://mmbiz.qpic.cn/mmbiz_png/C94aicOicyXpJ5GRUicD1e4gjwZTynxcW1cs3NgKibp9QdMJblhX5Lvw0gpZEAbf5IxFclSBGllVfG953BTqeRePPw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "markdown")

### margin

1、作为外边距，margin属性并不会参与盒子宽度的计算，但通过设置margin为负值，却能改变元素水平方向的尺寸：
   
    <div>asdf</div>
    <style>
        div {
            margin: 0 -100px;
        }
    </style>

此时div元素的宽度是比父级元素的宽度大200px的。但是这种情况只会发生在元素是流布局的时候，即元素width是默认的auto并且可以撑满一行的时候。如果元素设定了宽度，或者元素设置了float: left / position: absolute这样的属性改变了流体布局，那么margin为负也无法改变元素的宽度了。
2、块级元素的垂直方向会发生margin合并，存在以下三种场景：

- 相邻兄弟元素之间margin合并；

- 父元素margin-top和子元素margin-top，父元素margin-bottom和子元素margin-bottom；

- 空块级元素自身的margin-top和margin-botom合并，例子如下。

要阻止margin合并，可以：1. 给元素设置 bfc；2. 设置border或padding阻隔margin；3.  用内联元素（如文字）阻隔；4. 给父元素设定高度。

3、margin的百分比值跟padding一样，垂直方向的margin和水平方向上的一样都是相对于父元素宽度计算的。

    <div class="box">
    <div></div>
    </div>
    <style>
    .box{
        overflow: hidden;
        background-color: lightblue;
    }
    .box > div{
        margin: 50%;
    }
    </style>
此时 .box 是一个宽高比 2:1 的矩形，因为空块级元素自身的垂直方向的margin发生了合并。

这里父元素设置overflow: hidden是利用 bfc 的特性阻止子元素的margin和父元素合并，换成其他 bfc 特性或者设置 1px 的 border / padding都是可以达到效果的。
4、margin: auto能在块级元素设定宽高之后自动填充剩余宽高。margin: auto自动填充触发的前提条件是元素在对应的水平或垂直方向具有自动填充特性，显然默认情况下块级元素的高度是不具备这个条件的。典型应用是块级元素水平局中的实现：
    display: block;
    width: 200px;
    margin: 0 auto;
auto的特性是，如果两侧都是auto，则两侧均分剩余宽度；如果一侧margin是固定的，另一侧是auto，则这一侧auto为剩余宽度。栗子：

![markdown](https://mmbiz.qpic.cn/mmbiz_png/C94aicOicyXpJ5GRUicD1e4gjwZTynxcW1cGdNsWRJY92SFKM8TPuJhVaP4CWUrgsoFNma12EysjqxiccmvDfY6teg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "markdown")

这个特性鲜为人知，且很实用。

除了水平方向，垂直方向的margin也能实现垂直居中，但是需要元素在垂直方向具有自动填充特性，而这个特性可以利用position实现：

    position: absolute;
    left: 0; right: 0; top: 0; bottom: 0;
    width: 200px;
    height: 200px;
    margin: auto;

### border

border主要作用是做边框。border-style属性的值有none/solid/dashed/dotted/double等，基本看名字就能猜出什么来了:

![markdown](https://mmbiz.qpic.cn/mmbiz_png/C94aicOicyXpJ5GRUicD1e4gjwZTynxcW1cDf9nX09uozY1nBicUCic6GcqziaGTnDNIB3YNTNdcCTmeN2FGaP9QpKHA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "markdown")

border-width属性的默认值是3px，是为了照顾小弟border-style: double，你懂的。值得注意的是，border-color默认是跟随字体的颜色，相当于默认设置了border-color: currentColor一样。

border另一广受欢迎的功能就是图形构建，特别是做应用广泛的三角形，其原理可看下图的

    div{
    float: left;
    margin: 20px;
    }
    div:nth-child(1){
    width: 20px;
    height: 20px;
    border: 20px solid;
    border-color: blue red orange green;
    }
    div:nth-child(2){
    width: 20px;
    height: 20px;
    border: 20px solid;
    border-color: blue transparent transparent transparent;
    }
    div:nth-child(3){
    border: 20px solid;
    border-color: blue transparent transparent transparent;
    }
    div:nth-child(4){
    border-style: solid;
    border-width: 40px 20px;
    border-color: blue transparent transparent transparent;
    }
    div:nth-child(5){
    border-style: solid;
    border-width: 40px 20px;
    border-color: blue red transparent transparent;
    }

![markdown](https://mmbiz.qpic.cn/mmbiz_png/C94aicOicyXpJ5GRUicD1e4gjwZTynxcW1cRHsuwAVan6Zg9h5FI8rvqphIgOVPv72joJxtQ0uAHp4IPBBje7oSzQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "markdown")

其实就是将其他三个边框的颜色设置透明，并把宽高设为 0 。图中4-5两个图形，是通过调整边框宽度和颜色调整三角形的形状，把最后一个图的红色改为蓝色，则是一个直角三角形了。

###  `line-height`和`vertical-align`

line-height和vertical-align是控制元素垂直对齐的两大属性，也是最难理解搞懂的属性。

在内联元素的垂直方向对齐中，基线是最为重要的概念。line-height定义的就是两基线之间的距离，vertical-align的默认值就是基线。基线的定义则是字母 x 的下边缘。

css中有个概念叫x-height，指的是小写字母 x 的高度。vertical-align: middle对齐的就是基线往上1/2      x-height高度的地方，可以理解为近似字母 x 的交叉点。


### line-height

line-height各类属性值

normal：默认值normal其实是类型为数值的变量，根据浏览器和字体'font-family'不同而不同，一般约为 1.2 。

数值和百分比：最终会被计算为带单位的值，具体计算方法就是乘以字体大小font-size。

长度值：就是100px这样带单位的值。

这几类值的继承特性不同：line-height是数值的元素的子元素继承的就是这个数值，百分比/长度值继承的都是计算后得出的带单位的值（px）。

line-height的作用：

line-height属性用于设置多行元素的空间量，如多行文本的间距。

对块级元素来说，line-height决定了行框盒子的最小高度。注意是行框盒子的最小高度，而不是块级元素的实际高度。（图中两个div行高一样，div.one 的背景色区域就是行框盒子的高度，而 div.two 的背景区域则是实际高度，其行框盒子高度和 div.one 是一样的。）

![markdown](https://mmbiz.qpic.cn/mmbiz_png/C94aicOicyXpJ5GRUicD1e4gjwZTynxcW1cuGyGrmByl6icckGCyPiaYSQHFqBdRJrLgy7d434QB0flEOaic9oIvYPBw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "markdown")

对于非替代的 inline 元素，它用于计算行框盒子的高度。此时内联元素的行框盒子的高度完全由line-height决定，不受其他任何属性的影响。

![markdown](https://mmbiz.qpic.cn/mmbiz_png/C94aicOicyXpJ5GRUicD1e4gjwZTynxcW1clibRIia0oBrjXHAvV8m5SN6clLPVXYicg9yVJwC90HpYP2GNUHVMxYQaQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "markdown")

line-height实现垂直居中的本质：行距

行距是指一行文本和相邻文本之间的距离。行距 = line-height — font-size。行距具有上下等分的机制：意思就是文字上下的行距是一样的，各占一半，这也是line-height能让内联元素垂直居中的原因。下图中字母x上下行距各占一半，共同撑起了div。

![markdown](https://mmbiz.qpic.cn/mmbiz_png/C94aicOicyXpJ5GRUicD1e4gjwZTynxcW1cLxyV5ia5DjQtjVfol1SkTezAJJ8FnNdLa5OiakHbBQSY9ibnx14nqSIVg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "markdown")

下图中和上图唯一不同之处就是多了个display: inline-block的span元素，但是此处的span元素并没有影响div元素的高度，而只是靠着vertical-align: middle属性将自身中心点对齐了字母x的交叉点实现垂直居中而已。div元素的高度仍然和上图一模一样，由字母x和行距共同撑起。此时如果删除字母x，div的高度不变，因为span元素的行框盒子前会产生幽灵空白节点，而幽灵空白节点+行高也能撑起div。

![markdown](https://mmbiz.qpic.cn/mmbiz_png/C94aicOicyXpJ5GRUicD1e4gjwZTynxcW1cFBQrcfExAib4lhKIhFtcwFguRmuVicpKXeFo6biaptahPffZEKSic7a7lQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "markdown")

内联元素的大值特性

    <div class="box">
    <span>asdf</span>
    </div>

样式1：此时 .box 高度是多少？

    .box {
    line-height: 100px;
    background: lightgreen;
    }
    .box span {
    line-height: 30px;
    }

样式2：此时 .box 高度是多少？

    .box {
    line-height: 30px;
    background: lightgreen;
    }
    .box span {
    line-height: 100px;
    }

先说结论：无论内联元素的line-height如何设置，最终父元素的高度都是数值大的那个line-height决定的。

样式1中，span元素的行框盒子前存在一个幽灵空白节点，而这个幽灵空白节点的行高是100px；样式2中，幽灵空白节点的行高是30px，但是这时span元素的行高是100px。两种情况其实一样，取大值而已。

### vertical-align

vertical-align的属性值

线类：如baseline（默认值） top middle bottom（baseline使元素的基线与父元素的基线对齐，middle使元素的中部与父元素的基线往上x-height的一半对齐。top bottom使元素及其后代元素的底部与整行或整块的底部对齐。）

文本类：text-top text-bottom（使元素的顶部与父元素的字体顶部对齐。）

上标下标：sub super（使元素的基线与父元素的下标基线对齐。）

数值：20px 2em （默认值baseline相当于数值的 0 。使元素的基线对齐到父元素的基线之上的给定长度，数值正值是基线往上偏移，负值是往下偏移，借此可以实现元素垂直方向精确对齐。）

百分比：20% （使元素的基线对齐到父元素的基线之上的给定百分比，该百分比是line-height属性的百分比。）

vertical-align 的作用前提

vertical-align属性起作用的前提必须是作用在内联元素上。 即display计算值为inline inline-block inline-table table-cell的元素。所以如果元素设置了float: left或者position: absolute，则其vertical-align属性不能生效，因为此时元素的display计算值为block了。

好基友line-height、vertical-align和第三者幽灵空白节点的爱恨情仇

有时候会遇见下面这样高度和设置不一致的情况：

![markdown](https://mmbiz.qpic.cn/mmbiz_png/C94aicOicyXpJ5GRUicD1e4gjwZTynxcW1cBVZltibFdCMAicIF23y3Ycxm4NVMYHMZRyB3f1C8GGQeiak6OnEpFg48g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "markdown")

div的实际高度比设定的行高大了，为什么呢？

内联元素的默认对齐方式是baseline，所以此时此时span元素的基线是和父元素的基线相对齐的，而此时父元素的基线在拿呢？

父元素的基线其实就是行框盒子前的幽灵空白节点的基线。把幽灵空白节点具象化为字母x可能容易理解些：

由于div行高是30px，所以字母x和span元素的高度都是30px。但是字母x的font-size较小，span元素的font-size较大，而行高一样的情况下font-size越大基线的位置越偏下，所以两者的基线不在同一水平线上。如下图左边部分：

![markdown](https://mmbiz.qpic.cn/mmbiz_jpg/C94aicOicyXpJ5GRUicD1e4gjwZTynxcW1cHTHb8N2AhoSOXrmyXib9gEdDO974icZzf5vMXPibAyw8jibxDRn1eTwGlw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "markdown")

由于内联元素默认基线对齐，所以字母x和span元素发生了位移以使基线对齐，导致div高度变大。而此时字母x的半行距比span元素的半行距大，大出的部分就是div的高度增加的部分。

display: inline-block基线的不同之处

先看例子，图中span元素设置了display: inline-block和宽高，从而撑起了父元素div的高度，但span本身并无margin属性，那为什么底部和div下边缘之间会有空隙呢？地址

![markdown](https://mmbiz.qpic.cn/mmbiz_png/C94aicOicyXpJ5GRUicD1e4gjwZTynxcW1cEZLicNqwhGNDeCvdAz406Y15xq4Qb2oS5ZHCFyIghbAxfF0XwGeOpow/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "markdown")


这就要说到inline-block的不同之处了。一个设置了display: inline-block的元素：

如果元素内部没有内联元素，则该元素基线就是该元素下边缘；

如果元素设置了overflow为hidden auto scroll，则其基线就是该元素下边缘；

如果元素内部还有内联元素，则其基线就是内部最后一行内联元素的基线。

知道了这点，那么再回到上面的例子：

原来是第三者幽灵空白节点搞的鬼。此时span的行框盒子前，还存在一个幽灵空白节点。由于span元素默认基线对齐，所以span元素的基线也就是其下边缘是和幽灵空白节点的基线对齐的。从而导致幽灵空白节点基线下面的半行距撑高了div元素，造成空隙。如下图：

![markdown](https://mmbiz.qpic.cn/mmbiz_png/C94aicOicyXpJ5GRUicD1e4gjwZTynxcW1cuu4paM0zOYIV8libry4ewmbvQXLwXM97bUrHaWWze18s6TibGr5qybvA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "markdown")

如果span元素中存在内联元素呢？


![markdown](https://mmbiz.qpic.cn/mmbiz_png/C94aicOicyXpJ5GRUicD1e4gjwZTynxcW1cZxjeoqH9r0ZDhk7xabB1r5XZ5HjFpcTkOkjkaW9nPOUic9fvnKEguUg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "markdown")

可以看到，此时span元素下边缘的空隙没了，因为此时span元素的基线是内部最后一行内联元素的基线。

值得一提的是，由于替换元素内部不可能再有别的元素，所以其基线位置永远位于下边缘。

解决问题

间隙产生本质上是由基线对齐引发的错位造成的，源头上是vertical-align和line-height共同造成的，所以要想解决这个问题，只要直接或间接改造两个属性中的一个就行了：

给元素设置块状化display: block使vertical-align属性失效；

尝试不同的vertical-align值如bottom/middle/top；

直接修改line-height值；

如果line-height为相对值如1.4，设置font-size: 0间接改变line-height。

弹框dialog

利用vertical-align实现的水平垂直居中弹框


    <div class="container">
    <div class="dialog">自适应弹出层</div>
    </div>
    <style>
    .container{
    position: fixed;
    top: 0; right: 0; bottom: 0; left: 0;
    background-color: rgba(0, 0, 0, .15);
    text-align: center;
    font-size: 0;
    white-space: nowrap;
    overflow: auto;
    }
    .container:after{
    content: '';
    display: inline-block;
    height: 100%;
    vertical-align: middle;
    }
    .dialog{
    display: inline-block;
    width: 400px;
    height: 400px;
    vertical-align: middle;
    text-align: left;
    font-size: 14px;
    white-space: normal;
    background: white;
    }
    </style>


BFC


1、浮动元素 (float不为none的元素)；

2、绝对定位元素 (元素的position为absolute或fixed)；

3、inline-blocks(元素的 display: inline-block)；

4、表格单元格(元素的display: table-cell，HTML表格单元格默认属性)；

5、overflow的值不为visible的元素；

6、弹性盒 flex boxes (元素的display: flex或inline-flex)；





流的破坏

`float`属性的特性
`clear`的作用和不足
绝对定位`position: absolute`