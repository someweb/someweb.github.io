---
layout: post
title:  "Transforming a callback to Promise"
date:   2019-10-12 00:12:51 -0400
categories: js
tags: [js,promise]
---
I `promise` it will be quick ;). If you are new to Javascript or even if you have been working for while, we all have experienced callback functions when we are trying to run some tasks before we finish the predecessors. It gets tougher when the functions are not synchronous and are actually async in nature like getting a response from server which is not guaranteed to be responding right away which leads to errors in our applications. In order to solve such issues callback functions started getting used.

![Callback Hell](/assets/images/js/callbackhell.png){: .align-center}

So in order to prevent such situations Promises were developed. To understand promises the two main concepts we need to know are resolve and reject functions which are just a fancy way of saying what to do if our function runs successful i.e. resolve and if our function fails i.e. reject. So lets dive right into transforming a simple callback function into a promise based function:

{% highlight js %}
function phoneCall(callback, errorcallback){
  let technicalError= false;
  let userbusy = false;

  if(technicalError){
    errorcallback({
      name:'Technical Issues',
      message: 'Call cannot be connected due to technical issues'
    })
  }
  else if(userbusy){
    errorcallback({
      name: 'Busy',
      message: 'User is busy'
    })
  }
  else{
    callback('Hello Who's there?')
  }
}
{% endhighlight %}

In the above function a phone call is received when the user is not busy and there is no technical error and then the callback function is fired with the message.

So in order to transform it to promise function all we have to do is return a promise and change the callback functions to resolve and reject.

{% highlight js %}
function phoneCallPromise(){
  let technicalError= false;
  let userbusy = false;

  return new Promise((resolve, reject)=>{
    if(technicalError){
        reject({
          name:'Technical Issues',
          message: 'Call cannot be connected due to technical issues'
        })
      }
      else if(userbusy){
        reject({
          name: 'Busy',
          message: 'User is busy'
        })
      }
      else{
        resolve('Hello who is there?')
      }
  })
}
{% endhighlight %}

When the function returns promise then we pass in resolve and reject as the parameters to callback function that is passed to Promise. Then the reject and promise are fired depending on the conditions if userbusy changes to true then the promise will be rejected and it will return.

<pre>
Uncaught (in promise) {name: "Busy", message: "User is busy"}
</pre>
{: .notice--danger}

Now when we can call phoneCallPromise by using then where we pass a callback function with parameter which can be a return from the function or can show a message to show the success:

{% highlight js %}
phoneCallPromise().then((message)=>{
  console.log(message);
}).catch((error)=>{
  console.log("error : "+error.message)
})
{% endhighlight %}

In this way we can call the function and keep adding `.then()` that will happen when the previous promise resolves. It is worth noting that every `then()` returns a promise.

Now lets take 3 promises and try to chain them and see how this avoids callback hell when we are dealing with async tasks and execute them in sync way:

{% highlight js %}
const p1 = new Promise((resolve,reject)=>{
  setTimeout(()=>resolve("done p1"),1000);
})

const p2 = new Promise((resolve,reject)=>{
  setTimeout(()=>resolve("done p2"),1500);
})

const p3 = new Promise((resolve,reject)=>{
  setTimeout(()=>resolve("done p3"),2000);
})

// calling the promises
p1.then((msg)=>{
  console.log(msg);
  return p2;
})
.then((msg)=>{
  console.log(msg)
  return p3;
})
.then((msg)=>{
  console.log(msg)
})
.catch(e=>console.log(e))

//output :
done p1
done p2
done p3
{% endhighlight %}

So as long as they are chained the next promise next in chain will only be executed if the predecessor has resolved.

But in 2018 a new concept got popular which actually broke the chains and now we can make use of such functionality with async/await but without compromising the code readability and build clean code.

I will be covering async/await in next posts.

Peace.