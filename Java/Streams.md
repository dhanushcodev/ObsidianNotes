# Java Streams API - Complete Notes with Examples

# 1. What is Stream API?

The Stream API was introduced in Java 8 to process collections of data in a functional and declarative way.

Before Java 8:

```java
List<String> names = Arrays.asList("John", "Alex", "Bob");

for(String name : names) {
    System.out.println(name);
}
```

Using Streams:

```java
names.stream()
     .forEach(System.out::println);
```

A Stream is not a data structure.

It is a sequence of elements supporting operations such as:

- Filtering
- Mapping
- Sorting
- Grouping
- Aggregation
- Reduction

---

# 2. Why Streams?

Benefits:

- Less code
- More readable
- Functional programming style
- Parallel processing support
- No explicit iteration

Example:

Traditional:

```java
List<String> result = new ArrayList<>();

for(String name : names) {
    if(name.startsWith("A")) {
        result.add(name);
    }
}
```

Stream:

```java
List<String> result = names.stream()
                           .filter(name -> name.startsWith("A"))
                           .toList();
```

---

# 3. Stream Pipeline

A Stream pipeline consists of:

```text
Source
   ↓
Intermediate Operations
   ↓
Terminal Operation
```

Example:

```java
names.stream()
     .filter(name -> name.startsWith("A"))
     .map(String::toUpperCase)
     .forEach(System.out::println);
```

Source:
```java
names.stream()
```

Intermediate:
```java
filter()
map()
```

Terminal:
```java
forEach()
```

---

# 4. Creating Streams

## From Collection

```java
List<String> list = List.of("A", "B", "C");

Stream<String> stream = list.stream();
```

---

## From Array

```java
String[] arr = {"A", "B", "C"};

Stream<String> stream = Arrays.stream(arr);
```

---

## Using Stream.of()

```java
Stream<String> stream =
        Stream.of("A", "B", "C");
```

---

## Infinite Stream

```java
Stream<Integer> numbers =
        Stream.iterate(1, n -> n + 1);

numbers.limit(5)
       .forEach(System.out::println);
```

Output:

```text
1
2
3
4
5
```

---

## Generate

```java
Stream.generate(() -> "Hello")
      .limit(3)
      .forEach(System.out::println);
```

Output:

```text
Hello
Hello
Hello
```

---

# 5. Intermediate Operations

Intermediate operations return another stream.

Nothing executes until terminal operation is called.

---

## filter()

Filters data.

```java
List<Integer> nums =
        List.of(1,2,3,4,5,6);

nums.stream()
    .filter(n -> n % 2 == 0)
    .forEach(System.out::println);
```

Output:

```text
2
4
6
```

---

## map()

Transforms data.

```java
List<String> names =
        List.of("john", "alex");

names.stream()
     .map(String::toUpperCase)
     .forEach(System.out::println);
```

Output:

```text
JOHN
ALEX
```

---

## flatMap()

Used when each element contains another collection.

Without flatMap:

```java
List<List<String>> list =
        List.of(
                List.of("A","B"),
                List.of("C","D")
        );

Stream<List<String>> stream =
        list.stream();
```

With flatMap:

```java
list.stream()
    .flatMap(Collection::stream)
    .forEach(System.out::println);
```

Output:

```text
A
B
C
D
```

---

## distinct()

Removes duplicates.

```java
List<Integer> nums =
        List.of(1,2,2,3,3,4);

nums.stream()
    .distinct()
    .forEach(System.out::println);
```

Output:

```text
1
2
3
4
```

---

## sorted()

Ascending:

```java
nums.stream()
    .sorted()
    .forEach(System.out::println);
```

Descending:

```java
nums.stream()
    .sorted(Comparator.reverseOrder())
    .forEach(System.out::println);
```

---

## limit()

```java
Stream.iterate(1, n -> n + 1)
      .limit(5)
      .forEach(System.out::println);
```

Output:

```text
1
2
3
4
5
```

---

## skip()

```java
List.of(1,2,3,4,5)
    .stream()
    .skip(2)
    .forEach(System.out::println);
```

