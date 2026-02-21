# Polymorphism in Java

## Definition

Polymorphism means **"many forms"**.\
It allows one object to behave differently based on context.
	
In Java, polymorphism is of two types:

1.  Compile-Time Polymorphism (Method Overloading)
2.  Runtime Polymorphism (Method Overriding)

------------------------------------------------------------------------

# 1. Compile-Time Polymorphism (Method Overloading)

## Definition

When multiple methods have the **same name** but **different
parameters** in the same class.

The method call is resolved at **compile time**.

## Example

``` java
class Calculator {

    int add(int a, int b) {
        return a + b;
    }

    double add(double a, double b) {
        return a + b;
    }
}

public class Main {
    public static void main(String[] args) {
        Calculator calc = new Calculator();

        System.out.println(calc.add(5, 3));        // int version
        System.out.println(calc.add(5.5, 3.2));    // double version
    }
}
```

## Key Points

-   Same method name
-   Different parameters
-   No inheritance required
-   Decided by compiler

------------------------------------------------------------------------

# 2. Runtime Polymorphism (Method Overriding)

## Definition

When a child class provides a specific implementation of a method that
is already defined in its parent class.

The method call is resolved at **runtime**.

## Example

``` java
class Payment {
    void pay() {
        System.out.println("Payment processing...");
    }
}

class CreditCardPayment extends Payment {
    @Override
    void pay() {
        System.out.println("Payment done using Credit Card");
    }
}

class UPIPayment extends Payment {
    @Override
    void pay() {
        System.out.println("Payment done using UPI");
    }
}

public class Main {
    public static void main(String[] args) {

        Payment p;

        p = new CreditCardPayment();
        p.pay();   // Calls CreditCardPayment version

        p = new UPIPayment();
        p.pay();   // Calls UPIPayment version
    }
}
```

## Important Concept: Dynamic Method Dispatch

``` java
Payment p = new CreditCardPayment();
```

-   Reference type → Payment
-   Object type → CreditCardPayment
-   JVM decides method at runtime

------------------------------------------------------------------------

# Differences

|Feature|Compile Time|Runtime|
|---|---|---|
|Method|Overloading|Overriding|
|Decided by|Compiler|JVM at runtime|
|Inheritance needed?|❌ No|✅ Yes|
|Based on|Parameters|Object type|

------------------------------------------------------------------------

# Why Runtime Polymorphism is Important

-   Enables loose coupling
-   Supports Open/Closed Principle
-   Improves extensibility
-   Important for scalable system design

------------------------------------------------------------------------

# Interview Tips

-   Overloading → Same class, different parameters
-   Overriding → Parent-child relationship
-   Runtime polymorphism works only with inheritance
-   Method must not be static, final, or private for overriding
