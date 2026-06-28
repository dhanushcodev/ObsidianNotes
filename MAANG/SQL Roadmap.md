# SQL Roadmap Checklist (Basic → Advanced)

## Phase 1: Database Fundamentals
- [ ] What is a Database?
- [ ] Types of Databases
- [ ] RDBMS vs NoSQL
- [ ] Tables, Rows, Columns
- [ ] Data Types
- [ ] NULL Values
- [ ] Constraints
- [ ] Primary Key
- [ ] Foreign Key
- [ ] Candidate Key
- [ ] Composite Key
- [ ] Super Key
- [ ] Alternate Key
- [ ] Unique Key
- [ ] Default Values

---

## Phase 2: SQL Basics (DDL)
- [ ] SQL Syntax
- [ ] CREATE DATABASE
- [ ] DROP DATABASE
- [ ] CREATE TABLE
- [ ] ALTER TABLE
- [ ] TRUNCATE TABLE
- [ ] DROP TABLE
- [ ] RENAME TABLE

---

## Phase 3: CRUD Operations (DML)
- [ ] INSERT
- [ ] SELECT
- [ ] UPDATE
- [ ] DELETE

---

## Phase 4: Filtering Data
- [ ] WHERE
- [ ] AND
- [ ] OR
- [ ] NOT
- [ ] BETWEEN
- [ ] IN
- [ ] NOT IN
- [ ] LIKE
- [ ] Wildcards (% and _)
- [ ] IS NULL
- [ ] IS NOT NULL

---

## Phase 5: Sorting & Limiting
- [ ] ORDER BY
- [ ] ASC
- [ ] DESC
- [ ] LIMIT
- [ ] OFFSET
- [ ] TOP (SQL Server)

---

## Phase 6: Aggregate Functions
- [ ] COUNT()
- [ ] SUM()
- [ ] AVG()
- [ ] MIN()
- [ ] MAX()

---

## Phase 7: GROUP BY & HAVING
- [ ] GROUP BY
- [ ] HAVING
- [ ] Aggregate with GROUP BY
- [ ] Multiple Column GROUP BY

---

## Phase 8: SQL Functions

### String Functions
- [ ] CONCAT()
- [ ] SUBSTRING()
- [ ] LENGTH()
- [ ] CHAR_LENGTH()
- [ ] UPPER()
- [ ] LOWER()
- [ ] REPLACE()
- [ ] TRIM()
- [ ] POSITION()

### Numeric Functions
- [ ] ROUND()
- [ ] CEIL()
- [ ] FLOOR()
- [ ] ABS()
- [ ] MOD()
- [ ] POWER()

### Date Functions
- [ ] NOW()
- [ ] CURRENT_DATE
- [ ] CURRENT_TIMESTAMP
- [ ] DATE_ADD()
- [ ] DATE_SUB()
- [ ] DATEDIFF()
- [ ] EXTRACT()

---

## Phase 9: Joins ⭐⭐⭐⭐⭐
- [ ] INNER JOIN
- [ ] LEFT JOIN
- [ ] RIGHT JOIN
- [ ] FULL OUTER JOIN
- [ ] CROSS JOIN
- [ ] SELF JOIN
- [ ] Join Order
- [ ] Cartesian Product

---

## Phase 10: Set Operators
- [ ] UNION
- [ ] UNION ALL
- [ ] INTERSECT
- [ ] EXCEPT / MINUS

---

## Phase 11: Aliases
- [ ] Table Alias
- [ ] Column Alias

---

## Phase 12: CASE Statements
- [ ] CASE
- [ ] Nested CASE

---

## Phase 13: Subqueries ⭐⭐⭐⭐⭐
- [ ] Scalar Subquery
- [ ] Multiple Row Subquery
- [ ] Correlated Subquery
- [ ] Nested Subquery

---

## Phase 14: EXISTS / ANY / ALL
- [ ] EXISTS
- [ ] NOT EXISTS
- [ ] ANY
- [ ] ALL

---

## Phase 15: Common Table Expressions (CTE)
- [ ] Basic CTE
- [ ] Multiple CTE
- [ ] Recursive CTE

---

## Phase 16: Window Functions ⭐⭐⭐⭐⭐

### Ranking
- [ ] ROW_NUMBER()
- [ ] RANK()
- [ ] DENSE_RANK()
- [ ] NTILE()

### Aggregate Windows
- [ ] SUM() OVER()
- [ ] AVG() OVER()
- [ ] COUNT() OVER()

### Navigation
- [ ] LAG()
- [ ] LEAD()
- [ ] FIRST_VALUE()
- [ ] LAST_VALUE()

### Window Concepts
- [ ] PARTITION BY
- [ ] ORDER BY in OVER()
- [ ] ROWS Frame
- [ ] RANGE Frame

---

## Phase 17: Database Design
- [ ] ER Diagram
- [ ] Entity
- [ ] Relationship
- [ ] Cardinality

---

## Phase 18: Normalization ⭐⭐⭐⭐⭐
- [ ] 1NF
- [ ] 2NF
- [ ] 3NF
- [ ] BCNF
- [ ] 4NF
- [ ] 5NF
- [ ] Denormalization

---

## Phase 19: Constraints
- [ ] PRIMARY KEY
- [ ] FOREIGN KEY
- [ ] UNIQUE
- [ ] CHECK
- [ ] DEFAULT
- [ ] NOT NULL

---

## Phase 20: Indexing ⭐⭐⭐⭐⭐
- [ ] What is an Index?
- [ ] Clustered Index
- [ ] Non-Clustered Index
- [ ] Composite Index
- [ ] Covering Index
- [ ] Unique Index
- [ ] B+ Tree
- [ ] Index Scan
- [ ] Index Seek
- [ ] Full Table Scan
- [ ] Cardinality
- [ ] Selectivity

