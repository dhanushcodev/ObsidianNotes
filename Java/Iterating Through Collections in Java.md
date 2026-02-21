
## 1️⃣ List (ArrayList, LinkedList)

``` java
List<String> list = new ArrayList<>();
```

### 1. Index-based For Loop

``` java
for (int i = 0; i < list.size(); i++) {
    System.out.println(list.get(i));
}
```

-   Use when index is needed
-   Best with ArrayList

### 2. Enhanced For (For-each)

``` java
for (String item : list) {
    System.out.println(item);
}
```

-   Clean and simple
-   Cannot safely remove elements

### 3. Iterator (Safe removal)

``` java
Iterator<String> it = list.iterator();
while (it.hasNext()) {
    String item = it.next();
    if (item.equals("remove"))
        it.remove();
}
```

-   Safe removal
-   Works for all collections

### 4. ListIterator (Bidirectional)

``` java
ListIterator<String> li = list.listIterator();
while (li.hasNext()) {
    System.out.println(li.next());
}
```

-   Forward & backward traversal
-   Can modify during iteration

### 5. forEach (Java 8+)

``` java
list.forEach(System.out::println);
```

### 6. Stream API

``` java
list.stream()
    .filter(s -> s.startsWith("A"))
    .forEach(System.out::println);
```

-   Functional style
-   Used in modern applications

------------------------------------------------------------------------

## 2️⃣ Set (HashSet, LinkedHashSet, TreeSet)

``` java
Set<String> set = new HashSet<>();
```

⚠ No index-based loop

### For-each

``` java
for (String item : set) {
    System.out.println(item);
}
```

### Iterator

``` java
Iterator<String> it = set.iterator();
while (it.hasNext()) {
    System.out.println(it.next());
}
```

### forEach

``` java
set.forEach(System.out::println);
```

------------------------------------------------------------------------

## 3️⃣ Map (HashMap, TreeMap, LinkedHashMap)

``` java
Map<Integer, String> map = new HashMap<>();
```

### 1. entrySet (Best Way)

``` java
for (Map.Entry<Integer, String> entry : map.entrySet()) {
    System.out.println(entry.getKey() + " " + entry.getValue());
}
```

-   Most efficient

### 2. keySet

``` java
for (Integer key : map.keySet()) {
    System.out.println(key + " " + map.get(key));
}
```

-   Slightly slower (extra lookup)

### 3. values only

``` java
for (String value : map.values()) {
    System.out.println(value);
}
```

### 4. Iterator with entrySet

``` java
Iterator<Map.Entry<Integer, String>> it = map.entrySet().iterator();
while (it.hasNext()) {
    Map.Entry<Integer, String> entry = it.next();
    System.out.println(entry.getKey());
}
```

### 5. Java 8 forEach

``` java
map.forEach((key, value) -> 
    System.out.println(key + " " + value)
);
```

------------------------------------------------------------------------

## 4️⃣ Queue (LinkedList, PriorityQueue)

``` java
Queue<String> queue = new LinkedList<>();
```

### For-each

``` java
for (String item : queue) {
    System.out.println(item);
}
```

### Iterator

``` java
Iterator<String> it = queue.iterator();
while (it.hasNext()) {
    System.out.println(it.next());
}
```

### While polling (Processing style)

``` java
while (!queue.isEmpty()) {
    System.out.println(queue.poll());
}
```

-   Used in BFS / real processing logic

------------------------------------------------------------------------

## 5️⃣ Deque (ArrayDeque)

``` java
Deque<String> deque = new ArrayDeque<>();
```

### Forward

``` java
for (String item : deque) {
    System.out.println(item);
}
```

### Reverse

``` java
Iterator<String> it = deque.descendingIterator();
while (it.hasNext()) {
    System.out.println(it.next());
}
```

------------------------------------------------------------------------
# 🎯 Interview Summary Table

| Collection | Index Loop | For-each | Iterator | Stream | Special            |
| ---------- | ---------- | -------- | -------- | ------ | ------------------ |
| List       | ✅          | ✅        | ✅        | ✅      | ListIterator       |
| Set        | ❌          | ✅        | ✅        | ✅      | No duplicates      |
| Map        | ❌          | entrySet | ✅        | ✅      | key/value          |
| Queue      | ❌          | ✅        | ✅        | ✅      | poll()             |
| Deque      | ❌          | ✅        | ✅        | ✅      | descendingIterator |

------------------------------------------------------------------------

## 💡 Interview Tips

-   Modifying collection → Use Iterator
-   Just reading → Use for-each
-   Transforming/filtering → Use Streams
-   For Map → Prefer entrySet()
