# Spring Boot AOP (Aspect-Oriented Programming) — Complete Notes

---

# 1. What is AOP?

## Definition
AOP (Aspect-Oriented Programming) is a programming paradigm used to separate **cross-cutting concerns** from the main business logic.

Cross-cutting concerns are functionalities used in multiple places such as:

- Logging
- Security
- Transactions
- Exception handling
- Performance monitoring
- Auditing

Instead of writing the same code repeatedly, AOP allows you to write it once and apply it everywhere.

---

# 2. Why AOP?

Without AOP:

```java
public void saveUser() {
    System.out.println("Logging started");

    // business logic

    System.out.println("Logging ended");
}
```

You repeat logging in every method.

With AOP:

```java
@Aspect
public class LoggingAspect {

    @Before("execution(* com.example.service.*.*(..))")
    public void log() {
        System.out.println("Logging...");
    }
}
```

Now logging is automatically applied to all methods.

---

# 3. Real World Example

Imagine:

- Business logic = Cooking food
- Logging/Security = Washing hands

You should wash hands before cooking every time.

Instead of writing washing logic repeatedly, AOP automatically does it before cooking.

---

# 4. Important Terminologies

---

## 4.1 Aspect

A class containing cross-cutting logic.

Example:

```java
@Aspect
@Component
public class LoggingAspect {

}
```

---

## 4.2 Advice

Action performed by aspect.

Types:

| Advice | Executes |
|---|---|
| @Before | Before method |
| @After | After method |
| @AfterReturning | After successful execution |
| @AfterThrowing | After exception |
| @Around | Before + After (full control) |

---

## 4.3 Join Point

A point during execution where AOP can be applied.

Usually:
- Method execution

Example:
```java
public void saveUser()
```

This method execution is a join point.

---

## 4.4 Pointcut

Expression that selects methods where advice should run.

Example:

```java
execution(* com.example.service.*.*(..))
```

Meaning:
- Any return type
- Any method
- Any parameters
- Inside service package

---

## 4.5 Weaving

Process of connecting aspect with target object.

Done by Spring at runtime using proxies.

---

## 4.6 Target Object

Original object where aspect is applied.

Example:
```java
UserService
```

---

## 4.7 Proxy Object

Spring creates a proxy around target object.

Calls go through proxy first.

```text
Client → Proxy → Actual Object
```

Proxy executes aspect logic.

---

# 5. How Spring AOP Works Internally

Spring uses:

- JDK Dynamic Proxy
OR
- CGLIB Proxy

---

## JDK Dynamic Proxy

Used when class implements interface.

```text
Interface → Proxy → Implementation
```

---

## CGLIB Proxy

Used when no interface exists.

Creates subclass dynamically.

---

# 6. Dependencies

## Maven

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

---

# 7. Enable AOP

Spring Boot automatically enables AOP when starter dependency is added.

No extra configuration needed usually.

---

# 8. Basic Example

---

## Service Class

```java
@Service
public class UserService {

    public void saveUser() {
        System.out.println("User saved");
    }
}
```

---

## Aspect Class

```java
@Aspect
@Component
public class LoggingAspect {

    @Before("execution(* com.example.service.UserService.saveUser(..))")
    public void beforeAdvice() {
        System.out.println("Before method execution");
    }
}
```

---

## Output

```text
Before method execution
User saved
```

---

# 9. execution() Expression Explained

---

## Syntax

```text
execution(modifier return-type package.class.method(args))
```

---

## Example

```java
execution(* com.example.service.*.*(..))
```

Meaning:

| Part | Meaning |
|---|---|
| * | Any return type |
| com.example.service | Package |
| * | Any class |
| * | Any method |
| (..) | Any arguments |

---

# 10. Types of Advice

---

# 10.1 @Before

Runs before method execution.

```java
@Before("execution(* com.example.service.*.*(..))")
public void logBefore() {
    System.out.println("Before method");
}
```

---

# 10.2 @After

Runs after method completes (success or exception).

```java
@After("execution(* com.example.service.*.*(..))")
public void afterMethod() {
    System.out.println("After method");
}
```

---

# 10.3 @AfterReturning

Runs only if method succeeds.

```java
@AfterReturning("execution(* com.example.service.*.*(..))")
public void afterReturning() {
    System.out.println("Method successful");
}
```

---

## Returning Value

```java
@AfterReturning(
    pointcut = "execution(* com.example.service.*.*(..))",
    returning = "result"
)
public void afterReturning(Object result) {
    System.out.println(result);
}
```

