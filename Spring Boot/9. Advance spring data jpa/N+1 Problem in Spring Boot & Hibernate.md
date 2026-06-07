# What is N+1 Problem?

The **N+1 problem** happens when Hibernate executes:

```
1 query for parent data
+ N additional queries for child data
```

Example:

```text
1 query -> Departments
N queries -> Employees for each department
```

If there are 1000 departments:

```text
1 + 1000 queries
```

This creates:
- Slow APIs
- High DB load
- Network overhead
- Memory issues
- Timeout problems

---

# Example

## Department Entity

```java
@Entity
public class Department {

    @Id
    private Long id;

    private String name;

    @OneToMany(mappedBy = "department", fetch = FetchType.LAZY)
    private List<Employee> employees;
}
```

---

## Employee Entity

```java
@Entity
public class Employee {

    @Id
    private Long id;

    private String name;

    @ManyToOne
    private Department department;
}
```

---

# Problematic Code

```java
List<Department> departments = repository.findAll();

for (Department department : departments) {
    System.out.println(department.getEmployees().size());
}
```

---

# Queries Executed

## Query 1

```sql
SELECT * FROM department;
```

---

## Additional Queries

```sql
SELECT * FROM employee WHERE department_id = 1;

SELECT * FROM employee WHERE department_id = 2;

SELECT * FROM employee WHERE department_id = 3;
```

---

# Why It Happens

Because of:

```java
fetch = FetchType.LAZY
```

LAZY loading means:

```text
Load association only when accessed
```

So when:

```java
department.getEmployees()
```

is called,
Hibernate fires another query.

---

# Visualization

```text
findAll()
   |
   |---- Query 1 -> Departments
   |
Loop starts
   |
   |---- Query 2 -> Employees of Dept 1
   |---- Query 3 -> Employees of Dept 2
   |---- Query 4 -> Employees of Dept 3
```

---

# Main Cause of Performance Issue

The problem is NOT mainly data size.

The real problem is:

```text
Too many database round trips
```

Each query involves:
- Network call
- SQL parsing
- Query planning
- Execution
- Result transfer

This overhead repeats for every query.

---

# Example

Suppose:
- 1000 departments
- Each query takes 5 ms

## N+1

```text
1001 queries × 5 ms
≈ 5000 ms
≈ 5 seconds
```

---

## Fetch Join

```text
1 query × 50 ms
≈ 50 ms
```

Much faster.

---

# Important Concept

```text
Databases are optimized for set-based operations
```

Meaning:

```text
Give me all data at once
```

is much faster than:

```text
Give me small data repeatedly
```

---

# What is Fetch Join?

`FETCH JOIN` is:

```text
JPQL feature
```

NOT raw SQL.

---

# JPQL Fetch Join

```java
@Query("""
SELECT d
FROM Department d
JOIN FETCH d.employees
""")
List<Department> findAllWithEmployees();
```

Hibernate converts this into SQL internally.

---

# SQL Generated

```sql
SELECT d.*, e.*
FROM department d
JOIN employee e
ON d.id = e.department_id;
```

---

# Difference Between JOIN and FETCH JOIN

---

# Normal JOIN

```java
SELECT d
FROM Department d
JOIN d.employees e
```

Purpose:
- filtering
- searching
- conditions

But:

```text
employees are NOT loaded into entity memory
```

Hibernate may still execute lazy queries later.

---

# FETCH JOIN

```java
SELECT d
FROM Department d
JOIN FETCH d.employees
```

Purpose:
- load association immediately
- avoid extra queries
- store association in Hibernate session

---

# Internally What Happens

SQL result may look like:

```text
IT      Emp1
IT      Emp2
IT      Emp3
HR      Emp4
HR      Emp5
```

Hibernate reconstructs objects internally:

```text
Department IT
    -> [Emp1, Emp2, Emp3]

Department HR
    -> [Emp4, Emp5]
```

---

# EAGER vs LAZY

---

# LAZY Loading

```java
@OneToMany(fetch = FetchType.LAZY)
```

Meaning:

```text
Load only when accessed
```

Pros:
- Saves memory
- Better scalability

Cons:
- Can cause N+1

---

# EAGER Loading

```java
@OneToMany(fetch = FetchType.EAGER)
```

Meaning:

```text
Load association automatically
```

Important:

```text
EAGER does NOT mean single query
```

Hibernate decides HOW to fetch:
- join query
- separate queries
- batch queries

So EAGER can STILL cause N+1.

---

# Example