Output:

```text
3
4
5
```

---

# 6. Terminal Operations

Terminal operations trigger execution.

---

## forEach()

```java
list.stream()
    .forEach(System.out::println);
```

---

## collect()

Convert Stream into Collection.

```java
List<String> result =
        names.stream()
             .map(String::toUpperCase)
             .collect(Collectors.toList());
```

Java 16+:

```java
List<String> result =
        names.stream()
             .map(String::toUpperCase)
             .toList();
```

---

## count()

```java
long count =
        names.stream()
             .count();
```

---

## findFirst()

```java
Optional<String> first =
        names.stream()
             .findFirst();
```

---

## findAny()

```java
Optional<String> any =
        names.stream()
             .findAny();
```

Useful in parallel streams.

---

## anyMatch()

```java
boolean exists =
        names.stream()
             .anyMatch(name -> name.equals("Alex"));
```

---

## allMatch()

```java
boolean result =
        nums.stream()
            .allMatch(n -> n > 0);
```

---

## noneMatch()

```java
boolean result =
        nums.stream()
            .noneMatch(n -> n < 0);
```

---

# 7. reduce()

Used to combine all elements into a single value.

Sum:

```java
int sum =
        List.of(1,2,3,4)
            .stream()
            .reduce(0, Integer::sum); //.reduce(0,(a,b)->a+b)

System.out.println(sum);
```

Output:

```text
10
```

---

Maximum:

```java
Optional<Integer> max =
        List.of(5,3,10,1)
            .stream()
            .reduce(Integer::max); // .reduce(0,(a,b)->a>b)
```

---

String Concatenation:

```java
String result =
        List.of("A","B","C")
            .stream()
            .reduce("", String::concat);

System.out.println(result);
```

Output:

```text
ABC
```

---

# 8. Optional with Streams

findFirst() returns Optional.

```java
Optional<String> name =
        names.stream()
             .findFirst();
```

Safe retrieval:

```java
name.ifPresent(System.out::println);
```

Default value:

```java
String value =
        name.orElse("Unknown");
```

---

# 9. Primitive Streams

Avoids boxing/unboxing.

---

## IntStream

```java
IntStream.range(1, 6)
         .forEach(System.out::println);
```

Output:

```text
1
2
3
4
5
```

---

## Sum

```java
int sum =
        IntStream.rangeClosed(1,5)
                 .sum();
```

Output:

```text
15
```

---

## Average

```java
OptionalDouble avg =
        IntStream.of(10,20,30)
                 .average();
```

---

# 10. Collectors

Collectors help transform stream results.

Import:

```java
import java.util.stream.Collectors;
```

---

## toList()

```java
List<String> result =
        names.stream()
             .collect(Collectors.toList());
```

---

## toSet()

```java
Set<String> result =
        names.stream()
             .collect(Collectors.toSet());
```

---

## joining()

```java
String result =
        names.stream()
             .collect(Collectors.joining(", "));
```

Output:

```text
John, Alex, Bob
```

---

## counting()

```java
Long count =
        names.stream()
             .collect(Collectors.counting());
```

---

# 11. Grouping

Most asked interview topic.

---

## Group By Department

```java
class Employee {
    String name;
    String department;
}
```

```java
Map<String, List<Employee>> grouped =
        employees.stream()
                 .collect(Collectors.groupingBy(
                         Employee::getDepartment
                 ));
```

Output:

```text
IT -> [Emp1, Emp2]
HR -> [Emp3]
```

---

## Count Employees By Department

```java
Map<String, Long> result =
        employees.stream()
                 .collect(Collectors.groupingBy(
                         Employee::getDepartment,
                         Collectors.counting()
                 ));
```

Output:

```text
IT=5
HR=2
```

---

# 12. Partitioning

Creates exactly two groups.

```java
Map<Boolean, List<Integer>> result =
        nums.stream()
            .collect(Collectors.partitioningBy(
                    n -> n % 2 == 0
            ));
```

Output:

```text
true  -> [2,4,6]
false -> [1,3,5]
```

