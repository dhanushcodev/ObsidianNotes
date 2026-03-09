# Spring Core (Fundamental Module of Spring Framework)

Spring Core is the **foundation of the Spring Framework**. It provides the basic infrastructure required to build Spring applications.

It mainly provides:

- Inversion of Control (IoC)
- Dependency Injection (DI)
- Bean lifecycle management
- Configuration of objects (Beans)

Most Spring modules like **Spring Boot, Spring MVC, Spring Data, Spring Security** are built on top of Spring Core.

---

# 1. Inversion of Control (IoC)

## Concept

Normally in Java, objects create their own dependencies.

### Example (Without Spring)

```java
class Engine {
    void start() {
        System.out.println("Engine started");
    }
}

class Car {
    Engine engine = new Engine();

    void drive() {
        engine.start();
        System.out.println("Car driving");
    }
}
```

### Problem

- Tight coupling
- Hard to test
- Hard to replace dependencies

---

## With Spring (IoC)

Instead of the class creating dependencies, **Spring container creates and injects them**.

```java
class Engine {
    void start() {
        System.out.println("Engine started");
    }
}

class Car {

    Engine engine;

    Car(Engine engine) {
        this.engine = engine;
    }

    void drive() {
        engine.start();
        System.out.println("Car driving");
    }
}
```

Now the **Spring container decides which dependency to provide**.

This is called **Inversion of Control** because control of object creation is transferred from the developer to the Spring container.

---

# 2. Dependency Injection (DI)

Dependency Injection means:

> Spring automatically provides the required dependencies to a class.

There are three types of dependency injection.

---

## 1. Constructor Injection (Recommended)

```java
@Component
class Engine {
}

@Component
class Car {

    private Engine engine;

    @Autowired
    public Car(Engine engine) {
        this.engine = engine;
    }
}
```

Advantages:

- Immutable dependencies
- Easy unit testing
- Recommended in production

---

## 2. Setter Injection

```java
@Component
class Car {

    private Engine engine;

    @Autowired
    public void setEngine(Engine engine) {
        this.engine = engine;
    }
}
```

Used when dependency is **optional**.

---

## 3. Field Injection (Not Recommended)

```java
@Component
class Car {

    @Autowired
    private Engine engine;
}
```

Problems:

- Hard to test
- Breaks encapsulation
- Not recommended for production code

---

# 3. Spring Bean

A **Bean** is an object that is **created and managed by the Spring container**.

Example:

```java
@Component
class Engine {
}
```

Spring automatically creates and manages this object.

---

## Ways to Create Beans

### 1. Using `@Component`

```java
@Component
class UserService {
}
```

Spring scans the classpath and creates the bean automatically.

---

### 2. Using `@Bean`

```java
@Configuration
class AppConfig {

    @Bean
    public Engine engine() {
        return new Engine();
    }
}
```

Used for **third-party classes or manual configuration**.

---

# 4. Spring Container

The **Spring Container** is responsible for managing beans.

Responsibilities:

- Creating objects
- Injecting dependencies
- Managing lifecycle

There are two main container types.

---

## BeanFactory

- Basic container
- Lazy initialization
- Less commonly used

---

## ApplicationContext

Most commonly used container.

Example:

```java
ApplicationContext context =
    new AnnotationConfigApplicationContext(AppConfig.class);

Car car = context.getBean(Car.class);
```

---

# 5. Bean Lifecycle

Lifecycle of a Spring Bean:

```
Container Start
      ↓
Bean Instantiated
      ↓
Dependencies Injected
      ↓
@PostConstruct
      ↓
Bean Ready to Use
      ↓
Application Running
      ↓
@PreDestroy
      ↓
Bean Destroyed
```

Example:

```java
@Component
class DatabaseConnection {

    @PostConstruct
    public void init() {
        System.out.println("Connection created");
    }

    @PreDestroy
    public void destroy() {
        System.out.println("Connection closed");
    }
}
```

---

# 6. Important Spring Core Annotations

| Annotation | Purpose |
|------------|---------|
| `@Component` | Creates a bean |
| `@Autowired` | Inject dependency |
| `@Configuration` | Configuration class |
| `@Bean` | Define bean manually |
| `@ComponentScan` | Scan packages for beans |
| `@PostConstruct` | Run after bean creation |
| `@PreDestroy` | Run before bean destruction |

---

# 7. Specialized Component Annotations

Spring provides specialized stereotypes.

| Annotation | Layer |
|------------|------|
| `@Component` | Generic bean |
| `@Service` | Business logic layer |
| `@Repository` | Data access layer |
| `@Controller` | Web layer |

Example:

```java
@Service
class PaymentService {
}

@Repository
class PaymentRepository {
}
```

---

# 8. Real World Example

Example: **Food Delivery System**

Architecture:

```
UserController
      ↓
OrderService
      ↓
PaymentService
      ↓
PaymentGateway
```

Spring manages dependencies between these components.

```java
@Service
class OrderService {

    private PaymentService paymentService;

    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

Spring automatically injects `PaymentService`.

---

# 9. Benefits of Spring Core

- Loose coupling
- Easy testing
- Dependency management
- Scalable architecture
- Centralized object lifecycle management

---

# 10. Simple Architecture Diagram

```
        Spring Container
              │
              │ manages
              ▼
        ┌─────────────┐
        │   Beans     │
        │-------------│
        │ Engine      │
        │ Car         │
        │ UserService │
        │ OrderRepo   │
        └─────────────┘
              │
              │ Dependency Injection
              ▼
          Application Classes
```

---

# Interview Definition

**Spring Core is the fundamental module of the Spring Framework that provides Inversion of Control (IoC) and Dependency Injection (DI) to manage object creation, configuration, and lifecycle of beans.**