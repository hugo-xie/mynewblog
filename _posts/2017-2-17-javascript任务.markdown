---
layout: post
title:  "javascript中的task和microtask!"
date:   2017-02-17 20:51:48 +0800
categories: jekyll update
---
<h2>javascript中的task和microtask</h2>

<h3 style="color: green">先看一段代码</h3>
{% highlight javascript %}
    console.log('1');
    setTimeout(function(){
      console.log('2');
     },0);
    Promise.resolve().then(function(){
       console.log('3');
      });
    console.log("4");
{% endhighlight %}

这个代码最终输出的结果是：
<h3>1,4,3,2</h3>

你答对了吗？这里有两个坑，第一个就是setTimeout()的第二个参数如果是0，那么其中的函数也不会立即执行，而是会放在下一轮的event loop里面。第二个坑就是javascript里的任务分为正常任务task和微任务microtask.

<h4 style="color: lightblue">正常任务和微任务</h4>
  正常任务就是指那些不会在当前的event loop里面执行的任务，比如setTimeout()中的任务，不管它定义的执行时间是多久之后，它都是会被放在下一轮事件循环里执行的，但是微任务不一样，微任务是放在当前事件循环的末尾，也就是说在当前事件循环里的
任务都完成以后，就会去执行本轮事件循环末尾的微任务。

常见的回调任务都属于正常任务。

微任务当前只有以下两种
<div style="border-left: 5px solid lightblue;color: light" >
  <ul>
  <li>Promise</li>
  <li>nodejs中的process.nextTick</li>
</ul>
</div>


