# Spring Security Authentication Complete Notes

# Core Components

Spring Security authentication mainly works using:

1. Authentication
2. AuthenticationManager
3. AuthenticationProvider
4. UserDetailsService
5. UserDetails
6. SecurityContextHolder

---

# Overall Authentication Flow

```text
CLIENT
   ↓
UsernamePasswordAuthenticationFilter
   ↓
AuthenticationManager
   ↓
AuthenticationProvider
   ↓
UserDetailsService
   ↓
Database

If Success:
   ↓
Authenticated Token
   ↓
SecurityContextHolder
   ↓
Controller Access Granted
```

---

# 1. Authentication

`Authentication` represents:

- authentication request
- authenticated user
- current authentication state

It is an interface.

Usually implemented by:

```java
UsernamePasswordAuthenticationToken
```

---

# Example

```java
Authentication auth =
    new UsernamePasswordAuthenticationToken(
        "dhanush",
        "1234"
    );
```

Initially:

```text
username = dhanush
password = 1234
authenticated = false
```

After successful authentication:

```text
authenticated = true
authorities = ROLE_USER
```

---

# UsernamePasswordAuthenticationToken

This is the most commonly used implementation of `Authentication`.

Used for:

- login requests
- authenticated users

---

# Constructor for Login Request

```java
new UsernamePasswordAuthenticationToken(
    username,
    password
);
```

Used before authentication.

---

# Constructor for Authenticated User

```java
new UsernamePasswordAuthenticationToken(
    principal,
    null,
    authorities
);
```

Used after authentication.

---

# 2. AuthenticationManager

`AuthenticationManager` is the main manager responsible for authentication.

It DOES NOT directly authenticate users.

It delegates authentication to `AuthenticationProvider`.

---

# Interface

```java
public interface AuthenticationManager {

    Authentication authenticate(
        Authentication authentication
    ) throws AuthenticationException;
}
```

---

# Flow

```text
Login Request
    ↓
AuthenticationManager
    ↓
AuthenticationProvider
```

---

# Real-World Analogy

```text
AuthenticationManager = Team Lead
AuthenticationProvider = Specialist
UserDetailsService = Database Fetcher
```

The manager delegates work.

---

# 3. AuthenticationProvider

`AuthenticationProvider` performs actual authentication logic.

Responsibilities:

- extract username/password
- fetch user
- validate password
- return authenticated token

---

# Interface

```java
public interface AuthenticationProvider {

    Authentication authenticate(
        Authentication authentication
    );

    boolean supports(Class<?> authentication);
}
```

---

# Internal Steps

## Step 1 — Extract Credentials

```java
String username = authentication.getName();

String password =
    authentication.getCredentials().toString();
```

---

## Step 2 — Load User

```java
UserDetails user =
    userDetailsService.loadUserByUsername(
        username
    );
```

---

## Step 3 — Validate Password

```java
passwordEncoder.matches(
    rawPassword,
    user.getPassword()
);
```

---

## Step 4 — Return Authenticated Token

```java
return new UsernamePasswordAuthenticationToken(
    user,
    null,
    user.getAuthorities()
);
```

---

# 4. UserDetails

`UserDetails` represents authenticated user information.

It is NOT your entity directly.

Contains:

- username
- password
- roles
- account status

---

# Interface

```java
public interface UserDetails {

    Collection<? extends GrantedAuthority>
        getAuthorities();

    String getPassword();

    String getUsername();

    boolean isAccountNonExpired();

    boolean isAccountNonLocked();

    boolean isCredentialsNonExpired();

    boolean isEnabled();
}
```

---

# Custom UserDetails Example

```java
public class CustomUserDetails
        implements UserDetails {

    private User user;

    public CustomUserDetails(User user) {
        this.user = user;
    }

    @Override
    public Collection<? extends GrantedAuthority>
            getAuthorities() {

        return List.of(
            new SimpleGrantedAuthority(
                user.getRole()
            )
        );
    }

    @Override
    public String getPassword() {
        return user.getPassword();
    }

    @Override
    public String getUsername() {
        return user.getEmail();
    }

    @Override
    public boolean isEnabled() {
        return true;
    }
}
```