---

# 13. Stream Lazy Evaluation

Nothing executes until terminal operation.

```java
names.stream()
     .filter(name -> {
         System.out.println(name);
         return true;
     });
```

Output:

```text
No Output
```

Because terminal operation is missing.

---

# 14. Stream Chaining

```java
List<String> result =
        employees.stream()
                 .filter(emp -> emp.getSalary() > 50000)
                 .map(Employee::getName)
                 .sorted()
                 .toList();
```

Pipeline:

```text
employees
   ↓
filter
   ↓
map
   ↓
sorted
   ↓
toList
```

---

# 15. Parallel Streams

Sequential:

```java
list.stream()
```

Parallel:

```java
list.parallelStream()
```

Example:

```java
list.parallelStream()
    .forEach(System.out::println);
```

Uses ForkJoinPool internally.

---

Advantages:

- Faster for huge datasets
- Uses multiple CPU cores

Disadvantages:

- Thread overhead
- Ordering not guaranteed
- Not always faster

---

# 16. Real Interview Examples

## Find Even Numbers

```java
List<Integer> result =
        nums.stream()
            .filter(n -> n % 2 == 0)
            .toList();
```

---

## Square Numbers

```java
List<Integer> result =
        nums.stream()
            .map(n -> n * n)
            .toList();
```

---

## Sort Names

```java
names.stream()
     .sorted()
     .forEach(System.out::println);
```

---

## Find Maximum

```java
int max =
        nums.stream()
            .max(Integer::compareTo)
            .orElseThrow();
```

---

## Find Minimum

```java
int min =
        nums.stream()
            .min(Integer::compareTo)
            .orElseThrow();
```

---

## Sum All Numbers

```java
int sum =
        nums.stream()
            .reduce(0, Integer::sum);
```

---

## Count Elements

```java
long count =
        nums.stream()
            .count();
```

---

## Convert List To Map

```java
Map<Integer, String> map =
        employees.stream()
                 .collect(Collectors.toMap(
                         Employee::getId,
                         Employee::getName
                 ));
```

---

## Find Duplicate Elements

```java
Set<Integer> seen = new HashSet<>();

List<Integer> duplicates =
        nums.stream()
            .filter(n -> !seen.add(n))
            .toList();
```

---

## Find Second Highest Number

```java
int secondHighest =
        nums.stream()
            .distinct()
            .sorted(Comparator.reverseOrder())
            .skip(1)
            .findFirst()
            .orElseThrow();
```

---

## Group Employees By Department

```java
Map<String, List<Employee>> result =
        employees.stream()
                 .collect(Collectors.groupingBy(
                         Employee::getDepartment
                 ));
```

---

# 17. Your Example Converted to Stream

Traditional:

```java
List<Question> questionsOfQuiz = new ArrayList<>();

for(Question question : questions) {
    if(question.getQuizId().equals(id)) {
        questionsOfQuiz.add(question);
    }
}
```

Using Streams:

```java
List<Question> questionsOfQuiz =
        questions.stream()
                 .filter(question ->
                         question.getQuizId().equals(id))
                 .toList();
```

For Java 8:

```java
List<Question> questionsOfQuiz =
        questions.stream()
                 .filter(question ->
                         question.getQuizId().equals(id))
                 .collect(Collectors.toList());
```

---

# Stream Interview Cheat Sheet

```text
filter()      -> Remove unwanted data
map()         -> Transform data
flatMap()     -> Flatten nested collections
distinct()    -> Remove duplicates
sorted()      -> Sort elements
limit()       -> Take first N elements
skip()        -> Skip first N elements

forEach()     -> Iterate
collect()     -> Convert to List/Set/Map
count()       -> Count elements
reduce()      -> Aggregate to single value
findFirst()   -> First element
findAny()     -> Any element
anyMatch()    -> At least one matches
allMatch()    -> All match
noneMatch()   -> No match

groupingBy()      -> Group records
partitioningBy()  -> Split into true/false groups

stream()          -> Sequential
parallelStream()  -> Parallel
```