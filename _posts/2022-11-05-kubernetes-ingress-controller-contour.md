---
layout: post
title: Kubernetes Ingress Controller - Contour
date: '2022-11-05 18:46:09 +0530'
---
# Kubernetes Ingress Controller - Contour

## Getting Started

### Contour Setup

> kubectl apply -f https://projectcontour.io/quickstar
t/contour.yaml
> kubectl get pods -n projectcontour -o wide --watch
> kubectl get svc -n projectcontour

## Example Project

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

> kubectl describe httpproxy basic

### Test Service

http://localhost:8080/echo