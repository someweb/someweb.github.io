---
layout: post
title:  "pass by value vs pass by reference"
date:   2019-11-12 18:10:13 -0400
categories: js
tags: [js]
---
![pass-by-val-ref](/assets/images/js/pass-by-val-ref.jpg){: .align-center}

This very simple concept of setting and passing values can create a big mess and bugs in an application if the understanding of pass by ref and pass by value is not clear to a developer. Lets see how..

## pass by value

When we set a variable ot value to primitive types (number, string, boolean, undefined or null) that is anything on right hand side is copied to variable. When we call the variable we return the value that is set on the right hand side of `=` sign.

{% highlight js%}
let x = 10
let y = x
console.log(`x is: ${x} and y is ${y}`)
y = y+1 // lets add one to y
console.log(`x is: ${x} and y is ${y}`)
{% endhighlight %}

output:
<figure class="align-center">
  <img src="{{ '/assets/images/js/pass-by-val.png' | absolute_url }}" alt="pass-by-val">
  <figcaption>pass by val not affecting variable x</figcaption>
</figure>

#### so what happened? 🤨
**`y`** is set to 11 but **`x`** is still 10 because **`y`** is set to a value of 10 and is updating it to 11 whereas **`x`** is pointing to its own value 10.

## pass by reference
Pass by reference takes place when we use non primitive types like arrays. Lets assign `x` and `y` to arrays. Check the example below:

{% highlight js%}
let x =[1,2] // 0x01
let y = x // y also set to 0x01
console.log(`x is: ${JSON.stringify(x)} and y is ${JSON.stringify(y)}`) //just using json.stringify for pretty output
y.push(3) //update y
console.log(`x is: ${JSON.stringify(x)} and y is ${JSON.stringify(y)}`) //both x and y are updated!
console.log(`x === y is ${x===y}`) // returns true
{% endhighlight %}

<figure class="align-center">
  <img src="{{ '/assets/images/js/pass-by-ref1.png' | absolute_url }}" alt="pass-by-ref">
  <figcaption>pass by ref affecting both x and y</figcaption>
</figure>

#### so what happened? 🤨

In JS if we set assign array to x it doesn't mean that its value is set to 1 and 2 but instead it is only storing the address in the memory something for example `0x01`. So value of **`x`** is just a memory address which has a value **`[1,2]`** and **`x`** is now storing reference to that memory address. So when we call x we are not actually returning the memory address we are getting the value stored in that memory location which is on the right hand side of that memory location just like pass by val.

If we now do `let y = x` . Its going to copy the memory address from `x`. and no more memory is located. So `x` and `y` are referencing same  memory address. That is where we can run into issues for example if we try to copy array sometimes. Lets try adding a value to `y`. Now if we do **`y.push(3)`**. It is now changing both `x` and `y` as they both are same address. To check if they are actually same memory address if we compare them they return `true`.

Lets set `y` to brand new [1,2] which is now a new location in memory now. Our `x` is still set to old memory. Here we are now overwriting the memory address for `y` which is not affecting `x`.

{% highlight js %}
let x =[1,2] // 0x01
let y = x
y=[1,2] //overwriting mem address now to 0x02
console.log(`x is: ${JSON.stringify(x)} and y is ${JSON.stringify(y)}`)
y.push(3)
console.log(`x is: ${JSON.stringify(x)} and y is ${JSON.stringify(y)}`)
console.log(`x === y is ${x===y}`)
{% endhighlight %}

<figure class="align-center">
  <img src="{{ '/assets/images/js/pass-by-ref2.png' | absolute_url }}" alt="pass-by-ref">
  <figcaption>pass by ref affecting only y</figcaption>
</figure>

After pushing new value to `y`, it only updated y. It is because now y is set to a new memory address with value of **`[1,2]`** for example `0x02`. So if we compare `x` and `y`, they return `false`.

### pass by reference in action in functions

Another big mistake happens and confusions happen when we use this in functions. For example we have this function `append()` that adds value to array outside our function.

{% highlight js %}
let arr = [1,2]
console.log(`arr is: ${JSON.stringify(arr)}`)
append(arr,3) //updates our arr
console.log(`arr is: ${JSON.stringify(arr)}`)

function append(arr,item){ // arr that we are passing here is memory loc of arr variable
    arr.push(item)
    console.log(`arr inside func is: ${JSON.stringify(arr)}`)
}
{% endhighlight %}

<figure class="align-center">
  <img src="{{ '/assets/images/js/pass-by-ref-func1.png' | absolute_url }}" alt="pass-by-ref">
  <figcaption>pass by ref affecting arr variable</figcaption>
</figure>

In this example if we check the value after running the function the value is actually added to `arr` because when we are passing arr to function we are actually passing in the memory location which later on our function modifies.

Now lets try not to modify that because that variable maybe being referred by some other functions which in result will be causing hard to find bugs!

{% highlight js %}
let arr = [1,2]
console.log(`arr is: ${JSON.stringify(arr)}`)
append(arr,3)
console.log(`arr is: ${JSON.stringify(arr)}`)

function append(arr,item){
    arr=[...arr] // assigning our arr to a new memory location by copying our arr in global scope 
    arr.push(item)
    console.log(`arr inside func is: ${JSON.stringify(arr)}`)
}
{% endhighlight %}

<figure class="align-center">
  <img src="{{ '/assets/images/js/pass-by-ref-func1.png' | absolute_url }}" alt="pass-by-ref">
  <figcaption>pass by ref not affecting arr variable</figcaption>
</figure>

As we can see if we try to copy the array passed by ref to the `append()` function and assign back to arr again inside function scope, it only modifies the array in local scope of append function. Inside the function arr is overwritten with another memory address with same values.
