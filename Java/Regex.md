# Regular Expressions: From Basics to Advanced (with Examples)

---

## 🧠 What are Regular Expressions (Regex)?

Regular expressions are patterns used to match character combinations in strings. They are powerful tools for searching, matching, and manipulating text.

---

## 🧩 Basic Syntax

### 1. **Literal Characters**

- **Pattern**: `hello`
- **Matches**: The exact string "hello"
- **Example**: `"hello world".matches(".*hello.*")` ✅

### 2. **Meta-characters**

|Meta|Meaning|Example|Matches|
|---|---|---|---|
|`.`|Any character (except newline)|`a.c`|`abc`, `axc`, `a-c`|
|`\d`|Digit (0-9)|`\d\d`|`12`, `99`|
|`\D`|Non-digit|`\D`|`a`, `#`|
|`\w`|Word character (a-zA-Z0-9_)|`\w+`|`hello123`, `a_b`|
|`\W`|Non-word character|`\W`|`#`, `!`|
|`\s`|Whitespace|`a\sb`|`a b`, `a\tb`|
|`\S`|Non-whitespace|`\S`|`a`, `1`, `#`|
|`\b`|Word boundary|`\bthe\b`|`the` in `the dog`|
|`\B`|Not a word boundary|`\Bthe\B`|`athem`|
|`^`|Start of string|`^Hello`|`Hello world` ✅|
|`$`|End of string|`world$`|`Hello world` ✅|
|`\\`|Escape character|`\\.`|`.` as a literal|

### 🌀 Special Note on `^`:

- `^a` means "starts with a" (outside brackets)
- `[^a]` means "not a" (inside brackets)

|Regex|Location|Meaning|
|---|---|---|
|`^a`|Outside `[]`|Start of string|
|`[^a-z]`|Inside `[]`|Not a lowercase letter|

---

## 🎛️ Quantifiers

|Symbol|Meaning|Example|Matches|
|---|---|---|---|
|`*`|0 or more|`a*`|`"", "a", "aaa"`|
|`+`|1 or more|`a+`|`"a", "aaa"`|
|`?`|0 or 1|`a?`|`"", "a"`|
|`{n}`|Exactly n times|`a{3}`|`"aaa"`|
|`{n,}`|n or more times|`a{2,}`|`"aa", "aaa"`|
|`{n,m}`|Between n and m times|`a{2,4}`|`"aa", "aaa", "aaaa"`|

---

## 🧱 Character Classes

|Pattern|Meaning|Example|
|---|---|---|
|`[abc]`|Any of a, b, or c|`b`, `a`|
|`[^abc]`|Not a, b, or c|`x`, `1`|
|`[a-z]`|Lowercase a to z|`g`, `m`|
|`[A-Z]`|Uppercase A to Z|`M`, `F`|
|`[0-9]`|Digits 0 to 9|`5`, `9`|
|`[a-zA-Z]`|Any letter|`H`, `z`|

---

## 🔗 Groups & Alternation

### 1. **Grouping**: `()`

- Example: `(ab)+` → Matches `ab`, `abab`, etc.

### 2. **Alternation**: `|`

- Example: `cat|dog` → Matches `cat` or `dog`

---

## 🧹 Replacing Non-Alphabetic Characters

### Java Example:

```java
String input = "He110, W0r1d! @2025";
String result = input.replaceAll("[^a-zA-Z]", "");
System.out.println("Alphabetic only: " + result);
```

- Regex: `[^a-zA-Z]`
- Meaning: Remove all characters **except** letters (a-z and A-Z)

---

## 🔍 Lookahead & Lookbehind

### 1. **Positive Lookahead** `X(?=Y)`

- Match X only if followed by Y
- Example: `\d(?=%)` → matches digit only if followed by `%`
- Input: `50% and 30%` → Matches: `5`, `3`

### 2. **Negative Lookahead** `X(?!Y)`

- Match X only if **not** followed by Y
- Example: `\d(?!%)` → Matches digits not followed by `%`
- Input: `50 and 30%` → Matches: `5`, `0`

### 3. **Positive Lookbehind** `(?<=Y)X`

- Match X only if preceded by Y
- Example: `(?<=\$)\d+` → Matches digits preceded by `$`
- Input: `$200 and 300` → Matches: `200`

### 4. **Negative Lookbehind** `(?<!Y)X`

- Match X only if **not** preceded by Y
- Example: `(?<!\$)\d+` → Matches digits not preceded by `$`
- Input: `$200 and 300` → Matches: `300`

---

## ❓ Common Confusing Examples Explained

### 🔹 `^` — Two Meanings

|Regex|Location|Meaning|
|---|---|---|
|`^a`|Outside `[]`|Match "a" at **start of string**|
|`[^a-z]`|Inside `[]`|Match **any character except a-z**|

### 🔹 `.` — Dot Means Anything (except newline)

```java
"a.c" matches "abc", "a1c", "a-c"
```

### 🔹 Greedy vs Lazy Quantifiers

- Greedy: `a.+b` → matches as much as possible
- Lazy: `a.+?b` → matches as little as possible

### 🔹 Escaping Meta-characters

Use `\\` to escape in Java strings:

```java
String pattern = "\\d{3}-\\d{2}-\\d{4}"; // SSN format: 123-45-6789
```

---

## 📚 Regex Symbols Summary

|Symbol|Description|
|---|---|
|`.`|Any character except newline|
|`^`|Start of string (or NOT in class)|
|`$`|End of string|
|`*`|0 or more of previous token|
|`+`|1 or more of previous token|
|`?`|0 or 1 of previous token|
|`{n}`|Exactly n times|
|`{n,}`|n or more times|
|`{n,m}`|Between n and m times|
|`[]`|Character class|
|`[^]`|Negated character class|
|`()`|Grouping|
|`|`|
|`\`|Escape character|
|`\d`|Digit (0-9)|
|`\D`|Non-digit|
|`\w`|Word character|
|`\W`|Non-word character|
|`\s`|Whitespace|
|`\S`|Non-whitespace|
|`\b`|Word boundary|
|`\B`|Not a word boundary|
|`(?=X)`|Positive lookahead|
|`(?!X)`|Negative lookahead|
|`(?<=X)`|Positive lookbehind|
|`(?<!X)`|Negative lookbehind|

---

## 📚 Practice Problems

### 1. ✅ Match Valid Email Addresses

**Goal**: Extract or validate standard email addresses.

- **Regex**: `\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b`
- **Example Input**: `"Contact us at support@example.com or admin@domain.org."`
- **Expected Match**: `support@example.com`, `admin@domain.org`

### 2. 📞 Validate a 10-Digit Phone Number

**Goal**: Match a phone number with exactly 10 digits (no dashes/spaces).

- **Regex**: `^\d{10}$`
- **Example Input**: `"9876543210"` → ✅
- Allow formats like `123-456-7890`: `^\d{3}[-.\s]?\d{3}[-.\s]?\d{4}$`

### 3. 🔢 Extract All Numbers from Text

**Goal**: Capture all numeric values from a paragraph.

- **Regex**: `\d+`
- **Input**: `"There are 3 cats, 14 dogs, and 2025 birds."`
- **Matches**: `3`, `14`, `2025`

### 4. 🚫 Remove All Special Characters

**Goal**: Clean a string by removing all non-alphabetic characters.

- **Regex**: `[^a-zA-Z]`
- **Replacement**: `""`
- **Java Example**:

```java
String input = "He110, W0r1d! @2025";
String result = input.replaceAll("[^a-zA-Z]", "");
System.out.println(result); // Output: HelloWrd
```
