# 1️⃣ What is Dynamic Method Dispatch?

Dynamic Method Dispatch is the runtime mechanism by which Java determines which overridden method to execute based on the actual object, not the reference type.

It is also called:
- Runtime Polymorphism
- Late Binding

It works only for:
- Overridden methods
- Non-static methods

---

# 2️⃣ Low-Level Design Example  
## Problem: Design a Notification System

### Requirements:
- Support Email, SMS, Push notifications
- Extensible (add WhatsApp later)
- No if-else chains
- Follow SOLID principles

---

## Step 1: Create Abstraction

```java
public interface Notification {
    void send(String message);
}
```

---

## Step 2: Concrete Implementations

```java
public class EmailNotification implements Notification {
    @Override
    public void send(String message) {
        System.out.println("Sending EMAIL: " + message);
    }
}

public class SmsNotification implements Notification {
    @Override
    public void send(String message) {
        System.out.println("Sending SMS: " + message);
    }
}

public class PushNotification implements Notification {
    @Override
    public void send(String message) {
        System.out.println("Sending PUSH: " + message);
    }
}
```

---

## Step 3: Core Business Service

```java
public class NotificationService {

    public void notifyUser(Notification notification, String message) {
        notification.send(message);  // Dynamic Dispatch happens here
    }
}
```

---

## Step 4: Runtime Usage

```java
public class Main {
    public static void main(String[] args) {

        NotificationService service = new NotificationService();

        Notification notification = new EmailNotification();
        service.notifyUser(notification, "Welcome!");

        notification = new SmsNotification();
        service.notifyUser(notification, "OTP: 1234");
    }
}
```

---

## Where Dynamic Dispatch Happens

At this line:

```java
notification.send(message);
```

JVM checks the actual object:
- If EmailNotification → call Email implementation
- If SmsNotification → call SMS implementation

Resolution happens at runtime.

---

# 3️⃣ Why This is Good Design

### ✔ No if-else
Bad design:

```java
if(type == "EMAIL") { ... }
else if(type == "SMS") { ... }
```

Problems:
- Violates Open Closed Principle
- Hard to extend
- Not scalable

### ✔ Easily Extensible

Add WhatsApp:

```java
class WhatsAppNotification implements Notification {
    @Override
    public void send(String message) {
        System.out.println("Sending WhatsApp: " + message);
    }
}
```

No change required in existing service.

---

# 4️⃣ FAANG-Level Explanation

## What Problem Does It Solve?

It decouples:
- What to do (abstraction)
- How to do it (implementation)

Improves:
- Scalability
- Maintainability
- Extensibility

---

## How JVM Implements It

Internally:
- Each class maintains a Virtual Method Table (V-Table)
- At runtime JVM checks the actual object
- Resolves method using vtable lookup

Time Complexity: O(1)

---

# 5️⃣ Real Production Use Cases

## Strategy Pattern

```java
PaymentStrategy strategy = new CreditCardStrategy();
strategy.pay();
```

Used in:
- Pricing engines
- Tax calculation
- Fraud detection

---

## Spring Dependency Injection

```java
@Autowired
PaymentService paymentService;
```

Spring injects implementation at runtime.

---

## JDBC

```java
Connection conn = DriverManager.getConnection();
```

Actual implementation:
- MySQLConnection
- PostgresConnection

Resolved at runtime.

---

# 6️⃣ Interview Traps

## Does Dynamic Dispatch work for variables?

No.

```java
class A {
    int x = 10;
}
class B extends A {
    int x = 20;
}

A obj = new B();
System.out.println(obj.x);  // 10
```

Variables use compile-time binding.

---

## Does it work for static methods?

No. Static methods use compile-time binding.

---

# 7️⃣ FAANG One-Liner

Dynamic method dispatch is the JVM mechanism that enables runtime polymorphism by resolving overridden method calls using the actual object's class via virtual method table lookup, ensuring extensibility and adherence to the Open Closed Principle.

---

# 8️⃣ Key Takeaways

| Feature | Dynamic Dispatch |
|----------|------------------|
| Binding Time | Runtime |
| Requires | Method Overriding |
| Works On | Non-static methods |
| Enables | Runtime Polymorphism |
| Used In | Strategy Pattern, DI, JDBC |

---

End of Notes