---

# 5. UserDetailsService

`UserDetailsService` loads user data from DB.

Spring Security NEVER directly talks to DB.

It always uses this service.

---

# Interface

```java
public interface UserDetailsService {

    UserDetails loadUserByUsername(
        String username
    ) throws UsernameNotFoundException;
}
```

---

# Example

```java
@Service
public class CustomUserDetailsService
        implements UserDetailsService {

    @Autowired
    private UserRepository userRepository;

    @Override
    public UserDetails loadUserByUsername(
            String username) {

        User user =
            userRepository.findByEmail(username)
                .orElseThrow(() ->
                    new UsernameNotFoundException(
                        "User not found"
                    )
                );

        return new CustomUserDetails(user);
    }
}
```

---

# 6. Complete Authentication Flow

# STEP 1 — User Sends Login Request

```http
POST /login

{
   "username": "dhanush",
   "password": "1234"
}
```

---

# STEP 2 — Spring Creates Authentication Token

```java
Authentication auth =
    new UsernamePasswordAuthenticationToken(
        username,
        password
    );
```

Currently:

```text
authenticated = false
```

---

# STEP 3 — AuthenticationManager Receives Request

```java
authenticationManager.authenticate(auth);
```

---

# STEP 4 — AuthenticationManager Finds Provider

Usually:

```text
DaoAuthenticationProvider
```

---

# STEP 5 — Provider Calls UserDetailsService

```java
userDetailsService.loadUserByUsername(
    username
);
```

---

# STEP 6 — User Loaded from DB

```text
username = dhanush
password = encoded password
roles = ROLE_USER
```

---

# STEP 7 — Password Validation

```java
passwordEncoder.matches(
    rawPassword,
    encodedPassword
);
```

---

# STEP 8 — Authenticated Token Created

```java
Authentication authenticated =
    new UsernamePasswordAuthenticationToken(
        userDetails,
        null,
        userDetails.getAuthorities()
    );
```

Now:

```text
authenticated = true
```

---

# STEP 9 — Stored in SecurityContextHolder

```java
SecurityContextHolder
    .getContext()
    .setAuthentication(authenticated);
```

Now Spring knows current logged-in user.

---

# 7. SecurityContextHolder

Stores authenticated user information globally.

Accessible anywhere in application.

---

# Example

```java
Authentication auth =
    SecurityContextHolder
        .getContext()
        .getAuthentication();

System.out.println(auth.getName());
```

Output:

```text
dhanush
```

---

# 8. DaoAuthenticationProvider

Default authentication provider provided by Spring Security.

Works with:

- UserDetailsService
- PasswordEncoder

---

# Internal Flow

```text
DaoAuthenticationProvider
    ↓
UserDetailsService
    ↓
Database
```

---

# Important Rule

```text
DaoAuthenticationProvider
    REQUIRES UserDetailsService
```

---

# 9. Can We Skip UserDetailsService?

YES — but only in certain cases.

You can skip it if:

- using custom AuthenticationProvider
- using JWT directly
- using OAuth2
- using LDAP
- manually validating users
- hardcoded users

---

# Example Without UserDetailsService

```java
@Component
public class CustomAuthenticationProvider
        implements AuthenticationProvider {

    @Override  
public Authentication authenticate(Authentication authentication) throws AuthenticationException {  
    String username = (String) authentication.getName();  
    String password = (String) authentication.getCredentials();  
  
    JobPortalUser jobPortalUser = jobPortalUserRepository.findByEmail(username).orElseThrow(  
            ()-> new UsernameNotFoundException("Email not found"));  
  
    var authorities = List.of(  
            new SimpleGrantedAuthority("ROLE_" + jobPortalUser.getRole().getName())  
    );  
  
    if(passwordEncoder.matches(password,jobPortalUser.getPasswordHash())) {  
        return new UsernamePasswordAuthenticationToken(jobPortalUser,null, authorities);  
    }else{  
        throw new BadCredentialsException("Invalid Username or password");  
    }  
  
}

    @Override
    public boolean supports(
            Class<?> authentication) {

        return UsernamePasswordAuthenticationToken
                .class
                .isAssignableFrom(authentication);
    }
}
```

