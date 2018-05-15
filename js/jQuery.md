# jQuery
- 选择器选择h1:
```
jQuery("h1")/$("h1");
```

- 更改h1中的内容:
```
$("h1").text("......");
```

- 在用jQuery对DOM进行操作时,需要确定DOM在加载完后之后再和HTML交互
- 保证在DOM加载完之后jQuery才开始运行
```
jQuery(document).ready(function() {
    <code>
});
```
- css与jQuery中对dom的选择: 
```
#container{..} = $("#container");
.articles{...} = $(".articles");
```
- 选择id为destinations的ul下的所有li

```
$("#destinations li");
```

- 选择id为destinations的ul下的直属子元素

```
$("#destinations > li");
```
- 选择多个元素
```
$(".promo, #france");
```
- 选择id为destinations的ul下的第一个li,使用伪选择器

```
$("#destinations li:first");
```
  选择奇数位/偶数位的列表元素
```
$("#destinations li:odd");/$("#destinations li:even);
```
- 用遍历的方法选择id为destinations的ul下的所有li(速度更快)

```
$("#destinations").find("li");
```
遍历查找第一个li
```
$("li").first();
```
第一个的下一个li
```
$("li").first().next();
```
第一个的下一个的上一个li
```
$("li").first().next().prev();
```
向上遍历查找父元素

```
$("li").first().parent();
```
向下遍历查找(直属)子元素
```
$("#destinations").children("li");
```
- 创建一个p元素节点并赋值给price变量
```
var price = $('<p>From $399.99</p>');
```
- 把新节点加到DOM中
1. before()/insertBefore():把节点加到类名为vacation的节点前
```
price.insertBefore($('.vacation'));
```
2. after()/insertAfter():把节点加到类名为vacation的节点后前
```
$('.vacation').after(price);
price.insertAfter($('.vacation'));
```
3. prepend()/prependTo():把节点加到类名为vacation的第一个子节点处

```
$('.vacation').prepend(price);
price.prependTo($('.vacation'));
```

4. append()/appendTo():把节点加到类名为vacation的最后一个子节点处
```
$('.vacation').append(price);
price.appendTo($('.vacation'));
```
- 把button节点删除
```
$('button').remove();
```
- 点击按钮发生事件
```
$('button'). on('click', function() {
    
});
```
- closest()当它为某个类查找父节点时只会查找0或1,而parents()则会查找这个类的所有父节点
- parent()是当前元素的父元素,parents()则是当前元素的祖先元素
- 当使用关键字this的时候指的是触发事件的这个元素 $(this)
- data-xxx属性可以加到任何元素上来为元素提供额外的信息,使用data(<name>)来读取属性的值,使用data(<name>, <value>)来设置属性的值
- 事件委托
```
$('.vacation'). on('click', 'button', function() {
    
});
```
- 用filter筛选出特定的类
- 用addClass添加类,用removeClass来删除类,用toggleClass() 对设置或移除被选元素的一个或多个类进行切换
- '.slideDown()'用来显示,'.slideUp()'用来隐藏,'.slideToggle()'用来在两种状态之间轮流触发
- mouseenter事件:当鼠标进入了元素区,就能触发
- mousemove事件:当鼠标指针在指定的元素中移动时，就会发生 
- mousedown事件:当鼠标指针移动到元素上方并按下鼠标按键时会发生
- mouseover事件:当鼠标指针位于元素上方时会发生
- mouseup事件:当在元素上放松鼠标按钮时会发生
- mouseout事件:当鼠标指针从元素上移开时发生
- mouseleave事件:当鼠标指针离开元素时会发生,与 mouseout 事件不同，只有在鼠标指针离开被选元素时,才会触发 mouseleave 事件;如果鼠标指针离开任何子元素，同样会触发 mouseout 事件
- 当双击元素时会发生 dblclick 事件
- 当元素(或在其内的任意元素)获得焦点时发生 focusin 事件;当元素(或在其内的任意元素)失去焦点时发生 focusout 事件
- 当按钮被松开时发生 keyup 事件;keypress 事件与 keydown 事件类似,当按钮被按下时会发生该事件,不过与 keydown 事件不同,每插入一个字符就会发生 keypress 事件
- 在表达式前加一个+号把字符串转换成数字
- 可以用val设置值,也可以用".val"来获取值.该方法大多用于 input 元素
- fadeIn() 方法使用淡入效果来显示被选元素;fadeOut() 方法使用淡出效果来隐藏被选元素; fadeToggle() 方法可以在 fadeIn() 与 fadeOut() 方法之间进行切换
- event.stopPropagation():当事件发生并尝试冒泡时,其可以阻止它冒泡.但 并不能防止浏览器的默认行为
- event.preventDefault():阻止浏览器的默认行为
- 可以通过指定css的属性和值来设置css: .css(<attr>, <value>); 也可以通过指定css属性来获取当前的值: .css(<attr>); 也可以传入一个对象来实现: .css(<object>)
- show():如果被选元素已被隐藏则显示这些元素; hide():如果被选的元素已被显示则隐藏该元素
- 动画效果:.animate(<object>),可以直接为其提供css样式;加入第二个参数可以改变动画速度
- hasClass():查看某个节点是否包含了一个特别的类
- .prop()的方法让你来调整元素的属性
- .html()方法可以添加HTML标签和文字到元素，而元素之前的内容都会被方法的内容所替换掉

```
例:通过em[emphasize]标签来重写和强调标题文本
$("h3").html("<em>jQuery Playground</em>");
```
- appendTo()方法可以把选中的元素加到其他元素中

```
例:让target4从我们的从right-well移到left-well,
$("target4").appendTo("#left-well");
```
- clone()方法可以拷贝元素
```
把target2从left-well拷贝到right-well
$("target2").clone().appendTo("#right-well");
```
- jQuery用CSS选择器来选取元素,target:nth-child(n) CSS选择器允许你按照索引顺序(从1开始)选择目标元素的所有子元素
- 获取class为target且索引为奇数的所有元素，并给他们添加class(jQuery里的索引是从0开始的)
```
$(".target:odd").addClass("animated shake");
```
- 将class为message 的元素的文本改为：“Here is the message”

```
$(".massage").html("Here is the message");
```
- 当需要根据服务器返回的数据来动态改变页面的时候,应用程序接口(API)就派上用场了.许多网站的应用程序接口(API)都是通过一种称为JSON格式的数据来传输的

- 使用.forEach()函数来循环遍历JSON数据写到htmll变量中.首先我们定义一个HTML变量,然后使用函数来循环遍历JSON数据写到html变量中，最后把html变量显示到我们的页面中

```
var html = "";
json.forEach(function(val) {
  var keys = Object.keys(val);
  html += "<div class = 'cat'>";
  keys.forEach(function(key) {
    html += "<b>" + key + "</b>: " + val[key] + "<br>";
  });
  html += "</div><br>";
});
```
- 把其中 "id" 键的值为1的图片过滤掉
```
json = json.filter(function(val) {
  return (val.id !== 1);
});
```
- 可以通过浏览器navigator获得我们当前所在的位置geolocation

```
if (navigator.geolocation) {
  navigator.geolocation.getCurrentPosition(function(position) {
    $("#data").html("latitude: " + position.coords.latitude + "<br>longitude: " + position.coords.longitude);
  });
}
```


