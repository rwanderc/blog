---
layout:      post
title:       "BookStore WebServices"
subtitle:    "restful webservices with java and jersey"
description: "RESTful WebServices with Java and Jersey."
date:        2016-05-24 14:00:00
author:      "Wander Costa"
header-img:  "/img/post-bg-bookstore.jpg"
thumb-img:   "/img/thumb/thumb-bookstore.jpg"
---

[github]:https://github.com/rwanderc
[github-project]:https://github.com/rwanderc/BookStore
[postman]:http://www.getpostman.com/

In this post I will explain step-by-step how to create a  simple **BookStore** with RESTful WebServices.

This [code][github-project] is also available in my <i class="fa fa-github"></i> [GitHub][github] page. Feel free to comment if you have any doubt or suggestion. :smile:

For this application, I will use the following softwares, however Maven will take good care of 4 of them:

* Java JDK 8u73;
* NetBeans IDE 8.1 (Build 201510222201);
* Jetty 9.3.8.v20160314;
* Jersey 2.23;
* Jackson 1.9.13;
* Lombok 1.16.8.


### Licensing

This software is licensed under the MIT license, what basically means that you can use it for any purpose ___without any warranty___. :warning:


# Architeture

This application will be able to provide the features of querying, adding, updating and deleting books. All transactions will be available in JSON format.

To decrease complexity of the example, we will simulate the access to the database but will not persist the data into any conventional database. The data will be available within an in-memory database, represented by an abstract class.

Thus, our application will have these important structures:

* **Book**: is the entity class, representing a single book;
* **BookDB**: is the class responsible for handling the in-memory database;
* **BookResource**: is the class responsible for providing the service;
* **pom.xml**: the XML file to configure Maven to use the right dependencies;
* **web.xml**: is the XML to configure the service.

Note that this application intentionally does not guarantee any concern with design patterns. 


# Code

If you are willing to straightly get code, go to [code][github-project]. However if your intention is to understand the step-by-step creation, this post will try to help you.


## Creating the project

You will create a new maven web project, name it **BookStore** and associate it with **NO WEB SERVER**.

![img/bookstore/img1.jpg](/img/bookstore/img1.jpg)

![img/bookstore/img2.jpg](/img/bookstore/img2.jpg)

![img/bookstore/img3.jpg](/img/bookstore/img3.jpg)


## Configuring POM.XML

Go to the **Project Files** and double-click at **pom.xml** file.


### Configuring the dependencies

Now, you need to configure Maven to download and import all the necessary libraries. To do so, you will need to add the following dependencies into the tag **dependencies** in the **pom.xml** file.

<script src="https://gist.github.com/rwanderc/e33a5128e8a88eb020cc661e8baa8a5b.js"></script>

Note that only the dependency **jersey-container-servlet-core** is necessary to provide RESTful web services.
However the dependency **jersey-media-json-jackson** is being used to provide capacity to connect to Jackson's JSON parser. Please, refer to Jersey documentation if you can use other JSON parsers instead of this one, of if you want to provide a XML parser as well. 

Continuing with the other dependencies, we are using Jackson to provide the implementation of the parser.

<script src="https://gist.github.com/rwanderc/1fc31c09cc3ebc3715abff7ff7376764.js"></script>

And finally Lombok (which I recommend to always use in all projects), to provide less code and more funcionality.

<script src="https://gist.github.com/rwanderc/0b256d4480e6c12ec3a9a188a729aa60.js"></script>

After all dependencies added, our dependencies tag will be like this. 

<script src="https://gist.github.com/rwanderc/24d63c00bafe04b11ef6d5b02b3cc539.js"></script>


### Configuring the build

As previously informed, we avoided Netbeans to configure a Server for the project. THe reason is that we will not only configure Maven to manage the build, but we will also configure it to manage the deploy. To do so, we will add a build plug-in into the tag **build** >> **plugins** in the **pom.xml**. Once done, Maven will use this plug-in to run the project, and Netbeans will not be responsible for this task any more.

<script src="https://gist.github.com/rwanderc/c630e0c3da556b5b91738d5a38f27a04.js"></script>


## Configuring WEB.XML

If the file **web.xml** does not exist in the **WEB-INF** folder, you can create it. If it's already created, go to the configuration instructions.

Add a new file into the project, and select a Standard Deployment Descriptor.

![img/bookstore/img4.jpg](/img/bookstore/img4.jpg)

![img/bookstore/img5.jpg](/img/bookstore/img5.jpg)

After adding the file (or if it already exists), it's time to configure it, by inserting the servlet configuration to be used to provide the services and the base URL for the services. In the end, the file must have the following data:

<script src="https://gist.github.com/rwanderc/67dd86e389cb147bc09a85d92b799e14.js"></script>

Note that **servlet >> init-param >> param-value** will contain the base package were the Jersey will search for services to get loaded. So, all services exposed must be under this package (or the child packages), or you must provide many package paths separated by semicolon.

The **servlet-mapping >> url-pattern** describes the base URL for the services to be accessed. Thus, if the application is accessible from the URL **http://localhost:8080/BookStore/**, the services will be available at the URL **http://localhost:8080/BookStore/ws/**.

In general, if you will to have HTML or JSP files, you will need to reserve a sub-directory of the project to your WebService URL. However, if your project will only provide REST functionality (no HTML or JSP files), you may configure **url-pattern** to be accessed directly from root, as the following.

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