---

# 10.4 @AfterThrowing

Runs when exception occurs.

```java
@AfterThrowing(
    pointcut = "execution(* com.example.service.*.*(..))",
    throwing = "ex"
)
public void afterException(Exception ex) {
    System.out.println(ex.getMessage());
}
```

---

# 10.5 @Around

Most powerful advice.

Controls:
- Before
- After
- Return value
- Exception
- Method execution

---

## Example

```java
@Around("execution(* com.example.service.*.*(..))")
public Object aroundAdvice(ProceedingJoinPoint joinPoint) throws Throwable {

    System.out.println("Before");

    Object result = joinPoint.proceed();

    System.out.println("After");

    return result;
}
```

---

# 11. ProceedingJoinPoint

Used only with `@Around`.

Allows actual method execution.

```java
joinPoint.proceed();
```

Without it:
- original method never executes

---

# 12. Access Method Information

---

## Method Name

```java
joinPoint.getSignature().getName()
```

---

## Arguments

```java
joinPoint.getArgs()
```

---

## Full Example

```java
@Before("execution(* com.example.service.*.*(..))")
public void log(JoinPoint joinPoint) {

    String methodName = joinPoint.getSignature().getName();

    System.out.println(methodName);
}
```

---

# 13. Reusable Pointcuts

---

## Define Pointcut

```java
@Pointcut("execution(* com.example.service.*.*(..))")
public void serviceMethods() {

}
```

---

## Use Pointcut

```java
@Before("serviceMethods()")
public void log() {

}
```

---

# 14. Multiple Pointcuts

---

## AND

```java
@Before("pointcut1() && pointcut2()")
```

---

## OR

```java
@Before("pointcut1() || pointcut2()")
```

---

## NOT

```java
@Before("!pointcut1()")
```

---

# 15. Package-Level AOP

---

## Entire Package

```java
execution(* com.example.service.*.*(..))
```

---

## Subpackages Also

```java
execution(* com.example.service..*.*(..))
```

---

# 16. AOP with Custom Annotation

Very common in real projects.

---

# Step 1: Create Annotation

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface LogExecutionTime {

}
```

---

# Step 2: Use Annotation

```java
@LogExecutionTime
public void saveUser() {

}
```

---

# Step 3: Aspect

```java
@Around("@annotation(LogExecutionTime)")
public Object logTime(ProceedingJoinPoint joinPoint) throws Throwable {

    long start = System.currentTimeMillis();

    Object result = joinPoint.proceed();

    long end = System.currentTimeMillis();

    System.out.println(end - start);

    return result;
}
```

---

# 17. Measuring Execution Time

```java
@Around("execution(* com.example.service.*.*(..))")
public Object measure(ProceedingJoinPoint joinPoint) throws Throwable {

    long start = System.currentTimeMillis();

    Object result = joinPoint.proceed();

    long end = System.currentTimeMillis();

    System.out.println("Execution Time: " + (end - start));

    return result;
}
```

---

# 18. Exception Logging Example

```java
@AfterThrowing(
        pointcut = "execution(* com.example.service.*.*(..))",
        throwing = "ex"
)
public void logException(Exception ex) {

    System.out.println("Exception: " + ex.getMessage());
}
```

---

# 19. Order of Advice Execution

Use:

```java
@Order(1)
```

Lower number = higher priority.

---

## Example

```java
@Aspect
@Component
@Order(1)
public class SecurityAspect {

}
```

---

# 20. AOP in Spring Boot Real Projects

Common usages:

| Feature | Usage |
|---|---|
| Logging | Log requests/responses |
| Security | Role validation |
| Transactions | Database consistency |
| Monitoring | Execution time |
| Auditing | Track changes |
| Validation | Common validation |
| Retry Logic | Retry failed APIs |

---

# 21. Spring Transaction Internally Uses AOP

Example:

```java
@Transactional
public void transferMoney() {

}
```

Spring internally creates proxy.

Transaction starts before method.
Transaction commits after success.

---

# 22. Limitations of Spring AOP

---

## Only Method-Level

Spring AOP works only on:
- Method execution

Not:
- Constructors
- Fields

---

## Self Invocation Problem

```java
public void method1() {
    method2();
}
```

If `method2()` has AOP:
- It will NOT trigger

Because internal calls bypass proxy.

---

# 23. Full Working Example

---

## Controller

```java
@RestController
public class UserController {

    @Autowired
    private UserService userService;

