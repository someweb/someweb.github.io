---
layout: post
title:  "Handling private variables in JS"
date:   2019-12-01 14:20:45 -0400
categories: js
tags: [js,oops]
---
![private-variables](/assets/images/js/privatevars.jpg){: .align-center}

As compared to other languages there is no Private keyword that can be used to declare private variables when creating object and classes in JS. Thats where `closures` come in to help. We use the property of closures to keep the scope of variables closed inside the function. In JS its a general rule to put an underscore in front of private variable.

Here's an example of Stopwatch implemented using factory function method:
{% highlight js%}
function StopWatch(){
  let _duration = 0;
  let startTime = 0;
  let running = false;
  return{
    start(){
        console.log('Starting...');
        startTime  = Date.now();
        running = true;
    },
    stop(){
      if(running){
        console.log('Stopping...');
        _duration += (Date.now() - startTime)/1000; 
        running = false;
      }
    },
    getDuration(){
      return _duration;
    }
  }
}

let s1 = new StopWatch();
console.log(s1.start())
{% endhighlight %}

The scope of `_duration` in the above function is only available to the returning object. If we try to access it with `s1._duration` it returns `undefined` which is exactly what we expect from a private variable. We are accessing it via the function `getDuration()`.

In ES6 does not have a proper way to create private as its still just a syntactic sugar around the old methods of creating objects. So we can make use of `IIFE`(Immediately Invoked Function Expression) way to create a closure and keep the private variables. If we try to access the property duration it will be undefined but it can be accessed using our method `getDuration()`.

Here's an ES6 implementation:
{% highlight js %}
let StopWatch = (function(){
  let _duration = 0;
  class StopWatch{
    constructor(){
      this.startTime = 0;
      this.running = false;
    }
  
    start(){
      console.log('Starting...');
      this.startTime = Date.now();
      this.running = true;
    }
  
    stop(){
      if(this.running){
        console.log('Stopping...');
        _duration += (Date.now() - this.startTime)/1000;
        this.running = false;
      }
    }
    getDuration(){
      return _duration
    }
  }
  return StopWatch
})();
let s1 = new StopWatch();
console.log(s1.start())
{% endhighlight%}

Another way of doing is using `Weakmap` outside the class and use it to keep the private variable which is garbage collected after the function finishes executing.
