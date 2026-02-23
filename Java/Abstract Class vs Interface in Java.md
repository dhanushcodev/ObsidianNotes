# 1️⃣ Core Differences

| Feature              | Abstract Class                          | Interface                                      |
| -------------------- | --------------------------------------- | ---------------------------------------------- |
| Purpose              | Base class with shared state + behavior | Contract (what to do)                          |
| Methods              | Abstract + Concrete                     | Abstract (default/static allowed after Java 8) |
| Variables            | Instance variables allowed              | Only public static final constants             |
| Constructor          | Yes                                     | No                                             |
| Multiple Inheritance | Not supported                           | Supported                                      |
| Access Modifiers     | private/protected/public                | Methods are public by default                  |
| Relationship         | IS-A                                    | CAN-DO                                         |
| Inheritance          | extends                                 | implements                                     |

---

# 2️⃣ When To Use What?

## ✅ Use Interface When:
- You need loose coupling
- Multiple implementations required
- Designing plugin systems
- Following Dependency Inversion Principle
- Modeling capabilities (Flyable, Payable, Runnable)
- You want extensibility

## ✅ Use Abstract Class When:
- Classes share common state
- You need common base logic
- Template method pattern is required
- Strong IS-A relationship exists
- Partial implementation should be shared

---

# 3️⃣ Real-Time Example – Payment System (Interface Use Case)

## Problem:
Design a payment system supporting:
- UPI
- Credit Card
- PayPal
- Future payment methods

---

## Step 1: Define Contract

```java
public interface PaymentMethod {
    void pay(double amount);
    void refund(double amount);
}
```

---

## Step 2: Implement Different Payment Types

```java
public class UpiPayment implements PaymentMethod {

    @Override
    public void pay(double amount) {
        System.out.println("Paid using UPI");
    }

    @Override
    public void refund(double amount) {
        System.out.println("Refund to UPI");
    }
}
```

```java
public class CreditCardPayment implements PaymentMethod {

    @Override
    public void pay(double amount) {
        System.out.println("Paid using Credit Card");
    }

    @Override
    public void refund(double amount) {
        System.out.println("Refund to Card");
    }
}
```

---

## Why Interface Here?

- All payment types follow same contract
- Internals are completely different
- Easily extendable (add CryptoPayment later)
- Supports Open-Closed Principle
- Works with Strategy Pattern

---

## System Design Flow

OrderService  
→ PaymentService  
→ PaymentMethod (Interface)  
  ↳ UpiPayment  
  ↳ CreditCardPayment  
  ↳ PaypalPayment  

---

# 4️⃣ Real-Time Example – Vehicle System (Abstract Class Use Case)

## Problem:
All vehicles:
- Have engine
- Have fuel capacity
- Can start()
- Drive logic differs

---

## Step 1: Abstract Base Class

```java
public abstract class Vehicle {

    protected String engineType;
    protected int fuelCapacity;

    public Vehicle(String engineType, int fuelCapacity) {
        this.engineType = engineType;
        this.fuelCapacity = fuelCapacity;
    }

    public void start() {
        System.out.println("Vehicle starting...");
    }

    public abstract void drive();
}
```

---

## Step 2: Concrete Implementation

```java
public class Car extends Vehicle {

    public Car() {
        super("Petrol", 40);
    }

    @Override
    public void drive() {
        System.out.println("Car driving...");
    }
}
```

---

## Why Abstract Class Here?

- Shared state (engineType, fuelCapacity)
- Shared logic (start())
- Strong IS-A relationship
- Partial implementation reuse

---

# 5️⃣ System Design Level Example – Insurance Claim System

## Step 1: Define Contract

```java
public interface ClaimProcessor {
    void validateClaim();
    void calculateAmount();
    void processClaim();
}
```

---

## Step 2: Provide Base Template

```java
public abstract class BaseClaimProcessor implements ClaimProcessor {

    public void log() {
        System.out.println("Logging claim...");
    }

    @Override
    public void processClaim() {
        log();
        validateClaim();
        calculateAmount();
        System.out.println("Claim processed successfully");
    }
}
```

---

## Step 3: Concrete Implementations

- HealthClaimProcessor
- VehicleClaimProcessor
- LifeClaimProcessor

---

## Pattern Used:
Template Method Pattern

---

# 6️⃣ Interview-Level Insight

## Interface = Capability

Examples:
- Runnable
- Serializable
- Comparable
- Flyable
- Payable

It answers:

"What can this object do?"

---

## Abstract Class = Identity

Examples:
- Animal
- Vehicle
- Employee
- Account

It answers:

"What is this object?"

---

# 7️⃣ Golden Rule

If you ask:

"What is this object?"
→ Use Abstract Class

"What can this object do?"
→ Use Interface

---

# 8️⃣ Best Practice (Modern Java)

- Prefer interfaces for flexibility
- Use abstract classes when shared state or template logic is required
- Combine both for clean architecture
- Follow SOLID principles
- Prefer composition over inheritance

---

# 9️⃣ FAANG-Level Architecture Insight

In large-scale systems:

Service Layer → Interface  
Domain Base Layer → Abstract Class  
Concrete Implementations → Business Logic  

This ensures:
- Loose coupling
- Testability
- Scalability
- Easy extension
- Clean Architecture compliance

---

# 🔟 Final Summary

Interface:
- Contract
- Loose coupling
- Multiple inheritance
- Best for system extensibility

Abstract Class:
- Base identity
- Shared state
- Template logic
- Strong inheritance hierarchy