---

# Flow Without UserDetailsService

```text
AuthenticationManager
    ↓
CustomAuthenticationProvider
    ↓
Manual Validation
    ↓
Authenticated Token
```

---

# Important Distinction

```text
DaoAuthenticationProvider
    NEEDS UserDetailsService

CustomAuthenticationProvider
    DOES NOT NEED UserDetailsService
```

---

# 10. Can We Skip AuthenticationProvider?

Normally NO.

Some provider must authenticate users.

Even if you don't define one manually:

```text
Spring automatically creates
DaoAuthenticationProvider
```

when `UserDetailsService` exists.

---

# 11. Security Configuration Example

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Bean
    public AuthenticationProvider authenticationProvider(
            UserDetailsService userDetailsService,
            PasswordEncoder passwordEncoder) {

        DaoAuthenticationProvider provider =
            new DaoAuthenticationProvider();

        provider.setUserDetailsService(
            userDetailsService
        );

        provider.setPasswordEncoder(
            passwordEncoder
        );

        return provider;
    }

    @Bean
    public AuthenticationManager authenticationManager(
            AuthenticationConfiguration config)
            throws Exception {

        return config.getAuthenticationManager();
    }
}
```

---

# 12. Login Service Example

```java
@Service
@RequiredArgsConstructor
public class AuthService {

    private final AuthenticationManager
            authenticationManager;

    public String login(
            LoginRequest request) {

        Authentication authentication =
            authenticationManager.authenticate(

                new UsernamePasswordAuthenticationToken(
                    request.getEmail(),
                    request.getPassword()
                )
            );

        SecurityContextHolder
            .getContext()
            .setAuthentication(authentication);

        return "Login Success";
    }
}
```

---

# 13. JWT Authentication Flow

In JWT authentication:

```text
Request
   ↓
JWT Filter
   ↓
Extract Token
   ↓
Validate Token
   ↓
Load UserDetails (optional)
   ↓
Create Authentication Object
   ↓
SecurityContextHolder
```

No username/password authentication happens on every request.

---

# 14. Multiple AuthenticationProviders

Spring Security supports multiple providers.

Example:

```text
JWT Provider
OAuth Provider
Database Provider
LDAP Provider
```

Spring tries providers one by one.

---

# 15. Authentication vs UserDetails

| Authentication | UserDetails |
|---|---|
| Represents authentication state | Represents user |
| Contains auth status | Contains user data |
| Stored in SecurityContext | Used during authentication |

---

# 16. Most Important Concepts

```text
AuthenticationManager
    does delegation

AuthenticationProvider
    does authentication

UserDetailsService
    loads user

UserDetails
    represents user

Authentication
    represents authentication state
```

---

# 17. Interview Questions

# Q1 — Does AuthenticationManager authenticate directly?

NO.

It delegates to AuthenticationProvider.

---

# Q2 — Why UserDetails instead of Entity?

Because Spring Security expects standard structure.

Also avoids exposing entity directly.

---

# Q3 — What is stored in SecurityContextHolder?

Authenticated Authentication object.

---

# Q4 — Can we have multiple AuthenticationProviders?

YES.

Spring tries them sequentially.

---

# Q5 — Can JWT work without UserDetailsService?

YES.

But many projects still use UserDetailsService to:

- reload roles
- check account status
- invalidate disabled users

---

# Final Summary

```text
AuthenticationManager
    manages authentication flow

AuthenticationProvider
    validates credentials

UserDetailsService
    fetches user from DB

UserDetails
    stores user info

SecurityContextHolder
    stores current authenticated user
```