# ✅ System Design Roadmap Checklist (Basic → Advanced)

---

# Phase 1 — Foundations

## 1. System Design Basics
- [ ] What is System Design?
- [ ] Functional Requirements
- [ ] Non-functional Requirements
- [ ] Scalability
- [ ] Reliability
- [ ] Availability
- [ ] Maintainability
- [ ] Performance
- [ ] Latency vs Throughput
- [ ] Monolith vs Distributed Systems
- [ ] Stateless vs Stateful Applications

---

## 2. Networking Fundamentals
- [ ] OSI Model
- [ ] TCP/IP Model
- [ ] HTTP
- [ ] HTTPS
- [ ] HTTP Methods
- [ ] HTTP Status Codes
- [ ] REST APIs
- [ ] GraphQL Basics
- [ ] gRPC
- [ ] WebSockets
- [ ] Server-Sent Events (SSE)
- [ ] TCP
- [ ] UDP
- [ ] DNS
- [ ] SSL/TLS
- [ ] CDN Basics
- [ ] Browser Request Lifecycle

---

## 3. Databases
### SQL
- [ ] Primary Key
- [ ] Foreign Key
- [ ] Composite Key
- [ ] Unique Constraints
- [ ] Indexes
- [ ] Clustered vs Non-clustered Index
- [ ] Joins
- [ ] Transactions
- [ ] ACID Properties
- [ ] Isolation Levels
- [ ] Locks
- [ ] Deadlocks
- [ ] Optimistic Locking
- [ ] Pessimistic Locking

### NoSQL
- [ ] Key-Value Databases
- [ ] Document Databases
- [ ] Wide Column Databases
- [ ] Graph Databases
- [ ] SQL vs NoSQL

---

## 4. Database Scaling
- [ ] Vertical Scaling
- [ ] Horizontal Scaling
- [ ] Replication
- [ ] Read Replicas
- [ ] Sharding
- [ ] Partitioning
- [ ] Database Federation

---

## 5. CAP Theorem
- [ ] Consistency
- [ ] Availability
- [ ] Partition Tolerance
- [ ] CAP Trade-offs
- [ ] Eventual Consistency
- [ ] Strong Consistency

---

# Phase 2 — Intermediate Concepts

## 6. Load Balancing
- [ ] Why Load Balancers?
- [ ] Round Robin
- [ ] Least Connections
- [ ] Least Response Time
- [ ] IP Hash
- [ ] Sticky Sessions
- [ ] Health Checks
- [ ] Layer 4 vs Layer 7 Load Balancer

---

## 7. Reverse Proxy
- [ ] Forward Proxy
- [ ] Reverse Proxy
- [ ] Nginx
- [ ] HAProxy

---

## 8. Caching
- [ ] Why Cache?
- [ ] Cache Aside
- [ ] Read Through
- [ ] Write Through
- [ ] Write Back
- [ ] Refresh Ahead
- [ ] TTL
- [ ] Cache Invalidation
- [ ] Cache Stampede
- [ ] Cache Penetration
- [ ] Cache Avalanche
- [ ] LRU
- [ ] LFU

---

## 9. Redis
- [ ] Strings
- [ ] Hashes
- [ ] Lists
- [ ] Sets
- [ ] Sorted Sets
- [ ] Pub/Sub
- [ ] Expiry
- [ ] Persistence
- [ ] Redis Cluster

---

## 10. CDN
- [ ] Edge Servers
- [ ] Static Content
- [ ] Cache Expiration
- [ ] Geo Distribution

---

## 11. Message Queues
- [ ] Why Messaging?
- [ ] Producer
- [ ] Consumer
- [ ] Queue
- [ ] Topic
- [ ] Kafka Basics
- [ ] RabbitMQ Basics
- [ ] SQS Basics
- [ ] Partitions
- [ ] Offsets
- [ ] Consumer Groups
- [ ] Ordering
- [ ] Delivery Guarantees

---

## 12. API Gateway
- [ ] Routing
- [ ] Authentication
- [ ] Authorization
- [ ] Rate Limiting
- [ ] Logging
- [ ] API Aggregation

---

## 13. Rate Limiting
- [ ] Token Bucket
- [ ] Leaky Bucket
- [ ] Fixed Window
- [ ] Sliding Window
- [ ] Sliding Log

---

## 14. Consistent Hashing
- [ ] Hash Ring
- [ ] Virtual Nodes
- [ ] Rebalancing

---

## 15. Bloom Filters
- [ ] Bit Arrays
- [ ] Hash Functions
- [ ] False Positives
- [ ] Real-world Applications

---

# Phase 3 — Advanced Concepts

## 16. Distributed Systems
- [ ] Distributed Computing
- [ ] Consensus Algorithms
- [ ] Split Brain
- [ ] Leader Election
- [ ] Gossip Protocol

---

## 17. Distributed Transactions
- [ ] Two Phase Commit (2PC)
- [ ] Three Phase Commit (3PC)
- [ ] Saga Pattern
- [ ] Outbox Pattern
- [ ] Inbox Pattern

---

## 18. Distributed Locks
- [ ] Redis Lock
- [ ] ZooKeeper
- [ ] etcd
- [ ] RedLock

---

