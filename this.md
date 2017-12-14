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
 - obj.foo() 转换成 obj.foo.call(obj)
 - bar() 转换成 bar.call(undefined) 或者 bar.call() ,不在严格模式下"use strict",所以this就是默认的 window
 
 ## 总结
 1. this 就是你 call 一个函数时，传入的 context。
 2. 如果你的函数调用形式不是 call 形式，请按照「转换代码」将其转换为 call 形式。
 
 [参考](https://zhuanlan.zhihu.com/p/23804247)
 
 ```
let name = '小明'
let people = {
    name: '小张',
    sayName: function(){
        console.log(this.name)
    }
}
let sayAgain = people.sayName
function sayName(){
    console.log(this.name)
}

sayName()
/*
 解析：代码转换 `sayName.call(undefined)` ，因为是非严格模式，所以 this 默认是 Window，所以这里输出全局的 name 即 "小明"
*/
people.sayName()
/*
解析: 代码转换 `people.sayName.call(people)` ，所以这里输出 `people.name` 即 "小张"
*/

sayAgain()
/*
解析: 代码转换 `sayAgain.call(undefined)` ，，因为是非严格模式，所以 this 默认是 Window，所以这里输出全局的 name 即 "小明"
*/
```

```
var arr = []
for(var i=0; i<3; i++){
    arr[i] = function(){ console.log(this) }
}
var fn = arr[0]

arr[0]
/*
解析: 因为函数是个特殊的对象，所以 arr 相当于 { '0': function(){}, '1': function(){}, '2': function(){}, length:3}
arr[0]相当于 `arr['0']` 相当于 `arr.0` （当然这种写法不符合规范），所以 arr[0]代码转换  arr.0.call(arr), this就是 arr
*/

fn()
/*
解析: 代码转换  `fn.call(undefined)`， 所以 fn 里面的 this 是 Window
*/
```

## apply、call 、bind有什么作用，什么区别？
apply 、 call 、bind三者都是用来改变函数的this对象的指向的。
## apply、call的区别？
```
fn.call(undefined,1,2) 
fn.apply(undefined,[1,2])
```
call 需要把参数按顺序传递进去，而 apply 则是把参数放在数组里。

## bind
**bind 的执行的结果返回的是绑定了一个对象的新函数**（bind()方法会创建一个新函数），称为绑定函数，当调用这个绑定函数时，绑定函数会以创建它时传入 bind()方法的第一个参数作为 this，传入 bind() 方法的第二个以及以后的参数加上绑定函数运行时本身的参数按照顺序作为原函数的参数来调用原函数。
```
var bar = function(){
    console.log(this.x);
}
var foo = {
    x:3
}
bar(); // undefined
var func = bar.bind(foo);
func(); // 3
```
func是 bind() 创建的一个绑定函数，和bar()功能一样，它被执行的时候，它的 this 会被设置成 foo ， 而不是像我们调用 bar() 时的全局作用域。

```
var app = {
    container: document.querySelector('body'),
    bind: function(){
        this.container.addEventListener('click', this.sayHello)                  //点击的时候会执行 sayHello，sayHello 里面的 this 代表 body 对象
        this.container.addEventListener('click', this.sayHello.bind(this))  //点击的时候会执行 sayHello，sayHello 里面的 this 代表 app 对象
    },
    sayHello: function(){
       console.log(this)
    }
}
app.bind()
```
