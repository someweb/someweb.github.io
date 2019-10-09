---
layout: post
title:  "Updated macOS to Catalina where are some of my files gone!"
date:   2019-10-08 23:49:51 -0400
categories: macos
tags: [macos,linux]
---
I just installed Mac OSX Catalina and found out that some of my directories have been moved. I had a directory /data/ in the root(/) of my file system. 

I tried creating directory before looking up where its as I thought its gone forever. But if you try to create a directory in the root of you Mac OSX it is no longer possible due to security and potential vulnerabilites in the operating system. 

After a quick search I found that the files have been moved under:

`/Users/Shared/Relocated Items`

If you had setup any apache2 or mysql configurations they have been set to default and can be found inside `/Users/Shared/Relocated Items/Configuration` or the files have renamed for example my apache's vhosts file in `/etc/apache2/extra` was renamed to something like `httpd-userdir.conf~previous`. So just rename them back to after adjusting your vhosts and you are good to go!. 

