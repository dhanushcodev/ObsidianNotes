
A **functional interface** is an interface that contains exactly **one
abstract method**. It is mainly used with lambda expressions and method
references in Java 8 and later.

## 1. Why do we need functional interfaces?

Functional interfaces allow you to pass behavior (code) as an argument
without creating a separate implementation class.

### Without a lambda expression

``` java
interface Calculator {
    int add(int a, int b);
}

class CalculatorImpl implements Calculator {
    @Override
    public int add(int a, int b) {
        return a + b;
    }
}

public class Main {
    public static void main(String[] args) {
        Calculator c = new CalculatorImpl();
        System.out.println(c.add(10, 20)); // 30
    }
}
```

### With a lambda expression

``` java
interface Calculator {
    int add(int a, int b);
}

public class Main {
    public static void main(String[] args) {
        Calculator c = (a, b) -> a + b;
        System.out.println(c.add(10, 20)); // 30
    }
}
```

Here, `(a, b) -> a + b` implements the single abstract method `add()`.

**Key point:** A lambda expression needs a target functional interface
so Java knows which method the lambda implements.

## 2. The `@FunctionalInterface` annotation

Use `@FunctionalInterface` to tell the compiler that an interface is
intended to be functional.

``` java
@FunctionalInterface
interface Greeting {
    void sayHello();
}
```

This is valid because there is only one abstract method.

If you add another unrelated abstract method, the compiler reports an
error.

``` java
@FunctionalInterface
interface Greeting {
    void sayHello();
    void sayBye(); // Compilation error
}
```

The annotation is optional, but it is recommended because it helps catch
mistakes at compile time.

## 3. What methods can a functional interface contain?

A functional interface can have:

-   Exactly one abstract method.
-   Any number of `default` methods.
-   Any number of `static` methods.
-   Methods that override public methods of `Object`, such as
    `toString()`.

Example:

``` java
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b); // Abstract method

    default void display() {
        System.out.println("Calculator");
    }

    static void info() {
        System.out.println("Math operations");
    }
}
```

This is valid because `calculate()` is the only abstract method.

## 4. Four important built-in functional interfaces

Java provides several functional interfaces in the `java.util.function`
package.

### 1. `Predicate<T>`

Tests a condition and returns `boolean`.

``` java
Predicate<Integer> p = n -> n > 10;

System.out.println(p.test(15)); // true
```

Abstract method: `test(T t)`

### 2. `Function<T, R>`

Takes a value and transforms it into another value.

``` java
Function<Integer, Integer> f = n -> n * 2;

System.out.println(f.apply(5)); // 10
```

Abstract method: `apply(T t)`

### 3. `Consumer<T>`

Accepts a value and performs an action without returning a result.

``` java
Consumer<String> c = s -> System.out.println(s);

c.accept("Hello");
```

Abstract method: `accept(T t)`

### 4. `Supplier<T>`

Supplies a value without accepting any input.

``` java
Supplier<String> s = () -> "Hello";

System.out.println(s.get()); // Hello
```

Abstract method: `get()`

## 5. Functional interface comparison

  Interface             Input        Return type   Method
  --------------------- ------------ ------------- ------------
  `Predicate<T>`        One value    `boolean`     `test()`
  `Function<T,R>`       One value    R             `apply()`
  `Consumer<T>`         One value    `void`        `accept()`
  `Supplier<T>`         No input     T             `get()`
  `BiPredicate<T,U>`    Two values   `boolean`     `test()`
  `BiFunction<T,U,R>`   Two values   R             `apply()`
  `BiConsumer<T,U>`     Two values   `void`        `accept()`
  `UnaryOperator<T>`    One value    T             `apply()`
  `BinaryOperator<T>`   Two values   T             `apply()`

## 6. How are functional interfaces used in Streams?

They are commonly used with Java Stream operations.

``` java
import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Integer> numbers =
            Arrays.asList(1, 2, 3, 4, 5, 6);

        numbers.stream()
               .filter(n -> n % 2 == 0)
               .forEach(n -> System.out.println(n));
    }
}
```

What happens here?

1.  `filter()` accepts a `Predicate<Integer>` to select even numbers.
2.  `forEach()` accepts a `Consumer<Integer>` to print each selected
    number.
3.  Output: `2 4 6`

## 7. Functional interface vs ordinary interface

  -----------------------------------------------------------------------
  Functional interface                Ordinary interface
  ----------------------------------- -----------------------------------
  Exactly one abstract method         Can have multiple abstract methods

  Can be implemented using a lambda   Cannot be implemented by a single
                                      lambda if it has multiple abstract
                                      methods

  Often used for passing behavior     Often used to define broader
                                      contracts

  `Runnable`, `Predicate`, `Function` An interface with multiple
  are examples                        unrelated abstract methods is an
                                      example
  -----------------------------------------------------------------------

## 8. Interview question

**Question:** Why can a functional interface have only one abstract
method?

**Answer:** A lambda expression must map unambiguously to a single
abstract method. Having exactly one abstract method gives the compiler a
clear method signature and behavior to implement.

## Quick revision

-   Functional interface = exactly one abstract method.
-   `@FunctionalInterface` is optional but useful for compile-time
    checks.
-   Functional interfaces are commonly used with lambdas and method
    references.
-   `Predicate` → tests a condition (`test()`).
-   `Function` → transforms a value (`apply()`).
-   `Consumer` → performs an action (`accept()`).
-   `Supplier` → supplies a value (`get()`).
-   Functional interfaces are widely used in Streams and event handling.
