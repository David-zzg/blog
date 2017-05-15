今天在掘金上看到一篇文章，里面给出了一个试题，用广度优先查找实现一个dom结构的查询，并输出tag和类。dom结构如下：

    <div class="container">
            <section class="sidebar">
                <ul class="menu"></ul>
            </section>
            <section class="main">
                <article class="post"></article>
                <p class="copyright"></p>
            </section>
        </div>

一时手痒，赶紧写了如下的广度优先代码：

    function search(className){
        var obj = document.querySelector(className)
        //广度优先
        var searchItem = function(arr){
            if(arr.length==0)return
            var newarr = []
            for(var i=0,l=arr.length;i<l;i++){
                var item = arr[i]
                getTag(item)
                var children = item.children
                if(children&&children.length>0){
                    newarr = newarr.concat(Array.prototype.slice.call(children,0))
                }
            }
            searchItem(newarr)
        }
        //输出元素的tag和类
        function getTag(obj){
            console.log(obj.tagName+' .'+obj.className)
        }
        searchItem([obj])
    }

既然写了广度优先，顺便把深度优先写了吧。。与广度优先相比，只需重写searchItem方法即可.

    function search(className){
        var obj = document.querySelector(className)
        //深度优先
        var searchItem = function(obj){
            if(!obj)return 
            getTag(obj)
            var children = obj.children
            if(children&&children.length>0){
                for(var i=0,l=children.length;i<l;i++){
                    searchItem(children[i])
                }
            }
    
        }
        //输出元素的tag和类
        function getTag(obj){
            console.log(obj.tagName+' .'+obj.className)
        }
        searchItem(obj)
    }