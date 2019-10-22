---
layout: post
title:  "Using async/await in javascript"
date:   2019-10-22 14:20:13 -0400
categories: js
tags: [js, async, promise]
---
It all started with Callback hells which led to Promise that solved the problem with `.then()` chains but introduced another one where there was lot of repeating code and made the code the still a bit messy. This led to the arrival of `async` `await` in javascript.

Lets take a look at the following example where we have function call callApi that can only call facebook and twitter api. This function returns a promise which will resolve() only if twitter and facebook are passed but returns a reject() if anything other is passed. When this function is resolve then we call another function capitalizeResponse() which parses the response and always returns back a resolved promise with capitalized response.

{% highlight js%}
//returning promise based on url
function callApi(url){
  return new Promise((resolve,reject) => {
    console.log(`calling API: ${url}`)
    if(url==='twitter'|| url=='facebook'){
      resolve(`connected to ${url}`)
    }
    else{
      reject(`Sorry unsupported API : ${url}`)
    }
  });
}

//function always resolving and capitalizing the response from callApi()
function capitalizeResponse(resp){
  return new Promise((resolve,reject)=>{
    resolve(resp.toUpperCase())
  })
}

// calling facebook and twitter api
callApi('twitter')
.then((resp)=>capitalizeResponse(resp))
.then((caps)=>console.log(caps))
.then(()=>callApi('facebook'))
.then((resp)=>capitalizeResponse(resp))
.then((resp)=>console.log(resp))
.catch((e)=>console.log(e))

//failing chain
callApi('twitter')
.then((resp)=>capitalizeResponse(resp))
.then((caps)=>console.log(caps))
.then(()=>callApi('wrongurl'))
.then((resp)=>capitalizeResponse(resp))
.then((resp)=>console.log(resp))
.catch((e)=>console.log(e))

{% endhighlight %}

We execute the functions above by forming a promise chain, if we connect successfully to twitter, parse response then we connect to facebook if there is any error in any of the promises the execution stops and it is caught by catch() statement at the end of the chain.

<figure class="align-center">
  <img src="{{ '/assets/images/js/passing-chain.png' | absolute_url }}" alt="passing-promise-chain">
  <figcaption>Successful execution</figcaption>
</figure>

<figure class="align-center">
  <img src="{{ '/assets/images/js/failing-chain.png' | absolute_url }}" alt="failing-promise-chain">
  <figcaption>Failing promise chain and gracefully handling exceptions with catch()</figcaption>
</figure>

As we can clearly see there is a lot of repeating code with lots of thens and it get even bigger when we a chain of operations.

## async/ await to the rescue![red]
We can define the chain operations with one async and await function which makes it easy to handle the function calls and makes your code legible.

{% highlight js %}
async function processresponses(){
    const twitter = await callApi('twitter');
    const process_t = await capitalizeResponse(twitter)
    console.log(`#{process_t}`)
    const facebook = await callApi('facebook');
    const process_f = await capitalizeResponse(facebook)
    console.log(`${process_f}`);
}
{% endhighlight %}

So in order to use async and await, we define define function with async keyword in front of the function so that the compiler knows the function will be running some async code. Now we can use await keyword in front of any promise returning asynchronous functions. In our function above we have multiple await calls and they are executed just like then one after one. The output we get is exact same. 

But what happens if there are any errors happening while this async function runs where is the catch() you may ask. To handle any exceptions we can just simple use the classic try{} catch().

#### try-catch in async functions

So our code becomes:
{% highlight js %}
async function processresponses(){
  try{
        const twitter = await callApi('twitter');
        const process_t = await capitalizeResponse(twitter)
        console.log(`${process_t}`)
        const facebook = await callApi('wrongurl');
        const process_f = await capitalizeResponse(facebook)
        console.log(`${process_f}`);
  }
  catch(e){
    console.log(e)
  }
}

processresponses()
{% endhighlight%}

This way we can use async/await to replace promise chaining and make our code easy to understand.
