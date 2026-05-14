---
description: Channel integration development standards for third-party interface integration. Use when integrating with third-party APIs or adding new channel interfaces.
alwaysApply: false
---

# Channel Integration Development Standards

## 1. Trigger Conditions

When the user provides **specific third-party interface documentation/protocol** and asks to "integrate new interface", "add feature", or "implement this protocol", enter this mode.

---

## 2. Execution Flow

### Phase 1: Gap Analysis

1. **Locate reference point**: Based on the interface type provided by the user (e.g., marketing inquiry), find similar reference implementations in existing code.
   - **Example**: If adding "marketing inquiry", first search for existing `MarketingConsultService` or `MarketingConsultDispatcher`.
2. **Determine change points**: Compare with existing architecture, list files that need to be added/modified.
   - Facade layer: Add `Request/Response DTO`
   - Core layer: Add `Dispatcher` method implementation
   - Provider layer: Add `Dubbo Service` implementation

### Phase 2: Technical Design

**Must output the following standard design document**:

#### 2.1 Interface Definition
- **Facade interface**: Define method signature for `XxxService.java`
- **Data model**: Define input `XxxReqDTO` and output `XxxRespDTO` (including MapStruct conversion logic)

#### 2.2 Invocation Chain
Describe how data flows from entry to third party:
```
Facade (API) -> ServiceImpl (Core) -> Dispatcher (Adapter) -> Dubbo Provider (Atom) -> 3rd Party API
```

#### 2.3 Key Logic
- **Parameter mapping**: Explain how to map core domain model to third-party parameters (e.g., `userId` -> `jd_pin`)
- **Exception handling**: Define how third-party return codes map to internal `ChannelErrorCode`

### Phase 3: Task List Generation

Generate an atomic implementation checklist:

## Implementation Task List

- [ ] **Domain layer**: Define `XxxReqDTO` and `XxxRespDTO` (path: `xxx-domain/...`)
- [ ] **Facade layer**: Add interface definition in `XxxService`
- [ ] **Core layer (Dispatcher)**: Implement method in `XxxDispatcher`, complete parameter assembly
- [ ] **Provider layer**: Add `XxxService` in `Xxx-paycenter-core-xxx`, call third-party SDK
- [ ] **Configuration layer**: Add interface URL and key configuration in config center
- [ ] **Testing**: Write unit tests to verify message conversion

---

## 3. Constraints and Standards

1. **Consistency first**: Must follow existing code style (naming conventions, exception handling, log format). If existing code uses `MapStruct`, you must also use it, do not use `BeanUtils`.
2. **Anti-corruption layer principle**: Absolutely prohibit passing third-party SDK objects directly through Core layer; must convert to internal DTO in Provider layer.
3. **Configuration separation**: All hardcoded URLs and AppKeys must be extracted to configuration classes.

---

## 4. Example Interaction

**User**: "JD added a marketing inquiry interface, URL is `/marketing/query`, input params are `skuId`, `userPin`, output is `discountAmount`. Please provide a solution."

**AI**:
1. **Search**: Found existing `ChannelMarketingConsultService`.
2. **Solution**:
   - Reuse `ChannelMarketingConsultService`
   - Modify `JdpayDispatcher` to implement `ChannelMarketingConsultDispatcher` interface
   - Add `JdMarketingConsultRequest` (third-party structure)
3. **Review**: [List the above specific modification tasks]

---

## 5. Exception Handling

- **Network exception**: When calling third-party SDK fails, catch exception, log and return friendly message
- **Parameter error**: Validate input parameters in Facade layer; if parameters are null, throw `IllegalArgumentException`
- **Return data exception**: When Provider layer converts results, if return value is `null`, return default value or throw exception

---

## 6. Test Cases

1. **Normal flow**: Pass valid parameters, verify correct return result
2. **Empty parameters**: Test different parameter null scenarios, verify exception throwing is correct
3. **Network exception**: Simulate network errors, verify retry mechanism and error messages

---

## 7. Other Notes

1. **Code reuse**: Core logic must be abstracted into common utility classes to avoid duplicate development
2. **Logging standards**: Key nodes (input, output, exceptions) must print logs with unified format
3. **Compatibility**: Must be compatible with project technology stack version, avoid using incompatible APIs
4. **Documentation update**: After feature development is complete, must synchronize interface documentation and test reports