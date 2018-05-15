# Basic Javascript
- JavaScript提供七种不同的data types(数据类型)，它们是undefined（未定义）, null（空）, boolean（布尔型）, string（字符串）, symbol(符号), number（数字）, and object（对象）
- .push() 接受把一个或多个参数，并把它“推”入到数组的末尾
- .pop() 函数用来“抛出”一个数组末尾的值
- .shift() 函数用来移除的是第一个元素
- .unshift() 函数用来在数组的头部添加元素
- 一个程序中有可能具有相同名称的 局部 变量 和 全局 变量。在这种情况下，局部 变量将会优先于 全局 变量
- 队列（queue）是一个抽象的数据结构，新的条目会被加到 队列 的末尾，旧的条目会从 队列 的头部被移出
- 用delete删除对象属性
- 用.hasOwnProperty(propname)方法来检查对象是否有该属性。
- JSON使用JavaScript对象的格式来存储数据,允许 数据结构 是 字符串，数字，布尔值，字符串，和 对象 的任意组合
- Math.random()用来生成一个在0(包括0)到1(不包括1)之间的随机小数
-  Math.floor() 向下取整 获得它最近的整数
-  生成在两个指定的数之间的随机数
```
Math.floor(Math.random() * (max - min + 1)) + min
```
- 正则表达式: /the/gi (/ 是正则表达式的头部,the 是我们想要匹配的模式,/ 是正则表达式的尾部,g 代表着 global(全局),i 代表着忽略大小写)
- 数字选择器类似于: /\d/g,在选择器后面添加一个加号标记(+)允许这个正则表达式匹配一个或更多数字
- 使用正则表达式选择器 \s 来选择一个字符串中的空白
- 可以用正则表达式选择器的大写版本 来转化任何匹配(例：\s 匹配任何空白字符，\S 匹配任何非空白字符)
- map 方法可以方便的迭代数组

```
var timesFour = oldArray.map(function(val){
  return val * 4;
});
```
- 数组方法 reduce 用来迭代一个数组，并且把它累积到一个值中

```
var singleVal = array.reduce(function(previousVal, currentVal) {
  return previousVal - currentVal;
}, 0);
```
- filter 方法用来迭代一个数组，并且按给出的条件过滤出符合的元素,回调函数返回 true 的项会保留在数组中，返回 false 的项会被过滤出数组

```
array = array.filter(function(val) {
  return val !== 5;
});
```
- 使用 sort 方法，你可以很容易的按字母顺序或数字顺序对数组中的元素进行排序。

```
var array = [1, 12, 21, 2];
array.sort(function(a, b) {
  return a - b;
});
```
- 使用 reverse 方法来翻转数组

```
var myArray = [1, 2, 3];
myArray.reverse();
```
- concat 方法可以用来把两个数组的内容合并到一个数组中

```
newArray = oldArray.concat(otherArray);
```
- 使用 split 方法按指定分隔符将字符串分割为数组

```
var array = string.split('s');
```
- 使用 join 方法来把数组转换成字符串

```
var veggies = ["Celery", "Radish", "Carrot", "Potato"];
var salad = veggies.join(" and ");
console.log(salad); // "Celery and Radish and Carrot and Potato"
```

- 用String.replace()方法去除字符串中的标点符号和空格
```
var p=/[^0-9a-z]/gi;
str = str.replace(p,"");
```

- String.substr() 方法返回一个字符串中从指定位置开始到指定字符数的字符。
```
str.substr(start[, length])
```
- String.slice() 方法可从已有的数组中返回选定的元素.如果是负数，那么它规定从数组尾部开始算起的位置
```
arrayObject.slice(start,end)
```

- Array.splice() 方法通过删除现有元素和/或添加新元素来更改一个数组的内容,返回由被删除的元素组成的一个数组
```
array.splice(start)
array.splice(start, deleteCount) 
array.splice(start, deleteCount, item1, item2, ...)
```
- Array.slice()方法返回一个从开始到结束（不包括结束）选择的数组的一部分浅拷贝到一个新数组对象,原始数组不会被修改
```
arrayObject.slice(start,end)
```

- indexOf() 方法返回调用  String 对象中第一次出现的指定值的索引，开始在 fromIndex进行搜索,如果未找到该值，则返回-1
```
str.indexOf(searchValue[, fromIndex])
```
- Array.filter() 方法创建一个新数组,其包含通过所提供函数实现的测试的所有元素
```
var new_array = arr.filter(callback[, thisArg])
```
- charCodeAt() 方法返回0到65535之间的整数，表示给定索引处的UTF-16代码单元
- 静态 String.fromCharCode() 方法返回使用指定的Unicode值序列创建的字符串。
