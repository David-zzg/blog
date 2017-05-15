在css布局中，经常会有样式居中的需求。现在列举以下常见的布局方法。  
首先我们要明确，居中一定有一个参照物。所以会涉及到两个元素，分别是父元素和居中元素。而且父元素一定是块级元素。（父元素不可以设置宽高的话，居中就没意义了）,另外，我们只讨论父元素、居中元素的宽高不都确定的情况（比如垂直居中，父元素和居中元素的高度都知道的话，我们总可以通过指定具体的margin、padding来实现居中）  
1.水平居中  
居中元素如果是块级元素，通过改变margin的方式实现水平居中

    //细分居中元素有固定宽度和无固定宽度的情况
    //1.宽度固定，设置margin:0 auto
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
        <style type="text/css" media="screen">
            .parent{
                background: red;
            }
            .child{
                background: blue;
                width: 300px;
                height: 100px;
                margin: 0 auto;
            }
        </style>
    </head>
    <body >
        <div class="parent">
            <div class="child"></div>
        </div>
    </body>
    </html>

结果：  

![](//upload-images.jianshu.io/upload_images/2439144-c9da5bb01f67752c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
水平居中，居中块级元素宽度固定
    //2.宽度不固定，设置margin:0 <具体数值>
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
        <style type="text/css" media="screen">
            .parent{
                background: red;
            }
            .child{
                background: blue;
                height: 100px;
                margin: 0 100px;
            }
        </style>
    </head>
    <body >
        <div class="parent">
            <div class="child"></div>
        </div>
    </body>
    </html>

结果：
![](//upload-images.jianshu.io/upload_images/2439144-3ac27700c354cc06.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
水平居中，居中块级元素不固定
  
同样的，我们可以设置父元素的padding来达到居中效果。这里就不做展示了。

上面列举了居中元素是块级元素的两种情况。都是通过改变margin的数值来达到居中。原理是什么呢？我们知道盒子模型中，在水平格式中有7个属性
![](//upload-images.jianshu.io/upload_images/2439144-573566bb0ca99842.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
w3c标准盒模型
分别是，margin-left,border-left,padding-left,width,padding-right,border-right,margin-right。而且这7个数值加起来要等于元素的需要的总宽度（对于块级元素来说即父元素的内容宽度）。在这个7个属性值里面，是有默认值的，margin、width的默认值是auto，其他为0。这个auto属性使得元素会自动计算它该有的属性值。第一种情况，居中元素宽度固定，则只有margin需要计算。数值为（父content宽度-居中元素宽度-0 * 2（padding为0）-0*2（border为0））/2。因而实现了水平居中。第二种情况则相反。固定了margin值，width则自动计算为（父content宽度-(0+0+100(margin数值)*2)），从而达到居中。

上面讲述的两个例子都是块级元素，对于行内元素则不行了，因为行内元素的宽度受限于内容宽度。因而无法用margin实现居中.这种情况下，可以通过父元素设置text-align实现居中。

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
        <style type="text/css" media="screen">
            .parent{
                background: red;
                text-align:center;
            }
            .child{
                background: blue;
                color:white;
            }
        </style>
    </head>
    <body >
        <div class="parent">
            <span class="child">测试文字</span>
        </div>
    </body>
    </html>

结果：
![](//upload-images.jianshu.io/upload_images/2439144-66e5f852106628ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
水平居中，行内元素居中
如果针对行内块状元素，上述第一种和第三种方法均可适用。但是宽度auto，设置margin的方式则不行，毕竟行内块状元素不设置宽度的话，宽度则受限于内容。

2.垂直居中  
上一小节。我们提到适用margin来实现块级元素水平居中，原理则是水平格式7个属性之和要达到元素应有的宽度（块级元素应有宽度则为父元素的内容宽度）。对于垂直居中则无效了。因为正常流的块级元素magrin设置为auto时，会被计算成0.

对于单行文本的垂直居中，居中元素通常为行内元素和行内块状元素。一种常用的办法是设置height和line-height，并将两者的数值设置为相同（注意：父元素的高度一定要高于居中元素的高）。

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
        <style type="text/css" media="screen">
            .parent{
                background: red;
                position: relative;
                height: 300px;
                width: 300px;
            }
            .child{
                background: blue;
                color:white;
                margin: 0px 0px;
                position: absolute;
                top: 50%;
                transform: translateY(-50%);
            }
        </style>
    </head>
    <body >
        <div class="parent">
            <p class="child">测试文本测试文本测试文本测试文本测试文本测试文本测试文本测试文本测试文本测试文本测试文本测试文本</p>
        </div>
    </body>
    </html>

对于多行文本的垂直居中，一种常用的方法是通过相对定位和绝对定位相结合以及tansform的方式实现。
![](//upload-images.jianshu.io/upload_images/2439144-b5d945aebb7b6074.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/340)  
多行文本垂直居中
  
此方法要点是使用了top百分比属性，然后用translate来修正偏移。  
由于使用了transform属性，所以这个访问存在兼容性。需要Safari 3.1+、 Chrome 8+、Firefox 4+、Opera 10+、IE9+

另一种方式通过vertical-align实现。

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
        <style type="text/css" media="screen">
            .parent{
                background: red;
                height: 300px;
                width: 300px;
                font-size: 0px;
            }
            .child{
                background: blue;
                color:white;
                margin: 0px 0px;
                vertical-align: middle;
                display: inline-block;
                font-size:14px;
            }
            .shadow{
                display: inline-block;
                height: 100%;
                width: 0px;
                vertical-align: middle;
            }
        </style>
    </head>
    <body >
        <div class="parent">
            <span class="shadow"></span>
            <p class="child">测试文本测试文本测试文本测试文本测试文本测试文本测试文本测试文本测试文本测试文本测试文本测试文本</p>
        </div>
    </body>
    </html>
![](//upload-images.jianshu.io/upload_images/2439144-5071b495be9a14d0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/340)  
多行文本垂直居中
  
你可能注意到几个细节：  
1.有一个shadow元素，该元素是用于跟居中元素做对齐用的。  
2.shadow元素，居中元素、父元素都设置了font-size，因为在他们之间有个看不到而又实际存在的分段符，该分段符也占据了空间，设置了0font-size之后就正常了。

至此，水平居中和垂直居中就介绍的差不多了，那么垂直居中怎么实现呢？只要将以上方法结合一下就可以啦，这里就不展开篇幅说明了，只列举两个最常用的水平垂直居中例子。

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
        <style type="text/css" media="screen">
            .parent{
                background: red;
                height: 300px;
                width: 300px;
                font-size: 0px;
                text-align: center;
            }
            .child{
                background: blue;
                color:white;
                width: 100px;
                margin: 0px 0px;
                vertical-align: middle;
                display: inline-block;
                font-size:14px;
            }
            .shadow{
                display: inline-block;
                height: 100%;
                width: 0px;
                vertical-align: middle;
            }
        </style>
    </head>
    <body >
        <div class="parent">
            <span class="shadow"></span>
            <p class="child">测试文本测试文本测试文本测试文本测试文本测试文本测试文本测试文本测试文本测试文本测试文本测试文本</p>
        </div>
    </body>
    </html>

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
        <style type="text/css" media="screen">
            .parent{
                background: red;
                height: 300px;
                width: 300px;
                position: relative;
            }
            .child{
                background: blue;
                color:white;
                width: 100px;
                margin: 0px 0px;
                position: absolute;
                top: 50%;
                left: 50%;
                transform: translate(-50%,-50%);
            }
        </style>
    </head>
    <body >
        <div class="parent">
            <p class="child">测试文本测试文本测试文本测试文本测试文本测试文本测试文本测试文本测试文本测试文本测试文本测试文本</p>
        </div>
    </body>
    </html>
![](//upload-images.jianshu.io/upload_images/2439144-6ea321934c5481a2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/340)  
垂直水平居中
另外，使用flex布局能轻松实现居中，只是兼容性不太好。

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
        <style type="text/css" media="screen">
            .parent{
                background: red;
                height: 300px;
                width: 300px;
                display: flex;
                justify-content: center;
                align-items: center;
            }
            .child{
                background: blue;
                color:white;
                width: 100px;
                margin: 0px 0px;
            }
        </style>
    </head>
    <body >
        <div class="parent">
            <p class="child">测试文本测试文本测试文本测试文本测试文本测试文本测试文本测试文本测试文本测试文本测试文本测试文本</p>
        </div>
    </body>
    </html>