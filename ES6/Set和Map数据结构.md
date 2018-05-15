> # set

- 数据结构 Set它类似于数组，但是成员的值都是唯一的，没有重复的值。
- Set 本身是一个构造函数，用来生成 Set 数据结构
```
const s = new Set();

[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));

for (let i of s) {
  console.log(i);
}
// 2 3 5 4
```
上面代码通过add方法向 Set 结构加入成员，结果表明Set结构不会添加重复的值。

- Set 函数可以接受一个数组（或者具有iterable接口的其他数据结构）作为参数，用来初始化
```
function divs () {
  return [...document.querySelectorAll('div')];
}

const set = new Set(divs());
set.size // 56

// 类似于
divs().forEach(div => set.add(div));
set.size // 56
```
- 在 Set 内部，两个NaN是相等，两个对象总是不相等的

---

#### Set 实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）

### 操作方法
1. add(value)：添加某个值，返回Set结构本身
2. delete(value)：删除某个值，返回一个布尔值，表示删除是否成功
3. has(value)：返回一个布尔值，表示该值是否为Set的成员
4. clear()：清除所有成员，没有返回值
```
s.add(1).add(2).add(2);
// 注意2被加入了两次

s.size // 2

s.has(1) // true
s.has(2) // true
s.has(3) // false

s.delete(2);
s.has(2) // false
```
Array.from方法可以将 Set 结构转为数组
```
const items = new Set([1, 2, 3, 4, 5]);
const array = Array.from(items);
```

### 遍历方法
1. keys()：返回键名的遍历器
2. values()：返回键值的遍历器
3. entries()：返回键值对的遍历器
4. forEach()：使用回调函数遍历每个成员

- 由于 Set 结构没有键名，只有键值（或者说键名和键值是同一个值），所以keys方法和values方法的行为完全一致。
- Set 结构的实例默认可遍历，它的默认遍历器生成函数就是它的values方法。所以可以省略values方法，直接用for...of循环遍历 Set
```
Set.prototype[Symbol.iterator] === Set.prototype.values
// true

let set = new Set(['red', 'green', 'blue']);

for (let x of set) {
  console.log(x);
}
// red
// green
// blue
```
- Set结构的实例的forEach方法，用于对每个成员执行某种操作，没有返回值。
```
let set = new Set([1, 2, 3]);
set.forEach((value, key) => console.log(value * 2) )
// 2
// 4
// 6
```
- 扩展运算符（...）内部使用for...of循环，所以也可以用于 Set 结构。两者相结合，就可以去除数组的重复成员
```
let arr = [3, 5, 2, 2, 5, 5];
let unique = [...new Set(arr)];
// [3, 5, 2]
```
- 如果想在遍历操作中，同步改变原来的 Set 结构，有两种变通方法。一种是利用原 Set 结构映射出一个新的结构，然后赋值给原来的 Set 结构；另一种是利用Array.from方法。

---
- WeakSet 结构与 Set 类似，也是不重复的值的集合。它与 Set 有两个区别。

1. WeakSet 的成员只能是对象，而不能是其他类型的值
```
const ws = new WeakSet();
ws.add(1)
// TypeError: Invalid value used in weak set
ws.add(Symbol())
// TypeError: invalid value used in weak set
```
2. WeakSet 中的对象都是弱引用，即垃圾回收机制不考虑 WeakSet 对该对象的引用

由于 WeakSet 内部有多少个成员，取决于垃圾回收机制有没有运行，运行前后很可能成员个数是不一样的，而垃圾回收机制何时运行是不可预测的，因此 ES6 规定 WeakSet 不可遍历。

- 作为构造函数，WeakSet 可以接受一个数组或类似数组的对象作为参数。该数组的所有成员，都会自动成为 WeakSet 实例对象的成员。
```
const a = [[1, 2], [3, 4]];
const ws = new WeakSet(a);
// WeakSet {[1, 2], [3, 4]}
```
- WeakSet 结构有以下三个方法
1. WeakSet.prototype.add(value)：向 WeakSet 实例添加一个新成员
2. WeakSet.prototype.delete(value)：清除 WeakSet 实例的指定成员
3. WeakSet.prototype.has(value)：返回一个布尔值，表示某个值是否在 WeakSet 实例之中

- WeakSet没有size属性，没有办法遍历它的成员。
```
ws.size // undefined
ws.forEach // undefined

ws.forEach(function(item){ console.log('WeakSet has ' + item)})
// TypeError: undefined is not a function
```
- WeakSet 的一个用处，是储存 DOM 节点，而不用担心这些节点从文档移除时，会引发内存泄漏。

> # map