## 19. Event Driven Architecture
- [ ] Events
- [ ] Event Bus
- [ ] Event Sourcing
- [ ] CQRS
- [ ] Choreography
- [ ] Orchestration

---

## 20. Microservices
- [ ] Monolith vs Microservices
- [ ] Service Discovery
- [ ] API Gateway
- [ ] Configuration Server
- [ ] Circuit Breaker
- [ ] Service Mesh
- [ ] Sidecar Pattern

---

## 21. Service Discovery
- [ ] Eureka
- [ ] Consul
- [ ] Kubernetes Discovery

---

## 22. Circuit Breaker
- [ ] Closed State
- [ ] Open State
- [ ] Half Open State
- [ ] Retry Pattern
- [ ] Timeout Pattern
- [ ] Bulkhead Pattern

---

## 23. Authentication
- [ ] Sessions
- [ ] JWT
- [ ] OAuth2
- [ ] OpenID Connect
- [ ] Refresh Tokens
- [ ] API Keys

---

## 24. Authorization
- [ ] RBAC
- [ ] ABAC
- [ ] ACL

---

## 25. Search Systems
- [ ] Elasticsearch
- [ ] OpenSearch
- [ ] Inverted Index
- [ ] Tokenization
- [ ] Ranking

---

## 26. Object Storage
- [ ] Amazon S3
- [ ] MinIO
- [ ] Blob Storage
- [ ] Pre-signed URLs

---

## 27. Logging
- [ ] Centralized Logging
- [ ] ELK Stack
- [ ] Loki

---

## 28. Monitoring
- [ ] Prometheus
- [ ] Grafana
- [ ] Jaeger
- [ ] Zipkin
- [ ] OpenTelemetry

---

## 29. Metrics
- [ ] Latency
- [ ] Throughput
- [ ] Availability
- [ ] Error Rate
- [ ] Saturation

---

## 30. High Availability
- [ ] Redundancy
- [ ] Failover
- [ ] Disaster Recovery
- [ ] RPO
- [ ] RTO

---

## 31. Security
- [ ] HTTPS
- [ ] CORS
- [ ] CSRF
- [ ] XSS
- [ ] SQL Injection
- [ ] Secrets Management
- [ ] Encryption at Rest
- [ ] Encryption in Transit

---

## 32. Serialization
- [ ] JSON
- [ ] XML
- [ ] Protocol Buffers
- [ ] Avro
- [ ] MessagePack

---

## 33. Compression
- [ ] Gzip
- [ ] Brotli
- [ ] Snappy

---

## 34. Streaming Systems
- [ ] Kafka Streams
- [ ] Apache Flink
- [ ] Spark Streaming

---

## 35. Real-time Communication
- [ ] WebSockets
- [ ] SSE
- [ ] MQTT

---

## 36. Scheduling Systems
- [ ] Cron Jobs
- [ ] Quartz Scheduler
- [ ] Distributed Scheduling

---

# Phase 4 — Design Popular Systems

## Beginner
- [ ] URL Shortener
- [ ] TinyURL
- [ ] Pastebin
- [ ] Parking Lot
- [ ] Library Management
- [ ] Movie Ticket Booking
- [ ] ATM System
- [ ] Rate Limiter
- [ ] Chat Application

---

## Intermediate
- [ ] WhatsApp
- [ ] Instagram Feed
- [ ] Twitter Timeline
- [ ] Notification Service
- [ ] Dropbox
- [ ] Google Drive
- [ ] Food Delivery App
- [ ] Ride Sharing App
- [ ] Payment Gateway
- [ ] API Gateway

---

## Advanced
- [ ] YouTube
- [ ] Netflix
- [ ] Spotify
- [ ] Uber
- [ ] Amazon
- [ ] Google Search
- [ ] Banking System
- [ ] Distributed Cache
- [ ] Distributed File System
- [ ] Google Maps
- [ ] Discord
- [ ] Zoom
- [ ] Stock Trading Platform
- [ ] Online Multiplayer Game Server

---

# Bonus Topics (FAANG Interviews)

## Design Patterns
- [ ] Singleton
- [ ] Factory
- [ ] Builder
- [ ] Strategy
- [ ] Observer
- [ ] Decorator
- [ ] Adapter

---

## Concurrency
- [ ] Threads
- [ ] Thread Pools
- [ ] Mutex
- [ ] Semaphore
- [ ] Read Write Locks
- [ ] Deadlocks
- [ ] Race Conditions
- [ ] Atomic Operations

---

## Cloud
- [ ] AWS Basics
- [ ] EC2
- [ ] ECS
- [ ] EKS
- [ ] Lambda
- [ ] API Gateway
- [ ] S3
- [ ] CloudFront
- [ ] RDS
- [ ] DynamoDB
- [ ] IAM

---

# Final Checklist

- [ ] Understand trade-offs in every design decision.
- [ ] Design at least 20 real-world systems.
- [ ] Draw architecture diagrams using Excalidraw.
- [ ] Explain scalability decisions.
- [ ] Explain database choices.
- [ ] Explain cache strategy.
- [ ] Explain messaging strategy.
- [ ] Explain fault tolerance.
- [ ] Explain bottlenecks and optimizations.
- [ ] Practice mock system design interviews.