---

## Phase 21: Views
- [ ] Simple View
- [ ] Complex View
- [ ] Materialized View

---

## Phase 22: Transactions ⭐⭐⭐⭐⭐
- [ ] ACID Properties
- [ ] BEGIN
- [ ] COMMIT
- [ ] ROLLBACK
- [ ] SAVEPOINT

---

## Phase 23: Isolation Levels ⭐⭐⭐⭐⭐
- [ ] Read Uncommitted
- [ ] Read Committed
- [ ] Repeatable Read
- [ ] Serializable
- [ ] Dirty Read
- [ ] Non-repeatable Read
- [ ] Phantom Read

---

## Phase 24: Locks & Deadlocks
- [ ] Shared Lock
- [ ] Exclusive Lock
- [ ] Intent Lock
- [ ] Row Lock
- [ ] Page Lock
- [ ] Table Lock
- [ ] Deadlock Detection
- [ ] Deadlock Prevention

---

## Phase 25: Stored Procedures
- [ ] Create Procedure
- [ ] Input Parameters
- [ ] Output Parameters
- [ ] Calling Procedures

---

## Phase 26: Functions
- [ ] Scalar Functions
- [ ] Table-Valued Functions

---

## Phase 27: Triggers
- [ ] BEFORE Trigger
- [ ] AFTER Trigger
- [ ] INSTEAD OF Trigger

---

## Phase 28: Query Optimization ⭐⭐⭐⭐⭐
- [ ] Explain Plan
- [ ] Execution Plan
- [ ] Query Cost
- [ ] Nested Loop Join
- [ ] Merge Join
- [ ] Hash Join
- [ ] Query Refactoring
- [ ] Avoid SELECT *
- [ ] Avoid Leading Wildcards
- [ ] Avoid Functions on Indexed Columns
- [ ] Covering Index

---

## Phase 29: Advanced SQL Patterns ⭐⭐⭐⭐⭐
- [ ] Running Total
- [ ] Moving Average
- [ ] Top-N Per Group
- [ ] Gap & Island Problems
- [ ] Consecutive Records
- [ ] Duplicate Removal
- [ ] Median Query
- [ ] Pivot
- [ ] Unpivot
- [ ] Recursive Queries
- [ ] Hierarchical Queries

---

## Phase 30: Database Objects
- [ ] Sequences
- [ ] Identity Columns
- [ ] Synonyms

---

## Phase 31: Security
- [ ] Users
- [ ] Roles
- [ ] Privileges
- [ ] GRANT
- [ ] REVOKE

---

## Phase 32: Backup & Recovery
- [ ] Full Backup
- [ ] Incremental Backup
- [ ] Differential Backup
- [ ] Restore Database

---

## Phase 33: Partitioning
- [ ] Range Partitioning
- [ ] Hash Partitioning
- [ ] List Partitioning

---

## Phase 34: Replication
- [ ] Master-Slave Replication
- [ ] Master-Master Replication
- [ ] Read Replicas

---

## Phase 35: Sharding
- [ ] Horizontal Scaling
- [ ] Vertical Scaling
- [ ] Shard Key
- [ ] Rebalancing

---

## Phase 36: SQL with Java / Spring Boot ⭐⭐⭐⭐⭐
- [ ] JDBC
- [ ] PreparedStatement
- [ ] SQL Injection Prevention
- [ ] Batch Updates
- [ ] Transactions
- [ ] Connection Pooling
- [ ] Pagination
- [ ] Optimistic Locking
- [ ] Pessimistic Locking
- [ ] Lazy Loading
- [ ] Eager Loading
- [ ] N+1 Query Problem
- [ ] ORM Generated SQL

---

## Phase 37: Interview Problems ⭐⭐⭐⭐⭐
- [ ] Second Highest Salary
- [ ] Nth Highest Salary
- [ ] Duplicate Records
- [ ] Remove Duplicates
- [ ] Customers Without Orders
- [ ] Department Wise Highest Salary
- [ ] Top 3 Salaries Per Department
- [ ] Running Total
- [ ] Moving Average
- [ ] Consecutive Login Days
- [ ] Gap and Island Problems
- [ ] Rank Employees
- [ ] Pivot Rows to Columns
- [ ] Monthly Sales Report
- [ ] Rolling 7-Day Average
- [ ] Hierarchical Employee Tree

---

## Phase 38: Expert Database Internals ⭐⭐⭐⭐⭐
- [ ] Query Optimizer
- [ ] Cost-Based Optimization
- [ ] Cardinality Estimation
- [ ] Statistics & Histograms
- [ ] Buffer Pool
- [ ] B+ Tree Internals
- [ ] MVCC
- [ ] Write-Ahead Logging (WAL)
- [ ] Undo Logs
- [ ] Redo Logs
- [ ] Checkpointing
- [ ] Lock Escalation
- [ ] Online Index Rebuild
- [ ] Parallel Query Execution

---

# 🎯 Interview Readiness Checklist

## Beginner
- [ ] CRUD
- [ ] WHERE
- [ ] ORDER BY
- [ ] Aggregate Functions
- [ ] GROUP BY
- [ ] HAVING

## Intermediate
- [ ] Joins
- [ ] Subqueries
- [ ] CASE
- [ ] EXISTS
- [ ] CTE
- [ ] Window Functions

## Advanced
- [ ] Normalization
- [ ] Indexing
- [ ] Transactions
- [ ] Isolation Levels
- [ ] Locks
- [ ] Query Optimization

## Expert
- [ ] Execution Plans
- [ ] Partitioning
- [ ] Replication
- [ ] Sharding
- [ ] MVCC
- [ ] Database Internals