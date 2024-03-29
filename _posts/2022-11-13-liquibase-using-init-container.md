---
layout: post
title: 'Liquibase: Using Init Container'
date: '2022-11-13 23:02:42 +0530'
categories: [KUBERNETES, JAVA]
tags: [k8s, spring-boot, docker, liquibase]
---

## Approach 1: 

In this approach we will use Spring Boot application with DB configured to run as startup using Liquibase and running the spring boot application with `WebApplication.NONE` Mode.

[Using Spring Boot in Non Servlet Mode](https://github.com/nayakmk/liquibase-init-k8s/tree/feature/init-using-springboot-none-mode) 

## Approach 2

In this approach we will use Alpine Docker image and then try to run liquibase commands using liquibase commands and using JDBC drivers to establish the connection.

[Using Docker Container with Liquibase](https://github.com/nayakmk/liquibase-init-k8s/tree/feature/init-using-liquibase-container) 

