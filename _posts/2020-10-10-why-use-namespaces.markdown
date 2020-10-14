---
layout: post
title:  "Kubernetes: Why use namespaces?"
date:   2020-10-10 10:30:45 -0800
categories: devops
tags: [kubernetes, devops]
---
![kubernetes-namespaces](/assets/images/devops/namespaces.jpg){: .align-center}

In Kubernetes as the application grows, we often end up setting up different environments which could be based on:
- Different application modes like [staging, production, demo]
- Departments
- Geographic regions
- Number of users
- Feature type of application
- Splitting the app based on components like microservices

Namespaces are just like the scope in OOPS and helps us to divide and span the cluster resources among these different environments so that they end up living in their own abstractions away from other things being hosted on same cluster. Another one big benefit of using namespaces is that we can also easily apply resource limits on namespaces so that they dont end up taking too many cluster resources.

It is not necessary to setup new namespaces for your application if its only being used by small number of users or the application doesnt have many components.

In my personal experience when we setup our rails application on a cluster we followed a practice of having namespaces based on following:
- App Env like production, staging
- Monitoring/Logging components like fluentd, grafana, prometheus
- Kubernetes components like nginx-ingress, cert-manager with all in their own namespaces.
- Sandboxes for review apps for demoing or testing.

This allowed us for easy navigation around when debugging especially with tools like Rancher.

### Creating namespaces
```
kubectl create namespace testing
```

This command creates testing namespace and then later on if we are applying any deployments then we can specify the namespace with `-n testing` flag.

```
kubectl apply -f deployment.yaml -n testing
```