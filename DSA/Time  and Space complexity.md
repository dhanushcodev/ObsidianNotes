	## 🧠 What is Time Complexity?

**Time complexity** tells us **how fast** an algorithm runs as the **input size grows**. It doesn't measure exact time (like seconds); instead, it measures how the number of **operations** increases relative to the **input size `n`**.

---

## ⚡ Why is Time Complexity Important?

Imagine you’re solving a problem for an array with:

- 10 elements – it runs fast.
- 10,000 elements – still fast?
- 1,000,000 elements – maybe it starts to lag.

Time complexity helps us predict this behavior.

---

## 🧮 Common Time Complexities (with Examples)

|Time Complexity|Meaning|Example Code|
|---|---|---|
|**O(1)**|Constant time (not affected by input size)|`return arr[0];`|
|**O(log n)**|Logarithmic time (input shrinks each step)|Binary search|
|**O(n)**|Linear time (scales directly with input)|Loop over array|
|**O(n log n)**|Log-linear (common in efficient sorting)|Merge sort|
|**O(n²)**|Quadratic time (nested loops)|Bubble sort|
|**O(2ⁿ)**|Exponential time (doubles each step)|Solving subsets|
|**O(n!)**|Factorial time (very slow)|Permutations|

---

## 🔍 How to Analyze Time Complexity

Let’s say you have this code:

```python
def example(arr):
    for i in arr:             # O(n)
        print(i)

```

This loop runs once for each element in `arr`, so it's **O(n)**.

---

### 🧩 Nested Loops:

```python
for i in arr:                 # O(n)
    for j in arr:             # O(n)
        print(i, j)

```

This does `n * n` operations → **O(n²)**.

---

### 🧠 Ignore Constants & Low Impact Terms

```python
def test(arr):
    print("Hello")            # O(1)
    for i in arr:             # O(n)
        print(i)
    for i in arr:             # O(n)
        for j in arr:         # O(n)
            print(i, j)

```

We combine:

- O(1) + O(n) + O(n²) → drop constants and low-order terms
- Final time complexity: **O(n²)**

---

## 📏 Real-Life Meaning of Time Complexity

|Time Complexity|Max `n` you can handle efficiently|
|---|---|
|O(1), O(log n)|Millions or more|
|O(n)|Up to 10⁷ (with good code)|
|O(n log n)|~10⁶|
|O(n²)|~10³ to 10⁴ max|
|O(2ⁿ), O(n!)|Tiny inputs only (~20 or fewer)|

---

## ⏱️ Tips for Practice

- Always **think of the worst-case input size**.
- Know your **loops**, **recursion**, and **data structures**.
- Use time complexity to **choose the best algorithm** (e.g., use Merge Sort over Bubble Sort for large data).

## 🧠 What is Space Complexity?

**Space complexity** refers to the **total amount of memory** (RAM) your algorithm or program uses **relative to the input size** `n`.

It includes:

1. **Input space** (sometimes not counted, if it's reused).
2. **Auxiliary space** – extra space your algorithm needs to work (like recursion stacks, temporary arrays, hash tables, etc.)

---

## 🎯 Why is Space Complexity Important?

- Low space usage = better performance on limited-memory devices.
- Some problems prioritize memory efficiency over speed.
- Helps you choose the best algorithm depending on context.

---

## 📏 How is Space Complexity Measured?

Just like time complexity, we express it in **Big-O notation** to describe how memory use grows with input size `n`.

---

## 🔢 Common Space Complexities

|Space Complexity|Example Use Case|
|---|---|
|**O(1)**|Constant space, no matter the input|
|**O(log n)**|Recursive algorithms (binary search)|
|**O(n)**|Linear structures like arrays/lists|
|**O(n log n)**|Merge sort (due to splitting arrays)|
|**O(n²)**|Matrix-based algorithms|

---

## 🔍 Examples

### ✅ O(1) — Constant Space

```python
def sum_array(arr):
    total = 0       # just one variable
    for num in arr:
        total += num
    return total

```

- Regardless of how big `arr` is, you're only using **one variable (`total`)** → **O(1)**.

---

### ✅ O(n) — Linear Space

```python
def copy_array(arr):
    result = []
    for num in arr:
        result.append(num)
    return result

```

- You're storing a **new array of the same size** → **O(n)** space.

---

### ✅ O(log n) — Recursive Stack (Binary Search)

```python
def binary_search(arr, target, left, right):
    if left > right:
        return -1
    mid = (left + right) // 2
    if arr[mid] == target:
        return mid
    elif target < arr[mid]:
        return binary_search(arr, target, left, mid - 1)
    else:
        return binary_search(arr, target, mid + 1, right)

```

- In recursive calls, each call adds a **small frame to the call stack**, and it happens **log n times** → **O(log n)** space.

---

### ✅ O(n log n) — Merge Sort

Merge Sort uses recursion **and** creates temporary arrays for merging.

- **Recursive depth** → O(log n)
- **Temporary arrays** at each level → O(n)
- Total = **O(n log n)** space

(Though in-place versions of merge sort try to reduce this.)

---

## ⚠️ Space vs Time Trade-Off

Many algorithms **trade space for speed** or vice versa:

|Algorithm|Time Complexity|Space Complexity|
|---|---|---|
|Merge Sort|O(n log n)|O(n)|
|Quick Sort|O(n log n)|O(log n)|
|Hashing|O(1) lookup|O(n) space|

Example: Using a **hash map** to avoid nested loops in finding duplicates can **save time**, but **uses more memory**.

---

## ✅ Tips for Space Optimization

- Reuse variables instead of making new ones.
- Avoid creating new arrays or lists in loops.
- Use in-place operations when possible.
- Tail recursion or iteration can save stack space.

## 💼 Problem: Find the First Duplicate in an Array

> Given an array of integers, return the **first duplicate** value for which the second occurrence has the minimal index. If no duplicate exists, return -1.

---

### 🔢 Example Input:

```python
arr = [2, 1, 3, 5, 3, 2]
# Expected Output: 3 (because it's the first number to appear twice)

```

---

## ✅ Approach 1: **Brute Force (Nested Loops)**

```python
def first_duplicate(arr):
    for i in range(len(arr)):
        for j in range(i + 1, len(arr)):
            if arr[i] == arr[j]:
                return arr[i]
    return -1

```

### 🧠 Time Complexity:

- Outer loop = `n`
- Inner loop = up to `n`
- Total = `O(n^2)` (bad for large inputs)

### 💾 Space Complexity:

- No extra space used → **O(1)**

---

## ✅ Approach 2: **Using a Set (Optimized)**

```python
def first_duplicate(arr):
    seen = set()
    for num in arr:
        if num in seen:
            return num
        seen.add(num)
    return -1

```

### 🧠 Time Complexity:

- Each element is visited once → **O(n)**
- `in` and `add` in a `set` are O(1)

### 💾 Space Complexity:

- At most `n` elements in the `set` → **O(n)**

---

## 🔍 Summary Comparison

|Approach|Time Complexity|Space Complexity|Notes|
|---|---|---|---|
|Brute Force|O(n²)|O(1)|Slow but low memory|
|Set-based|O(n)|O(n)|Fast but uses extra memory|

---

## ⚖️ Space-Time Tradeoff

- If memory is **not a concern**, the second method is much faster.
- If you're on a **memory-constrained device**, the first one may be acceptable for small inputs.

---

## 🧠 Bonus Thought:

If the input has constraints like:

- Only values from `1` to `n`, and
- You're allowed to **modify the array**

→ You can solve it in **O(n)** time and **O(1)** space using index marking tricks (let me know if you want that too!).