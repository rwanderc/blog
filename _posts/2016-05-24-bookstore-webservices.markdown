---
shorturl:    "http://goo.gl/gdvgtj"
layout:      post
title:       "BookStore WebServices"
subtitle:    "restful webservices with java and jersey"
description: "RESTful WebServices with Java and Jersey."
date:        2016-05-24 14:00:00
author:      "Wander Costa"
header-img:  "/img/post-bg-bookstore.jpg"
thumb-img:   "/img/thumb/thumb-bookstore.jpg"
category:    "qlqr coisa"
tags:
- java
- jersey
- jetty
- web service
- webservice
---

[github]:https://github.com/rwanderc
[github-project]:https://github.com/rwanderc/book-store
[postman]:http://www.getpostman.com/

In this post I will explain step-by-step how to create a  simple **BookStore** with RESTful WebServices.

This [code][github-project] is also available in my <i class="fa fa-github"></i> [GitHub][github] page. Feel free to comment if you have any doubt or suggestion. :smile:

For this application, I will use the following softwares:

* Java JDK 8u73;
* NetBeans IDE 8.1 (Build 201510222201);
* Maven 3.3.9.

Maven will download all necessary packages used in the application. The most important are:

* Jersey 2.25.1 for the RESTful implementation;
* Lombok 1.16.16 for productivity;
* JUnit 4.12 for tests;
* Mockito 1.10.19 to tests;
* Apache Commons 2.6 for utility classes;
* Jetty Maven Plugin 9.4.4 to run easily in Jetty;
* JaCoCo 0.7.9 to code coverage report.


### Licensing

This software is licensed under the MIT license, what basically means that you can use it for any purpose ___without any warranty___. :warning:


# Architecture

This application will be able to provide the features of querying, adding, updating and deleting books. All transactions will be available in JSON format.

To decrease complexity of the example, instead of implementing real persistence with database, we will implement a very simple in-memory database, that does not offer good scalability and is not recommended for real use. For real applications, consider using a real database like MySQL, Postgre, MongoDB, etc.

Our application is very simple and will have these important structures:

* **Book**: is the entity class, representing a single book;
* **BookDB**: is the class responsible for handling the in-memory database;
* **BookResource**: is the class responsible for providing the RESTful web service;
* **pom.xml**: the XML file to configure Maven to use the right dependencies;
* **web.xml**: is the XML to configure the service.

Note that this application intentionally has no concern with design patterns, however it follows some MVC conventions.


# Code

If you are willing to straightly get code, go to [code][github-project]. However if your intention is to understand the step-by-step creation, this post will try to help you.


## Creating the project

You will create a new maven web project, name it **BookStore** and associate it to **NO WEB SERVER**, as we will use Jetty plugin to run the application.

![img/bookstore/img1.jpg](/img/bookstore/img1.jpg)

![img/bookstore/img2.jpg](/img/bookstore/img2.jpg)

![img/bookstore/img3.jpg](/img/bookstore/img3.jpg)


## Configuring POM.XML

Go to the **Project Files** and double-click at **pom.xml** file. It's an XML file with all the information that Maven will need to download packages and build the application.


### Configuring the dependencies

Now, you need to configure Maven to download and import all the necessary libraries. To do so, you will need to add the following dependencies into the tag **dependencies** in the **pom.xml** file.

<script src="https://gist.github.com/rwanderc/e33a5128e8a88eb020cc661e8baa8a5b.js"></script>

Note that only the dependency **jersey-container-servlet-core** is necessary to provide RESTful web services.
However the dependency **jersey-media-json-jackson** is being used to provide capacity to connect to Jackson's JSON parser. Please, refer to Jersey documentation if you can use other JSON parser implementation instead of this one, of if you want to provide a XML parser as well.

We need to add Lombok (which I recommend to always use in all projects), to provide less code and more functionality.

<script src="https://gist.github.com/rwanderc/0b256d4480e6c12ec3a9a188a729aa60.js"></script>

And finally, we should add the dependencies that will be used in the tests.

<script src="https://gist.github.com/rwanderc/dda4544b29b589b20799a8126cddb3fc.js"></script>

After all dependencies added, our dependencies tag will be like this.

