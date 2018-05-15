# this 是如何创建的?
每调用一次 JavaScript 函数时，都会创建一个新的对象，该对象中还有一个重要的属性是 this 引用, 函数是哪个对象的方法，this 就会自动绑定到该对象
```
var car = {
  brand: "Nissan",
  getBrand: function(){
    console.log(this.brand);
  }
};

car.getBrand();
// output: Nissan
```
在这个例子中, 使用的是 this.brand, 这是 car 对象的引用。所以此时, this.brand 等价于 car.brand 
# this 指向的是谁
在所有 function 中, this 的值都是根据上下文来决定的。this 的作用域 与函数定义的位置没有关系, 而是取决于函数在哪里被调用.
this 指向的对象在每次进入新的执行上下文后是固定的, 直到跳转到另一个不同的上下文才发生改变。决定执行上下文(以及 this 的绑定)需要我们去找出调用点, 调用点即函数在代码中调用的位置。

```
ar brand = 'Nissan';
var myCar = {brand: 'Honda'};

var getBrand = function() {
  console.log(this.brand);
};

myCar.getBrand = getBrand;
myCar.getBrand();
// output: Honda

getBrand();
// output: Nissan
```
虽然 myCar.getBrand() 和 getBrand() 指向的是同一个函数, 但其中的 this 是不同的,因为它取决于 getBrand() 在哪个上下文中调用,第一次函数调用对应的是 myCar 对象, 而第二次对应的是 window

# 调用上下文 
1. 直接调用一个独立的函数
```
function simpleCall(){
  console.log(this);
}

simpleCall();
// output: the Window object
```
this值没有被 call 设置。因为代码不是运行在严格模式下, this 又必须是一个对象, 所以他的值默认为全局对象。
如果是在严格模式(strict mode)下, 进入执行上下文时设置为什么值那就是什么值。如果没有指定, 那么就一直是undefined

2. 在对象的方法中使用 this 
```
var message = {
  content: "I'm a JavaScript Ninja!",
  showContent: function() {
    console.log(this.content);
  }
};

message.showContent();   
// output: I'm a JavaScript Ninja!
```

将函数保存为对象的属性, 这样就转化为一个方法, 可以通过对象调用这个方法。当函数被当成对象的方法来调用时, 里面的 this 值就被设置为调用方法的对象。

3. 在构造函数中使用 this
```
function Message(content){
  this.content = content;
  this.showContent = function(){
    console.log(this.content);
  };
}

var message = new Message("I'm JavaScript Ninja!");

message.showContent();
// output: I'm JavaScript Ninja!
```
可以通过 new 操作符来调用函数。在这种情况下,这个函数就变成了构造函数,构造函数调用会传入一个全新的对象作为 this 的值, 并且隐式地返回新构造的这个对象作为结果.如果没使用 new 关键字, 那么他就只是一个普通的函数, this 将指向 window 对象。

4. apply() & call()
 我们可以通过这两个方法来改变函数的上下文, 在任何时候都有效, 用来显式地设置 this 的值

# 在拆出来的方法中使用 this
最常见的==错误==,就是将对象的方法(method)赋值给一个变量, 并认为 function 中的 this 仍然指向原来那个对象

```
var car = {
  brand: "Nissan",
  getBrand: function(){
    console.log(this.brand);
  }
};

var getCarBrand = car.getBrand;

getCarBrand();   // output: undefined
```
看起来 getCarBrand 似乎引用的是 car.getBrand(), 但实际上他指向的仅仅是 getBrand() 自身。上面的代码中,调用点是 getCarBrand(), 这只是一个普通的函数调用, 它不再是 car 对象的方法在这种情况下, this.brand 实际上会被转换为 window.brand

如果我们从对象中拆取出某个方法, 那么这个方法就会变成了一个普通的函数。他和原来对象的联系被切断了, 所以不再按照原来的方式运行.如果还想指向原来的那个对象的话,就需要在赋值给 getCarBrand 的时候显式地将 getBrand() 函数绑定到car对象
```
var getCarBrand = car.getBrand.bind(car);
getCarBrand();   // output: Nissan
```

# 在回调函数中使用 this

