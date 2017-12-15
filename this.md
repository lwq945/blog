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
解析: 因为数组是个特殊的对象，所以 arr 相当于 { '0': function(){}, '1': function(){}, '2': function(){}, length:3}
arr[0]相当于 `arr['0']` 相当于 `arr.0` （当然这种写法不符合规范），所以 arr[0]代码转换  arr.0.call(arr), this就是 arr
*/

fn()
/*
解析: 代码转换  `fn.call(undefined)`， 所以 fn 里面的 this 是 Window
*/
```
## 测试题
```
var name = 'hunger'
var obj = {
   name: 'valley',
   fn: function(){
       this.name = 'jirengu'  //this是 obj
       var name = 'world'
       function fn2(){
        var name = 'hello'
         console.log(this.name)  //打印出 'hunger'
        }
       fn2()
   }
}
obj.fn()
```
obj.fn() 代码转换为 obj.fn.call(obj), 执行，this是obj，name: 'valley'就变为 'jirengu'，然后fn2() 代码转换为 fn2.call(),this为undefined，默认为window，打印出 'hunger'

```
var name = 'hunger'
var obj = {
   name: 'valley',
   fn: function(){
       this.name = 'jirengu' //this 是 obj
       var name = 'world'
       function fn2(){
        var name = 'hello'
         console.log(this.name)
       }
       return fn2
   }
}
var obj2 = {
    name: 'oh my god'
}
obj2.fn = obj.fn() // obj2.fn = fn2
obj2.fn()
```
解析：obj.fn() 代码转换为 obj.fn.call(obj) ,执行完返回 fn2,所以obj2.fn = fn2, obj2.fn() 代码转换为 obj2.fn.call(obj2),this指向obj2，所以打印出'oh my god'

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
## ES6 箭头函数
```
var obj={  
    fn:function(){  
        setTimeout(function(){  
            console.log(this);  
        });  
    }  
}  
obj.fn();//window  
```
this出现在全局函数setTImeout()中的匿名函数里，并没有某个对象进行显示调用，所以this指向window对象

```
var p= {
   data:{
      flag: true
   },
   init: ()=>{
     console.log(this.data.flag)
   }
}

p.init()
```

JS 每一个 function 有自己独立的运行上下文，而箭头函数不属于普通的 function，所以没有独立的上下文。所以在箭头函数里写的 this 其实是包含该箭头函数最近的一个 function 上下文中的 this（如果没有最近的 function，就是全局）。

```
var obj={  
    num:3,  
    fn:function(){  
        setTimeout(function(){  
            console.log(this.num);  
        });  
    }  
}  
obj.fn();//undefined  
//............................................................  
var obj1={  
    num:4,  
    fn:function(){  
        setTimeout(() => {  
            console.log(this.num);  
        });  
    }  
}  
obj1.fn();//4  
```
在没有使用箭头函数的情况下，this指向了window（匿名函数，没有调用的宿主对象），而window对象并没有num属性，而在使用箭头函数的情况下，this指向对象obj1

## 多层嵌套的箭头函数
```
var obj1={  
    num:4,  
    fn:function(){  
        var f=function(){      
            console.log(this); //window,因为函数f定义后并没有对象调用，this直接绑定到最外层的window对象  
            setTimeout(() => {  
                console.log(this);//window，外层this绑定到了window,内层也相当于定义在window层（全局环境）  
            });  
        }  
        f();  
    }  
}  
obj1.fn();  
```

```
var obj1={  
    num:4,  
    fn:function(){  
        var f=() => {      
            console.log(this); //object,f()定义在obj1对象中，this就指向obj1,这就是箭头函数this指向的关键  
            setTimeout(function() {  
                console.log(this);//window，非箭头函数的情况下还是要看宿主对象是谁，如果没有被对象调用，函数体中的this就绑定的window上  
            });  
        }  
        f();  
    }  
}  
obj1.fn();  
```
1. 箭头函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
2. 箭头函数根本没有自己的this，导致**内部的this就是外层代码块的this**。正是因为它没有this，所以也就不能用作构造函数。
