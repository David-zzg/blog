之前对javascript的浅拷贝和深拷贝只是一知半解，现在好好梳理一下～

首先我们知道，对于引用类型的变量。变量实际上储存的是变量的一个指针，它指向内存中的地址。所以在以下代码中

    var a = {
      name:'david'
    }
    var b = a

实际上a和b保存的都是指针，该指针指向内存中的变量。所以对b做的修改，a也会同时发生改变。

    b.name = 'jimmy'
    console.log(a.name)//'jimmy'

同样让初学javascript的同学困惑的例子还有一个。我们知道在js中，函数的参数是按值传递传递的。在下面这个例子中

    var value1 = "test"
    var value2 = {
      name:'david'
    }
    function a(v1,v2){
        v1 = "change"
        v2.name = "jimmy"
    }
    a(value1,value2)
    console.log(value1)//test
    console.log(value2)//{name:"jimmy"}

在执行a后，value1没有发生变化，然而value2的name发生改变了。因为在函数执行时，首先会在函数内部初始化变量v1，v2。value1是属于基本类型，所以直接复制了数值。而value2时引用类型的变量，复制操作实际上复制了指针。因而内部对v2的操作会影响到内存中的变量。因而外部函数的value2发生了改变。

现在让我们回到最初的话题，看一下什么叫浅拷贝和深拷贝。

浅拷贝只是简单的复制引用(Reference)。（甚至对象的赋值操作也可以视为是浅拷贝）

    function copy(target){
        var obj = Array.isArray(target)?[]:{}
        for(let i in target){
            obj[i] = target[i]
        }
        return obj
    }
    var david = {
        name:'david',
        info:{
            birth:1993
        }
    }
    var david_copy = copy(david)
    david_copy.info===david.info//true

（在一些例子中，浅拷贝会剔除了对象的原型方法，但我个人认为不必太深就。因为是否剔除原型方法只是浅拷贝的两种实现方式而已，完全可以看项目需求决定用哪种）

在es6中，有一个Object.assign，该方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）。其内部实现其实是浅拷贝，并剔除了目标对象的原型方法。

    var david_copy = Object.assign({},david)
    david_copy.info===david.info//true

深拷贝则更好理解了，产生了一个完全新的对象。如果说浅复制只复制一层对象的属性，而深复制则递归复制了所有层级。

关于深拷贝的实现方式大同小异，这里我提供两种种方法：  
1.利用JSON的api

    function deepCopy(target){
        return JSON.parse(JSON.stringify(target))
    }

不过这个方法有弊端：  
1.无法复制函数  
2.无法复制原型方法和属性  
第二种方法：

    function deepCopy(target){
        var isArrayOrObject = function(o){
            return Object.prototype.toString.call(o)=="[object Array]"||Object.prototype.toString.call(o)=="[object Object]"
        }
        if(!isArrayOrObject(target)){
            //不是对象或者数组直接返回
            return target
        }
        var createObj = function(o){
            return Object.prototype.toString.call(o)=="[object Array]"?[]:{}
        }
    
        var obj = createObj(target)
        var copy = function(target,container){
            for(var i in target){
                if(isArrayOrObject(target[i])){
                    container[i] = createObj(target[i])
                    copy(target[i],container[i])
                }else{
                    container[i] = target[i]
                }
            }
            return container
        }
        return copy(target,obj)
    }