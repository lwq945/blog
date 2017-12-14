## 函数调用
### 在ES5里函数的调用有以下方式：
```
fn(arg1, arg2)) 
obj.child.method(arg1, arg2)
fn.call(context, arg1, arg2) 或 fn.apply(context,[arg1,arg2])
```

然而函数正常调用形式是最后一种，其他的调用方式是语法糖。
### 函数调用形式： `fn.call(context, arg1, arg2) 或 fn.apply(context,[arg1,arg2])`
## this，就是call的第一个参数 context
```
function fn(a,b){
    console.log(this)
}

fn(1, 2)
```
等价于
```
function fn(a,b){
    console.log(this)
}
fn.call(undefined,1,2) 或 fn.apply(undefined,[1,2])
```
浏览器规定：
> 如果你传的 context 是 null 或者 undefined，那么 window 对象就是默认的 context（严格模式下默认 context 是 undefined）

所以上面的this打印出来就是window

## 例子
```
var obj = {
  foo: function(){
    console.log(this)
  }
}

var bar = obj.foo
obj.foo() // this 是 obj
bar() // this 是 window
```
**代码转换**: 
 - obj.foo() 转换成 obj.foo(obj)
 - bar() 转换成 bar.call(undefined) 或者 bar.call() ,不在严格模式下"use strict",所以this就是默认的 window
 
 ## 总结
 1. this 就是你 call 一个函数时，传入的 context。
 2. 如果你的函数调用形式不是 call 形式，请按照「转换代码」将其转换为 call 形式。
