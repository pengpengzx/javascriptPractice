# javascriptPractice
 javascriptPractice

## this 绑定的优先级 

1. 默认绑定： 函数直接调用 foo()
2. 隐式绑定： obj.foo() 看调用的地方  可能会隐式丢失       
        function foo(){
            console.log(this.a)
        }
        obj{
            a:1,
            foo:foo
        }  
        obj.foo()

- 默认绑定优先级最低
- 显示绑定大于隐式绑定
- new绑定大于硬绑定
