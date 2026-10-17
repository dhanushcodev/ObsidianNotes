	## 1. What is a lambda expression?

A **lambda expression** is a concise way to implement a functional
interface without writing a separate class or method.

Before Java 8:

``` java
Runnable r = new Runnable() {
    public void run() {
        System.out.println("Hello");
    }
};
```

Using a lambda:

``` java
Runnable r = () -> System.out.println("Hello");
```

**Remember:** Lambda expressions were introduced in Java 8.

## 2. Lambda syntax

Basic syntax:

``` java
(parameters) -> { body }
```

-   `(parameters)`: input values
-   `->`: lambda operator
-   `{ body }`: code to execute

### Common syntax variations

``` java
// No parameters
() -> System.out.println("Hi");

// One parameter — parentheses optional
x -> x * 2;

// Multiple parameters
(a, b) -> a + b;

// Multiple statements — braces required
(a, b) -> {
    int sum = a + b;
    return sum;
};

// Single expression — return is implicit
(a, b) -> a + b;
```

**Quick rules:** - No parameters: `()` - One parameter: `x -> ...` -
Multiple parameters: `(x, y) -> ...` - Single expression: braces and
`return` can be omitted. - Multiple statements: use `{ }`; use `return`
when returning a value.

## 3. The 4 most important functional interfaces

Java provides built-in functional interfaces in `java.util.function`.

### 1. `Consumer<T>`

Takes input and returns nothing.

``` java
Consumer<String> c = s -> System.out.println(s);
c.accept("Hello");
```

### 2. `Predicate<T>`

Takes input and returns a boolean.

``` java
Predicate<Integer> p = n -> n > 10;
System.out.println(p.test(15)); // true
```

### 3. `Function<T, R>`

Takes input of type `T` and returns type `R`.

``` java
Function<Integer, Integer> f = n -> n * 2;
System.out.println(f.apply(5)); // 10
```

### 4. `Supplier<T>`

Takes no input and returns a value.

``` java
Supplier<Double> s = () -> Math.random();
System.out.println(s.get());
```

## 4. Functional interface reference table

  Interface             Input   Output      Main method
  --------------------- ------- ----------- -------------
  `Predicate<T>`        T       `boolean`   `test()`
  `Function<T,R>`       T       R           `apply()`
  `Consumer<T>`         T       `void`      `accept()`
  `Supplier<T>`         None    T           `get()`
  `UnaryOperator<T>`    T       T           `apply()`
  `BinaryOperator<T>`   T, T    T           `apply()`
  `BiPredicate<T,U>`    T, U    `boolean`   `test()`
  `BiFunction<T,U,R>`   T, U    R           `apply()`
  `BiConsumer<T,U>`     T, U    `void`      `accept()`

**Memory trick:** - Predicate = test a condition. - Function = transform
a value. - Consumer = consume a value. - Supplier = supply a value.

## 5. Lambda expressions with collections

### Sort a list

``` java
List<Integer> nums =
    new ArrayList<>(Arrays.asList(5, 2, 8, 1));

nums.sort((a, b) -> a - b);

System.out.println(nums);
// [1, 2, 5, 8]
```

For safer, clearer sorting, prefer:

``` java
nums.sort(Integer::compareTo);
```

### Filter elements using streams

``` java
List<Integer> nums =
    Arrays.asList(1, 2, 3, 4, 5, 6);

nums.stream()
    .filter(n -> n % 2 == 0)
    .forEach(n -> System.out.println(n));

// 2 4 6
```

### Map values

``` java
nums.stream()
    .map(n -> n * n)
    .forEach(System.out::println);

// 1 4 9 16 25 36
```

### Find the sum

``` java
int sum = nums.stream()
              .mapToInt(n -> n)
              .sum();

System.out.println(sum); // 21
```

## 6. Lambda expressions with threads

``` java
Thread t = new Thread(
    () -> System.out.println("Running in a thread")
);

t.start();
```

A lambda works here because `Runnable` is a functional interface with
one abstract method, `run()`.

## 7. Method references (`::`)

A method reference is a shorter alternative to certain lambda
expressions.

  Lambda                         Method reference
  ------------------------------ -----------------------
  `s -> System.out.println(s)`   `System.out::println`
  `s -> s.toUpperCase()`         `String::toUpperCase`
  `() -> new ArrayList<>()`      `ArrayList::new`
  `s -> Integer.parseInt(s)`     `Integer::parseInt`

Example:

``` java
List<String> names =
    Arrays.asList("Ava", "Raj", "Sam");

names.forEach(System.out::println);
```

## 8. Predicate composition

Predicates can be combined using `and()`, `or()`, and `negate()`.

``` java
Predicate<Integer> positive = n -> n > 0;
Predicate<Integer> even = n -> n % 2 == 0;

System.out.println(positive.and(even).test(8)); // true
System.out.println(positive.or(even).test(-2)); // true
System.out.println(even.negate().test(3));      // true
```

## 9. Common mistakes

  -----------------------------------------------------------------------
  Mistake                             Correct approach
  ----------------------------------- -----------------------------------
  `x -> return x * 2;`                `x -> x * 2`

  Missing parentheses for zero        `() -> doSomething()`
  arguments                           

  Multiple statements without braces  `x -> { ... }`

  Returning a value from a `Consumer` Use `Function` if a result is
                                      needed

  Using a lambda with any ordinary    Target must be a functional
  interface                           interface

  Modifying captured local variables  Captured locals must be final or
                                      effectively final
  -----------------------------------------------------------------------

A **functional interface** has exactly one abstract method. It may still
have default and static methods.

``` java
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);
}

Calculator add = (a, b) -> a + b;

System.out.println(add.calculate(10, 5)); // 15
```

## 10. Quick revision quiz

1.  **Which operator is used in a lambda expression?**\
    Answer: `->`

2.  **Which interface returns a boolean?**\
    Answer: `Predicate`

3.  **Which method executes a `Consumer`?**\
    Answer: `accept()`

4.  **What does `x -> x * 2` mean?**\
    Answer: It takes `x` as input and returns twice its value.

5.  **Which Java version introduced lambda expressions?**\
    Answer: Java 8.

## Final 30-second revision

-   `->` means lambda expression.
-   `::` means method reference.
-   `Predicate` → `test()`
-   `Function` → `apply()`
-   `Consumer` → `accept()`
-   `Supplier` → `get()`
-   `filter()` selects elements.
-   `map()` transforms elements.
-   `forEach()` performs an action on each element.
-   Lambdas were introduced in Java 8.
-   Lambdas implement functional interfaces.
