# 1. What is `@Query` in Spring Data JPA?

`@Query` is used to write custom queries inside the repository layer.

Instead of relying only on method names like:

```java
findByName()
```

you can directly write:

- JPQL queries
- Native SQL queries

---

# Why do we need `@Query`?

Method name queries become difficult for complex conditions.

Example:

```java
findByNameAndDepartmentAndSalaryGreaterThan()
```

This becomes ugly and hard to maintain.

So we use:

```java
@Query(...)
```

---

# Basic Example

## Entity

```java
@Entity
public class Employee {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    private String department;

    private double salary;

    // getters setters
}
```

---

# Repository Example

```java
@Repository
public interface EmployeeRepository extends JpaRepository<Employee, Long> {

    @Query("SELECT e FROM Employee e WHERE e.department = :dept")
    List<Employee> findEmployeesByDepartment(@Param("dept") String department);
}
```

---

# Important Point

Inside JPQL:

```java
SELECT e FROM Employee e
```

`Employee` = Entity class name  
NOT table name.

---

# Calling Service

```java
List<Employee> employees =
        employeeRepository.findEmployeesByDepartment("IT");
```

---

---

# 2. What is JPQL?

JPQL = Java Persistence Query Language

It is similar to SQL but works with:

- Entity classes
- Object fields

NOT database table names.

---

# SQL vs JPQL

| SQL | JPQL |
|---|---|
| Uses table name | Uses entity class |
| Uses column names | Uses field names |
| Database specific | Database independent |
| Returns rows | Returns objects |

---

# Example

## SQL

```sql
SELECT * FROM employee WHERE department='IT';
```

---

## JPQL

```java
SELECT e FROM Employee e WHERE e.department='IT'
```

---

# JPQL Works on Objects

Suppose:

```java
private String department;
```

JPQL uses:

```java
e.department
```

NOT actual DB column name.

---

# Common JPQL Examples

# Fetch All

```java
@Query("SELECT e FROM Employee e")
List<Employee> getAllEmployees();
```

---

# Find By Salary Greater Than

```java
@Query("SELECT e FROM Employee e WHERE e.salary > :salary")
List<Employee> findHighSalaryEmployees(@Param("salary") double salary);
```

---

# Multiple Conditions

```java
@Query("""
       SELECT e FROM Employee e
       WHERE e.department = :dept
       AND e.salary > :salary
       """)
List<Employee> getEmployees(
        @Param("dept") String dept,
        @Param("salary") double salary);
```

---

# LIKE Query

```java
@Query("SELECT e FROM Employee e WHERE e.name LIKE %:keyword%")
List<Employee> searchByName(@Param("keyword") String keyword);
```

---

# Sorting

```java
@Query("SELECT e FROM Employee e ORDER BY e.salary DESC")
List<Employee> sortBySalary();
```

---

# Aggregate Functions

## Count

```java
@Query("SELECT COUNT(e) FROM Employee e")
long countEmployees();
```

---

## Max Salary

```java
@Query("SELECT MAX(e.salary) FROM Employee e")
double getMaxSalary();
```

---

# Update Query

For update/delete queries:

- Use `@Modifying`
- Use `@Transactional`

---

## Example

```java
@Transactional
@Modifying
@Query("UPDATE Employee e SET e.salary = :salary WHERE e.id = :id")
int updateSalary(
        @Param("id") Long id,
        @Param("salary") double salary);
```

---

# Delete Query

```java
@Transactional
@Modifying
@Query("DELETE FROM Employee e WHERE e.department = :dept")
int deleteByDepartment(@Param("dept") String dept);
```

---

---

# 3. What is Native Query?

Native Query = actual SQL query.

Used when:

- JPQL cannot do something
- Need DB-specific features
- Complex joins
- Performance tuning

---

# Native Query Example

```java
@Query(
    value = "SELECT * FROM employee WHERE department = :dept",
    nativeQuery = true
)
List<Employee> getEmployeesNative(@Param("dept") String dept);
```

---

# Difference Between JPQL and Native Query

| JPQL | Native Query |
|---|---|
| Uses entity names | Uses table names |
| DB independent | DB dependent |
| Object-oriented | Pure SQL |
| Easier portability | More powerful |

---

# Example

## JPQL

```java
SELECT e FROM Employee e
```

---

## Native SQL

```sql
SELECT * FROM employee
```

---

# When to Prefer Native Query?

Use native query when:

- Complex joins
- Window functions
- Stored procedures
- Database-specific syntax
- Optimized SQL needed

---

---

# 4. Named Query

