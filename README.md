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
> `bind(...)`可以对参数柯里化  

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

***

## 对象
为什么typeof null 返回字符串'object'    
>原理：不同得对象在底层都表示为二进制，在Javascript中二进制得前三位都为0得话会被判断为object类型，null得二进制表示是全是0，自然前三位也是0，这其实算一个bug

>javascript万物皆对象其实是错误的 ❌

>函数是一等公民✔

对象可通过两种形式定义  

1. 声明（文字）

        var myObj = {
            key: value
        }
2. 构造形式

        var myObj = new Object();
        myObj.key = value;

#### 类型
JavaScript has six main type 
- string
- number
- boolean
- null
- undefined
- object
  
> 基本简单类型(string,boolean,numbe,null)is not Object
javaScript有很多对象子类，我们称之为复杂基本类型（内置对象）
#### 内置对象
- String
- Number
- Boolean
- Object
- Function
- Array
- Date
- RegExp
- Errpr
> 他们都可以当作构造函数 （由new产生的函数调用）
检查内置对象 sub-type
    Object.prototype.toString.call(subTypeObject)

null和undefined没有对应的构造形式，Date只有构造，没有文字

#### 复制对象
- 深拷贝
  对于JSON安全的🔐的对象来说

        var newObj = JSON.parse(JSON.stringify( someObj ))
- 浅拷贝   

        Object.assign({}, myObject)

#### 属性描述符

    Object.getOwnPropertyDescriptor(myObject, 'a');
    Object.defineProperty( myObject , 'a', {
        value: 2,
        writable: true,
        configurable: true,
        enumerable: true
    })

#### 不变性
1.  对象常量    

    结合`writable:false`和`configurable:false`就可以创建一个真正的常量
    
2.  🈲️止扩展   
    
    `Object.preventExtensions(myObject)` 禁止对象添加新的属性   

3.  密封

    `Object.seal(myObject)`密封之后不能添加新的属性，并且不能配置任何现有属性。

4.  冻结

    `Object.freeze(myObject)`这个方法是你可以应用在对象上的级别最高的不可变性   
    可以循环遍历冻结

#### 存在性
`('a' in myObject);` 
> `in`操作符会检查对象以及[[Prtotype]]原型链

`myObject.hasOwnProperty('a')`
> 只检查属性是否在myObject对象中，不会检查原型链

`for in` 对应着对象的枚举属性   

判断对象的某个属性是否可以枚举    
`myObject.prertyIsEnumerable('a')`  

`Object.keys()`返回一个数组，包含所有可枚举的属性
`Object.getOwnPropertyNames()`返回一个数组，包含所有属性，不论他们是否可枚举。  

`every`()和`some()`中特殊的返回值和普通的`break`语句类似，他们会提前终止遍历。

>遍历对象的顺序是不确定的，跟javscript引擎有关

`for of` 循环   
普通对象没有内置`@@iterator`    
掉用`next`方法来遍历数据




