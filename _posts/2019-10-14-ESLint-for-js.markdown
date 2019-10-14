---
layout: post
title:  "Setting up eslint for JS in VS Code"
date:   2019-10-14 14:49:51 -0400
categories: js
tags: [js,eslint]
---
Recently i was trying to setup eslinting for one of my projects based on JS. I have switched from atom to using VS Code for all my projects as its more stable and have recently gained so much popularity due to so many available plugins and the backing of Microsoft. So as I was working on setting up a JS project i am trying to make sure that my code follows strict guidelines for ES6 and the code is more consistent. VS Code has a very nice plugin for eslint which is called ESLint.

![ESLint for VS Code](/assets/images/js/eslintvscode.png)

After ESlint is installed on VSCode then in order for it to start providing you linting suggestions you need to create a .eslintrc file inside you project root directory.

### Installing npm eslint package

When setting up eslint it should be setup for each project separately inside the project root rather than installing globally and using a global configuration for all your projects . This can lead to situations like 'It works on my setup' which should be avoided. To ensure other team members who try to work on the project will have same dependencies installed we should install the package globally and then install inside the project added as a dev dependency. The quickest way to setup is using the predefined settings like airbnb, google settings. So use the following commands to setup and enforce code style for your JS project.

{% highlight bash %}
npm install eslint -g
npm install eslint --save-dev
{% endhighlight %}

Now since we installed it globally we can run run `eslint --init` inside project root directory and setup `.eslintrc.json` the eslint using [airbnb](https://github.com/airbnb/javascript) settings or custom configuration.

![eslint-init](/assets/images/js/eslint-init.png)

## Disabling eslint on a single line 
{% highlight js %}
// eslint-disable-next-line
const test = testsum()
{% endhighlight %}

## Disabling eslint multiple lines
{% highlight js %}
/* eslint-disable */
const p1 = new Promise((resolve,reject)=>{
  setTimeout(()=>resolve("done p1"),1000);
})

const p2 = new Promise((resolve,reject)=>{
  setTimeout(()=>resolve("done p2"),3000);
})

const p3 = new Promise((resolve,reject)=>{
  setTimeout(()=>resolve("done p3"),2000);
})
/* eslint-enable */
//now eslinting is enabled again
console.log('foo');
console.log('bar');
{% endhighlight %}

happy linting :)