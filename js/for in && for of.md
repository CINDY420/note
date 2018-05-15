> # for in

```
Array.prototype.method=function(){
　　console.log(this.length);
}
var myArray=[1,2,4,5,6,7]
myArray.name="数组"
for (var index in myArray) {
  console.log(myArray[index]);
}


// 1
// 2
// 4
// 5
// 6
// 7
// 数组
// function() {
//　　 console.log(this.length);
// }
```
- 使用for in会遍历数组所有的可枚举属性，包括原型。例如上面的原型方法method和name属性
- index索引为字符串型数字，不能直接进行几何运算

- 遍历顺序有可能不是按照实际数组的内部顺序
#### 所以for in更适合遍历对象，不要使用for in遍历数组。

- 用for in来遍历对象的键名

```
Object.prototype.method=function(){
　　console.log(this);
}
var myObject={
　　a:1,
　　b:2,
　　c:3
}
for (var key in myObject) {
  console.log(key);
}


// a
// b
// c
// method
```
如果不想遍历原型方法和属性的话，可以在循环内部判断一下,hasOwnPropery方法可以判断某属性是否是该对象的实例属性
```
for (var key in myObject) {
　　if(myObject.hasOwnProperty(key)){
　　　　console.log(key);
　　}
}
```


---

> # for of 

- 更简单的正确的遍历数组（即不遍历method和name）,可以使用ES6中的for of

```
Array.prototype.method=function(){
　　console.log(this.length);
}
var myArray=[1,2,4,5,6,7]
myArray.name="数组";
for (var value of myArray) {
  console.log(value);
}

// 1
// 2
// 4
// 5
// 6
// 7
```
#### for in遍历的是数组的索引（即键名），而for of遍历的是数组元素值

- 可以通过ES5的Object.keys(myObject)获取对象的实例属性组成的数组，不包括原型方法和属性
```
Object.prototype.method=function(){
　　console.log(this);
}
var myObject={
　　a:1,
　　b:2,
　　c:3
}
Object.keys(myObject).forEach(function(key,index) {
    console.log(key,myObject[key])
})

// a 1
// b 2
// c 3
```
