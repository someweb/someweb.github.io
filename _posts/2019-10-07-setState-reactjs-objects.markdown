---
layout: post
title:  "Updating state object using useState in ReactJS functional components "
date:   2019-10-07 19:49:51 -0400
categories: reactjs
tags: [js,reactjs]
---
For this post I will be going over on updating state object like with the help of useState hook in functional components of ReactJS:

{% highlight js %}const [device,setDevices] = useState({'name':'iphone 6S','os':''});{% endhighlight %}

The state object already has value for property defined. Now we want to update the property os to `iOS 12` while preserving the state of name property. Here's how to do it:

{% highlight js %}
const field = 'os'
setDevices(prevState => {
    return { ...prevState, [field]: 'iOS 12' }
});
{% endhighlight %}

What if we have a list/array of objects in our device state like:
{% highlight js %}
[
  {
  'name':'samsung s10',
  'os':''
  },
  {
  'name':'iphone 6S',
  'os':''
  },
  {
  'name':'Blackberry',
  'os':''
  }
]
{% endhighlight %}

Now before we update we need to do a quick lookup, here's how we update the state when we find the item we want to update:
{% highlight js %}
const update={'name':'iphone 6S','os':'iOS 12'}
setDevices(device.map(function (item) {
    if (item.name !== update.name) return item;
    return update;
}))
{% endhighlight %}

That is it. Your state is now updated. This can be useful when we are updating list of objects and performing CRUD operations on the list while updating the db in the backend. Enjoy!