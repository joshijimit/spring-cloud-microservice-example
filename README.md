http://www.kennybastani.com/2015/07/spring-cloud-docker-microservices.html

1) Install Docker toolkit.
2) Unstall VM if on creation of docker machine, hot only adapter won't create.
3) Install VM from installer given in toolkit installation folder.
4) Follow - https://docs.docker.com/machine/get-started/
	a) docker-machine create --driver virtualbox default or docker-machine start default
	b) docker-machine env default - in case of cert error, run docker-machine regenerate-certs default
	c) Execute all statements output from above command in cmd to set env variables.
	d) Check docker ps
	e) Check docker version
5) If you are behind corporate proxy, you need to add proxy in your maven setting.xml file.
6) If everything is ok, you can now run mvn clean install.
7) Once done, cd to docker folder and run docker-compose up
8) Connect to http://docker-ip:8761/ (IP can be found through docker-machine env default)

Run servce individually

- In case you want to run the service individually without using docker, follow below steps.

1) You need to comment out below code of application.java to avoid any binding errors.
	//@EnableDiscoveryClient
	//@EnableZuulProxy
	//@EnableHystrix
2) You can now cd to docker dir of individual service and run below command
	docker run -p 0.0.0.0:9005:9005 kbastani/movie-microservice (9005 is tomcat port)
3) Now you can access it from your URL http://<vm IP>:9005/movies

-------------------------------------------------------
# Spring Cloud Example Project

An example project that demonstrates an end-to-end cloud-native platform using Spring Cloud for building a practical microservices architecture.

Tutorial available here: [Building Microservices with Spring Cloud and Docker](http://www.kennybastani.com/2015/07/spring-cloud-docker-microservices.html)

Demonstrated concepts:

* Integration testing using Docker
* Polyglot persistence
* Microservice architecture
* Service discovery
* API gateway

## Docker

Each service is built and deployed using Docker. End-to-end integration testing can be done on a developer's machine using Docker compose.

## Polyglot Persistence

One of the core concepts of this example project is how polyglot persistence can be approached in practice. Microservices in the project use their own database, while integrating with the data from other services through REST or a message bus.

* Neo4j (graph)
* MongoDB (document)
* MySQL (relational)

## Movie Recommendations

This example project focuses on movies and recommendations.

### Data Services

![http://i.imgur.com/NXLHvjR.png](http://i.imgur.com/NXLHvjR.png)

### Domain Data

![http://i.imgur.com/VlwSw2q.png](http://i.imgur.com/VlwSw2q.png)

## Microservice architecture

This example project demonstrates how to build a new application using microservices, as opposed to a monolith-first strategy. Since each microservice in the project is a module of a single parent project, developers have the advantage of being able to run and develop with each microservice running on their local machine. Adding a new microservice is easy, as the discovery microservice will automatically discover new services running on the cluster.

## Service discovery

This project contains two discovery services, one on Netflix Eureka, and the other uses Consul from Hashicorp. Having multiple discovery services provides the opportunity to use one (Consul) as a DNS provider for the cluster, and the other (Eureka) as a proxy-based API gateway.

## API gateway

Each microservice will coordinate with Eureka to retrieve API routes for the entire cluster. Using this strategy each microservice in a cluster can be load balanced and exposed through one API gateway. Each service will automatically discover and route API requests to the service that owns the route. This proxying technique is equally helpful when developing user interfaces, as the full API of the platform is available through its own host as a proxy.

# License

This project is licensed under Apache License 2.0.
