---
layout: post
title: Handling CORS in Java with Spring and Spring Boot
date: '2023-06-29 22:25:11 +0530'
categories: [Java, Spring, Spring Boot, CORS]
tags: [Java, Spring, Spring Boot, CORS]
---
Cross-Origin Resource Sharing (CORS) is an important security mechanism that allows web browsers to make cross-origin requests from a web page to a different domain. In this post, we will explore how to handle CORS in Java applications using both Spring and Spring Boot frameworks.

## What is CORS?

CORS is a security mechanism implemented by web browsers to restrict cross-origin requests. By default, web browsers enforce the same-origin policy, which prevents requests from different origins. CORS allows server-side applications to specify which origins are allowed to access their resources.

## Handling CORS in Spring

### Enabling CORS globally

In Spring, you can enable CORS globally by configuring it in your application's `WebMvcConfigurer`. Here's an example:

```java
@Configuration
public class CorsConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
            .allowedOrigins("http://example.com")
            .allowedMethods("GET", "POST", "PUT", "DELETE")
            .allowedHeaders("Content-Type")
            .allowCredentials(true);
    }
}
```

In this example, we enable CORS for all endpoints (`/**`) and specify the allowed origin, methods, headers, and credentials.

### Enabling CORS for specific endpoints

If you need to enable CORS for specific endpoints, you can use the `@CrossOrigin` annotation. Here's an example:

```java
@RestController
public class MyController {

    @CrossOrigin(origins = "http://example.com")
    @GetMapping("/api/data")
    public String getData() {
        // Handle request
    }
}
```

In this example, the `@CrossOrigin` annotation allows requests from `http://example.com` to access the `/api/data` endpoint.

## Handling CORS in Spring Boot

In Spring Boot, handling CORS is simplified through auto-configuration. You can configure CORS properties in your `application.properties` or `application.yml` file. Here's an example using `application.properties`:

```
spring.mvc.cors.enabled=true
spring.mvc.cors.allowed-origins=http://example.com
spring.mvc.cors.allowed-methods=GET,POST,PUT,DELETE
spring.mvc.cors.allowed-headers=Content-Type
spring.mvc.cors.allow-credentials=true
```

In this example, we enable CORS and specify the allowed origins, methods, headers, and credentials.

You can also use the `@CrossOrigin` annotation in Spring Boot similar to the Spring example mentioned earlier.

## Conclusion

In this post, we explored how to handle CORS in Java applications using both Spring and Spring Boot frameworks. We learned how to configure CORS globally or for specific endpoints, and saw examples of the configuration in both Spring and Spring Boot.

Remember to carefully configure CORS to ensure the security of your application. Limit the allowed origins, methods, and headers to trusted sources to prevent potential security risks.

By properly handling CORS, you can allow secure cross-origin communication in your Java applications.

