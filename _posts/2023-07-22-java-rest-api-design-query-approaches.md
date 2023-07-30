---
layout: post
title: Java REST API Design - Query Approaches
date: '2023-07-22 23:14:03 +0530'
tags: [Java, REST API, Query Parameters, Examples]
categories: [Technology, Java]
---

In this blog post, we will explore various query approaches for designing Java REST APIs, specifically focusing on handling single and multi-value query parameters with comparison operators. We'll present the different approaches in a tabular format for easy comparison and understanding.

## Query Approaches

| Approach | Description | Example |
|----------|-------------|---------|
| Single Value | Use a single query parameter to filter resources based on one attribute. | `/api/users?role=admin` |
| Multiple Values | Use a comma-separated list in the query parameter for filtering on multiple attribute values. | `/api/users?role=admin,moderator` |
| Comparison Operator - Equal To | Use a query parameter with the attribute name and value for equality comparison. | `/api/users?age=30` |
| Comparison Operator - Not Equal To | Use a query parameter with the attribute name and value for inequality comparison. | `/api/users?status=active` |
| Comparison Operator - Greater Than | Use a query parameter with the attribute name and value for greater than comparison. | `/api/users?score>100` |
| Comparison Operator - Less Than | Use a query parameter with the attribute name and value for less than comparison. | `/api/users?price<50` |
| Comparison Operator - Greater Than or Equal To | Use a query parameter with the attribute name and value for greater than or equal to comparison. | `/api/users?rating>=4.5` |
| Comparison Operator - Less Than or Equal To | Use a query parameter with the attribute name and value for less than or equal to comparison. | `/api/users?discount<=20` |
| Logical Operator - AND | Combine multiple query parameters to filter resources using the logical AND operation. | `/api/users?role=admin&status=active` |
| Logical Operator - OR | Combine multiple query parameters to filter resources using the logical OR operation. | `/api/users?role=admin,moderator` |

## Examples

### Single Value Query

```
GET /api/users?role=admin
```

### Multiple Values Query

```
GET /api/users?role=admin,moderator
```

### Comparison Operator - Equal To

```
GET /api/users?age=30
```

### Comparison Operator - Not Equal To

```
GET /api/users?status=active
```

### Comparison Operator - Greater Than

```
GET /api/users?score>100
```

### Comparison Operator - Less Than

```
GET /api/users?price<50
```

### Comparison Operator - Greater Than or Equal To

```
GET /api/users?rating>=4.5
```

### Comparison Operator - Less Than or Equal To

```
GET /api/users?discount<=20
```

### Logical Operator - AND

```
GET /api/users?role=admin&status=active
```

### Logical Operator - OR

```
GET /api/users?role=admin,moderator
```

## Conclusion

Designing REST APIs with appropriate query approaches for single and multi-value parameters is essential to provide flexible and efficient filtering options to clients. Understanding how to use comparison and logical operators in the query parameters helps build powerful and user-friendly APIs.