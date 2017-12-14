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
