---
layout: post
title:  "Hello World! git and jekyll it is"
date:   2019-10-03 19:49:51 -0400
categories: git jekyll
---
Finally I have been able to setup a blog. The times have changed now no more Blogger or Wordpress, few years ago I would have never imagined I will be using a version control system like Git to host a blog. This is truly amazing how the site is built on Github in background once jekyll configures the code. I had a little bit of hard time trying to setup confugration to use the right theme but alas its working. yay!. 

Here's a configuration of Gemfile:
{% highlight yml %}
source 'https://rubygems.org'
gem 'github-pages', group: :jekyll_plugins
{% endhighlight %}

I am really looking forward to sharing code snippets and whatever code practices I learn and aleady know.

Jekyll also offers powerful support for code snippets:
Here's JS example : 
{% highlight javascript %}
//closures
function f(y) {
  var z = 1;
  return function g(y) {
    console.log(y + z);
  }
}
{% endhighlight %}

Here's Python snippet example:
{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll.
[jekyll-docs]: https://jekyllrb.com/docs/home
