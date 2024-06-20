# authentications_on_java

# Authentication Types

## 1. Inherit auth from parent
Not directly applicable in Spring Security as it is more relevant to API testing tools rather than authentication mechanisms in applications.

## 2. No Auth
Simply configure Spring Security to allow unsecured access to certain endpoints.

**Example:**
```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
            .antMatchers("/public/**").permitAll()
            .anyRequest().authenticated();
    }
}
```
## 3. Basic Auth
Supported by Spring Security

Example:

```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
            .anyRequest().authenticated()
            .and()
            .httpBasic();
    }
}
```
## 4. Bearer Token
Supported by Spring Security

Typically implemented using OAuth 2.0 Resource Server configuration.

Example:

```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
            .anyRequest().authenticated()
            .and()
            .oauth2ResourceServer()
            .jwt();
    }
}
```
## 5. JWT Bearer
Supported by Spring Security

Example:

```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
            .anyRequest().authenticated()
            .and()
            .oauth2ResourceServer()
            .jwt();
    }
}
```
## 6.  Digest Auth
Supported by Spring Security (but not commonly used due to complexity and security concerns)

Example:

```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
            .anyRequest().authenticated()
            .and()
            .httpDigest();
    }
}
```
## 7. OAuth 1.0
Not directly supported by Spring Security as OAuth 1.0 is largely deprecated.

## 8. OAuth 2.0
Supported by Spring Security

Example:

```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
            .anyRequest().authenticated()
            .and()
            .oauth2Login();
    }
}
```
## 9. Hawk Authentication
Not directly supported by Spring Security. Custom implementation would be required.

## 10. AWS Signature
Not directly supported by Spring Security. AWS SDK handles this authentication type.

## 11. NTLM Authentication [Beta]
Supported by Spring Security (with additional configuration)

Example:

```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
            .anyRequest().authenticated()
            .and()
            .authenticationProvider(new NtlmAuthenticationProvider());
    }
}
```
## 12. API Key
Supported by Spring Security (custom implementation required)

Example:

```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
            .anyRequest().authenticated()
            .and()
            .addFilterBefore(new ApiKeyAuthFilter("x-api-key"), BasicAuthenticationFilter.class);
    }
}

public class ApiKeyAuthFilter extends OncePerRequestFilter {
    private String headerName;

    public ApiKeyAuthFilter(String headerName) {
        this.headerName = headerName;
    }

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
        throws ServletException, IOException {
        String apiKey = request.getHeader(headerName);
        if ("your_api_key".equals(apiKey)) {
            // Authenticate the user
        }
        filterChain.doFilter(request, response);
    }
}
```
## 13. Akamai EdgeGrid
Not directly supported by Spring Security. Custom implementation would be required.

14. ASAP (Atlassian)
Not directly supported by Spring Security. Custom implementation would be required.
