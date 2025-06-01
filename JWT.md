
<h1><center>JSON Web Token</center></h1><br>


``` java
import io.jsonwebtoken.*;
import io.jsonwebtoken.security.Keys;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

import java.nio.charset.StandardCharsets;
import java.security.Key;
import java.util.Date;

@Component
public class JwtUtil {

    @Value("${app.jwt.secret}")
    private String secret;

    @Value("${app.jwt.expiration}")
    private long jwtExpirationInMs; 
    
    
    
    
    private Key getSigningKey() {
        byte[] keyBytes = secret.getBytes(StandardCharsets.UTF_8);
        return Keys.hmacShaKeyFor(keyBytes);
    }

    public String generateToken(String username) {
        return Jwts.builder()
                .setSubject(username)
                .setIssuedAt(new Date())
                .setExpiration(new Date(System.currentTimeMillis() + jwtExpirationInMs))
                .signWith(getSigningKey(), SignatureAlgorithm.HS512)
                .compact();
    }

    public boolean validateToken(String token) {
        try {
            Jwts.parserBuilder()
                    .setSigningKey(getSigningKey())
                    .build()
                    .parseClaimsJws(token);
            return true;
        } catch (JwtException e) {
            return false;
        }
    }

    public String extractUsername(String token) {
        return Jwts.parserBuilder()
                .setSigningKey(getSigningKey())
                .build()
                .parseClaimsJws(token)
                .getBody()
                .getSubject();
    }

    public String extractRole(String token) {
        return (String) Jwts.parserBuilder()
                .setSigningKey(getSigningKey())
                .build()
                .parseClaimsJws(token)
                .getBody()
                .get("role");
    }
}     

```
---



## JWT Utility Class Explained 🔐

This `JwtUtil` class is a Spring component for handling JSON Web Tokens (JWT) in a secure way.  
It provides methods to generate, validate, and extract information from JWTs.

---

### What is this? 🤔

A utility class for:
- **Generating JWT tokens** for authenticated users.
- **Validating** incoming JWT tokens.
- **Extracting** user information (like username and role) from tokens.

---

### Why do we need it? 💡

- **Stateless Authentication:**  
  JWTs allow you to authenticate users without storing session data on the server.
- **Security:**  
  Tokens are signed with a secret key, ensuring data integrity and authenticity.
- **Scalability:**  
  Since no session is stored, your app can scale horizontally with ease.

---

### What happens if you don't use it? 🚫

- You'd have to manage sessions manually, which is less scalable and more error-prone.
- Without proper JWT handling, your app could be vulnerable to security risks (like token forgery).
- Hardcoding secrets or not rotating them can lead to leaks and compromised security.

---

### Internal Working (Behind the Scenes) ⚙️

1. **Secret Injection:**  
   Injects the secret key from your `application.properties` for signing tokens.

2. **Expiration Injection:**  
   Sets how long a token is valid.

3. **Signing Key Generation:**  
   Converts the secret string into a cryptographic key for signing and verifying tokens.

4. **Token Generation:**  
   Creates a JWT with the username as the subject, sets issued and expiration dates, and signs it.

5. **Token Validation:**  
   Parses the token and checks its signature and expiration. Returns `true` if valid.

6. **Extracting Claims:**  
   Retrieves the username and role from the token's payload.

---

### Real-World Example 🌍

**Scenario:**  
A user logs in to your web app.  
- The backend uses `generateToken(username)` to create a JWT and sends it to the frontend.
- On each request, the frontend sends the JWT in the `Authorization` header.
- The backend uses `validateToken(token)` to check if the token is valid.
- If valid, it uses `extractUsername(token)` to identify the user and `extractRole(token)` to check permissions.

**Example Usage:**
```java
// On login
String token = jwtUtil.generateToken("alice");

// On API request
if (jwtUtil.validateToken(token)) {
    String user = jwtUtil.extractUsername(token);
    String role = jwtUtil.extractRole(token);
    // Proceed with user-specific logic
}
```

---

### What if the secret is missing or weak? ⚠️

