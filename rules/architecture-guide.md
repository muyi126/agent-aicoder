---
description: Architecture design guide, including architecture layering, module division, and dependency direction rules. Use when designing service architecture or understanding project structure.
alwaysApply: false
---

# Architecture Design Guide

## 1. Architecture Layering

### 1.1 Standard Module Structure

#### Layer Structure
```
xxx-domain:   Domain models (DTO/VO/Constants/Validation)
xxx-facade:   External interface definitions + Error code enums
xxx-core:     Business orchestration + Domain services
xxx-dal:      Data access + Mappers
xxx-provider: Dubbo/HTTP service exposure
xxx-admin-*:  Admin backend
xxx-test:     Tests
```

#### Dependency Direction
```
provider → core → dal
    ↓
facade + domain
```

**Prohibited**:
- Controller → Mapper (cross-layer prohibited)
- Provider → Dal (direct database connection prohibited)
- X provider/service-impl directly connects to dal
- X cross-layer calls
- X domain contains business logic

### 1.2 Module Dependency Rules

- **domain**: Depends on no business modules
- **facade**: Only depends on domain
- **core**: Depends on facade + domain + dal
- **dal**: Only depends on domain
- **provider**: Depends on facade + core

---

## 2. Naming Conventions

| Type | Rule | Example |
|------|------|---------|
| Request DTO | `{Action}ReqDTO` | `CreateUserReqDTO` |
| Response DTO | `{Action}RespDTO` | `CreateUserRespDTO` |
| Manager | `{Domain}Manager` | `UserManager` |
| Adapter | `{System}Adapter` | `WeChatAdapter` |
| MapStruct | `{Entity}BeanMapper` | `UserBeanMapper` |
| Facade | `{Domain}Service` | `UserService` |
| ServiceImpl | `{Domain}ServiceImpl` | `UserServiceImpl` |
| ErrorCode | `{Module}ErrorCode` | `UserErrorCode` |
| **Package naming** | `com.{company}.{product}.{module}.{layer}` | |

---

## 3. File Location Standards

### 3.1 Controller Layer
- **Path**: `xxx-api/src/main/java/.../controller/`
- **Naming**: `Xxx[Module]Controller.java`

### 3.2 Service Layer
- **Path**: `xxx-core/src/main/java/.../service/`
- **Interface**: `xxx-facade/service/`
- **Implementation**: `xxx-core/service/impl/`

### 3.3 Mapper Layer
- **Java**: `xxx-dal/src/main/java/.../mapper/XxxMapper.java`
- **XML**: `xxx-dal/src/main/resources/mappers/XxxMapper.xml`

### 3.4 DTO/VO Layer
- **Path**: `xxx-domain/src/main/java/.../dto/`
- **Path**: `xxx-domain/src/main/java/.../vo/`
- **Naming**: `XxxDTO.java`, `XxxVO.java`, `XxxRequest.java`, `XxxResponse.java`

### 3.5 Error Codes
- **Path**: `xxx-facade/src/main/java/.../enums/`
- **Naming**: `XxxErrorCode.java`

### 3.6 Configuration Files
- **application.properties**: `xxx-provider/src/main/resources/`
- **dubbo config**: `xxx-provider/src/main/resources/META-INF/spring/`
- **SQL scripts**: `xxx-dal/src/main/resources/sql/` or `init/`

---

## 4. Common Task Locations

### 4.1 Adding New Interface
1. Define DTO: `xxx-domain/dto/`
2. Define Facade: `xxx-facade/service/`
3. Implement Service: `xxx-core/service/impl/`
4. Expose Controller: `xxx-api/controller/` (HTTP) or `xxx-provider/` (Dubbo)

### 4.2 Adding New Database Table
1. CREATE TABLE SQL: `xxx-dal/resources/sql/`
2. DO definition: `xxx-dal/entity/` or `domain/XxxDO.java`
3. Mapper interface: `xxx-dal/mapper/XxxMapper.java`
4. Mapper XML: `xxx-dal/resources/mappers/XxxMapper.xml`

### 4.3 Adding New Business Rules
1. Rule definition: `xxx-core/rule/` or `service/rule/`
2. Aggregation orchestration: `xxx-core/service/`
3. External interface: `xxx-facade/`

---

## 5. Architecture Rules

### 5.1 Cross-Layer Calls
- ✅ Controller → Service (Facade)
- ✅ Service → Mapper (Dal)
- ❌ Controller → Mapper (prohibited cross-layer)
- ❌ Provider → Dal (prohibited direct database connection)

### 5.2 Usage Recommendations

When precise paths are needed, use tools:
- `list_dir("service-name")` - View module structure
- `grep("ClassName")` - Precise file search
- `codebase_search("function description")` - Semantic search

---

## 6. Documentation Principles

1. All text must be in simplified Chinese
2. Keep diagrams clean and clear, avoid over-complexity
3. Use standard technical icons and symbols
4. Professional color scheme, generally use blue, green, orange technical colors
5. Clear hierarchy, primary and secondary relationships are distinct

### 6.1 Technical Standards
1. Architecture diagrams should have clear layers: presentation layer, gateway layer, service layer, data layer
2. Use dashed lines for async calls, solid lines for sync calls
3. Mark key technology stacks and middleware
4. Add necessary text explanations, but not too many
5. Consider high availability, load balancing and other architectural elements

### 6.2 Color Scheme
- Presentation layer: Blue series (#3498db)
- Business layer: Green series (#2ecc71)
- Data layer: Orange series (#e67e22)
- Middleware: Purple series (#9b59b6)
- Error/Warning: Red series (#e74c3c)