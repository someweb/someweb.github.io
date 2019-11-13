---
layout: post
title:  "Debugging bash scripts"
date:   2019-11-12 21:10:13 -0400
categories: bash
tags: [bash,linux]
---
![bash-debug](/assets/images/linux/bash-debug.jpg){: .align-center}

When the bash scripts get big, its often hard to debug while relying only on echo statements trying to trace the error causing statements and functions.

Thankfully there is a setting that can be used to enable debugging and can help you start debugging in a second.

## using xtrace -x

At the beginning of a script just add **`set -x`** which enables the xtrace mode and then prints out each command it executes before outputting the result. 

## enabling and disabling -x

The option can also be enabled for specific parts of the script and then disabled by just using `set +x`. So anything between `set -x` and `set -x` will have the trace enabled.

{% highlight bash linenos%}
#!/bin/bash

var1='A'
var2='B'

my_function () {
  local var1='C'
  var2='D'
  echo "Inside function: var1: $var1, var2: $var2"
}

echo "Before executing function: var1: $var1, var2: $var2"

set -x 
my_function
set +x

echo "After executing function: var1: $var1, var2: $var2"
{% endhighlight %}

#### Output:
```
Before executing function: var1: A, var2: B
+ my_function
+ local var1=C
+ var2=D
+ echo 'Inside function: var1: C, var2: D'
Inside function: var1: C, var2: D
+ set +x
After executing function: var1: A, var2: D
```

so the debugging is enabled only around the function which allows us to see what variables are passed like for `line 9` it uncovers the variables:

`echo "Inside function: var1: $var1, var2: $var2"`<br>
to<br>
`+ echo 'Inside function: var1: C, var2: D'
Inside function: var1: C, var2: D`

## enabling -x debug via command line arguments

If you do not want to add the option to script itself then we can pass the -x as commandline argument to enable debugging for the whole script like:

{% highlight console %}
foo@bar- myscripts $ bash -x script.sh
{% endhighlight %}

## Output debug logs to a file (works with bash 4.1 or higher)

If you do not want to clutter the terminal with the debug logs then we can also output the logs to a file using **`exec 5> logs.txt`** which will output the debug logs to logs.txt rather than printing them on console. 
Then to format the log output you can use **`PS4='[${BASH_SOURCE[0]:-inherited}:${LINENO}:${FUNCNAME[0]:-main}] '`** to add filename and line number to every log statement.

{% highlight bash %}
#!/bin/bash
exec 5> logs.txt
BASH_XTRACEFD="5"
PS4='[${BASH_SOURCE[0]:-inherited}:${LINENO}:${FUNCNAME[0]:-main}] '

var1='A'
var2='B'

my_function () {
  local var1='C'
  var2='D'
  echo "Inside function: var1: $var1, var2: $var2"
}
echo "Before executing function: var1: $var1, var2: $var2"

set -x 
my_function
set +x

echo "After executing function: var1: $var1, var2: $var2"
{% endhighlight %}

#### Output:
```
[./debug.sh:19:main] my_function
[./debug.sh:11:my_function] local var1=C
[./debug.sh:12:my_function] var2=D
[./debug.sh:13:my_function] echo 'Inside function: var1: C, var2: D'
[./debug.sh:20:main] set +x
```
