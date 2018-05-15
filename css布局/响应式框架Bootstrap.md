# 响应式框架Bootstrap
- 添加代码到HTML开头来添加Bootstrap: 
```
<link rel="stylesheet" href="//cdn.bootcss.com/bootstrap/3.3.1/css/bootstrap.min.css"/>
```
- 把所有的HTML内容放在 container-fluid class属性下
- 给图片添加 img-responsive class属性使图片的宽度适配页面
- text-center class 属性使元素居中显示
-  button 元素添加 btn class 属性
-  添加btn-block class 属性到按钮使其成为块级元素
-  Bootstrap自带了一些预定义的按钮颜色如btn-primary /btn-info /btn-danger
-  class属性 col-md-* 中md 表示 medium (中等的)，* 代表一个数字，它指定了这个元素所占的列宽。通过此图表的属性设置可知，在中等大小的屏幕上(例如笔记本电脑)，元素的列宽被指定了。
-  在BootStrap中，给<img>添加 .img-responsive样式就可以实现图片响应式
-  用 span 标签来创建行内元素,可以把几个元素放在一起
-  将 Font Awesome 图标库增添至应用中,在HTML 头部增加代码：
```
<link rel="stylesheet" href="//cdn.bootcss.com/font-awesome/4.2.0/css/font-awesome.min.css"/>
```
- 可以将 Font Awesome 中的 class 属性添加到 i 元素中，把它变成一个图标
```
<i class="fa fa-thumbs-up"></i>
```
- 给表单的文本输入框增加 form-control class属性使input响应式
- 通过使用拥有 row class 属性的 div 元素和其它在它之内的具有 col-xs-* class 属性的 div 元素可以将不同元素放入同一行
- Bootstrap 有一个 class 属性叫做 well，它的作用是为设定的列创造出一种视觉上的深度感