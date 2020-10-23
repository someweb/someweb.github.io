---
layout: post
title:  "How to give limited permissions to user with RBAC in K8s"
date:   2020-10-21 07:15:19 -0800
categories: devops
tags: [kubernetes, devops, nodejs]
---
![ingress-k8s-nodejs](/assets/images/devops/tls/cert-manager.jpg){: .align-center}

cmds
openssl genrsa -out mike.key 2048
openssl req -new -key mike.key -out mike.csr -subj "/CN=mike/O=developer"
openssl x509 -req -in mike.csr -CA ~/.minikube/ca.crt -CAkey ~/.minikube/ca.key -CAcreateserial -out mike.crt -days 365
kubectl config set-credentials mike --client-certificate=mike.crt --client-key=mike.key
kubectl config set-context mike-context --cluster=minikube --user=mike
kubectl config view

<figure class="align-center">
  <img src="{{ '/assets/images/devops/tls/issued-cert.png' | absolute_url }}" alt="browser-app-running">
  <figcaption>After importing or allowing insecure hosts we can check the cert details and see they match to what we entered in our certificate.yaml</figcaption>
</figure>