```
var car = {
  brand: "Nissan",
  getBrand: function(){
    console.log(this.brand);
  }
};

var el = document.getElementById("btn");
el.addEventListener("click", car.getBrand);
```
虽然使用的是 car.getBrand, 但实际上只是将 getBrand() 函数关联到 button 对象.将参数传递给一个函数是一种隐式的赋值, 所以此处和上一个示例基本上是一样的。区别在于此时 car.getBrand 没有显式地赋值。结果是一样的,得到的只是一个普通函数,绑定的对象是 button

# 在闭包(Closure)之中使用 this
```
var car = {
  brand: "Nissan",
  getBrand: function(){
    var closure = function(){
      console.log(this.brand);
    };
    return closure();
  }
};

car.getBrand();   // output: undefined
```
闭包函数(即内部函数)不能访问到外部函数的 this 变量。所以最终的结果就是 this.brand 等价于 window.brand, 因为内部函数中的 this 绑定的是全局对象。

针对这种问题, 我们可以把 this 绑定给 getBrand() 函数。

```
var car = {
  brand: "Nissan",
  getBrand: function(){
    var closure = function(){
      console.log(this.brand);
    }.bind(this);// 注意这里
    return closure();
  }
};

car.getBrand();   // output: Nissan
```

# 箭头函数
与一般的函数不一样, 箭头函数从其所处的封闭范围中取得 this值 。箭头函数的语法绑定不会被覆盖, 即使使用了 new 操作符。

```
var car = {
  brand: "Nissan",
  getBrand: function(){
    // 箭头函数保留了语法上的 "this" 作用域.
    var closure = () => {   
      console.log(this.brand);
    };
    return closure();
  }
};

car.getBrand();   // output: Nissan
```

---
# 四种绑定

想要知道函数在执行过程中是如何绑定this的，首先要知道函数的调用位置。
```
function baz() {  
    //当前调用栈：baz  
   //调用位置是全局作用域  
    console.log("baz");  
    bar();//bar的调用位置  
}  
function bar() {  
    //当前调用栈：baz -> bar  
   //调用位置是baz中  
    console.log("bar");  
    foo();//foo的调用位置  
}  
function foo() {  
    //当前调用栈：baz -> bar -> foo  
   //调用位置是bar中  
    console.log("foo");  
}  
baz();//baz的调用位置  
```
执行foo函数是的调用栈为baz -> bar -> foo 。

