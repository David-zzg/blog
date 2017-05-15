callee和caller这两个属性在命名上非常相似，总是后搞混，现在梳理一下。  
callee是arguments的属性，返回当前函数的引用。所以常用来做递归。

    function factorial(num){
        if(num>1){
            return num*arguments.callee(num-1)
        }else{
            return 1
        }
    }

caller用于返回调用当前函数的函数。听起来有点拗口，其实就是返回当前函数栈的上一层。

    function a(){
        console.log(a.caller)
    }
    function b(){
      a()
    }
    b()//b

即使是匿名函数调用也会返回

    (function(){
      a()
    })

但是直接访问、对象访问均会返回null。

另外，箭头函数不支持caller和callee，严格模式下为了安全也不支持，所以在开发中应该避免使用。