-  Map 数据结构类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键
```
const m = new Map();
const o = {p: 'Hello World'};

m.set(o, 'content')
m.get(o) // "content"

m.has(o) // true
m.delete(o) // true
m.has(o) // false
```
- Set和Map都可以用来生成新的 Map
```
const set = new Set([
  ['foo', 1],
  ['bar', 2]
]);
const m1 = new Map(set);
m1.get('foo') // 1

const m2 = new Map([['baz', 3]]);
const m3 = new Map(m2);
m3.get('baz') // 3
```
- 如果对同一个键多次赋值，后面的值将覆盖前面的值。
- 如果读取一个未知的键，则返回undefined。
- 只有对同一个对象的引用，Map 结构才将其视为同一个键。这一点要非常小心。
```
const map = new Map();

map.set(['a'], 555);
map.get(['a']) // undefined
```
- Map 的键实际上是跟内存地址绑定的，只要内存地址不一样，就视为两个键。

---
- size属性返回 Map 结构的成员总数。
- set方法设置键名key对应的键值为value，然后返回整个 Map 结构。如果key已经有值，则键值会被更新，否则就新生成该键。

set方法返回的是当前的Map对象，因此可以采用链式写法。
```
let map = new Map()
  .set(1, 'a')
  .set(2, 'b')
  .set(3, 'c');
```
- get方法读取key对应的键值，如果找不到key，返回undefined。
- has方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。
- delete方法删除某个键，返回true。如果删除失败，返回false。
- clear方法清除所有成员，没有返回值。

---
- Map 结构原生提供三个遍历器生成函数和一个遍历方法。
1. keys()：返回键名的遍历器。
2. values()：返回键值的遍历器。
3. entries()：返回所有成员的遍历器。
4. forEach()：遍历 Map 的所有成员。

Map 的遍历顺序就是插入顺序

- Map 结构转为数组结构，比较快速的方法是使用扩展运算符（...）
```
const map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

[...map.keys()]
// [1, 2, 3]
```
- 结合数组的map方法、filter方法，可以实现 Map 的遍历和过滤（Map 本身没有map和filter方法）。
```
const map0 = new Map()
  .set(1, 'a')
  .set(2, 'b')
  .set(3, 'c');

const map1 = new Map(
  [...map0].filter(([k, v]) => k < 3)
);
// 产生 Map 结构 {1 => 'a', 2 => 'b'}

const map2 = new Map(
  [...map0].map(([k, v]) => [k * 2, '_' + v])
    );
// 产生 Map 结构 {2 => '_a', 4 => '_b', 6 => '_c'}
```
- 将数组传入 Map 构造函数，就可以转为 Map
```
new Map([
  [true, 7],
  [{foo: 3}, ['abc']]
])
// Map {
//   true => 7,
//   Object {foo: 3} => ['abc']
// }
```

- 如果所有 Map 的键都是字符串，它可以转为对象。
```
function strMapToObj(strMap) {
  let obj = Object.create(null);
  for (let [k,v] of strMap) {
    obj[k] = v;
  }
  return obj;
}

const myMap = new Map()
  .set('yes', true)
  .set('no', false);
strMapToObj(myMap)
// { yes: true, no: false }
```
- 对象转为 Map
```
function objToStrMap(obj) {
  let strMap = new Map();
  for (let k of Object.keys(obj)) {
    strMap.set(k, obj[k]);
  }
  return strMap;
}

objToStrMap({yes: true, no: false})
// Map {"yes" => true, "no" => false}
```
- Map 转为 JSON
1. Map 的键名都是字符串，这时可以选择转为对象 JSON
```
function strMapToJson(strMap) {
  return JSON.stringify(strMapToObj(strMap));
}

let myMap = new Map().set('yes', true).set('no', false);
strMapToJson(myMap)
// '{"yes":true,"no":false}'
```
2. Map 的键名有非字符串，这时可以选择转为数组 JSON
```
function mapToArrayJson(map) {
  return JSON.stringify([...map]);
}

let myMap = new Map().set(true, 7).set({foo: 3}, ['abc']);
mapToArrayJson(myMap)
// '[[true,7],[{"foo":3},["abc"]]]'
```
- JSON 转为 Map
```
function jsonToStrMap(jsonStr) {
  return objToStrMap(JSON.parse(jsonStr));
}

jsonToStrMap('{"yes": true, "no": false}')
// Map {'yes' => true, 'no' => false}
```

---

- WeakMap结构与Map结构类似，也是用于生成键值对的集合,与Map的区别有两点
1. WeakMap只接受对象作为键名（null除外），不接受其他类型的值作为键名
2. WeakMap的键名所指向的对象，不计入垃圾回收机制

- WeakMap的键名所引用的对象都是弱引用，即垃圾回收机制不将该引用考虑在内
- WeakMap 弱引用的只是键名，而不是键值。键值依然是正常引用
```
const wm = new WeakMap();
let key = {};
let obj = {foo: 1};

wm.set(key, obj);
obj = null;
wm.get(key)
// Object {foo: 1}
```
- WeakMap 与 Map 在 API 上的区别主要是两个，一是没有遍历操作（即没有key()、values()和entries()方法），也没有size属性。二是无法清空，即不支持clear方法。
- WeakMap 应用的典型场合就是 DOM 节点作为键名