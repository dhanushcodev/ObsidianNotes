# 🌐 1. What is a Servlet?

A **Servlet** is a Java class used to handle HTTP requests and generate responses in a web application.

👉 Think of it like:

- A **controller at the lowest level**
- It directly talks to the **web server (like Apache Tomcat)**

### 🔁 Flow:

1. Client sends request (browser / Postman)
2. Web server forwards it to Servlet
3. Servlet processes it
4. Sends response back

### 🧠 Key Points:

- Runs on a **Servlet Container** (Tomcat, Jetty)
- Handles **GET, POST, PUT, DELETE**
- Lifecycle managed by container

---

## 🔄 Servlet Lifecycle

1. **init()** → called once when servlet is created
2. **service()** → called for every request
3. **destroy()** → called before servlet is removed

---

## 💡 Example (Basic Servlet)

public class MyServlet extends HttpServlet {  
  
    protected void doGet(HttpServletRequest req, HttpServletResponse res) {  
        System.out.println("Handling GET request");  
    }  
}

---

# 🚀 2. What is DispatcherServlet?

The **DispatcherServlet** is the **heart of Spring MVC**.

👉 It is a special Servlet provided by the Spring Framework.

### 🧠 Idea:

Instead of multiple servlets, Spring uses **one central servlet**:  
👉 `DispatcherServlet` = **Front Controller**

---

## 🔁 Request Flow in Spring MVC

Client → DispatcherServlet → Controller → Service → Repository → DB  
                        ← Response ←

---

## 🎯 Responsibilities of DispatcherServlet:

- Receives **all incoming HTTP requests**
- Decides **which controller should handle it**
- Calls controller methods
- Handles response (JSON / View)

---

# 🧩 3. Important Related Terminologies

Let’s break down everything around it 👇

---

## 🧭 3.1 Front Controller Pattern

- Design pattern used in Spring
- One central handler → `DispatcherServlet`

👉 Benefits:

- Centralized request handling
- Cleaner architecture

---

## 🎮 3.2 Controller

- Handles incoming requests

@RestController  
public class UserController {  
  
    @GetMapping("/users")  
    public String getUsers() {  
        return "Users list";  
    }  
}

👉 DispatcherServlet maps request → Controller

---

## 🧠 3.3 Handler Mapping

- Decides **which controller method to call**

Example:

/users → UserController.getUsers()

---

## ⚙️ 3.4 Handler Adapter

- Actually **invokes the controller method**
- Acts as a bridge between DispatcherServlet and Controller

---

## 🧾 3.5 View Resolver

- Resolves view names into actual UI pages

Example:

"home" → /WEB-INF/views/home.jsp

👉 Used in traditional MVC (not much in REST APIs)

---

## 📦 3.6 Model

- Used to pass data from Controller → View

model.addAttribute("name", "Dhanush");

---

## 🎨 3.7 View

- UI part (JSP, Thymeleaf, HTML)

---

## 🧠 3.8 ModelAndView

Combines:

- Model (data)
- View (UI)

return new ModelAndView("home", "name", "Dhanush");

---

## 🔌 3.9 Servlet Container

Software that runs servlets:

- Apache Tomcat
- Jetty
- Undertow

👉 Responsibilities:

- Lifecycle management
- Thread handling
- Request/Response handling

---

## 📡 3.10 HttpServletRequest & HttpServletResponse

### HttpServletRequest:

- Contains client request data
    - headers
    - parameters
    - body

### HttpServletResponse:

- Used to send response
    - status code
    - response body

---

## 🔄 3.11 Filters

- Intercept requests **before reaching servlet**

👉 Example uses:

- Authentication
- Logging
- CORS

public class MyFilter implements Filter {  
    public void doFilter(...) {  
        // pre-processing  
        chain.doFilter(request, response);  
        // post-processing  
    }  
}

---

## 🔐 3.12 Interceptors (Spring)

- Similar to filters but **Spring-specific**

👉 Used for:

- Authentication
- Logging
- Pre/Post processing of controller calls

---

## 🧩 3.13 ServletConfig vs ServletContext

### ServletConfig:

- Config for a specific servlet

### ServletContext:

- Shared data across entire application

---

# 🔥 4. Servlet vs DispatcherServlet

|Feature|Servlet|DispatcherServlet|
|---|---|---|
|Type|Core Java|Spring MVC|
|Control|Developer handles everything|Spring handles flow|
|Mapping|Manual|Automatic|
|Architecture|Low-level|High-level MVC|
|Usage|Rare nowadays|Standard in Spring|