<script src="https://gist.github.com/rwanderc/24d63c00bafe04b11ef6d5b02b3cc539.js"></script>


### Configuring the build

As previously informed, we avoided Netbeans to configure a Server for the project. The reason is that we will not only configure Maven to manage the build, but we will also configure it to manage the deployment. To do so, we will add a build plug-in into the tag **build** >> **plugins** in the **pom.xml**. Once done, Maven will use this plug-in to run the project, and Netbeans will not be responsible for this task any more. The configuration block is preventing Jetty from keep scanning resources for some time, but it's not mandatory.

<script src="https://gist.github.com/rwanderc/c630e0c3da556b5b91738d5a38f27a04.js"></script>


## Configuring WEB.XML

If the file **web.xml** does not exist in the **WEB-INF** folder, you can create it. If it's already created, go to the configuration instructions.

Add a new file into the project, and select a Standard Deployment Descriptor.

![img/bookstore/img4.jpg](/img/bookstore/img4.jpg)

![img/bookstore/img5.jpg](/img/bookstore/img5.jpg)

After adding the file (or if it already exists), it's time to configure it, by inserting the servlet configuration to be used to provide the services and the base URL for the services. In the end, the file must have the following data:

<script src="https://gist.github.com/rwanderc/67dd86e389cb147bc09a85d92b799e14.js"></script>

Note that **servlet >> init-param >> param-value** will contain the base package were the Jersey will search for services to get loaded. So, all services exposed must be under this package (or the child packages), or you must provide many package paths separated by semicolon.

The **servlet-mapping >> url-pattern** describes the base URL for the services to be accessed. Thus, if the application and services will be accessible from the URL **http://localhost:8080/BookStore/**.

In general, if your application will only provide RESTful resources, you can map the **url-pattern** to be `*/**`, but if you also need to provide static files (e.g.: HTML or JSP files), you will need to map the RESTful resources to a path like `/ws/**` and provide a folder inside the context for the files.

<script src="https://gist.github.com/rwanderc/f41316721b08c0fe9616dd2ab6e5b83b.js"></script>


## The entity

Now, we will create the class **Book**.

Note that the annotation **@Data** comes from Lombok and will generate getters and setters automatically in compilation time.

<script src="https://gist.github.com/rwanderc/018726626b7074320fc387adba50f3ff.js"></script>

In this simple entity, we only have 4 attributes to describe the book. You can add more if you want to.


## In-memory Database

The following class will provide CRUD functionalities to an in-memory database of books, to simplify the project.

<script src="https://gist.github.com/rwanderc/a8b4418bc6886bbe8f448b5107fcca5b.js"></script>


## Adding the service

The service will mostly reflect the CRUD operations from the database to the Book entity, exposing 5 services: find all, find, save, update, delete.

Note that the service consumes and produces JSON data, configured in the class annotations.
It also returns an object of the class **javax.ws.rs.core.Response** in all methods. This class is the default class used by Jersey to provide full control of the responses for the services, so you can configure the HTTP status code and the entity of the response.

<script src="https://gist.github.com/rwanderc/d28424b0e9cc60e660461edfec936261.js"></script>

However, you can also return a personalized entity, decreasing the amount of necessary code within the method. In this case, you will not be able to control the HTTP status code of the response.


# Running

Now you can compile and deploy the project. Jetty must be able to initialize everything and the services will start.
You can run from the Jetty plugin with the following command: `mvn jetty:run`.

To interact with the service, you can use [Postman][postman], which is a very usefull Chrome plug-in to access RESTful services, or you can run the following cURL commands directly from bash:


#### Get all ####
<script src="https://gist.github.com/rwanderc/e3975a329153cc9a9cac0cd9605f61cd.js"></script>


#### Get specific ####
<script src="https://gist.github.com/rwanderc/d44a993bb9ded5e9d3ea3fb1ae0b3208.js"></script>


#### Add ####
<script src="https://gist.github.com/rwanderc/b969bcd1fa5d5da235a878dca25ce9cf.js"></script>


#### Update ####
<script src="https://gist.github.com/rwanderc/928b38bfc3e6d559ee115f00947dbd24.js"></script>


#### Delete ####
<script src="https://gist.github.com/rwanderc/6683c2c14b409c4085d7a94bbe24564b.js"></script>
