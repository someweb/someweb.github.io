---
layout: post
title:  "Destructuring is awesome!"
date:   2019-10-24 01:10:13 -0400
categories: js
tags: [js]
---
![builder-pattern](/assets/images/js/destructuringjs.jpg){: .align-center}

Destructuring is a great way to unpack arrays and objects in Javascript. After unpacking you can assign to variables, create copies and do more in less lines of code.

## Array Destructuring
{% highlight js %}
let arr1 = ['Hello','World'];
let arr2 = ['Destructing','is','awesome!']
let arr3 = [1,2,4,5]
{% endhighlight %}

### Assigning to variables
{% highlight js %}
let [str1,str2] = arr1
console.log(str1)
//Output: Hello
{% endhighlight %}
In order to de-structure array, on the left side we put variables to be declared inside **`[]`** and the array to de-structure on the right side of equal sign. Now **`str1`** has value of **`arr1[0]`**, **`str2`** has **`arr[1]`** and can go on.

### With functions returning arrays
{% highlight js %}
function returnHello(){
  return ['Hello','World']
}
let [str1,str2] = returnHello()
console.log(str1);
//Output: Hello
{% endhighlight %}
We can also use the same method of assigning to variables above to catch the function returns and assign to variables.

### Concatenating Arrays
{% highlight js %}
let combined = [...arr1,...arr2,...arr3];
console.log(combined);
//Output: ["Hello", "World", "Destructing", "is", "awesome!", 1, 2, 4, 5]
{% endhighlight %}
Just put the arrays like **`...arr1`** (3 dots spread operator) inside square braces **`[]`** in any order you want and the result is combined array.

## Object Destructuring

Similar to array destructuring we can unpack the objects using **`{}`** curly braces. We used a similar approach in my last post where I wrote about the builder pattern and passing object inside constructor to initialize instance.

{% highlight js %}
let User1 = {
  'name':'Sherlock',
  'age':39,
  'city':'Victoria',
  'phone':'123-456-7890',
  'university':'TRU'
}
let User2 = {
  'city':'Waterloo',
  'phone':'321-456-7891',
  'university':'WLU'
}
{% endhighlight %}

### Assigning to variables
{% highlight js %}
let {name,age} = User1
console.log(name,age)
//Output: Sherlock,39
{% endhighlight %}

For assigning object properties to variables we pass in the keys inside the **`{}`** which then creates them as variables. For example we created name variable. We can then also assign those key to custom variable name by doing this:
{% highlight js %}
let {name:username,age} = User1
console.log(username)
//Output: Sherlock
{% endhighlight %}

### Creating a new Object by combining objects
For example we wanted to create a new from the existing User1 but with keys of User2. We can use spread operator **`(...)`** inside **`{}`** just like:
{% highlight js %}
let User3= {...User1,...User2}
console.log(User3)
//Output: {name: "Sherlock", age: 39, city: "Waterloo", phone: "321-456-7891", university: "WLU"}
{% endhighlight %}

So in the new object the User1's city,phone,university gets overwritten with User2's keys and our new User3 becomes sherlock as well!.

### Passing in as functional parameters
Rather than passing in list of parameters in a specific sequence or a full big object we can just setup our function to accept only specific keys:
{% highlight js %}
function welcomeUser({name,city}){
  console.log(`Welcome ${name}!, from ${city}`)
}
welcomeUser(User1);
//Output: Welcome Sherlock!, from Victoria
welcomeUser(User3);
//Output: Welcome Sherlock!, from Waterloo
{% endhighlight %}

So even though we passed in full object **User1** and **User3** the function only picked up **name** and **city**.
