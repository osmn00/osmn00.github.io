# CSS浮动面面观

04-17 / 2011

- 原文地址：http://www.alistapart.com/articles/css-floats-101/
- 堂主译文地址：http://www.osmn00.com/translation/232.html

转载请原作者及译者劳动，附带以上信息，谢谢合作~

翻译此文的目的一是回顾并梳理基础知识；二是为初学者提供一份学习文档，遥记得06年下半年刚接触CSS的时候，正经的一篇汉语版基础技术文章都很难找到，更别说书了；三是浮动的相关知识也是后续自己酝酿的一篇博文将会涉及到的知识点，所以翻译此文也是为了避免届时的新博文出现无引用文章的尴尬。

--------------------

对于所有使用HTML和CSS的设计师、开发者而言，浮动都是一款利器。不爽的是，如果对于它的工作原理不能完好理解把握的话也会导致很多烦人的问题。在过去，因为浮动经常和一些浏览器兼容性的BUG扯上关系，所以大家在应用浮动的时候都会很紧张。现在请大家都稍微放松，我会告诉你究竟浮动是如何对元素工作的，并且你会知道一旦你掌握了浮动，它会为你提供多么强大的帮助。

纸质印刷中我们随处可浮动的存在，随手拿起一张报纸，几乎都可以看到有文字围绕着图片的情况。在HTML/CSS的世界里，文字会像杂志里那样围绕在一个被设置了浮动的图片周围。图片只是我们许许多多用例中的一个，我们还可以用浮动制作一个流行的两栏布局。实际上你可以在页面里对任何元素应用浮动。学习浮动的同时配套阅读下面这篇文章：[《CSS 定位》](http://alistapart.com/articles/css-positioning-101/)，你会对任何布局技术都自信满满。相关CSS定位技术的文章，还可配套阅读堂主之前翻译的《全透视：CSS z-index 属性》一文。

## 定义

学习浮动，从定义开始。我们遵循W3C的定义：

> 浮动是指一个BOX在当前行向左或向右移动。浮动最有趣的特性是内容会围绕在它的旁边。对于一个左浮动的box来说内容会围绕在它的右边，相反则会围绕在左边。

对于浮动属性我们可以设置4个属性值：left、right、inherit以及none，每一个属性值名称本身就有非常好的解释性。举个例子，如果你对一个元素定义一个float:left;，它就会移动到其父容器最左边的边界处，同样设置float:right;会移动到其父容器最右边的边界。inherit值使得元素继承其父容器的浮动值。none是元素默认情况下的值，意味着不做任何浮动。

这里有一个类似于杂志里效果的[例子1](http://www.alistapart.com/d/css-floats-101/example_a.html)，使用了如下的标记：

    img {
         float: right;
         margin: 10px;
    } 

## 浮动是如何表现的

上面的代码虽简单却高效，你会说：“切！小孩都会”。好吧，在我们深入浮动的世界之前，先花几秒钟来回视一下这里究竟发生了什么。在网络的世界里，HTML是被一些规则约束的，特别是[文档流](https://www.w3.org/TR/CSS21/visuren.html#normal-flow)（Normal Flow，国人常译为“文档流”，其实译作“常规流”更贴合其原意。扫盲贴详见[这里](http://bbs.blueidea.com/forum.php?mod=viewthread&tid=2636904&page=1)。从习惯角度考虑，本文将延续“文档流”的译法。关于文档流的另一篇汉语文章[详见这里](http://www.swordair.com/blog/2010/08/415)）。在文档流中，块级元素（div、p、h1等）在窗口范围内沿着垂直方向首尾相接自上而下的排列。浮动元素首先按照文档流的方式排列，之后脱离文档流而按照浮动设置的方向尽可能的贴近父容器的左、右边界。换句话说，浮动的元素不再首尾相接而是在父容器给予的空间内排排而坐。对于开发网站的你，一定要记住浮动的这个重要特性。

多看几个实例。在[例子2](http://www.alistapart.com/d/css-floats-101/example_b.html)中，有三个没设置浮动的box：

    .block { 
      width: 200px;
      height: 200px;
    }

注意看它们是如何首尾相接的，这就是文档流的基本概念。还是这个例子，只不过这次我们给box添加了浮动。[例子3](http://www.alistapart.com/d/css-floats-101/example_c.html)

    .block { 
      float: left;
      width: 200px;
      height: 200px;
    }

现在box们排排坐了。好的，“排排坐”是什么意思这回是明白了，但上面提到的“在父容器给予的空间内排排而坐”又是怎么回事呢？沿用上面最后一个例子，这次把里面已有的box组复制5份，它们的父元素是文档中的body元素。注意在目前浏览器窗口尺寸情况下，浮动的box出现了换行。这是因为其父容器的空间不足以在一行内容纳下所有的子辈元素。当你调整浏览器窗口尺寸时，你会发现浮动的box们会自动的跟随调整排列。你自己来尝试一下[例子4](http://www.alistapart.com/d/css-floats-101/example_d.html)。

## 使用 clear

浮动有一个干兄弟，就是clear属性，这2个属性在某种程度上的互补能令你的堆码过程变得愉悦。你可能还记得，浮动元素首先按照文档流布局，之后脱离文档流。这意味凡是出现在浮动元素之后的那个元素在布局上的行为都会和你预期的不一致。这就是我认为的初学者会陷入浮动带来的麻烦的开始之处。来看看[例子5](http://www.alistapart.com/d/css-floats-101/example_e.html)，粉色和蓝色box是浮动的，绿色和橙色box是未浮动的。

    <div class="block pink float"></div>  
    <div class="block blue float"></div>  
    <div class="block green"></div>  
    <div class="block orange"></div>    
    
    .block {
        width: 200px;    
        height: 200px;  
    }  
    .float { float: left; }  
    .pink { background: #ee3e64; }  
    .blue { background: #44accf; }  
    .green { background: #b7d84b; }  
    .orange { background: #E2A741; }    

你认为绿色的box会怎么样？哦，等一下，它在哪？原来是在粉色的box下面，它被粉色的box覆盖住了（堂主注：因为宽度值触发了layout的缘故，所以如果你是用IE6/IE7浏览例子5的话，你会发现绿色的并未被粉色的覆盖，而是水平方向上位于蓝色box的右侧）。粉色和蓝色的box因为被设置了浮动而按照我们的意愿排列。但因为它们脱离了文档流，所以绿色和橙色的box会视它们不存在一般排列。这就是为什么我们的绿色box会被遮盖在粉色的box下面。那么我们如何能使绿色的box重见天日呢？使用clear。

clear有5个属性值可用：left、right、both、inherit以及none。设置clear:left意味着这个元素的上边界必须位于一个被设置了float: left的元素之下。clear:right和clear:both也是以此类推。inherit值使得元素继承其父容器的clear值。none是不设置clear，是元素的默认值。

下面来看[例子6](http://www.alistapart.com/d/css-floats-101/example_e2.html)，这次我们应用了clear：

    <div class="block pink float"></div>  
    <div class="block blue float"></div>  
    <div class="block green clear"></div>  
    <div class="block orange"></div>    
    
    .block {
        width: 200px;    
        height: 200px;  
    }    
    .float { float: left; }  
    .clear { clear: left; }  
    .pink { background: #ee3e64; }  
    .blue { background: #44accf; }  
    .green { background: #b7d84b; }  
    .orange { background: #E2A741; } 
   
这就等于是告诉了绿色的box，虽然粉色的box目前是脱离了文档流，但依然要将其看作是处于文档流中般的处理，要首尾相接的排在它的下面。这是一个无比强大的属性，你能看到，它帮助了我们未浮动的元素在文档流中正常地显示。正确的理解float和clear会为你的前端之路增添许多色彩。

堂主这里插一句，在[《写给大家看的CSS书》](http://book.douban.com/subject/3413615/)（第2版）中，作者及译者对clear属性进行了一番这样的描述：读者可以将clear属性理解为通过设置清除元素上外边距来填满（或补足）浮动元素旁边的空间，以便清除元素从浮动元素的下方开始布局。事实上，clear属性的原理，就是清除元素的上外边距能够能够被自动地重写并设置，从而使该元素只有可见部分会显示在浮动元素下方。因此，如果我们为清除元素设置了上外边距也将被clear声明撤销。（P112页底部）

## 基于浮动的布局

现在来谈谈布局，这是浮动异常有用之所在。我们可以用很多方法实现传统的两栏布局，这些方法中的大部分都应用到了1至2个浮动的元素。来看一个很简单的例子，一个传统的2栏布局，左侧是包含主体内容的区域，右侧是包含导航的边栏，另外上面有一个页顶，下面有一个页脚。[例子7](http://www.alistapart.com/d/css-floats-101/example_f.html)

    #container {
      width: 960px;
      margin: 0 auto;
    }
    
    #content {
      float: left;
      width: 660px;
      background: #fff;
    }
    
    #navigation {
      float: right;
      width: 300px;
      background: #eee;
    }
    
    #footer {
      clear: both;
      background: #aaa;
      padding: 10px;
    }

详细说一下，这个例子中父容器被命名为#container，它用来包含浮动的元素并为它们定界，如果没有它，我们的浮动元素就会跑到很远的浏览器窗口边界那。另外设定了#content和#navigation容器，它们都被设置了浮动，一个左浮动一个右浮动来实现2栏布局。我为它们2个设置了宽度来填满父容器。最后为#footer设置了clear:both来清除浮动好令它正常的出现在上面2个浮动元素的下方。

如果我们忘了清除浮动会发生什么呢？来看看[例子8](http://www.alistapart.com/d/css-floats-101/example_g.html)就知道了。

此时页脚出现在了边栏导航的下方，这是因为边栏的下面还有空间可以放置页脚，在文档流情况下便出现了这个情况，实际上这种显示是正确的。但这一定不是我们想要的效果。

如果你像我一样有强迫症，你会发现[例子7](http://www.alistapart.com/d/css-floats-101/example_f.html)中的2栏不等高。得到等高2栏的方法可以参考Dan Cederholm的《[人造2栏](https://alistapart.com/article/fauxcolumns)》的方法，这不在本文讨论范围之内。

堂主这里再额外的插话了：采用背景图模拟2栏等高是一种不错的方式，但这会导致增加一个额外的背景图片的HTTP请求，延缓页面的加载速度；同时也只有等到背景图被完全加载以后才会在视觉上实现“等高”，而在这之前其实还是不等高的。第二种方法是采用超大的负值下外边距+下内填充的方法来实现，这里有一篇[中文的讲的比较详细的文章](http://www.zhangxinxu.com/wordpress/2010/03/%E7%BA%AFcss%E5%AE%9E%E7%8E%B0%E4%BE%A7%E8%BE%B9%E6%A0%8F%E5%88%86%E6%A0%8F%E9%AB%98%E5%BA%A6%E8%87%AA%E5%8A%A8%E7%9B%B8%E7%AD%89/)做参考。这个方法不会增加额外的HTTP请求也无需烦闷的等待那个背景图加载完成再实现视觉等高，可谓优点多多，只需要给父容器加一个overflow:hidden。但不好的地方在于，如果恰巧你的2栏被设置了边框，那你会损失掉下边框。第三种办法是采用display:table-cell的方式将需要等高的容器模拟成表格列，我们知道表格列是默认等高的。只是此属性IE7-不支持。

## 浮动先行

目前位置我们只是看了几个不会产生头疼问题的简单例子。然而，当你使用浮动属性的时候，那里依然存在着很多陷阱。其中最令人惊讶的陷阱和CSS无关却是关乎HTML代码本身。当你把浮动元素放入HTML中时会导致很多不同的结果，见[例子9](http://www.alistapart.com/d/css-floats-101/example_h.html)

这里我们展示了一个小巧的box，它里面有一个右浮动的img同时又有一些文字围绕在img旁边。用到的CSS是很基本的：

    #container {
      width: 280px;
      margin: 0 auto;
      padding: 10px;
      background: #aaa;
      border: 1px solid #999;
    }
    
    img {
      float: right;
    }

HTML代码是：

    <div id="container">
        <img src="image.gif" />    
        <p>This is some text contained within a   
        small-ish box. I'm using it as an example  
        of how placing your floated elements in different   
        orders in your HTML can affect your layouts. For  
        example, take a look at this great photo   
        placeholder that should be sitting on the right.</p>  
    </div>   
 
还是这个例子，如果我们稍微重新排列一下某些元素的位置，会发生什么呢？见[例子10](http://www.alistapart.com/d/css-floats-101/example_i.html)，我把图片放在了段落文字的后面。

    <div id="container">   
        <p>This is some text contained within a   
        small-ish box. I'm using it as an example  
        of how placing your floated elements in different   
        orders in your HTML can affect your layouts. For  
        example, take a look at this great photo   
        placeholder that should be sitting on the right.</p>  
        <img src="image.gif" /> 
    </div> 
   
这回的结果就不是很理想了，我们的图片浮动到了右边，但却没出现在我们希望的右上角，而是出现在了段落文字的下方。更糟糕的是，它似乎是飘出了我们的#container容器。究竟是哪里出了问题？首先，对于布局能够良好的进行我发现了一条规则，就是“浮动先行”。也就是说，在HTML中，应该把浮动元素放在非浮动元素之前。其次，img似乎是飘出了我们#container容器的这种情况被称为“高度塌陷”。下面就来谈谈何谓“高度塌陷”以及如何去解决它。

## 高度塌陷

高度塌陷是指当容器内包含浮动元素，容器并未像如同这些元素不浮动般的自动扩展延伸。像上面的例子，我们的容器#container似乎是当浮动的img元素不存在般，在自身高度延伸问题上忽略了它，产生了高度塌陷问题。这不是浏览器的BUG，而属于预料中的正确显示。因为浮动元素移出了文档流，所以#container容器在计算自身高度的时候便忽略了它。Eric Meyer在《[包含浮动](http://complexspiral.com/publications/containing-floats)》一文中详细了阐述了这个现象，这是一篇很有用的笔记资料。幸运的是，我们有一大堆的办法可用来补救这种情形。如果你猜测可以使用clear属性来解决它，那我会说你上道了。

常规的解决方案中的一个便是在浮动元素之后放置一个clear元素，因为浮动元素虽然脱离了文档流，但其自身的所占据的空间还在哪里，所以后面放置的这个处于文档流内的clear元素会使父容器在计算自身高度时也把浮动元素的那部分高度也计算进去。实际的例子会更直观，看[例子11](http://www.alistapart.com/d/css-floats-101/example_j.html)，它和我们上面的例子是一样的，只是多了一行清除代码：

    <div id="container">   
        <p>This is some text contained within a   
        small-ish box. I'm using it as an example  
        of how placing your floated elements in different   
        orders in your HTML can affect your layouts. For  
        example, take a look at this great photo   
        placeholder that should be sitting on the right.</p>  
        <img src="image.gif" />
        <div style="clear: right;"></div>  
    </div>    

现在高度塌陷问题解决了，但这并不是优雅的办法，因为这会增加一个无意义的标记。所以，采用纯CSS的方式会优雅很多，现在我们就来接触其中的一种。

考虑下面的这种情况，一个容器内部有三个浮动的图片，结构代码如下：

    <div id="container">
        <img src="image.gif" />    
        <img src="image.gif" />    
        <img src="image.gif" />  
    </div>    

CSS样式代码如下：

    #container {
      width: 260px;
      margin: 0 auto;
      padding: 10px 0 10px 10px;
      background: #aaa;
      border: 1px solid #999;
    }
    
    img {
      float: left;
      margin: 0 5px 0 0;
    }

现在你一看到这种情况，就会意识到容器在计算高度时候不会把里面浮动的图片计算在内。见[例子12](http://www.alistapart.com/d/css-floats-101/example_k.html)

现在我们使用CSS的方式而非之前添加额外的HTML标记的方式来处理这个问题。这一次我们用到的技巧是overflow:hidden，请注意overflow属性其本身存在的用意并不是为了应对此种情况，并且应用overflow:hidden来解救高度塌陷还可能会导致诸如内容区被隐藏或者出现无必要的滚动条。你可以从这里的[文章1](http://www.mezzoblue.com/archives/2005/03/03/clearance/)和[文章2](http://www.quirksmode.org/blog/archives/2005/03/clearing_floats.html)处知道它是怎么回事和应该注意什么。（堂主注：另外大师级的PPK还在他的个人网站上给出了一个单独的测试讲解页面，[点击这里](https://www.quirksmode.org/css/clearing.html)可看。）下面我们采用这个属性来做一下实验：

    #container {
      overflow: hidden;
      width: 260px;
      margin: 0 auto;
      padding: 10px 0 10px 10px;
      background: #aaa;
      border: 1px solid #999;
    }

结果如[例子13](http://www.alistapart.com/d/css-floats-101/example_l.html)所示。很酷是吧？另一种麻烦较少的方法是采用CSS伪类选择器:after。还是这个例子，代码是：

    #container:after {
      content: ".";
      display: block;
      height: 0;
      clear: both;
      visibility: hidden;
    }

这里我们在容器的后面创建了一个有一点内容（这里是一个英文句号）的元素，并且通过把高度设置为0的办法把它变为不可见。你可以阅读[这篇文章](http://www.positioniseverything.net/easyclearing.html)详细来详细了解这个办法。

最后，我们上面提到过的Eric Meyer的《[包含浮动](http://complexspiral.com/publications/containing-floats/)》这篇文章介绍了第三种解决高度塌陷的办法，我们这里引用其中关键点一句话：

浮动元素本身可以清除任何因其内部子元素浮动产生的高度塌陷问题。
所以对于我们的例子，把父容器#container本身设置为浮动也能达到上面介绍的几种办法的效果。

上面介绍的几种方法都能完成同样的事情，它们都能使父容器在计算高度的时候把浮动的子元素的高度也计算在内。每一种方法都有其优点，你应该学习这些方法并在你需要的时候视情形选择使用某一种。

## 因浮动导致的其他问题

除了本文提到的高度塌陷问题之外，因浮动还会导致其他一些会令人烦躁得快速脱发的问题，如“[双倍边距问题](http://positioniseverything.net/explorer/doubled-margin.html)”和“[三像素文本慢移](http://www.positioniseverything.net/explorer/threepxtest.html)”问题。相信我，任何BUG都很容易解决，只是这不是本文要阐述的问题。

结论

浮动是一项非常有用的布局技术，了解它的工作机制和在规则内是如何表现的会为你良好的利用浮动打下坚实的基础。