- **Missing:**  
  The application will fail to start with an error.
- **Weak:**  
  Tokens can be forged, leading to security breaches.  
  **Best Practice:** Use a strong, random secret (at least 64 characters for HS512).

---

### Summary Table

| Method                | Purpose                                 | What if not used?                |
|-----------------------|-----------------------------------------|----------------------------------|
| `generateToken`       | Create JWT for user                     | No stateless authentication      |
| `validateToken`       | Check token validity                    | Accepts invalid/expired tokens   |
| `extractUsername`     | Get username from token                 | Can't identify user from token   |
| `extractRole`         | Get user role from token                | Can't enforce role-based access  |

---

**In short:**  
This class is essential for secure, scalable, and stateless authentication in modern Spring Boot applications! 🚀

---

### Code Example

```java
import io.jsonwebtoken.*;
import io.jsonwebtoken.security.Keys;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

import java.nio.charset.StandardCharsets;
import java.security.Key;
import java.util.Date;

@Component
public class JwtUtil {

    @Value("${app.jwt.secret}")
    private String secret;

    @Value("${app.jwt.expiration}")
    private long jwtExpirationInMs; 

    private Key getSigningKey() {
        byte[] keyBytes = secret.getBytes(StandardCharsets.UTF_8);
        return Keys.hmacShaKeyFor(keyBytes);
    }

    public String generateToken(String username) {
        return Jwts.builder()
                .setSubject(username)
                .setIssuedAt(new Date())
                .setExpiration(new Date(System.currentTimeMillis() + jwtExpirationInMs))
                .signWith(getSigningKey(), SignatureAlgorithm.HS512)
                .compact();
    }

    public boolean validateToken(String token) {
        try {
            Jwts.parserBuilder()
                    .setSigningKey(getSigningKey())
                    .build()
                    .parseClaimsJws(token);
            return true;
        } catch (JwtException e) {
            return false;
        }
    }

    public String extractUsername(String token) {
        return Jwts.parserBuilder()
                .setSigningKey(getSigningKey())
                .build()
                .parseClaimsJws(token)
                .getBody()
                .getSubject();
    }

    public String extractRole(String token) {
        return (String) Jwts.parserBuilder()
                .setSigningKey(getSigningKey())
                .build()
                .parseClaimsJws(token)
                .getBody()
                .get("role");
    }
}
```
### Function Explanations

---

#### 1. `generateToken(String username)`

**What it does:**  
Creates a JWT token for a given username.

**How it works:**  
- Sets the username as the token's subject.
- Adds the current time as the issue date.
- Sets the expiration date based on the configured duration.
- Signs the token using the secret key and HS512 algorithm.

**Why:**  
To provide the client with a signed token that can be used for authentication in future requests.

**Use Case:**  
After a successful login, the server generates a token for the user. The client includes this token in the `Authorization` header for subsequent API calls.

---

#### 2. `validateToken(String token)`

**What it does:**  
Checks if a JWT token is valid.

**How it works:**  
- Parses the token using the secret key.
- Verifies the signature and checks if the token is expired.
- Returns `true` if valid, otherwise `false`.

**Why:**  
To ensure only valid and unexpired tokens are accepted, preventing unauthorized access.

**Use Case:**  
Every time a protected API endpoint is accessed, the server validates the provided token before processing the request.

---

#### 3. `extractUsername(String token)`

**What it does:**  
Extracts the username (subject) from the JWT token.

**How it works:**  
- Parses the token using the secret key.
- Retrieves the `subject` claim from the token payload.

**Why:**  
To identify which user is making the request based on the token.

**Use Case:**  
Used in authentication filters or controllers to get the current user's identity from the token.

---

#### 4. `extractRole(String token)`

**What it does:**  
Extracts the user's role from the JWT token.

**How it works:**  
- Parses the token using the secret key.
- Retrieves the `role` claim from the token payload.

**Why:**  
To determine the user's permissions and access level.

**Use Case:**  
Used for role-based access control, e.g., allowing only admins to access certain endpoints.

---

