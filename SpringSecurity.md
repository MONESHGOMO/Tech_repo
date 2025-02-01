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
  ***

### CSRF (Cross-Site Request Forgery) in Security and Spring Boot

**What is CSRF?**

Cross-Site Request Forgery (CSRF) is a type of attack where a malicious actor tricks a user into performing unwanted actions on a web application in which they are authenticated. The attacker exploits the trust that a website has in the user's browser, causing unauthorized actions on behalf of the user.

**How CSRF Works:**
- An attacker tricks a logged-in user into making an HTTP request (like submitting a form) to a website where the user has valid credentials.
- The malicious request is sent with the user's credentials (like cookies), so the server thinks the request is legitimate.

**CSRF Protection in Spring Security**

Spring Security has built-in support for preventing CSRF attacks. By default, CSRF protection is enabled in Spring Security. Here’s how it works:

**How CSRF Protection Works:**

1. **CSRF Token**: Every request that modifies state (e.g., POST, PUT, DELETE) requires a CSRF token, which is a unique token sent along with the request to validate that the request is coming from an authenticated source.
2. **Token Storage**: The CSRF token is stored in the user's session and is included as a hidden field in forms. It must match the token provided in the HTTP request to prevent CSRF.
3. **Token Verification**: On the server side, Spring Security verifies that the token sent with the request matches the token stored in the session. If it does not match, the request is rejected.

**Disabling CSRF Protection in Spring Boot**
In some cases, such as when using a stateless REST API, you may want to disable CSRF protection. Here’s how to disable CSRF protection in a Spring Boot application:

- Without lamda expression
``` java
package com.gomocodes.gomo.configurations;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.web.SecurityFilterChain;

import java.beans.Customizer;

@Configuration
@EnableWebSecurity
public class SecurityConfiguration {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity httpSecurity) throws Exception {
        return httpSecurity
                .csrf(Customizer-> Customizer.disable())   //spring 6 springboot 3
                .build();
    }
}

```
-With lamda expression
```java
package com.gomocodes.gomo.configurations;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configurers.AbstractHttpConfigurer;
import org.springframework.security.web.SecurityFilterChain;

import java.beans.Customizer;

@Configuration
@EnableWebSecurity
public class SecurityConfiguration {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity httpSecurity) throws Exception {
        return httpSecurity
                .csrf(AbstractHttpConfigurer::disable)  //spring 6 springboot 3
                .build();
    }
}
```

## If we want to get the CSRF Token :
    - enable spring security 
    - go for basic auth
    
    

``` java
@GetMapping("/token")
    public CsrfToken getToken(HttpServletRequest request){
        return (CsrfToken) request.getAttribute("_csrf");
    }
```
- output 
``` json
{
    "parameterName": "_csrf",
    "token": "GhI_Fg38SlTkbAOFM9eirG3czUoLbXiXMvWmudq-Dep_1RkSLncJJDqee2LJWmbmAPqWygjl4CtoD0u6B8bDge6JOdIetisl",
    "headerName": "X-CSRF-TOKEN"
}

```


## Inmemory User
``` java
 @Bean
    public UserDetailsService userDetailsService() {

        UserDetails user1 = User
                .withDefaultPasswordEncoder()   // default password encoder , not use in production ,use only for learning purpose
                .username("kiran")
                .password("kiran@123")
                .roles(("USER"))
                .build(); // return the instance of userDetails
        UserDetails user2 = User
                .withDefaultPasswordEncoder()
                .username("harsh")
                .password("harsh@123")
                .roles(("ADMIN"))
                .build();

        return new InMemoryUserDetailsManager(user1,user2);
    }
```
## Without using default password encoder for in memory user's , we can use the hashed password
``` java
@Bean
    public UserDetailsService userDetailsService() {

        UserDetails user1 = User.builder()
                
                .username("kiran")
                .password("$2a$12$JMObOadZlZSpKVxpPuJxIOAYIRDqACLJKYCdv1bAvMNLJnT9TG31.") // kiran@123
                .roles(("USER"))  
                .build(); // return the instance of userDetails
        UserDetails user2 = User.builder()
                .username("harsh")
                .password("$2a$12$hJtY4OJUyZFe1fDnozaMtOtfy10fsM1HVBNKpYh1y71ubJLu2ua6a") // harsh@123
                .roles(("ADMIN"))
                .build();

        return new InMemoryUserDetailsManager(user1,user2);
    }

```

