# 📚 Java Collections – Detailed Notes with Examples

## ⚐ What is the Java Collections Framework?

Java Collections Framework (JCF) is a **set of classes and interfaces** that implement commonly reusable **data structures** such as **lists, sets, queues, maps**, etc.

---

## ✅ Core Interfaces in Java Collections

|Interface|Description|
|---|---|
|`Collection`|Root interface of the collection hierarchy|
|`List`|Ordered collection (can contain duplicates)|
|`Set`|Unordered collection (no duplicates)|
|`Queue`|FIFO collection|
|`Deque`|Double-ended queue|
|`Map`|Stores key-value pairs|

---

## 📃 List Interface

### 🔹 Implementations:

- `ArrayList`
    
- `LinkedList`
    
- `Vector` (legacy)
    

### 🔸 Example:

```java
import java.util.*;

public class ListExample {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("Apple");
        list.add("Banana");
        list.add("Apple"); // Duplicates allowed
        System.out.println(list); // [Apple, Banana, Apple]
    }
}
```

### 🔹 Common List CRUD Methods:

- `add(E e)`
    
- `add(int index, E element)`
    
- `get(int index)`
    
- `set(int index, E element)`
    
- `remove(int index)` or `remove(Object o)`
    
- `contains(Object o)`
    
- `size()`
    
- `clear()`
    
- `isEmpty()`
    

---

## 🗑 Set Interface

### 🔹 Implementations:

- `HashSet` (no order)
    
- `LinkedHashSet` (insertion order)
    
- `TreeSet` (sorted order)
    

### 🔸 Example:

```java
import java.util.*;

public class SetExample {
    public static void main(String[] args) {
        Set<String> set = new HashSet<>();
        set.add("Apple");
        set.add("Banana");
        set.add("Apple"); // Duplicate, won't be added
        System.out.println(set); // Output order not guaranteed
    }
}
```

### 🔹 Common Set CRUD Methods:

- `add(E e)`
    
- `remove(Object o)`
    
- `contains(Object o)`
    
- `size()`
    
- `clear()`
    
- `isEmpty()`
    
- `iterator()`
    

---

## 🗑 Queue Interface

### 🔹 Implementations:

- `LinkedList` (also a List)
    
- `PriorityQueue` (elements are ordered by priority)
    

### 🔸 Example:

```java
import java.util.*;

public class QueueExample {
    public static void main(String[] args) {
        Queue<Integer> queue = new LinkedList<>();
        queue.add(10);
        queue.add(20);
        System.out.println(queue.poll()); // 10 (removed)
        System.out.println(queue.peek()); // 20 (head element)
    }
}
```

### 🔹 Common Queue CRUD Methods:

- `add(E e)` / `offer(E e)`
    
- `remove()` / `poll()`
    
- `element()` / `peek()`
    
- `isEmpty()`
    
- `size()`
    

---

## 🔐 PriorityQueue Example

```java
import java.util.*;

public class PriorityQueueExample {
    public static void main(String[] args) {
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        pq.add(30);
        pq.add(10);
        pq.add(20);

        while (!pq.isEmpty()) {
            System.out.println(pq.poll()); // 10 20 30 (ascending order)
        }
    }
}
```

---

## 🔐 Deque Interface (Double-ended Queue)

### 🔹 Implementations:

- `ArrayDeque`
    

### 🔸 Example:

```java
import java.util.*;

public class DequeExample {
    public static void main(String[] args) {
        Deque<String> deque = new ArrayDeque<>();
        deque.addFirst("Start");
        deque.addLast("End");
        System.out.println(deque); // [Start, End]
    }
}
```

### 🔹 Common Deque CRUD Methods:

- `addFirst(E e)` / `addLast(E e)`
    
- `removeFirst()` / `removeLast()`
    
- `getFirst()` / `getLast()`
    
- `peekFirst()` / `peekLast()`
    
- `isEmpty()`
    
- `clear()`
    

---

## 🖘️ Map Interface (Key-Value Pairs)

### 🔹 Implementations:

- `HashMap` (no order)
    
- `LinkedHashMap` (insertion order)
    
- `TreeMap` (sorted order by keys)
    
- `Hashtable` (legacy, thread-safe)
    

### 🔸 Example:

```java
import java.util.*;
public class MapExample {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        map.put("Alice", 90);
        map.put("Bob", 80);
        System.out.println(map.get("Alice")); // 90
    }
}
```

### 🔹 Common Map CRUD Methods:

- `put(K key, V value)`
    
- `get(Object key)`
    
- `remove(Object key)`
    
- `containsKey(Object key)`
    
- `containsValue(Object value)`
    
- `keySet()`
    
- `values()`
    
- `entrySet()`
    
- `size()`
    
- `clear()`
    
- `isEmpty()`
    

---

## ⚙️ Utility Class – `Collections`

- Static methods for sorting, searching, reversing, etc.

### 🔸 Example:

```java
import java.util.*;

public class CollectionsUtility {
    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(3, 1, 2);
        Collections.sort(list);
        System.out.println(list); // [1, 2, 3]
    }
}
```

---

## 🧕 Thread-safe Collections

- `Vector`, `Hashtable` (legacy)
- `Collections.synchronizedList()`
- `ConcurrentHashMap` (modern concurrent map)

