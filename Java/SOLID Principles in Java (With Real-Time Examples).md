
SOLID is a set of 5 object-oriented design principles that help you write:

- Clean code
- Scalable systems
- Maintainable architecture
- Testable applications

---

# 1️⃣ S — Single Responsibility Principle (SRP)

> A class should have only one reason to change.

## ❌ Wrong Design

```java
class Invoice {
    void calculateTotal() { }
    void printInvoice() { }
    void saveToDatabase() { }
}
```

This class:
- Calculates
- Prints
- Saves to DB

Multiple responsibilities ❌

## ✅ Correct Design

```java
class Invoice {
    void calculateTotal() { }
}

class InvoicePrinter {
    void print(Invoice invoice) { }
}

class InvoiceRepository {
    void save(Invoice invoice) { }
}
```

✔ One class = One job  
✔ Easy to test  
✔ Easy to maintain  

## 🧠 Real-Life Example

Restaurant:
- Chef → cooks
- Cashier → billing
- Cleaner → cleaning

Each has one responsibility.

---

# 2️⃣ O — Open/Closed Principle (OCP)

> Open for extension, closed for modification.

You should add new behavior without modifying existing code.

## ❌ Wrong Design

```java
class DiscountCalculator {
    double calculate(String type) {
        if(type.equals("REGULAR")) return 10;
        if(type.equals("PREMIUM")) return 20;
        return 0;
    }
}
```

New type → modify class ❌

## ✅ Correct Design

```java
interface Discount {
    double calculate();
}

class RegularDiscount implements Discount {
    public double calculate() { return 10; }
}

class PremiumDiscount implements Discount {
    public double calculate() { return 20; }
}
```

Add new discount:

```java
class VIPDiscount implements Discount {
    public double calculate() { return 30; }
}
```

✔ No modification required  
✔ Extensible system  

## 🧠 Real-Life Example

Adding new payment methods in an app:
- UPI
- Card
- NetBanking

You shouldn’t modify old code each time.

---

# 3️⃣ L — Liskov Substitution Principle (LSP)

> Subtypes must be replaceable for their base types without breaking behavior.

## ❌ Wrong Design (Refund Example)

```java
class Payment {
    void pay(double amount) { }
    void refund(double amount) { }
}

class CreditCardPayment extends Payment {
    void pay(double amount) { }
    void refund(double amount) { }
}

class CashOnDelivery extends Payment {
    void pay(double amount) { }

    void refund(double amount) {
        throw new UnsupportedOperationException();
    }
}
```

Problem:
If system expects Payment and calls refund(),
CashOnDelivery breaks behavior ❌

## ✅ Correct Design

```java
interface Payment {
    void pay(double amount);
}

interface Refundable {
    void refund(double amount);
}

class CreditCardPayment implements Payment, Refundable {
    public void pay(double amount) { }
    public void refund(double amount) { }
}

class CashOnDelivery implements Payment {
    public void pay(double amount) { }
}
```

Refund only applies to Refundable types:

```java
void processRefund(Refundable payment) {
    payment.refund(100);
}
```

✔ No runtime error  
✔ Correct abstraction  
✔ Proper behavior  

## 🧠 Real-Life Example

Not all payment methods support refund.
System should not assume all payments are refundable.

---

# 4️⃣ I — Interface Segregation Principle (ISP)

> Do not force a class to implement methods it does not use.

## ❌ Wrong Design (User System)

```java
interface UserOperations {
    void login();
    void logout();
    void updateProfile();
    void changePassword();
    void deleteUser();
}
```

GuestUser forced to implement everything ❌

```java
class GuestUser implements UserOperations {
    public void login() { }
    public void logout() { }

    public void updateProfile() {
        throw new UnsupportedOperationException();
    }

    public void changePassword() {
        throw new UnsupportedOperationException();
    }

    public void deleteUser() {
        throw new UnsupportedOperationException();
    }
}
```

## ✅ Correct Design

```java
interface AuthOperations {
    void login();
    void logout();
}

interface ProfileOperations {
    void updateProfile();
    void changePassword();
}

interface AdminOperations {
    void deleteUser();
}

class AdminUser implements AuthOperations, ProfileOperations, AdminOperations {
    public void login() { }
    public void logout() { }
    public void updateProfile() { }
    public void changePassword() { }
    public void deleteUser() { }
}

class GuestUser implements AuthOperations {
    public void login() { }
    public void logout() { }
}
```

✔ Small focused interfaces  
✔ Role-based design  
✔ No dummy methods  

## 🧠 Real-Life Example

Intern ≠ Admin  
Different roles → different permissions.

---

# 5️⃣ D — Dependency Inversion Principle (DIP)

> High-level modules should not depend on low-level modules.  
> Both should depend on abstractions.

## ❌ Wrong Design

```java
class MySQLDatabase {
    void connect() { }
}

class UserService {
    private MySQLDatabase db = new MySQLDatabase();
}
```

Tightly coupled ❌

## ✅ Correct Design

```java
interface Database {
    void connect();
}

class MySQLDatabase implements Database {
    public void connect() { }
}

class MongoDatabase implements Database {
    public void connect() { }
}

class UserService {
    private Database database;

    UserService(Database database) {
        this.database = database;
    }
}
```

Usage:

```java
UserService service = new UserService(new MySQLDatabase());
```

✔ Loosely coupled  
✔ Easy to test  
✔ Easy to switch DB  

## 🧠 Real-Life Example

Switch should not care whether it controls:
- LED bulb
- Smart light
- Tube light

Depend on abstraction, not concrete object.

---

# 🔥 Quick Revision Table

| Principle | Meaning | Memory Trick |
|-----------|----------|--------------|
| S | Single Responsibility | One class = One job |
| O | Open/Closed | Extend, don’t modify |
| L | Liskov Substitution | Child should behave like parent |
| I | Interface Segregation | Small, focused interfaces |
| D | Dependency Inversion | Depend on abstraction |

---

# 🎯 Why SOLID Matters

Without SOLID:
- Code becomes rigid
- Hard to extend
- Hard to test
- High coupling

With SOLID:
- Clean architecture
- Better system design
- Easier maintenance
- Used in real production systems

---

# 🚀 Interview Tip

If interviewer asks:

“What happens if SOLID is violated?”

Answer:
- Tight coupling
- Code duplication
- Fragile system
- Hard refactoring
- Difficult unit testing
