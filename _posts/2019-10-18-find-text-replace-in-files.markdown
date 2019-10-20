---
layout: post
title:  "Replace specific strings in multiple files without even opening them"
date:   2019-10-18 18:00:51 -0400
categories: linux
tags: [linux, commands, sed, find]
---
Sometimes we might run into a situation when we have to replace some specific strings in multiple files and it could be tiresome to go inside the files and replace the strings one by one. But linux command line offers tools like grep, find and sed which makes life so easy.

I ran into a situation where I had to change the directories for my apache vhost configuration files after the recent MacOS Catalina update as i was storing my some local development websites on under `/root`.

For example in my vhosts file all the apache configs were pointing to this directory `/data/virtual_www/dev`. After i moved my `dev` dir under `/Users/foo`. So I wanted to change it to my /Users/foo/dev

Here's how to do it go to the directory where you want to replace the strings, in my case it was `/etc/apache2/extra/`:

{% highlight console %}
foo@bar- extra $ find . -type f -name '*.conf' | xargs sed -i '' 's/data\/virtual_www/Users\/foo/g'
foo@bar- extra $ grep -r "\/data\/virtual_www" *
{% endhighlight%}

So the find command does a recursive search in the directory and any files it finds it passes it as an argument(`xargs`) to `sed` which then runs a search-replace command `s/find-string/replace-string/g`. In the syntax the slashes can be confusing because we have to escape them so that they can be searched and replaced.

To allow the slashes to be replaced as well we use a backslash : **s/`\`/data/`\`/Users/g**

So this way /data will be replaced by /Users in the whole file.

Now after the replace I ran a `grep` command to check if the strings were actually replaced, if no results are returned then that means it was a success!.