Even with EAGER:

```java
List<Department> departments = repository.findAll();
```

Hibernate may still execute:

```sql
SELECT * FROM department;

SELECT * FROM employee WHERE department_id = 1;

SELECT * FROM employee WHERE department_id = 2;
```

Still N+1.

---

# Most Important Difference

| Feature | EAGER | FETCH JOIN |
|---|---|---|
| Automatic loading | Yes | No |
| Controls SQL strategy | No | Yes |
| Guarantees single query | No | Yes |
| Prevents N+1 fully | No | Yes |
| Loads unnecessary data | Often | Controlled |

---

# Why EAGER Is Dangerous

---

# 1. Loads Unnecessary Data

Suppose:

```text
Department
    -> Employees
    -> Projects
    -> Managers
    -> Addresses
```

If everything is EAGER:

```java
repository.findById(1);
```

loads EVERYTHING,
even if only department name was needed.

---

# 2. Cartesian Product Explosion

Suppose:

```text
Department
    -> 10 Employees
    -> 20 Projects
```

Join result becomes:

```text
10 × 20 = 200 rows
```

for ONE department.

This is called:

```text
Cartesian Explosion
```

---

# 3. Infinite Recursion

Bidirectional relation:

```text
Department -> Employees
Employee -> Department
```

EAGER loading can create:
- recursive loading
- stack overflow
- huge JSON responses

---

# Professional Hibernate Rule

```text
Keep relationships LAZY by default
```

Fetch only when needed.

---

# Best Solutions for N+1

---

# 1. Fetch Join

```java
@Query("""
SELECT DISTINCT d
FROM Department d
JOIN FETCH d.employees
""")
List<Department> findAllWithEmployees();
```

Best for:
- medium-sized associations
- simple object graphs

---

# 2. EntityGraph

```java
@EntityGraph(attributePaths = {"employees"})
List<Department> findAll();
```

Cleaner alternative to fetch join.

---

# 3. DTO Projection

Best for REST APIs.

```java
public record DepartmentDTO(
    Long id,
    String departmentName,
    String employeeName
) {}
```

---

## Query

```java
@Query("""
SELECT new com.example.DepartmentDTO(
    d.id,
    d.name,
    e.name
)
FROM Department d
JOIN d.employees e
""")
List<DepartmentDTO> fetchDepartmentData();
```

Benefits:
- lightweight
- less memory
- faster APIs

---

# 4. Batch Fetching

## Configuration

```properties
spring.jpa.properties.hibernate.default_batch_fetch_size=10
```

Instead of:

```sql
SELECT * FROM employee WHERE department_id = 1;
SELECT * FROM employee WHERE department_id = 2;
SELECT * FROM employee WHERE department_id = 3;
```

Hibernate does:

```sql
SELECT * FROM employee
WHERE department_id IN (1,2,3);
```

---

# Fetch Join Problems

---

# 1. Multiple Collection Fetch

```java
JOIN FETCH employees
JOIN FETCH projects
```

may throw:

```text
cannot simultaneously fetch multiple bags
```

---

# 2. Pagination Problem

```java
Page<Department>
```

with fetch join is dangerous because rows duplicate.

Correct approach:
1. Fetch paginated IDs
2. Fetch associations separately

---

# Open Session In View (OSIV)

Spring Boot enables OSIV by default.

This keeps Hibernate session open till response ends.

So lazy loading works even inside controllers.

---

# Why OSIV Is Dangerous

During JSON serialization:

```java
department.getEmployees()
```

may trigger lazy queries automatically.

Result:
- hidden N+1 issues
- hundreds of queries

---

# Best Practice

Disable OSIV:

```properties
spring.jpa.open-in-view=false
```

---

# Most Important Interview Points

---

# Q1: What is N+1 problem?

```text
1 query for parent entities
+ N additional queries for child entities
```

causing performance issues.

---

# Q2: Why does it happen?

Because of lazy loading.

---

# Q3: Can EAGER solve it?

No.

EAGER:
- may still execute multiple queries
- loads unnecessary data
- causes performance problems

---

# Q4: Best solution?

- Fetch Join
- EntityGraph
- DTO projection
- Batch fetching

---

# Q5: Best practice?

```text
LAZY by default
FETCH only when required
```

---

# Golden Rules

```text
EAGER is convenience
FETCH JOIN is optimization
```

---

```text
N+1 problem is mainly about TOO MANY QUERIES,
not too much data
```

---

```text
LAZY by default
FETCH only when required
```