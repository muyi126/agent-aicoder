---
description: Microservice development standards, including code conventions, layered responsibilities, and exception handling. Use when writing code or reviewing implementations.
alwaysApply: false
---

# Microservice Development Standards

## 1. Code Conventions

### 1.1 Annotation Usage

#### DTO Definition
```python
from dataclasses import dataclass
from typing import Optional

@dataclass
class XxxReqDTO:
    field: str
    count: Optional[int] = None
```

#### Service Implementation
```python
from flask import current_app
import logging

logger = logging.getLogger(__name__)

class XxxServiceImpl:
    def __init__(self, manager):
        self.manager = manager
```

### 1.2 Parameter Validation

```python
from pydantic import BaseModel, validator

class XxxReqDTO(BaseModel):
    field: str

    @validator('field')
    def validate_field(cls, v):
        if not v or len(v) < 3 or len(v) > 20:
            raise ValueError('Field length must be between 3-20 characters')
        return v
```

### 1.3 Logging Conventions

```python
import logging

logger = logging.getLogger(__name__)

def xxx_method(self, req):
    logger.info(f"xxx start | traceId={trace_id} | key={key}")
    start = time.time()
    try:
        resp = self.manager.xxx(req)
        logger.info(f"xxx success | traceId={trace_id} | cost={int((time.time() - start) * 1000)}ms")
        return resp
    except CommonBusinessException as e:
        logger.error(f"xxx business error | traceId={trace_id} | errorCode={e.code} | cost={int((time.time() - start) * 1000)}ms", exc_info=True)
        raise
    except Exception as e:
        logger.error(f"xxx system error | traceId={trace_id} | cost={int((time.time() - start) * 1000)}ms", exc_info=True)
        raise
```

**Standards**:
- Use structured logging: `key=value | key=value`
- Exceptions must record stack traces
- **Prohibited**: Recording sensitive information (phone numbers/ID cards/passwords must be masked)
- **Prohibited**: INFO logs in loops

### 1.4 Exception Handling

```python
# Manager layer: throw directly
if user is None:
    raise BusinessException(UserErrorCode.USER_NOT_FOUND, "User does not exist")

# Service layer: catch and log
try:
    result = self.manager.xxx(req)
except BusinessException as e:
    logger.error(f"business error, errorCode={e.code}", exc_info=True)
    raise
```

**Standards**:
- Unified exception class for throwing
- Service layer catches and logs
- Manager layer throws directly, does not catch
- **Prohibited**: Swallowing exceptions

---

## 2. Layered Responsibilities

### 2.1 Service Layer (Thin Layer)
- Parameter validation (Pydantic + business rules)
- Logging (INFO at entry, ERROR on exceptions)
- Exception handling (unified exception handling)
- Business orchestration (delegated to Manager)
- **Prohibited**: Contains business logic

### 2.2 Manager Layer (Core)
- Business process orchestration
- Database access (through DAL)
- External calls (through Adapter)
- Cache operations
- Message sending
- **Prohibited**: Catching exceptions (throw directly)

---

## 3. Mandatory Standards (P0)

### 3.1 Object Mapping
- **Mandatory**: Use data classes or Pydantic models
- **Prohibited**: Hand-writing field copies
- **Use**: Type hints

### 3.2 SQL Standards
```python
# Correct
query = "SELECT id, name, status, create_time FROM t_xxx WHERE id = %s ORDER BY create_time DESC LIMIT %s OFFSET %s"

# Prohibited
query = "SELECT * FROM t_xxx"
```

**Standards**:
- **Prohibited**: SELECT *
- **Pagination must have**: ORDER BY
- **WHERE must hit**: Indexes
- **Avoid**: N+1, prefer batch queries

---

## 4. Development Checklist

### 4.1 Interface Development
- [ ] domain defines ReqDTO/RespDTO
- [ ] domain/errorcode defines error codes
- [ ] facade defines interface (with documentation)
- [ ] service-impl implements (validation + orchestration only)
- [ ] manager implements business logic
- [ ] Logging (INFO entry + cost, ERROR exception + stack)
- [ ] Unit test coverage: 70%+

### 4.2 SQL Development
- [ ] Prohibited SELECT *
- [ ] Pagination with ORDER BY
- [ ] WHERE hits index
- [ ] EXPLAIN analysis for slow queries

### 4.3 Pre-commit
- [ ] DTO uses data class or Pydantic
- [ ] Logs use structured format
- [ ] Exceptions thrown through unified exception class
- [ ] Unit test coverage ≥70%
- [ ] No compilation warnings

### 4.4 Pre-release
- [ ] SQL slow query optimization
- [ ] External interface timeout and retry settings
- [ ] Rate limiting and degradation configuration
- [ ] Monitoring and alerting configuration

---

## 5. Standard Priority

**P0 (Mandatory)**: Layer constraints, parameter validation, error codes, logging standards, unit test ≥70%, SQL prohibited *, type hints
**P1 (Important)**: Cache strategy, transaction boundaries, exception handling, configuration externalization, sensitive data masking
**P2 (Recommended)**: Performance optimization, concurrency control, monitoring alerting, gray release

---

## 6. Common Tools and Libraries

### 6.1 Data Validation
- Pydantic: Data validation and serialization
- dataclasses: Data class definition

### 6.2 Logging
- logging: Python standard logging library
- Structured logging format

### 6.3 Exception Handling
- Custom exception classes
- Unified exception handling middleware

**Standard Note**: This standard is the core development standard, P0 level must be strictly followed.