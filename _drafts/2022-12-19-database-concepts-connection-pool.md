---
layout: post
title: 'Database Concepts: Connection Pool'
date: '2022-12-19 23:55:30 +0530'
---
### Database Connection

The process of the application server connecting to the database server to obtain data is achieved through a mechanism called Database Connection. To build a Database connection, we need to provide the information like server URL that contains the host, port and the name of the database, driver, user name, and password etc.

Once a connection to the Database is created, it can be opened and closed at any time and we can even set the timeout for them.

Creating a connection with the database system with the number of steps involved is an expensive and time-consuming operation and if you create a connection on the fly, the application will hang and users will be experiencing slowness in the page loading. Moreover, if your application has a large scale of users and if you open a connection for every request, the number of simultaneous connections increases which in turn increases the CPU and memory resources which is very dangerous.

That is the reason we have Connection Pools by which we don't create new connections every time but reuse the existing connections. Without a connection pool, a new database connection is created for each client.