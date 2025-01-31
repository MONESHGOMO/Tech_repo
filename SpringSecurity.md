### Getting Started with Spring Security

Spring Security is a powerful and highly customizable framework for securing Spring-based applications. It provides comprehensive security features for authentication, authorization, and protection against common security vulnerabilities. Whether you're building a web application, a REST API, or a microservice, Spring Security can help you secure your application effectively.

***Key Features***

**Authentication**
- Supports form-based, OAuth2, JWT, LDAP, and more.

**Authorization**
- Role-based and permission-based access control.
- Method-level security using annotations like `@PreAuthorize` and `@PostAuthorize`.

**Protection Against Attacks**
- CSRF protection.
- Session fixation protection.

**Integration**
- Works seamlessly with Spring Boot, Spring MVC, and Spring WebFlux.
- Extensible for custom security requirements.

**Documentation**
For more details, visit the official Spring Security documentation:  
[Spring Security](https://spring.io/projects/spring-security)


### Setting our own username and password

- In the application.properties file
``` text
spring.application.name=PageTurner
spring.security.user.name=gomo
spring.security.user.password=gomo
```
```java
package com.gomocodes.gomo.controllers;

import jakarta.servlet.http.HttpServletRequest;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HomeController {

    @GetMapping("/")
    public String hello(HttpServletRequest httpServletRequest){
        return "welcome Spring" + " " + "Session ID : " + httpServletRequest.getSession().getId();
    }
}
```
``` output
welcome Spring Session ID : 7454F2459EF2F2C8845F043B22D982D2
```
- Login : ***http://localhost:8080/login***
- userName : gomo
- Password : gomo
