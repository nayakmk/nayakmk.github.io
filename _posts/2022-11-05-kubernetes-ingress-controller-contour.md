---
layout: post
title: Kubernetes Ingress Controller - Contour
date: '2022-11-05 18:46:09 +0530'
categories: [KUBERNETES, CLOUD]
tags: [k8s, ingress]     # TAG names should always be lowercase
---
# Kubernetes Ingress Controller - Contour

## What is Ingress

Ingress is an API object that manages external access to the services in a cluster, typically HTTP.

Ingress may provide load balancing, SSL termination and name-based virtual hosting.

Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the Ingress resource.

![alt](https://d33wubrfki0l68.cloudfront.net/91ace4ec5dd0260386e71960638243cf902f8206/c3c52/docs/images/ingress.svg)

## What is Ingress Controller

An Ingress controller is responsible for fulfilling the Ingress, usually with a load balancer, though it may also configure your edge router or additional frontends to help handle the traffic.

Ingress objects refer to allowing HTTP or HTTPS traffic through to your cluster services. They do not expose other ports or protocols to the wider world. For this, a service type of LoadBalancer or NodePort should be used.

A service is an external interface to a logical set of Pods. Services use a ‘virtual IP address’ local to the cluster, external services would have no way to access these IP addresses without an Ingress.

## Contour Setup

Contour is an open source Kubernetes ingress controller providing the control plane for the Envoy edge and service proxy.

> kubectl apply -f https://projectcontour.io/quickstar
t/contour.yaml

> kubectl get pods -n projectcontour -o wide --watch

> kubectl get svc -n projectcontour

## Example Project

Instructions to follow: [Using KinD and Contour for Ingress](https://alankrantas.medium.com/trying-out-kubernetes-gateway-api-beta-using-contour-with-kind-b5a6491096c1) 

### Deployment

> kubectl apply -f deploy.yaml

```yaml
# deploy.yaml
#
#
# Deployment
#
apiVersion: apps/v1
kind: Deployment
metadata:
  name: echo-deployment
  namespace: default
spec:
  selector:
    matchLabels:
      app: echo
  replicas: 3
  template:
    metadata:
      labels:
        app: echo
    spec:
      containers:
        - name: echo
          image: ealen/echo-server:latest
          env:
          - name: PORT
            value: "3000"
          ports:
            - containerPort: 3000
---
#
# Service
#
apiVersion: v1
kind: Service
metadata:
  name: echo-service
  namespace: default
spec:
  type: ClusterIP
  selector:
    app: echo
  ports:
    - name: http
      protocol: TCP
      port: 8080
      targetPort: 3000
```

#### Now test it using Port-forward

> kubectl port-forward -n projectcontour service/envoy 8080:80

### Ingress Way

> kubectl apply -f ingress.yaml

```yaml
# ingress.yaml
#
#
# Contour Ingress
#
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: echo-ingress
  namespace: default
  annotations:
    kubernetes.io/ingress.class: contour
spec:
  rules:
    - http:
        paths:
          - path: /echo
            pathType: Prefix
            backend:
              service:
                name: echo-service
                port:
                  number: 8080
```

### Contour HTTPProxy as Alternative CRD

> kubectl apply -f httpproxy.yaml 

```yaml
# httpproxy.yaml
#
#
# Contour HTTPProxy
#
apiVersion: projectcontour.io/v1
kind: HTTPProxy
metadata:
  name: basic
spec:
  virtualhost:
    fqdn: localhost
  routes:
    - conditions:
      - prefix: /echo
      services:
        - name: echo-service
          port: 8080
```

#### Describe HTTPProxy

If you want to know if the Ingress is misconfigured or configured properly then you can describe it and see the details.

> kubectl describe httpproxy basic

### Test Service

http://localhost:8080/echo