## 类数组对象
拥有一个 length 属性和若干索引属性的任意对象(只有索引值和长度，没有数组的方法)
```
var arrayLike = {0:'name',1:'age',2:'sex',length:3}
//类数组对象转换成数组

// 1.slice
var arrTrue = Array.prototype.slice.call(arrayLike)
console.log(arrTrue) //['name','age','sex']

//2.ES6 Array.from
var arrTrue = Array.from(arrayLike)
console.log(arrTrue) //['name','age','sex']
```