---

## 📌 Key Differences Summary

|Feature|List|Set|Map|
|---|---|---|---|
|Order|Yes|Depends|Depends|
|Duplicates|Allowed|Not Allowed|Keys: No, Values: Yes|
|Key-Value|No|No|Yes|
## Summary Table

|Interface|Implementation|Order|Duplicates|Thread-Safe|
|---|---|---|---|---|
|List|ArrayList, LinkedList|Yes|Yes|No|
|Set|HashSet, TreeSet|No / Sorted|No|No|
|Queue|LinkedList, PriorityQueue|FIFO|Yes|No|
|Map|HashMap, TreeMap|Key-based|Keys: No|No|

---

## ⏲️ Time Complexity of CRUD Operations in Java Collections

| Implementation    | Insert (Create) | Read (Access) | Update   | Delete    |
| ----------------- | --------------- | ------------- | -------- | --------- |
| **ArrayList**     | O(1)–Amortized  | O(1)          | O(1)     | O(n)      |
| **LinkedList**    | O(1) at ends    | O(n)          | O(n)     | O(1)–O(n) |
| **HashSet**       | O(1)            | O(1)          | O(1)     | O(1)      |
| **LinkedHashSet** | O(1)            | O(1)          | O(1)     | O(1)      |
| **TreeSet**       | O(log n)        | O(log n)      | O(log n) | O(log n)  |
| **HashMap**       | O(1)            | O(1)          | O(1)     | O(1)      |
| **LinkedHashMap** | O(1)            | O(1)          | O(1)     | O(1)      |
| **TreeMap**       | O(log n)        | O(log n)      | O(log n) | O(log n)  |
| **Hashtable**     | O(1)            | O(1)          | O(1)     | O(1)      |
| **PriorityQueue** | O(log n)        | O(1) (peek)   | O(log n) | O(log n)  |
| **ArrayDeque**    | O(1)            | O(1)          | O(1)     | O(1)      |

Note: Actual performance can depend on load factors, collision handling (for hash-based structures), and Java implementation details.

---

## 🔢 Java Collections Algorithms

### 🔸 **Sorting Example**

```java
import java.util.*;

public class SortExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Zoe", "Adam", "Mia");
        Collections.sort(names);  // Sort in ascending order
        System.out.println(names); // Output: [Adam, Mia, Zoe]
    }
}

```

### 🔸 **Custom Sorting with Comparator**

```java
import java.util.*;

public class CustomSortExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(5, 1, 3, 2);
        Collections.sort(numbers, (a, b) -> b - a);  // Sort in descending order
        System.out.println(numbers); // Output: [5, 3, 2, 1]
    }
}

```

### 🔍 **Binary Search Example**

```java
import java.util.*;

public class SearchExample {
    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(10, 20, 30, 40);
        int index = Collections.binarySearch(list, 30); // Search for 30
        System.out.println("Index of 30: " + index); // Output: Index of 30: 2
    }
}

```

---

## 🔄 Other Utility Methods for Collections

### 🔸 **Reversing a List**

```java
import java.util.*;

public class ReverseExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("One", "Two", "Three");
        Collections.reverse(list);  // Reverse the list
        System.out.println(list);    // Output: [Three, Two, One]
    }
}

```

### 🔸 **Shuffling a List**

```java
import java.util.*;

public class ShuffleExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("One", "Two", "Three", "Four");
        Collections.shuffle(list);  // Shuffle the list randomly
        System.out.println(list);    // Output: Random order of elements
    }
}

```

### 🔸 **Min and Max**

```java
import java.util.*;

public class MinMaxExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(10, 20, 30, 40);
        System.out.println("Min: " + Collections.min(numbers)); // Min: 10
        System.out.println("Max: " + Collections.max(numbers)); // Max: 40
    }
}

```

### 🔸 **Filling a List with a Specific Value**

```java
import java.util.*;

public class FillExample {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>(Arrays.asList("A", "B", "C"));
        Collections.fill(list, "X");  // Fill the list with "X"
        System.out.println(list);      // Output: [X, X, X]
    }
}

```

### 🔸 **Frequency of Elements**

```java
import java.util.*;

public class FrequencyExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("Apple", "Banana", "Apple", "Orange");
        int frequency = Collections.frequency(list, "Apple"); // Count occurrences of "Apple"
        System.out.println("Frequency of Apple: " + frequency); // Output: 2
    }
}

```

### 🔸 **Copying a List**

```java
import java.util.*;

public class CopyExample {
    public static void main(String[] args) {
        List<String> sourceList = Arrays.asList("Apple", "Banana", "Orange");
        List<String> destList = new ArrayList<>(sourceList.size());
        Collections.copy(destList, sourceList);  // Copy elements from sourceList to destList
        System.out.println(destList);             // Output: [Apple, Banana, Orange]
    }
}

```

### 🔸 **Synchronization (Thread-Safe Collection)**

```java
import java.util.*;

public class SynchronizedExample {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("Apple");
        list.add("Banana");
        List<String> syncList = Collections.synchronizedList(list);  // Create synchronized version of the list
        synchronized (syncList) {
            for (String item : syncList) {
                System.out.println(item);  // Output: Apple, Banana
            }
        }
    }
}

```