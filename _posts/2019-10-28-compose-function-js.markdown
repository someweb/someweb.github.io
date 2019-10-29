---
layout: post
title:  "compose functions in JS"
date:   2019-10-28 18:10:13 -0400
categories: js
tags: [js,compose,functional]
---
![functional](/assets/images/js/functional.png){: .align-center}

Recently I came across this problem where you have to write a function that takes initial value and n number of functions. The initial value is then passed as parameter to the first function which then returns a value that is passed as parameter to the next function and so on. At the end this compose function returns one value.

Lets look at this problem
{% highlight js%}
var doubleTheValue = function(val) { return val * 2; }
var addOneToTheValue = function(val) { return val + 1; }

compose(5, doubleTheValue, addOneToTheValue) // should === 11
{% endhighlight %}

So we pass 5 as the initial value which is first doubled then the return value is passed to addOneToTheValue and then return 11 as the answer.

This **compose** function is a higher order function that takes other functions as arguments and converts them to array of functions and then executes them in order from left to right or right to left reducing them to a single value in the end.

In order to solve this `reduce` functions are very handy. With the help of reduce we can apply function to each element of an array usually.

{% highlight js %}
const compose = (initvalue, ...funcs) => funcs.reduce((a,f)=> f(a),initvalue)
{% endhighlight%}

Here we define our compose function that takes parameters `initvalue` and `n` number of functions using spread **`(...)`** operator. Then we use arrow function that returns and runs a reduce function on every function `f()` where `a` is **accumulator** that keeps a tally of functions calculations which gets passed as parameter to the next function in `...funcs` array. The last value `initvalue` is the initial value that we have to pass to the reduce function which is 5.

{% highlight js %}
console.log(compose(5, doubleTheValue, addOneToTheValue))
//Output 11
{% endhighlight %}

The order of solving the problem in our compose function is left to right which is also called **piping**. However we can also execute the compose function in the opposite order that is right to left which is true composition `f(g(x))`(here value of `g(x)` is passed to `f()`). To do so we can use `reduceRight` function which executes addOneToTheValue and then doubleTheValue function.

{% highlight js %}
const compose = (initvalue, ...funcs) => funcs.reduceRight((a,f)=> f(a),initvalue)
console.log(compose(5, doubleTheValue, addOneToTheValue))
//Output 12
{% endhighlight %}

**reduce** gives out 11

**reduceRight** gives out 12
