---
layout: post
title: 'Spring Boot Thymeleaf with Webjars: Examples and Usage'
date: '2023-06-25 21:06:38 +0530'
categories: [Spring Boot, Thymeleaf, Webjars]
tags: [Spring Boot, Thymeleaf, Webjars, Web Development]
---
Thymeleaf is a popular server-side templating engine for Java-based web applications. It provides seamless integration with Spring Boot, allowing developers to build dynamic and responsive web pages. In this post, we will explore how to leverage Thymeleaf with Webjars to easily include external JavaScript and CSS libraries in your Spring Boot application.

## Prerequisites

Before we begin, make sure you have the following prerequisites:

1. Java Development Kit (JDK) installed on your machine.
2. Spring Boot and Thymeleaf dependencies added to your project.
3. Basic knowledge of Spring Boot and Thymeleaf.

## Setting up Webjars

Webjars is a convenient way to manage client-side web dependencies in your project. To use Webjars with Spring Boot, you need to include the necessary dependencies in your `pom.xml` file:

```xml
<!-- Webjars for JavaScript libraries -->
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>webjars-locator-core</artifactId>
</dependency>

<!-- Webjars for CSS libraries -->
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>webjars-locator</artifactId>
</dependency>
```

Once the dependencies are added, you can include Webjars resources in your Thymeleaf templates.

## Including Webjars Resources in Thymeleaf Templates

To include JavaScript or CSS resources from Webjars in your Thymeleaf templates, follow these steps:

1. Add the Webjars resource in the `<head>` section of your HTML template using the `th:href` attribute:

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <!-- Include Webjars resource -->
    <link th:href="@{/webjars/bootstrap/5.0.0/css/bootstrap.min.css}" rel="stylesheet" />
</head>
<body>
    <!-- Your content here -->
</body>
</html>
```

In this example, we include the Bootstrap CSS library from the Webjars repository.

2. Include the Webjars library in your Spring Boot application by adding the library to the `resources/static` directory or using the `classpath:/static` prefix in the `th:href` attribute:

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <!-- Include Webjars resource -->
    <link th:href="@{/webjars/bootstrap/5.0.0/css/bootstrap.min.css}" rel="stylesheet" />
</head>
<body>
    <!-- Your content here -->
    
    <!-- Include Webjars resource -->
    <script th:src="@{/webjars/jquery/3.6.0/jquery.min.js}"></script>
</body>
</html>
```

In this example, we include both the Bootstrap CSS and jQuery JavaScript libraries from Webjars.

3. Build and run your Spring Boot application. Thymeleaf will automatically resolve the Webjars resources and include them in your web pages.

By following these steps, you can easily include external JavaScript and CSS libraries from Webjars in your Spring Boot Thymeleaf templates.

## Conclusion

In this post, we explored how to use Spring Boot Thymeleaf with Webjars to include external JavaScript and CSS libraries in your web application. By leveraging Webjars, you can easily manage and include popular client-side dependencies in your Spring Boot projects. Thymeleaf's integration with Spring Boot provides a seamless experience for building dynamic and responsive web pages. Happy coding!