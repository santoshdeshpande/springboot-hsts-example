## Introduction

The `hsts` project is a sample learning project to setup a spring boot application to be able to send the hsts header when it is terminated via SSL using a reverse proxy such as Nginx or AWS ALB. 
The project consists of two parts
* Spring Boot Application - This is a standard spring boot application that makes use of spring security to create few paths that can be accessed anonymously and a few authenticated paths. Also the Spring security sets up the http hsts header using the `http.headers().httpStrictTransportSecurity()`. The project is http by default but for the hsts header can be sent only when Spring is able to ensure that the request is secure. The request from nginx to the spring tomcat server is `http` and not `https` Spring security does not set the header. However, by setting the following properties in `application.properties`
  * `server.tomcat.remoteip.remote-ip-header`
  * `server.tomcat.remoteip.protocol-header`
    we will be able to let spring boot detect that the request is secure and set the header. For more information see [here](https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/html/howto-security.html#howto-enable-https)
    
* Nginx configuration - the main configuration is the default.conf and sets up a secure http server using self signed certificates. The default.conf is set as a reverse proxy to the Spring application server and also the following headers are set on the proxy to make sure Spring is able to detect the application as secure.
  * `proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;`
  * `proxy_set_header X-Forwarded-Proto $scheme;`
    
## Running the application
1. `mvn package`
2. `docker-compose up --build`

The access the application using [https://localhost](https://localhost) and when the network tab in the browser is inspected for headers you should see something like this.

* `strict-transport-security: max-age=31536000 ; includeSubDomains`
