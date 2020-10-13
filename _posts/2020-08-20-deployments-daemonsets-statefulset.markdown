---
layout: post
title:  "When to use Deployments vs Daemonsets vs Statefulsets"
date:   2020-09-20 14:00:45 -0800
categories: js
tags: [kubernetes, devops]
---
![deployments-statefulsets](/assets/images/devops/dep-sta-daem.jpg){: .align-center}

Hello its been a while and throughtout this year have been learning so many new technologies in devops & development. So I have so much material that have been wanting to share for a long time. Here is one of many topics to come from world of kubernetes or as we call it K8s to save some time :D.

In kubernetes we have different components to deploy the applications running in a containers depending on if the application is stateless or if it is an application where we need the containers to have a state for example in case of Databases (eg mongodb) or any applcation that stores data to keep track of its state whereas in stateless application there is no state saved for a pod and each request is treated as new without caring about the state of the application.

## Deployments

For example there is a nodejs application that is connected to mongodb. Now if a request is made to nodejs application container it processes the request with the app logic & algorithm and if needs to save or modify any data then it forwards the request to database which is mongodb in our case which then executes the operation and modify it based on the payload that was in the request. So in other words mongodb is where the state of application lives and nodejs app does not have to care about what is being stored in the DB or the last state of application.

With deployments the K8s puts the pods where the applications run depending on which nodes has the resources required to run the container. Every pod that is created with deployment is assigned a random hash as suffix to its name `node-app-<hashed-id>`. Usually the deployments are for stateless applications but there is way to save the state as well by attaching Volumes. But what ends up happening is all the pods in the replicaset inside the deployment will end up using the same shared volume and end up modifying the same data on disk.

When any new update to the deployment is made the pods running are updated in rolling-update fashion meaning at first the K8s makes sure the new pods are able to succesfuly start with successful health checks and then start terminating the old pods which ensure their is no downtime in our application.


## Statefulsets
As the name suggest the statefulsets are specifically used to make sure the app state is saved by each running pod. In statefulsets each replica pod created has an index number starting from 0 and it will only setup the next replica if the previous one is running. Each new pod in statefulset then have a new PV attached to them to make sure they have their own state and for data persistance and in case one goes down data is not lost. So PV does not depend on the pod life cycle and is always still there even if the pods die and the new ones can then attach back to PV and we still can access the DB. In case of our DB we also have to ensure that there are no DB inconsistencies so we need to make sure the data is always in sync with Master and Slaves. Usually the zero index pod allows to write and read where as the other pods only allows read. They have to continuously make sure any new DB transactions are synced to them. The K8s doesnt however give us the ability to do so out of the box but it can be setup.

StatefulSets don’t create ReplicaSet or anything of that sort, so you cant rollback a StatefulSet to a previous version. You can only delete or scale up/down the Statefulset. 

Its important to note and remember that PV has no namespace and are avbailable to whole cluster. PV should not be tied to specific node and should exist even if cluster dies.

## Daemonset
![daemonsets](/assets/images/devops/daemonsets.png){: .align-center}
These are controlllers which are used to ensure that our pod runs on every node when its deployed. They are used for very special use cases like getting the logging data from all the nodes like Prometheus node exporters,etc. 

The pods from daemonsets can however be prevented from running on a node with help `Taints` on nodes which can sometimes say `NoSchedule` which means the node has either not enough resources or it could be a Master control plane node.

```
taints:
  - effect: NoSchedule
    key: node-role.kubernetes.io/master
```

{% highlight yaml%}

{% endhighlight %}