通过函数的互相调用找出调用栈，便可以找出函数的真正调用位置:
![imamge](http://images.cnitblog.com/blog2015/628067/201505/101951434546534.png)
上图右侧向上的箭头所滑过的就是foo函数执行的调用栈，栈中的第二个元素就是真正的调用位置。

### this的绑定符合的绑定规则：

#### 1.隐式绑定： 

看调用位置是否有上下文对象，即是否被某个对象拥有或包含（这里说的包含可不是用花括号包起来呦）。
```
var a = 3;  
function foo() {  
    console.log(this.a);  
}  
var obj = {  
    a: 2,  
    foo: foo  
};  
obj.foo(); 
```
foo函数的调用位置是obj的上下文来引用函数，也就是说他的落脚点是obj对象，那他的this自然也就指向他的上下文对象。

（ps：如果对象属性的引用是多层的，只有最后一层会影响调用位置）。

***
#### 2.显式绑定： 

   上面的隐式绑定是通过调用一个对象中绑定了函数的属性来把this隐式的绑定在这个对象上的。所以显示绑定就是使用一些方法强制将函数绑定在某个对象上，这些方法就是 call(...)、apply(...) 这两种方法的工作方式是类似的。
   
   这两个方法第一个参数是一个对象，而被绑定的函数的this就会绑定在这个对象上。
```
   function foo() {  
    console.log(this.a);  
}  
var obj = {  
    a: 2  
};  
foo.call(obj); 
```

***
#### 3.new 绑定
```
function Foo(a) {  
   this.a = a;  
}  
var bar = new Foo(2);  
console.log(bar.a); 
```
使用 new 来调用函数，即发生构造函数调用时，会执行下面操作

- 创建一个全新的对象
- 这个对象会被执行[__proto__]的链接(bar.__proto__=Foo.prototype)
- 这个新对象会绑定到函数调用的this上
- 如果函数没有返回其他对象，那么new表达式中的函数调用会自动返回这个新对象

***
#### 4.默认绑定   

默认绑定就是不符合上面任何一种规则是的默认规则
```
function foo() {  
    console.log(this.a);  
}  
var a = 2;  
foo();  
```
 上面这段代码foo函数不带任何修饰的直接使用，不符合1-3中的任何一种绑定规则，所以只能使用默认绑定规则，而默认绑定规则就是在非严格模式下被绑定在全局对象上（严格模式下与foo函数的调用位置无关为undefined）。

#### 隐式绑定丢失上下文
思考：
//第一种写法
```
var a = 3;  
function foo() {  
    console.log(this.a);  
}  
var obj = {  
    a: 2,  
    foo: foo  
};  
var bar = obj.foo;  
bar();  
```
//第二种写法
```
var a = 3;  
function foo() {  
    console.log(this.a);  
}  
var obj = {  
    a: 2,  
    foo: foo  
};  
obj.foo(); 
```

由于在 javascript 中，函数是对象，对象之间是引用传递，而不是值传递。因此， bar = obj.foo = say ，也就是bar = foo ，obj.foo 只是起了一个桥梁的作用，bar 最终引用的是 foo 函数的地址，而与 obj 这个对象无关了。这就是所谓的”丢失上下文“。最终执行 bar 函数，只不过简单的执行了foo函数，输出3。

#### 四种绑定的优先级
默认绑定的优先级是最低的。这是因为只有在无法应用其他this绑定规则的情况下，才会调用默认绑定。那隐式绑定和显式绑定呢？
```
function speak() {
	console.log(this.name)
}
var obj1 = {
	name: 'obj1',
	speak: speak
}
var obj2 = {
	name: 'obj2'
}
obj1.speak() // obj1 (1)
obj1.speak.call(obj2) // obj2 (2)
```
在上面代码中，执行了obj1.speak(),speak函数内部的this指向了obj1，因此(1)处代码输出的当然就是obj1，但是当显式绑定了speak函数内的this到obj2上，输出结果就变成了obj2，所有从这个结果可以看出显式绑定的优先级是要高于隐式绑定的。事实上我们可以这么理解obj1.speak.call(obj2)这行代码，obj1.speak只是间接获得了speak函数的引用，这就有点像前面所说的隐式绑定丢失了上下文。好，既然显式绑定的优先级要高于隐式绑定，那么接下来再来比较一下new 绑定和显式绑定。
```
function foo(something) {
	this.a = something
}
var obj1 = {}
var bar = foo.bind(obj1)  // 返回一个新函数bar，这个新函数内的this指向了obj1  (1)
bar(2) // this绑定在了Obj1上，所以obj1.a === 2
console.log(obj1.a)
var baz = new bar(3)  // 调用new 操作符后，bar函数的this指向了返回的新实例baz  (2)
console.log(obj1.a)
console.log(baz.a)
```
在(1)处，bar函数内部的this原本指向的是obj1，但是在(2)处，由于经过了new操作符调用，bar函数内部的this却重新指向了返回的实例，这就可以说明new 绑定的优先级是要高于显式绑定的。  
至此，四种绑定规则的优先级排序就已经得出了,分别是:

>new 绑定 > 显式绑定 > 隐式绑定 > 默认绑定
#### 箭头函数的绑定
箭头函数的this是根据外层的（函数或者全局）作用域来决定的。函数体内的this对象指的是定义时所在的对象，而不是之前介绍的调用时绑定的对象。
```
// 定义一个构造函数
function Person(name,age) {
	this.name = name
	this.age = age 
	this.speak = function (){
		console.log(this.name)
		// 普通函数（非箭头函数),this绑定在调用时的作用域
	}
	this.bornYear = () => {
		// 本文写于2016年，因此new Date().getFullYear()得到的是2016
		// 箭头函数，this绑定在实例内部
		console.log(new Date().getFullYear() - this.age)
		}
	}
}
var zxt = new Person("zxt",22)
zxt.speak() // "zxt"
zxt.bornYear() // 1994
// 到这里应该大家应该都没什么问题
var xiaoMing = {
	name: "xiaoming",
	age: 18  // 小明永远18岁
}
zxt.speak.call(xiaoMing)
// "xiaoming" this绑定的是xiaoMing这个对象
zxt.bornYear.call(xiaoMing)
// 1994 而不是 1998,这是因为this永远绑定的是zxt这个实例
```
箭头函数的 this 强制性的绑定在了箭头函数定义时所在的作用域，而且无法通过显示绑定，如apply,call方法来修改。
