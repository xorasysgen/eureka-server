# Spring Boot application
Spring Boot application to become a full Microservices Architecture, using Spring Cloud Eureka, Ribbon, Zuul and Hystrix to implement Service Discovery, Load Balancing, the API Gateway pattern and a Circuit Breaker

Spring Cloud Netflix Stack Component : The patterns provided include 
1. Service Discovery (Eureka Lib)
2. Circuit Breaker (Hystrix Lib)  #fault tolerance 
3. Intelligent Routing (Zuul Lib) #Zuul to Proxy your Microservices, It provides a unified “front door” to your System
4. Client Side Load Balancing (Ribbon Lib).

Service Discovery (Eureka) - summary
......................................................................................................................................
In a microservices application, the set of running service instances changes dynamically. Instances have dynamically assigned network locations. Consequently, in order for a client to make a request to a service it must use a service-discovery mechanism.

> A key part of service discovery is the service registry.
> The service registry is a database of available service instances.
> The service registry provides a management API and a query API.
> Service instances are registered with and deregistered from the service registry using the management API.
> The query API is used by system components to discover available service instances.


There are two main service-discovery patterns: 
1. client-side discovery - in a system clients query the service registry, select an available instance, and make a request.
2. service-side discovery- in a system clients make requests via a router, which queries the service registry and forwards the request to an available instance.

point by point 
1.Netflix Eureka is good example of a service registry. It provides a REST API for registering and querying service instances.

2. The service registry is a key part of service discovery. It is a database containing the network locations of service instances. A service registry needs to be highly available and up to date.

3. A service instance registers its network location using a POSTrequest. Every 30 seconds it must refresh its registration using a PUT request. A registration is removed by either using an HTTP DELETE request or by the instance registration timing out. As you might expect, a client can retrieve the registered service instances by using an HTTP GET request.

4. The Eureka client handles all aspects of service instance registration and deregistration.


----------------begin How to Include Eureka Client. Using the EurekaClient---------------
 1.use the starter with a group ID of [org.springframework.cloud] and an artifact ID of [spring-cloud-starter-netflix-eureka-client]
 2.@EnableDiscoveryClient


 Registering with Eureka
When a client registers with Eureka, it provides meta-data about itself — such as host, port, health indicator URL, home page, and other details. Eureka receives heartbeat messages from each instance belonging to a service. If the heartbeat fails over a configurable timetable, the instance is normally removed from the registry.

----------------end How to Include Eureka Client. Using the EurekaClient---------------



----------------begin How to Include Eureka Server. Using the Eureka Server---------------
1.use the starter with a group ID of [org.springframework.cloud] and an artifact ID of [spring-cloud-starter-netflix-eureka-server]
2.@EnableEurekaServer
3. The server has a home page with a UI and HTTP API endpoints for the normal Eureka functionality under /eureka/*.

----------------end How to Include Eureka Server. Using the Eureka Server---------------

FEW IMPORTANT POINTS#
# By default, every Eureka server is also a Eureka client and requires (at least one) service URL to locate a peer. If you do not provide it, the service runs and works, but it fills your logs with a lot of noise about not being able to register with the peer.



Links : https://blog.heroku.com/managing_your_microservices_on_heroku_with_netflix_s_eureka
......................................................................................................................................
_______________________________________________________________________________________________________________________________________
# Circuit Breaker (Hystrix)
Circuit Breaker (Hystrix) # Hystrix to implement circuit breaker while invoking underlying microservice. Hystrix is a latency and fault tolerance library designed to isolate points of access to remote systems, services and 3rd party libraries, stop cascading failure and enable resilience in complex distributed systems where failure is inevitable. 

In other words Hystrix is  required to enable fault tolerance in the application where some underlying service is down/throwing error permanently, we need to fall back to different path of program execution automatically. 

Defensive Programming with Short Circuit Breaker Pattern

https://spring.io/guides/gs/circuit-breaker/
https://dzone.com/articles/microservice-spring-cloud-eureka-server-configurat
Hystrix configuration is done in four major steps.

1. Add Hystrix starter and dashboard dependencies.   - spring-cloud-starter-hystrix &  spring-cloud-starter-hystrix-dashboard
2. Add @EnableCircuitBreaker (annotation at application classs below @SpringBootApplication)
3. Add @EnableHystrixDashboard annotation
4. Add annotation @HystrixCommand(fallbackMethod = "myFallbackMethod")

# Hystrix
Hystrix is designed to:

1. Provide protection and control over failures and latency from services typically accessed over the network
2. Stop cascading of failures resulting from some of the services being down
3. Fail fast and rapidly recover
4. Degrade gracefully where possible
5. Real time monitoring and alerting of command center on failures

Turbine - @EnableTurbine
Looking at an individual instance’s Hystrix data is not very useful in terms of the overall health of the system. Turbine is an application that aggregates all of the relevant /hystrix.stream endpoints into a combined /turbine.stream for use in the Hystrix Dashboard. Individual instances are located through Eureka. Running Turbine requires annotating your main class with the @EnableTurbine annotation


summary : Netflix’s Hystrix library provides an implementation of the Circuit Breaker pattern , when we apply a circuit breaker to a method, Hystrix watches for failing calls to that method, and if failures build up to a threshold, Hystrix opens the circuit so that subsequent calls automatically fail. While the circuit is open, Hystrix redirects calls to the method, and they’re passed on to our specified fallback method.

Imp links 
1. : https://github.com/Netflix/Hystrix/tree/master/hystrix-contrib/hystrix-javanica#configuration
2. : https://spring.io/guides/gs/circuit-breaker/
_______________________________________________________________________________________________________________________________________
***************************************************************************************************************************************
https://dzone.com/articles/spring-cloud-netflix-zuul-edge-serverapi-gatewayga
https://blog.heroku.com/using_netflix_zuul_to_proxy_your_microservices
https://spring.io/guides/gs/routing-and-filtering/

#Zuul is a JVM-based router and server side load balancer by Netflix.
#Using Netflix Zuul to Proxy your Microservices


Routing is an integral part of a microservice architecture. For example, /api/users is mapped to the user service and /api/shop is mapped to the shop service. Zuul is a JVM-based router and server side load balancer by Netflix.


# Using Netflix Zuul to Proxy your Microservices

It provides a unified “front door” to your system, which allows a browser, mobile app, or other user interface to consume services from multiple hosts without managing cross-origin resource sharing (CORS) and authentication for each one. You can integrate Zuul with other Netflix projects like Hystrix for fault tolerance and Eureka for service discovery, or use it to manage routing rules, filters, and load balancing across your system.

1. it provides a unified “front door” to your system.
2. Use it to manage routing rules, filters, and load balancing across your system.
3. If Zuul is connected to a Eureka server, it can automatically add fault tolerance and client-side load balancing to the services it proxies.



# Client Side Load Balancer: Ribbon

1. Ribbon is a client-side load balancer that gives you a lot of control over the behavior of HTTP and TCP clients.

2. To include Ribbon in your project, use the starter with a group ID of [org.springframework.cloud] and an artifact ID of 
[spring-cloud-starter-netflix-ribbon]

3. Ribbon API works based on the concept called “Named Client”. While configuring Ribbon in our application configuration file we provide a name for the list of servers included for the load balancing.

# Let’s take it for a spin.

Complete microsystem example

Link:  https://thepracticaldeveloper.com/2017/06/27/hystrix-fallback-with-zuul-and-spring-boot/
github : https://github.com/microservices-practical/microservices-v8
https://medium.com/netflix-techblog/netflix-oss-and-spring-boot-coming-full-circle-4855947713a0