Named Query = predefined JPQL query.

Defined once and reused everywhere.

---

# Why Use Named Query?

Advantages:

- Centralized query
- Reusable
- Cleaner repositories
- Queries validated during startup

---

# Syntax

Named queries are written in entity class.

---

# Example

## Entity

```java
@Entity
@NamedQuery(
    name = "Employee.findByDepartment",
    query = "SELECT e FROM Employee e WHERE e.department = :dept"
)
public class Employee {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    private String department;

    private double salary;
}
```

---

# Repository

```java
@Repository
public interface EmployeeRepository extends JpaRepository<Employee, Long> {

    @Query(name = "Employee.findByDepartment")
    List<Employee> findByDepartment(@Param("dept") String dept);
}
```

---

# Flow

Application startup:

1. Hibernate loads entity
2. Reads named query
3. Validates query
4. Stores in memory

Later:

```java
findByDepartment()
```

executes stored query.

---

# Major Advantage

Errors detected during startup itself.

NOT at runtime.

---

---

# 5. Named Native Query

Same as Named Query but uses SQL instead of JPQL.

---

# Example

## Entity

```java
@Entity
@NamedNativeQuery(
    name = "Employee.findByDepartmentNative",
    query = "SELECT * FROM employee WHERE department = :dept",
    resultClass = Employee.class
)
public class Employee {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    private String department;

    private double salary;
}
```

---

# Repository

```java
@Repository
public interface EmployeeRepository extends JpaRepository<Employee, Long> {

    @Query(name = "Employee.findByDepartmentNative", nativeQuery = true)
    List<Employee> findByDepartmentNative(@Param("dept") String dept);
}
```

---

# Important

For named native query:

```java
resultClass = Employee.class
```

is needed so Hibernate knows how to map result rows to objects.

---

---

# Full Comparison

| Feature | JPQL | Native Query | Named Query | Named Native Query |
|---|---|---|---|---|
| Query Type | JPQL | SQL | JPQL | SQL |
| Reusable | No | No | Yes | Yes |
| Startup Validation | No | No | Yes | Yes |
| Uses Entity Names | Yes | No | Yes | No |
| Uses Table Names | No | Yes | No | Yes |
| Database Independent | Yes | No | Yes | No |

---

# Which One Should You Use?

| Situation | Best Choice |
|---|---|
| Simple custom query | `@Query + JPQL` |
| Complex SQL needed | Native Query |
| Reusable JPQL | Named Query |
| Reusable SQL | Named Native Query |

---

# Best Practice Recommendation

## Prefer This Order

### 1. Derived Query Methods

```java
findByDepartment()
```

Use for simple cases.

---

### 2. JPQL (`@Query`)

Best for most applications.

---

### 3. Native Query

Use only when necessary.

---

### 4. Named Queries

Useful for very large enterprise applications.

---

# Real Interview Questions

# Q1. Difference between JPQL and SQL?

JPQL works on entities and fields.  
SQL works on tables and columns.

---

# Q2. Why use native query?

For complex SQL and DB-specific features.

---

# Q3. Why use `@Modifying`?

Because update/delete queries change data.

---

# Q4. Why use `@Transactional`?

To ensure DB transaction management.

---

# Q5. Advantage of Named Query?

Validated at startup and reusable.

---

# Q6. Can JPQL use `SELECT *`?

No.

JPQL works with entities:

```java
SELECT e FROM Employee e
```

---

# Q7. Which is faster JPQL or Native Query?

Usually:

- Native query can be faster
- But JPQL is cleaner and portable

Use native only if needed.

---

# Important Internal Understanding

# How JPQL Works Internally

When you write:

```java
@Query("SELECT e FROM Employee e")
```

Hibernate converts it into SQL:

```sql
SELECT * FROM employee;
```

Then:

1. Executes SQL
2. Fetches rows
3. Converts rows into Employee objects

---

# Internally Flow

```text
JPQL
   ↓
Hibernate Parser
   ↓
SQL Generation
   ↓
Database Execution
   ↓
ResultSet
   ↓
Entity Objects
```

---

# Important Mistake Beginners Make

Wrong:

```java
@Query("SELECT * FROM employee")
```

This is SQL.

---

Correct JPQL:

```java
@Query("SELECT e FROM Employee e")
```

---

# Final Summary

| Type | Uses |
|---|---|
| JPQL | Entity-oriented query language |
| `@Query` | Write custom JPQL/SQL |
| Native Query | Raw SQL query |
| Named Query | Reusable JPQL |
| Named Native Query | Reusable SQL |