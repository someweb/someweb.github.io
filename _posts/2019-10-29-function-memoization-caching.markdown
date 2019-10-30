---
layout: post
title:  "Function memoization/caching"
date:   2019-10-29 18:10:13 -0400
categories: js
tags: [js,memoization,caching]
---
![functional](/assets/images/js/memoization.jpg){: .align-center}
**Memoization or Function caching** is an optimization technique in which the results of expensive/big function calls are stored in a cache and are retrieved when the same function call is made again. This helps save time and reduces load on CPU as it does not have to perform the same calculations again.

### Time for an example:
Suppose we have a function called `complexfunction` that performs some heavy calculation that takes long time and can slow down our application. So we have to write up a function **`cache()`** that will be caching the results of the function call and in future when the function is called again with same arguments the result from cache is returned.

{% highlight js %}
var complexFunction = function(arg1, arg2) { return arg1+arg2 };
var cachedFunction = cache(complexFunction); //function to write

cachedFunction('foo', 'bar'); // complex function should be executed
cachedFunction('foo', 'bar'); // complex function should not be invoked again, instead the cached result should be returned
{% endhighlight %}

#### `cache` function implementation
{% highlight js linenos %}
function cache(func) {
  let cache = {};
  return (...args) => {
    let arg1 = args[0];
    let arg2 = args[1];
    let cache_key = JSON.stringify(args);
    if (cache_key in cache) {
      console.log('cached result found!');
      console.log(cache)
      return cache[cache_key];
    }
    else {
      console.log(cache)
      console.log('running for first time and caching the result');
      cache[cache_key] = func(arg1,arg2);
      return cache[cache_key];
    }
  }
}

console.log( cachedFunction('foo', 'bar') ); //first run
console.log( cachedFunction('foo', 'bar') ); // second run returning the cached result
{% endhighlight %}

#### How does cache function works

- **`line 2`**: We declare our cache variable which will be storing the results in a hash table.
- **`line 3`** : We pass our args array using spread operator.
- **`line 4-6`**: We are storing the args in to variables and creating our cache key as arguments `["foo","bar"]`.
- **`line 7-16`**: this is where the juice is now. When we execute the function for the first time the cache doesn't exist and so does the key. So (**`line 13-16`**) the function is executed and cached. Now next time when we run the function again with same arguments, the cache function looks up for the cached key created during the first run. If the cache key exists we returned the cached result for that execution and the heavy execution is not done.

We can now use the cache function we implemented with almost any function that returns some calculation with 2 arguments. If you think what about the cache!? Are the different functions going to use the same cache!. Well the answer is No! because every time we insert another function in cache, it is run with its own scope of cache variable thanks to closures!.

Lets add another complex function:
{% highlight js %}
var complexFunction = function(arg1, arg2) { return arg1+arg2 };
var cachedFunction = cache(complexFunction);
var complexFunction2 = function(arg1, arg2) { return arg1+'as'+arg2 }; //another complexfunction concatenating 'as'
var cachedFunction2 = cache(complexFunction2);

console.log( cachedFunction('foo', 'bar') );
console.log( cachedFunction('foo', 'bar') );//outputs foobar from its own cache and keys ["foo","bar"]
console.log( cachedFunction2('foo', 'bar') );
console.log( cachedFunction2('foo', 'bar') );//outputs fooasbar from its own cache and keys ["foo","bar"]
{% endhighlight %}

