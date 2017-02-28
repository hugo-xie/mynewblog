---
layout: post
title:  "Generator函数"
date:   2017-02-17 20:51:48 +0800
categories: jekyll update
---
<h2>javascript中的Generator函数</h2>
Generator函数是ES6提出的一个新的函数方式，它不同于普通的函数:
<h3>首先</h3>
它的声明方式和普通函数不同，它需要在function关键字和函数名之间有一个星号；
<h3>其次</h3>
函数体内部使用yield语句，yield语句只能用在generator函数中，用在普通函数中会报错;Generator函数调用方法和普通函数一样，但是如果generator函数中包含yield语句的话，那么Generator函数不会立刻执行完，而是会在遇到yield语句后停止运行，这是如果需要继续执行，则需要调用next方法。


yield语句和return语句有些相似，都能返回紧跟在yield语句后面的那个表达式的值,比如说:
<div style="background-color: rgb(238,243,240);border:2px dotted rgb(34,195,170);border-radius: 8px;color: green;">
{% highlight javascript %}
function* helloworldgenerator(){
  yield function test(){
    console.log('函数已经执行')；
    return "test";
  };
  yield 'world';
  return 'ending';
}
let hw = helloworldgenerator();
console.log(hw.next());
{% endhighlight %}
</div>
上面的代码会输出
<h3>{ value: [Function: test], done: false }</h3>
可以看到后面的function并没有执行，但是，如果改成下面的代码
<div style="background-color: rgb(238,243,240);border:2px dotted rgb(34,195,170);border-radius: 8px;color: green;">
{% highlight javascript %}
function* helloworldgenerator(){
  yield function test(){
    console.log('函数已经执行')；
    return "test";
  }();
  yield 'world';
  return 'ending';
}
let hw = helloworldgenerator();
console.log(hw.next());
{% endhighlight %}
</div>
结果是
<h3>{ value: 'test', done: false }</h3>
可以看到yield会执行函数，并把函数的范围值作为value.

yield语句本身并没有返回值，或者说返回的是undefiend。但是next方法可以带一个参数，该参数就会被当作上一个yield语句的返回值。比如
<div style="background-color: rgb(238,243,240);border:2px dotted rgb(34,195,170);border-radius: 8px;color: green;">
  {% highlight javascript %}
    function* f(){
    for(let i = 0;true;i++){
    let reset = yield i;
    if(reset){ i = -1;}
  }
  }
  let g = f();
  console.log(g.next());
  console.log(g.next());
  console.log(g.next(true));
{% endhighlight %}
</div>

这段代码的输出是
{ value: 0, done: false }
{ value: 1, done: false }
{ value: 0, done: false }
有的时候需要在一个Generator函数中去调用另一个Generator函数，这个时候直接调用是没有效果的，这个时候句需要yield*语句,比如
<div style="background-color: rgb(238,243,240);border:2px dotted rgb(34,195,170);border-radius: 8px;color: green;">
{% highlight javascript %}
function* foo(){
  console.log("foo 函数运行");
  yield 'a';
  console.log("foo函数继续运行");
  yield 'b';
 }
 function* bar(){
  yield 'x';
  yield* foo();
  yield 'y';
 }
{% endhighlight %}
</div>
当我们调用其他Generator函数时,我们可能无法知道有多少个yield,这时如果我们想要一次运行完所有的代码，我们如果一个next方法一个next方法调用，未免有一些太冗余了，这是我们可以调用for...of...来运行，比如上面的bar()函数，我们可以像下面这样运行。
<div style="background-color: rgb(238,243,240);border:2px dotted rgb(34,195,170);border-radius: 8px;color: green;">
{% highlight javascript %}
for (let v of bar()){
  console.log(v);
}
{% endhighlight %}
</div>
最终的运行结果是
<div style="background-color: #FFF0F5;border:2px dotted red;border-radius: 8px;color:gray;">
{% highlight javascript %}
//x
//foo函数运行
//a
//foo函数继续运行
//b
//y
{% endhighlight %}
</div>
更多的关于Generator函数的知识可以参考官方文档,或者观看阮一峰老师的<a href="http://es6.ruanyifeng.com/">ECMAScript 6 入门</a>