---

# 🧠 Final Intuition

👉 Without Spring:

- You manually write multiple Servlets 😓

👉 With Spring:

- One **DispatcherServlet** handles everything 😎
- You just write controllers

---

# 🌱 1. Spring Container

The **Spring Container** is the core of the Spring Framework.

👉 It is responsible for:

- Creating objects (Beans)
- Injecting dependencies
- Managing lifecycle

### 🧠 Think of it like:

> A factory + manager for all your objects

---

## 🔑 Types of Containers

### 1. BeanFactory (basic)

- Lazy initialization
- Lightweight

### 2. ApplicationContext (advanced) ⭐

- Eager initialization (default)
- More features (events, i18n, etc.)

---

# 🧩 2. ApplicationContext (Very Important)

👉 `ApplicationContext` = **Advanced Spring Container**

ApplicationContext context =   
    new AnnotationConfigApplicationContext(AppConfig.class);

### 🔥 What it does:

- Loads configuration
- Creates beans
- Wires dependencies
- Manages lifecycle

---

## 📦 Types of ApplicationContext

- `AnnotationConfigApplicationContext`
- `ClassPathXmlApplicationContext`
- `WebApplicationContext`

---

# 🔄 3. Spring Bean Lifecycle

This is the lifecycle of an object managed by Spring.

---

## 🧬 Steps:

1. Bean Instantiation  
2. Dependency Injection  
3. BeanNameAware (optional)  
4. BeanFactoryAware (optional)  
5. Pre-Initialization (BeanPostProcessor)  
6. @PostConstruct  
7. Ready to use  
8. @PreDestroy  
9. Destroy

---

## 💡 Example

@Component  
public class MyBean {  
  
    @PostConstruct  
    public void init() {  
        System.out.println("Bean Initialized");  
    }  
  
    @PreDestroy  
    public void destroy() {  
        System.out.println("Bean Destroyed");  
    }  
}

---

## 🧠 Key Idea:

👉 Spring controls **object creation + destruction**

---

# 🚀 4. Spring Boot Lifecycle

Built on top of Spring — but automates everything.

👉 From Spring Boot perspective:

---

## 🔄 Startup Flow

1. main() method runs  
2. SpringApplication.run()  
3. Create ApplicationContext  
4. Auto-configuration starts  
5. Beans created  
6. Embedded server starts (Tomcat)  
7. Application ready

---

## 💡 Example

@SpringBootApplication  
public class App {  
    public static void main(String[] args) {  
        SpringApplication.run(App.class, args);  
    }  
}

---

## 🔥 Important Events

- `ApplicationStartingEvent`
- `ApplicationEnvironmentPreparedEvent`
- `ApplicationReadyEvent`

---

## 🧠 Key Idea:

👉 Spring Boot = Spring + Auto Configuration + Embedded Server (like Apache Tomcat)

---

# 🧠 5. Context vs Container

|Concept|Meaning|
|---|---|
|Container|Manages beans|
|ApplicationContext|Advanced container|

👉 So:

> All ApplicationContexts are containers, but not all containers are ApplicationContexts.

---

# 🗄️ 6. Hibernate (ORM Framework)

👉 Hibernate ORM is used for database interaction.

---

## 🧠 What it does:

- Converts Java objects ↔ Database tables

👉 This concept is called:

> ORM = Object Relational Mapping

---

## 🔄 Example

### Entity:

@Entity  
public class User {  
    @Id  
    private int id;  
    private String name;  
}

### Table:

USER TABLE  
id | name

---

## ⚙️ Hibernate Lifecycle (Simplified)

1. Transient (new object)  
2. Persistent (saved in DB)  
3. Detached (no longer tracked)  
4. Removed (deleted)

---

## 🔥 Hibernate vs Spring

|Feature|Hibernate|Spring|
|---|---|---|
|Purpose|DB handling|App framework|
|Works with|Entities|Beans|
|Manages|Persistence|Lifecycle|

---

# 🔗 7. How Everything Connects

Spring Boot  
   ↓  
Spring Container (ApplicationContext)  
   ↓  
Creates Beans (Controllers, Services, Repositories)  
   ↓  
Repository uses Hibernate  
   ↓  
Hibernate interacts with Database

---

# ⚡ Final Intuition

👉 **Spring Container**  
= creates & manages objects

👉 **ApplicationContext**  
= advanced container

👉 **Spring Lifecycle**  
= how beans are created & destroyed

👉 **Spring Boot Lifecycle**  
= how app starts & runs

👉 **Hibernate**  
= handles database mapping