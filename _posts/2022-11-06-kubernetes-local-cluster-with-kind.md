---
layout: post
title: Kubernetes - Local Cluster with KinD
date: '2022-11-06 13:34:46 +0530'
categories: [KUBERNETES]
tags: [k8s, kind]
---
# Getting started with KinD

## Prerequisites

While it can be installed in various approaches as documented in KinD website, but this article will focus on Windows WSL2 based Linux and running KinD as a cluster there.

Please ensure that you have completed the Linux onboarding as documented here: [Alpine on Windows WSL](https://dev.to/bowmanjd/install-docker-on-windows-wsl-without-docker-desktop-34m9)

## Installing KinD

Step 1: Install Required Binaries [Kind Install](https://kind.sigs.k8s.io/docs/user/quick-start#installation)

```bash 
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.17.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```

Step 2: Once step 1 is completed, your local k8s is a command away

`kind create cluster`

For more information you can also visit: https://kind.sigs.k8s.io/docs/user/using-wsl2/