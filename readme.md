# javascript线程及与线程有关的性能优化

# 声明

这些内容都是按我自己的理解来组织和写的，可能术语什么的有些不是很严谨，所以有些概念模糊的应也专业的术语为准，<span style="color:#ccc;">这里介绍的有些“术语”并不等同于你已经知道的那些“术语”，所以不要硬套概念在这里去理解！</span>

## javascript线程

我觉得在开始描述相关问题之前，需要理解一下javascript里面的线程概念，首先需要知道：

*  javascript是单线程的，也就是说，一段代码，js执行的时候是从上往下一句一句的执行，前面的代码永远要先于后面的代码执行，如：

``` javascript
	var a = 15;
	var b = 16;
	//这里js代码在运行的时候，肯定先执行把15赋值给a的操作，再来执行把16赋值给b的操作
```

* 同步操作、异步操作

+ 按我的理解来说，javascript只是“同步”的，没有“异步”一说！只不过因为javascript代码借助了代码所在的宿主环境，由宿主来管理这些“异步”的代码，从而让javascript得以实现“异步”一说！那么宿主是怎么管理“异步代码”的呢？简单来说就是通过一种排队机制实现的！可以这样子来理解：假设当前有一段代码正在执行，而且大概需要执行20ms,当执行到10ms时候突然触发了一个点击事件，这里如果是多线程的话，那么不用等待，监听器直接触发，可是js单线程的，所以事件监听器不能执行，那怎么办呢？此时，宿主的管理作用就出来了，宿主并没有让事件监听器立即执行，而是把监听器的代码用排队的方式放在当前执行代码的后面，当当前代码在20ms之后执行完成之后，再来执行事件监听器代码！可以用一张图片把这个过程描述如下：
![图片]("https://github.com/woai30231/javascriptThreadStudy/blob/master/images/demo_1.png")

