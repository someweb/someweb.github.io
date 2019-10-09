---
layout: post
title:  "How to dockerize a ReactJS app"
date:   2019-10-09 02:00:51 -0400
categories: reactjs
tags: [js,reactjs,docker]
---
I recently was developing a small demo single page application in ReactJS and was decided to use docker to run. With my setup i was able to run my react app served through nginx. Lets get into how to define the configuration for Docker.

## Step 1 - Defining the Dockerfile.
Create a file named `Dockerfile` inside your react project root directory:
{% highlight dockerfile linenos %}
# Building the app and installing dependencies
FROM node:12.2.0-alpine as build
WORKDIR /app
ENV PATH /app/node_modules/.bin:$PATH
COPY package.json /app/package.json
RUN npm install --silent
COPY . /app
RUN npm run build

# Setting up nginx for prod enivronment
FROM nginx:1.16.0-alpine
COPY --from=build /app/build /app
RUN rm /etc/nginx/conf.d/default.conf
COPY config/default.conf /etc/nginx/conf.d
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
{% endhighlight %}

The line number 2-8 is the first stage of our build process where we specify the node version which is 12 alpine i.e very stripped down version of node which is very beneficial in reducing the size of the container. We specify the `WORKDIR /app` where we will be installing the dependencies and running the app.

Then in line numbers 11-16 is our post build process and this is where we define how we will serve the built project via nginx. Similar to our build process we define the alpine version of nginx. Then in line number we 13 copy over the nginx virtual host config from our project directory.

## Step 2 - Setting up our NGINX config
{% highlight nginx %}
server {

  listen 80;

  location / {
    root   /app;
    index  index.html index.htm;
    try_files $uri $uri/ /index.html;
  }

  error_page   500 502 503 504  /50x.html;

  location = /50x.html {
    root   /usr/share/nginx/html;
  }

}
{% endhighlight %}

Inside the project direcoty I created a config directory where I created a default.config that will be replacing the default.config file inside our containers `/etc/nginx/conf.d` directory as defined in our `Dockerfile` in **Step 1**

## Step 3 - Running Docker (macOS, Linux)
```
cd my-react-project
docker build -t myreactapp .
docker run -it -p 80:80 -d newreact
```
And thats now your react app will be served via port 80 on your localhost. I was able to achieve a file size of just around 22MB which is pretty impressive!. The size will however depend on how big the app is and what libraries are being used. But nevertheless its a great way to run react apps and integrate the development of the app in your CI.