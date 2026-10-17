
A **callback function** is a method or piece of code that you pass to
another method so it can be executed later, usually when a task is
completed or an event occurs.

In Java, callbacks are commonly implemented using:

1.  Interfaces
2.  Lambda expressions
3.  Functional interfaces
4.  Method references

## 1. Implement a callback using an interface

This is the traditional approach and is useful for understanding how
callbacks work internally.

### Step 1: Create a callback interface

``` java
interface Callback {
    void onComplete(String message);
}
```

The `onComplete()` method is the callback method.

### Step 2: Create a method that accepts the callback

``` java
class Task {

    void execute(Callback callback) {
        System.out.println("Task is running...");

        // Task completed
        callback.onComplete("Task completed successfully");
    }
}
```

The `execute()` method receives a callback object and invokes it when
the task finishes.

### Step 3: Pass the callback

``` java
public class Main {

    public static void main(String[] args) {

        Task task = new Task();

        task.execute(new Callback() {
            @Override
            public void onComplete(String message) {
                System.out.println(message);
            }
        });
    }
}
```

Output:

``` text
Task is running...
Task completed successfully
```

### How it works

1.  `Main` calls `task.execute(callback)`.
2.  `Task.execute()` performs the task.
3.  `Task.execute()` calls `callback.onComplete(message)`.
4.  The callback implementation runs and prints the message.

The important point is that `Task` doesn't need to know what the
callback does. It only needs to know that the callback implements
`onComplete()`.

## 2. Implement a callback using a lambda expression

Since `Callback` has only one abstract method, you can use a lambda
instead of an anonymous class.

``` java
interface Callback {
    void onComplete(String message);
}

class Task {

    void execute(Callback callback) {
        System.out.println("Task is running...");

        callback.onComplete("Task completed successfully");
    }
}

public class Main {

    public static void main(String[] args) {

        Task task = new Task();

        task.execute(message ->
            System.out.println(message)
        );
    }
}
```

Output:

``` text
Task is running...
Task completed successfully
```

The lambda:

``` java
message -> System.out.println(message)
```

is the callback implementation.

**Why does this work?** Because the functional interface has exactly one
abstract method, Java knows that the lambda implements `onComplete()`.

## 3. Implement a callback using built-in functional interfaces

You don't always need to create your own interface. Java already
provides functional interfaces in `java.util.function`.

For example, use `Consumer<T>` when the callback accepts a value and
returns nothing.

``` java
import java.util.function.Consumer;

class Task {

    void execute(Consumer<String> callback) {
        System.out.println("Task is running...");

        callback.accept("Task completed");
    }
}

public class Main {

    public static void main(String[] args) {

        Task task = new Task();

        task.execute(message ->
            System.out.println(message)
        );
    }
}
```

Here, `Consumer<String>` acts as the callback.

  Interface          Callback use case
  ------------------ --------------------------------------------
  `Consumer<T>`      Receive a result or message
  `Runnable`         Execute code without input or return value
  `Function<T, R>`   Transform input into a result
  `Predicate<T>`     Check a condition and return `boolean`

## 4. Real-world example: asynchronous callback

Callbacks are particularly useful when a task takes time, such as
downloading data or calling an API.

``` java
interface Callback {
    void onSuccess(String data);
    void onError(Exception error);
}

class DataService {

    void fetchData(Callback callback) {

        new Thread(() -> {
            try {
                Thread.sleep(2000); // Simulate delay

                callback.onSuccess("Data received");
            } catch (Exception e) {
                callback.onError(e);
            }
        }).start();
    }
}

public class Main {

    public static void main(String[] args) {

        DataService service = new DataService();

        service.fetchData(new Callback() {
            @Override
            public void onSuccess(String data) {
                System.out.println(data);
            }

            @Override
            public void onError(Exception error) {
                System.out.println("Failed: " + error.getMessage());
            }
        });

        System.out.println("Main thread continues...");
    }
}
```

Possible output:

``` text
Main thread continues...
Data received
```

The output order may vary depending on scheduling. In this example, the
callback is invoked on the worker thread, not the main thread.

This pattern is common in Android development and asynchronous
operations.

## 5. Which approach should you use?

-   **Custom interface:** When you need multiple callback events, such
    as `onSuccess()` and `onError()`.
-   **Lambda expression:** When you want concise callback code for a
    single-method interface.
-   **Built-in functional interface:** When `Consumer`, `Function`,
    `Predicate`, or `Runnable` fits your use case.
-   **Asynchronous callback:** When an operation completes later, such
    as a network request.

## Interview takeaway

A callback is not a special Java keyword. It is a design pattern in
which you pass behavior to another method and let that method invoke the
behavior at the appropriate time.
