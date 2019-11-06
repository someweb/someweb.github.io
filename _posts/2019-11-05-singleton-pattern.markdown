---
layout: post
title:  "Design Patterns - Singleton"
date:   2019-11-05 19:10:13 -0400
categories: js
tags: [js,design-patterns]
---
![functional](/assets/images/js/singleton.jpg){: .align-center}

Singleton design pattern is one of the patterns that looks good at first but can really make your app go out of hands as the application gets bigger and complex. The only possible place where you can see the singleton being is used is classic Logger and sometimes when application needs to have only one Sql connection. The logger is a good acceptable reason to use Singleton because there is information flow from application to logger but nothing from logger to application so in nutshell it does not have any impact on the execution of our app logic. We can remove or add logger but the app execution will be unaffected.

Lets see how a simple logger can be implemented using singleton in JS. For this example we use the module exports pattern of JS:

{% highlight js%}
//Logger class: we make sure only one instance can be made with help DebugLogger.instance check.
class DebugLogger{
    constructor(){
        if(DebugLogger.instance == null){
            this.logs = [];
            DebugLogger.instance = this;
        }
        return DebugLogger.instance;
    }
    
    log(message){
        this.logs.push(message);
        console.log(`debug ${this.logs.length}: ${message}`)
    }

    logCount(){
        console.log(`total numbder of logs : ${this.logs.length}`)
    }
}

const logger = new DebugLogger();
Object.freeze(logger);
export default logger;

//our components importing and calling logger..
//componentA.js
import logger from './DebugLogger.js';

export default function componentA() {
    logger.log("ComponentA did something..")
    logger.logCount();
}

// componentB.js
import logger from './DebugLogger.js';

export default function componentB() {
    logger.log("ComponentB did something..")
    logger.logCount();
}

//our entry point where we use our components.
//index.js
import componentA from './componentA.js';
import componentB from './componentB.js';

componentA();
componentB();

{% endhighlight %}

In the above example we have our DebugLogger class that is only allowed to instantiate once using a check `if(DebugLogger.instance == null)` if the instance is not made we make one instance and if the call is made again to DebugLogger we return the same instance. Plus its important to note we are not exporting the full class we are only exporting the one instantiated object and then using Object.freeze to stop allowing class to be changed with more properties or functions.

Then we have our components that will be importing and calling logger object. They add messages to it and we check for the count of total messages using `logCount()` in `DebugLogger.js`.

When the componentsA and componentsB are imported and called in index.js the same logger object is shared across and new messages are added and the updated length is returned. This one logger object act like a source of truth and gives state access to any component its imported and used in.

But with sharing a common state across an application introduces BIG issues in the applications that makes people scared of using Singleton

## Why <span style="color:red">NOT</span> to use Singleton

#### Failing tests!

To prove the logical correctness of algorithm or our app developers write unit tests which also helps catch various bugs inside our code with different test cases. Now assume in our application we have this singleton state which is being accessed by various objects. If some function introduces a change in the value of the singleton state then all our tests will no longer be outputting the expected value and will start failing and it is often hard to track down what function is introducing the change and ultimately gives developers a lot of headaches. Singleton also tightly couples the code to the global state which makes it tough to test different functions and classes in isolation.

So making sure the global state is immutable is very important so that it never gets updated and stays as a constant.

#### Maintenance Nightmare

Since the state is used in all the code spread in the application it gets really tough to change or update the app if any changes are made to our global state it could be a value of path to a value of constant. It becomes really tough to weed it out later as you will end up grepping the hell out of our code to find what functions are using and we can image how bad it will get when the application is very big.

#### Memory Usage

If the application starts making use of lots of singleton states then it may increase the memory usage by the app as the memory allocated to singletons is never freed up.

Application only ever create single object that is shared across the whole app and act as only source of truth. Only and can only have one single instance of this which is shared among other objects.. it has the state they are like global to the application.. which can create issues like coupling and tough to change the object race conditions.. but it can be good for sharing small amount of info across the app.

#### DB Connections

A DB connection should not normally be a Singleton. Most of the db drivers are not safe and using a singleton pattern over DB connection may work if the application is only single threaded but for multi-threaded use it means that it will not allow multi thread to share that single state. In such situations db pool is always a better idea so that your thread picks up the connections does its work and closes and return back the connection to pool again.