    @GetMapping("/save")
    public String save() {

        userService.saveUser();

        return "done";
    }
}
```

---

## Service

```java
@Service
public class UserService {

    public void saveUser() {

        System.out.println("Saving user...");
    }
}
```

---

## Aspect

```java
@Aspect
@Component
public class LoggingAspect {

    @Before("execution(* com.example.service.*.*(..))")
    public void before() {

        System.out.println("Before execution");
    }

    @After("execution(* com.example.service.*.*(..))")
    public void after() {

        System.out.println("After execution");
    }
}
```

---

## Output

```text
Before execution
Saving user...
After execution
```

---

# 24. Important Interview Questions

---

## What is AOP?

Separates cross-cutting concerns from business logic.

---

## Difference Between OOP and AOP

| OOP | AOP |
|---|---|
| Handles business logic | Handles cross-cutting concerns |
| Uses classes/objects | Uses aspects |
| Vertical separation | Horizontal separation |

---

## Why Does Spring Use Proxy?

To intercept method calls and apply advice.

---

## Difference Between Filter, Interceptor, and AOP

| Feature | Filter | Interceptor | AOP |
|---|---|---|---|
| Works On | Servlet request | Spring MVC request | Method execution |
| Layer | Servlet | Controller | Service/business |
| Spring Specific | No | Yes | Yes |

---

# 25. Best Practices

---

## Keep Aspects Small

Each aspect should have one responsibility.

---

## Use Custom Annotations

Cleaner than large execution expressions.

---

## Avoid Heavy Logic in Aspects

AOP should support business logic, not replace it.

---

## Prefer @Around Carefully

`@Around` is powerful but can become complex.

---

# 26. Summary

---

## AOP Flow

```text
Client
   ↓
Proxy Object
   ↓
Aspect Logic
   ↓
Target Method
```

---

## Core Concepts

| Concept | Meaning |
|---|---|
| Aspect | Cross-cutting class |
| Advice | Action |
| Join Point | Execution point |
| Pointcut | Method selector |
| Weaving | Linking aspect |
| Proxy | Interceptor object |

---

# 27. Common Pointcut Expressions

---

## Any Method

```java
execution(* *(..))
```

---

## Specific Package

```java
execution(* com.example.service.*.*(..))
```

---

## Specific Method

```java
execution(* com.example.service.UserService.saveUser(..))
```

---

## Methods Starting with save

```java
execution(* save*(..))
```

---

## Methods Returning String

```java
execution(String *(..))
```

---

# 28. Advanced Example — Logging All API Requests

```java
@Aspect
@Component
public class ApiLoggingAspect {

    @Before("within(@org.springframework.web.bind.annotation.RestController *)")
    public void log(JoinPoint joinPoint) {

        System.out.println(
                "API Called: " +
                joinPoint.getSignature().getName()
        );
    }
}
```

---

# 29. Around Advice Flow

```text
Before Logic
     ↓
joinPoint.proceed()
     ↓
Actual Method
     ↓
After Logic
```

---

# 30. Internal Working Summary

```text
Spring Container
      ↓
Creates Bean
      ↓
Creates Proxy
      ↓
Applies Aspect Logic
      ↓
Returns Proxy Bean
```

---

# 31. Complete Dependency + Structure

---

## Project Structure

```text
src/main/java
    ├── aspect
    │      └── LoggingAspect
    ├── controller
    ├── service
    └── SpringBootApplication
```

---

## Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

---

# 32. Most Important Concepts to Remember

---

## Spring AOP Uses Proxy

No proxy → No AOP.

---

## Internal Method Calls Skip AOP

```java
this.method()
```

bypasses proxy.

---

## @Around Requires proceed()

Without:

```java
joinPoint.proceed();
```

original method won't execute.

---

# 33. Mini Cheat Sheet

---

## Aspect

```java
@Aspect
@Component
```

---

## Before Advice

```java
@Before("execution(* package.*.*(..))")
```

---

## Around Advice

```java
@Around("execution(* package.*.*(..))")
```

---

## Pointcut

```java
@Pointcut("execution(* package.*.*(..))")
```

---

## Access Method Name

```java
joinPoint.getSignature().getName()
```

---

# 34. Final Real Project Example

Suppose:

```text
Banking Application
```

Requirements:
- Log every transaction
- Measure execution time
- Handle exceptions
- Security checks

Instead of repeating code everywhere:

```text
AOP handles all centrally
```

That is why AOP is extremely important in enterprise Spring Boot applications.