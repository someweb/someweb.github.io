---
layout: post
title:  "Design patterns : Builder Pattern in JS"
date:   2019-10-22 16:10:13 -0400
categories: design-patterns
tags: [js, builder-pattern]
---
![builder-pattern](/assets/images/js/builder.jpg){: .align-center}

When we have an application with different working parts and all have to be closed in one object builder pattern can help make things pretty easy when making objects out of our class. Generally in builder pattern rather than setting the object properties inside the constructor we set them using set() methods. This makes our class objects flexible and let us define or not define the properties we want and do not want.

Lets dive straight into it with an example of User class and Address class, our User object will have address as Address object.
{% highlight js%}

class Address{
  constructor(street,city,country){
    this.street = street;
    this.city = city;
    this.country = country;
  }
}

class User{
  constructor(name,age,phone,email,address){
    this.name = name;
    this.age = age;
    this.phone = phone;
    this.email = email;
    this.address = address;
  }
}

//creating instance of User
let u1 = new User('Matt',undefined,'19123213',undefined,new Address('12 Victoria St.','Vancouver'))
console.log(u1);
// Output: User {name: "Matt", age: undefined, phone: "19123213", email: undefined, address: Address}
{% endhighlight %}

In this code above when we are not defining values for age and email we have to pass `undefined`, if undefined is not passed the value of phone get assigned to age. This type of approach is clearly a problem as it will get tough to define properties for an object with many properties. It will be a nightmare tbh!

So we can fix this by creating another class that will help us build a User but with no pain. 

## Builder Pattern with a builder
Lets start by defining a **Builder** class and define some methods for age,phone,email and address.

{% highlight js%}
class Builder{
  constructor(name){
    this.user = new User(name)
  }

  setAge(age){
    this.user.age = age;
    return this // returns the Builder object
  }

  setPhone(phone){
    this.user.phone = phone;
    return this
  }

  setAddress(address){
    this.user.address = address;
    return this
  }

  build(){
    return this.user
  }
}

//creating instance of Builder
let b1 = new Builder('Matt');
console.log(b1.setAge(19)) // this returns Builder object here 
// but we need user so we use the build method that returns the user
b1.setEmail('adasd@ads.com')
let user = b1.build();
console.log(user)
// Output: User {name: "Matt", age: 19, phone: undefined, email: "adasd@ads.com", address: undefined}
{% endhighlight %}

## Passing Objects to create instance
Another way of building object can be achieved by simply passing in an object when we create an instance of User. If we pass an object to define the properties of User it then automatically can take care of what to set and what not.

So we just refactor our User class into something like
{% highlight js%}
class User{
  constructor(name,{age,phone,email,address}={}){
    this.name = name;
    this.age = age;
    this.phone = phone;
    this.email = email;
    this.address = address;
  }
}
let u1 = new User('Matt',{age:19,email:'adasd@ads.com',phone:'19123213',address:new Address('12 Victoria St.','Vancouver','Canada')})
console.log(u1);

{% endhighlight %}

<figure class="align-center">
  <img src="{{ '/assets/images/js/passing-object-constructor.png' | absolute_url }}" alt="passing-object-constructor">
  <figcaption>Output of User object when passing constructor to set properties</figcaption>
</figure>

In the above code we are assigning `{age,phone,email,address}` to = `{}` in the constructor parameters which means if nothing is passed then pass in an empty object. We can also set default values for object if nothing is set by assigning it like this `{age=10,phone,email,address}`.

Both ways of implementing builder patterns helps us fix the issue of instantiating big objects. However using a builder class method can still prove to be a better approach when the object we constructing is complex.

happy building!
