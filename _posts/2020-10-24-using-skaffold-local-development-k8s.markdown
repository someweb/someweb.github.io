---
layout: post
title:  "Using skaffold for continuous development on local/remote K8s"
date:   2020-10-24 09:15:19 -0800
categories: devops
tags: [kubernetes, devops, skaffold]
---
![skaffold](/assets/images/devops/skaffold/skaffold.gif){: .align-center}

Skaffold is a great continuous development tool that helps test build, push and deploying the application. Especially for developers it easily allow app to be built and deployed to their local cluster or even remote clusters allowing them to easily detect any anomalies that their changes may cause to the building or deployment process. 

Components involved:
- [skaffold](https://skaffold.dev/docs/install/)
- minikube / kind cluster
- app with Dockerfile and your kubernetes manifests

In this quick tutorial we will be continuing the setup on our mynode-app: [https://github.com/matharoo/mynode-app](https://github.com/matharoo/mynode-app).

### Adding skaffold to your app
First we have to ensure that our project has working `Dockerfile` and `k8s` directory with kubernetes manifests like deployments and services at least.

Once you have installed skaffold client on your machine, all we need to do is run `skaffold init` in our projects root dir which generates a skaffold yaml and automatically detect the Dockerfile and K8s manifests in the project. It makes it easy for developers to deploy to different namespaces in your local cluster.

#### Running the app
Now we can just run either of the following commands to deploy to the current selected cluster-context: <br>
- `skaffold dev -n nodejs` if we want to build and deploy the new changes continuosly and watch for any file changes inside our project root dir. And after we exit skaffold it removes the deployments.
<figure class="align-center">
  <img src="{{ '/assets/images/devops/skaffold/dev.jpg' | absolute_url }}" alt="skaffold-dev">
  <figcaption>running skaffold in dev mode continously watches and rebuilds and redeploys</figcaption>
</figure>
- `skaffold run -n nodejs` just create the image once if doesnt exist and deploy it and test.
<figure class="align-center">
  <img src="{{ '/assets/images/devops/skaffold/run.jpg' | absolute_url }}" alt="skaffold-run">
  <figcaption>it builds and deploy to k8s which can then be deleted with `skaffold delete -n nodejs`</figcaption>
</figure>
- `skaffold deploy -n nodejs` to just deploy the pre built image.

These commands generate a new docker image for the project and then automatically deploys it to the cluster according the deployment manifest.

To finally access the app on browser we can use the kubectl port forward method like:
```
kubectl port-forward deployment/mynode-app 3000:3000 -n development
Output:
Forwarding from 127.0.0.1:3000 -> 3000
Forwarding from [::1]:3000 -> 3000
```

Now app can be tested at : [http://localhost:3000/](http://localhost:3000/)

So skaffold can simplify the life of devs who are entering in the realm of devloping and testing their app on kubernetes without even have to worry about learning and running the kubectl commands.
