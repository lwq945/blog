## 数组的检测方法
 - ES5的isArray()
 ```
 function is_array(arr) {
    return isArray(arr)
 }
 ```
 - constructor
 ```
 let arr1 = [1,2]
 console.log(arr1.constructor === Array)
 ```
 - instanceof
 ```
 let arr2 = [3,4]
 console.log(arr2 instanceof Array)
 ```
 - Object.prototype.toString: 返回一个表示该对象的字符串("[object type]")
 ```
 let arr3 = [5,6]
 console.log(Object.prototype.toString.call(arr3) === "[object Array]")
 ```


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
