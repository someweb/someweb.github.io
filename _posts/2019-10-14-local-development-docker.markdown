---
layout: post
title:  "Using docker for local development environment"
date:   2019-10-14 16:00:51 -0400
categories: reactjs
tags: [js,reactjs,docker]
---
Yes you can use Docker for setting up your local development environment. The ability of docker to create containerized environments can be really helpful when you are working on applications that have conflicting versions of libraries.

When you try to install different versions of a library for example nodejs or python on same system then you have to do some configuration changes for your application and system itself so that that application picks up the right version. This is where the containerized technology of docker solves the problem. Docker offers docker-compose tool when you install docker on your machine (macOS in my case). It makes it very easy to run your microservices based application who are communicating with each other and need different runtime environments.

### docker-compose.yml file
In order to implement this we need to define a docker-compose.yml file that defines what applications will be run in different containers and how they will be built up. For example I created a reactapp using nodejs 12 that will be talking to expressjs api that is running on nodejs 10. For the project structure I have created the reactapp and expressjs api in one common directory where we create our docker-compose.yml file. My project structure looks like:

<pre>
project
│   <b>docker-compose.yml</b>
│
└───reactapp
│
└───api
</pre>

You can check my github repo here [docker-compose](https://github.com/matharoo/docker-compose)

Here is a sample configuration of my **docker-compose.yml** :

{% highlight yml linenos %}
version: '3'
services:
  reactapp:
    image: node:12.2.0-alpine
    build: ./reactapp
    command: sh -c "npm install && npm start"
    environment:
      - NODE_ENV=development
      - PORT=3000
    ports:
      - '3000:3000'
    working_dir: /root/reactapp
    volumes:
      - ./reactapp:/root/reactapp:cached
  api:
    image: node:10
    build: ./api
    command: sh -c "npm install && npm start"
    environment:
      - NODE_ENV=development
      - PORT=3001
    ports:
      - '3001:3001'
    working_dir: /root/api
    volumes:
      - ./api:/root/api:cached
{% endhighlight %}

Lets go over what the main variables in configuration define:
  * `services`: under this setting we start to define the containers we will be launching like reactapp and api.
  * `image`: what environment we are using for our project like i am using nodejs 12 for my react and 10 for api.
  * `build`: this defines the location of your application in your project directory.
  * `environment`: this lets us define any environment variables for our node app like ports, remote databases,etc
  * `command`: what command we run to install dependencies for our node project and start the nodemon server
  * `ports`: this lets us define what ports the app will accessible on.
  * `working_dir`: this is location of app on the container.
  * `volumes`: this helps us attach a directory from our machine to container just like we attach or mount a hard drive on a linux system.

The `volume` setting feature in docker allows us to edit the files from our machine and the changes take affect in real-time on the container which is what completes our development environment. Now whenever you make changes
from your favorite editor on your machine the apps get refreshed and changes are reflected just like they do on local environment but you get the flexibility to run different environments on docker containers.

## How to run

![docker-compose](/assets/images/js/docker-compose.png){: .align-center}

In order to start the containers we have the following commands:
```
# start the containers and keep docker attached to terminal which shows the logs of containers when they update
docker-compose up

# or you can use start to detach from terminal
docker-compose start

# to stop the containers
docker-compose stop
```

Also when you stop the container and start it again, Docker reruns the same saved containers keeping the configurations intact unless we delete them.
