# 你不知道的javascript（上）🤖  
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

## 判断`this`

1.  函数是否在new中调用，如果是的话 `this` 绑定的是新创建的对象。   

        var bar = new foo()
        var bar = new foo()
2.  call apply 显示调用，this 绑定指定的对象
3.  函数在上下文章中调用，this指向上下文对象
4.  如果都不是的话，使用默认绑定，严格模式下绑定到 undefined，否则绑定到全局对象。

### 更安全的`this`
> apply绑定null可以展开数组  
> bind(...)可以对参数柯里化  

***
使用Ф建立一个安全的对象

### 软绑定
> 给默认绑定指定一个全局对象undefined以外的值，那就可以实现和硬绑定相同的效果，同时保留隐式绑定或者显示绑定修改`this`的能力

```javascript 
if(!Function.prototype.softBind) {
    Function.prototype.softBind = function (obj) {
        var fn = this;
        //捕获所有curried参数
        var curried = [].slice.call( arguments, 1);
        var bound = function() {
            return fn.apply(
                (!this || this === (window || global)) ?
                obj : this
                curried.concat.apply( curried, arguments)
            );
        };
        bound.prototype = Object.create( fn.prototype )
        return bound;
    }
}
```
### this词法
~~胖箭头~~箭头函数不使用this的四种标准规则，而是根据外层（函数或者全局）作用域来决定`this`

```javascript
function foo(){
    return (a) => {
        console.log( this. a)
    }
}

var obj1 = {
    a: 2
}

var obj2 = {
    a: 4
}

var bar = foo.call(obj1)
```
