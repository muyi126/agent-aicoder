---
description: Microservice core standards, including core development principles and best practices. Use when designing services or defining service boundaries.
alwaysApply: false
---

# Microservice Core Standards

## 1. Core Principles

### 1.1 Single Responsibility Principle (SRP)
Every service, module, and class should have one and only one responsibility.

### 1.2 Dependency Inversion Principle (DIP)
- High-level modules should not depend on low-level modules; both should depend on abstractions
- Abstractions should not depend on details; details should depend on abstractions

### 1.3 Open-Closed Principle (OCP)
- Open for extension, closed for modification
- Achieve extensibility through interfaces and abstract classes

### 1.4 Interface Segregation Principle (ISP)
- Clients should not depend on interfaces they don't use
- Interfaces should be small and focused, not large and general

### 1.5 Liskov Substitution Principle (LSP)
- Subclasses should be able to replace parent classes
- Inheritance relationships should align with business semantics

---

## 2. Service Boundaries and Interface Contracts

### 2.1 Service Boundary Definition
- **Core objects and ownership**: Clearly define the owner of each data entity
- **Read/write boundaries**: Clarify which services can read/write which data
- **State progression**: State updates must be executed by the data owner

### 2.2 Interface Contracts
- **External HTTP**: Keep paths unchanged; idempotent fields must be unique
- **Internal RPC**: Freeze Facade interfaces; define version, timeout, retry policies
- **Data format**: Unify data format and protocol

### 2.3 Unified Glossary
- Define unified meanings for business terminology
- Avoid ambiguity and misunderstanding

---

## 3. State Machine Design

### 3.1 State Definition
- Clearly define all possible states
- State names should be clear and semantically meaningful

### 3.2 State Transition Constraints
- Atomic updates only through data owner's Facade
- Define conditions and rules for state transitions
- Prohibit illegal state transitions

### 3.3 State Machine Example
```
INIT → PROCESSING → SUCCESS | FAIL
```

---

## 4. Idempotency, Concurrency, and Retry

### 4.1 Idempotency
- **Idempotency key**: Define unique identifier to ensure duplicate requests don't produce side effects
- **Idempotent processing**: Return same result for duplicate requests

### 4.2 Concurrency Control
- Use distributed locks to prevent concurrent updates
- Set reasonable lock timeout
- Implement lock renewal mechanism

### 4.3 Retry Strategy
- Distinguish between retryable and non-retryable errors
- Set maximum retry count
- Implement exponential backoff strategy

---

## 5. Time Windows and Scheduling

### 5.1 Time Window Definition
- Define business time windows (e.g., 7:00-22:00)
- Handle requests outside time windows

### 5.2 Scheduling Strategy
- Use scheduled tasks to scan pending data
- Implement randomized shuffling strategy
- Support task rescheduling

---

## 6. Data Consistency

### 6.1 Strong Consistency
- Critical business data must guarantee strong consistency
- Use transactions to ensure data consistency

### 6.2 Eventual Consistency
- Non-critical business can accept eventual consistency
- Use message queues for async processing

### 6.3 Compensation Mechanism
- Implement compensation logic for failure scenarios
- Support manual and automatic compensation

---

## 7. Error Handling

### 7.1 Error Classification
- **Business errors**: User input errors, business rule violations
- **System errors**: System exceptions, external service exceptions
- **Network errors**: Timeout, connection failures

### 7.2 Error Code Definition
- Unified error code format
- Error codes include error type and description
- Error codes are traceable

### 7.3 Error Handling Strategy
- Business errors: Return error message directly
- System errors: Log and return friendly message
- Network errors: Retry mechanism, degradation handling

---

## 8. Performance Optimization

### 8.1 Database Optimization
- Use indexes appropriately
- Avoid N+1 queries
- Use batch operations

### 8.2 Caching Strategy
- Use caching reasonably
- Set cache expiration time
- Handle cache penetration, breakdown, avalanche

### 8.3 Async Processing
- Use async processing for non-critical paths
- Use message queues for decoupling
- Implement async task monitoring

---

## 9. Security Standards

### 9.1 Input Validation
- All inputs must be validated
- Prevent SQL injection, XSS attacks
- Sanitize sensitive information

### 9.2 Access Control
- Implement fine-grained access control
- Prevent unauthorized access
- Log operations

### 9.3 Data Encryption
- Encrypt sensitive data storage
- Use HTTPS for data transmission
- Manage keys securely

---

## 10. Monitoring and Alerting

### 10.1 Monitoring Metrics
- API response time
- Error rate
- System resource usage

### 10.2 Alerting Strategy
- Set reasonable alert thresholds
- Include context in alert information
- Avoid alert storms

---

## 11. Documentation Standards

### 11.1 Code Documentation
- Public interfaces must have documentation
- Complex logic must have comments
- Use type hints

### 11.2 API Documentation
- Provide complete API documentation
- Include request and response examples
- Explain error codes and handling

**Standard Note**: This standard is the microservice core standard, all services must follow.