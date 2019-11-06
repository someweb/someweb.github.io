---
layout: post
title:  "Python OOPs part 1"
date:   2019-11-04 18:10:13 -0400
categories: js
tags: [python]
---
![functional](/assets/images/python/pythonoops.jpg){: .align-center}
Lets dive straight into the basic concepts of OOPs (Object Oriented Programming) in python:

### Classes and Objects
{% highlight python %}
class Employee:

    def __init__(self,first,last,salary):
        self.first = first
        self.last = last
        self.email = first +'.'+last +'@test.com'
        self.salary = salary
    
    def fullname(self):
        return '{} {}'.format(self.first,self.last)

emp1 = Employee('Singh','Matharoo',30000)
# emp2.second = "lol"
# emp2 = Employee()

print "Hello world "+emp1.email

# print emp1.fullname()

print Employee.fullname(emp1)

print emp1.salary
#31000
{% endhighlight %}

Just like standard class is the keyword for declaring classes. When defining a class in Python we use `__init__` as the constructor method for the class and have to pass self as the first parameter so that when we declare Object of such class it bind the attributes that we pass as arguments.

#### Object variables vs class variables:

Object variables are only tied to the current object where as class variables are variables that shared among all objects or instances.

{% highlight python %}
class Employee:
    ......
    increment_salary = 1.06

    def increasesalary(self):
        self.salary = int(self.salary * self.increment_salary)

emp1.increasesalary()
#incraeses salary by 1.06
print emp1.salary
#31800
{% endhighlight %}

In the example above `increment_salary` is a class variable which can be applied similarly to all the object via the method `increment_salary()`

#### Class Methods and Static Methods

Class methods can be used to change a class variable or also usually used as alternative constructors. We declare a  classmethod by adding decorator `@classmethod` to the top of function that we are adding to the class. For example we add functions `change_increment_salary` and `from_string`. Also when we declare a class method we also pass class as first argument, we cannot use `class` as its reserved. but the first argument pass is just a class followed by other parameters.

{% highlight python %}
class Employee:
    ......
    @classmethod
    def change_increment_salary(cls,amt):
        cls.increment_salary = amt

    @classmethod
    def from_string(cls,emp_string):
        first, last, salary = emp_string.split('-')
        return cls(first,last,salary)

emp_str = 'Singh-Matharoos-30000'
emp2 = Employee.from_string(emp_str)
print emp2.email
#prints Singh.Matharoos@test.com
{% endhighlight %}

Static methods on the other hand are just like regular methods but we dont pass any class or instance as argument, we only the parameters. We add a decorator `@staticmethod` to the top of function to declare the method as static. The reason we add them to a class is that they have to have some kind of logical connection to the class. for example in our we add `is_workday()` to check if the day is working day.

{% highlight python %}
class Employee:
    ......
    @staticmethod
    def is_workday(day):
        if day.weekday() == 5 or day.weekday() == 6:
            return False
        return True

import datetime
a_date = datetime.date(2019,10,22)

print(Employee.is_workday(a_date))
{% endhighlight %}