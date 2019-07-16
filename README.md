# 你不知道的javascript（上）  
读书笔记

## this 绑定的优先级 

1.  默认绑定： 函数直接调用 `foo()`
2.  隐式绑定： `obj.foo()` 看调用的地方  可能会隐式丢失 

    ```javascript
    function foo(){
        console.log(this.a)
    }
    obj{
        a:1,
        foo:foo
    }  
    obj.foo()
    ```

- 默认绑定优先级最低
- 显示绑定大于隐式绑定
- new绑定大于硬绑定

## 判断this

1.  函数是否在new中调用，如果是的话 `this` 绑定的是新创建的对象。   

        var bar = new foo()
        var bar = new foo()
2.  call apply 显示调用，this 绑定指定的对象
3.  函数在上下文章中调用，this指向上下文对象
4.  如果都不是的话，使用默认绑定，严格模式下绑定到 undefined，否则绑定到全局对象。

###更安全的this
> apply绑定null可以展开数组
> bind(...)可以对参数柯里化
===
使用Ф建立一个安全的对象