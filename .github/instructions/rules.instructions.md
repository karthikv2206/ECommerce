---
applyTo: '**'
---
# rules

## Naming Conventions
### Classes

- **Domain Entities**: Use singular nouns in PascalCase (e.g., `Cart`, `Product`, `User`)
- **Controllers**: Append "Controller" suffix to entity name (e.g., `CartController`, `ProductController`)
- **Services**: Append "Service" suffix with camelCase (e.g., `productService`, `userService`, `categoryService`)

### Methods and Functions

- Use camelCase for method names
- HTTP endpoint methods should reflect their purpose despite using GET method consistently
- Delete operations follow pattern: `delete/{entity_id}` (e.g., `/admin/categorys/delete/{category_id}`)

### Variables

- Use camelCase for variable names
- Service dependencies should follow pattern: `{entity}Service` (e.g., `categoryService`)

### Files and Modules

- **Controllers**: Place in appropriate controller packages
- **Entities**: Group domain entities together
- **Services**: Organize service classes in service packages
- Use consistent file naming that matches class names

### URL Patterns

- Admin endpoints: `/admin/{entity}/{action}`
- Entity operations: `/{entity}/{action}/{id}`
- Note: Some inconsistencies exist (e.g., `categorys` instead of `categories`)

### Interface Implementation

- All domain entities must implement `Serializable` interface
- Follow Java naming conventions for interface implementations

## Architecture Rules
### Layer Boundaries

#### Controller Layer Rules
- Controllers must only handle HTTP request/response concerns
- Controllers must delegate business logic to the service layer
- Controllers must inject required service dependencies (productService, userService, categoryService)
- Controllers should return String types for view names or redirect paths

#### Service Layer Rules
- Services contain all business logic and validation
- Services must not directly handle HTTP concerns
- Services coordinate between controllers and DAO layer
- Services must use DAO layer for all data access operations

#### DAO Layer Rules
- DAOs handle only data access and persistence operations
- DAOs must not contain business logic
- DAOs provide abstraction over data storage mechanisms

#### Model Layer Rules
- All domain entities must implement Serializable interface
- Models represent pure domain concepts without framework dependencies
- Models should not contain business logic methods

### Import Rules

#### Allowed Dependencies
- Controllers may import: Services, Models, Spring MVC annotations
- Services may import: DAOs, Models, Spring annotations
- DAOs may import: Models, persistence frameworks, Spring Data annotations
- Models may import: Standard Java libraries, validation annotations

#### Forbidden Dependencies
- Models must not import Spring MVC or web-specific classes
- DAOs must not import controller or web layer classes
- Services must not import HTTP servlet classes directly

### Forbidden Patterns

#### HTTP Method Misuse
- **Issue**: Using GET method for delete operations
- **Rule**: DELETE operations must use HTTP DELETE method, not GET
- **Current Violation**: Delete endpoints incorrectly use GET method

#### Direct Database Access
- Controllers must not directly access database or persistence layer
- All data access must go through the DAO layer

#### Business Logic in Controllers
- Controllers must not contain business rules or validation logic
- All business logic must reside in the service layer

#### Cross-Layer Dependencies
- Lower layers (DAO, Model) must not depend on higher layers (Service, Controller)
- Circular dependencies between layers are forbidden

## Security Rules
### Authentication & Authorization

- **Controller Access Control**: All controllers must implement proper authentication checks before processing requests
- **Service Layer Security**: Service dependencies (productService, userService, categoryService) must validate user permissions before data operations
- **Session Management**: Implement secure session handling for user authentication state

### Input Validation

`{category_id}`

### HTTP Method Security

### Data Protection

- **Serialization Security**: Entities implementing Serializable interface must not expose sensitive data during serialization
- **Sensitive Data Handling**: Never log or expose user credentials, payment information, or personal data
- **Database Security**: Use parameterized queries to prevent SQL injection

### Secret Management

- **Configuration Security**: Store database credentials

## Async & Concurrency
### Required Async Patterns

- **Service Layer Operations**: All service layer methods that perform database operations or external API calls should be designed to support asynchronous execution
- **Controller Dependencies**: Controllers must properly inject service dependencies to enable async processing patterns
- **Serializable Entities**: All domain entities must implement `Serializable` interface to support async processing and caching mechanisms

### Forbidden Sync Patterns

### Implementation Guidelines

- Use dependency injection for service components to enable proper async execution context
- Ensure all entity classes implement `Serializable` for session storage, data access layers

## Error Handling
### Required Patterns

#### Exception Handling in Controllers
- All controller methods must implement proper exception handling
- Use try-catch blocks for service layer operations that may throw exceptions
- Return appropriate error views or redirect to error pages

#### Service Layer Error Propagation
- Service methods should throw specific business exceptions
- Use custom exception classes for different error scenarios
- Maintain exception context and error messages

#### HTTP Status Code Compliance
- Return appropriate HTTP status codes for different error conditions
- Use 404 for resource not found scenarios
- Use 400 for validation errors
- Use 500 for internal server errors

#### Logging Requirements
- Log all exceptions with appropriate severity levels
- Include relevant context information (user ID, operation details)
- Use structured logging for better error tracking

### Forbidden Patterns

#### Silent Failures
- **FORBIDDEN**: Catching exceptions without proper handling or logging
- **FORBIDDEN**: Returning null or empty responses without error indication
- **FORBIDDEN**: Ignoring validation failures

#### Generic Exception Handling
- **FORBIDDEN**: Using generic Exception catch blocks without specific handling
- **FORBIDDEN**: Throwing generic RuntimeException without context
- **FORBIDDEN**: Using printStackTrace() instead of proper logging

#### Inconsistent Error Responses
- **FORBIDDEN**: Different error response formats across endpoints
- **FORBIDDEN**: Exposing internal system details in error messages
- **FORBIDDEN**: Inconsistent HTTP status codes for similar error conditions

#### Resource Management
- **FORBIDDEN**: Not properly closing resources in finally blocks or try-with-resources
- **FORBIDDEN**: Leaving database connections open on exceptions
- **FORBIDDEN**: Memory leaks due to improper exception handling

## Database Access
### ORM Usage

- **Entity Requirements**: All domain entities must implement the `Serializable` interface for proper ORM functionality and session management
- **Service Layer Pattern**: Database access must be performed through the service layer, not directly from controllers
- **DAO Pattern**: Implement the Data Access Object (DAO) pattern for database operations to maintain separation of concerns

### Service Dependencies

- **Controller Injection**: Controllers must inject required service dependencies:
  - `productService` for product-related operations
  - `userService` for user management
  - `categoryService` for category operations
- **Interface-Implementation**: Follow interface-implementation pattern for service and DAO layers

### Data Access Guidelines

- **Layered Architecture**: Maintain strict layering - Controllers → Services → DAOs → Database
- **Business Logic Separation**: Keep business logic in the service layer, not in DAOs or controllers
- **Transaction Management**: Handle database transactions at the service layer level

### Migration Conventions

- **Entity Serialization**: Ensure all new entities implement `Serializable` interface
- **Service Integration**: New database entities must have corresponding service classes
- **Controller Integration**: Controllers accessing new entities must inject appropriate service dependencies

## Testing Standards
### Test Coverage Requirements

- **Unit Tests**: Minimum 80% code coverage for service layer components
- **Integration Tests**: Required for all controller endpoints and database operations
- **End-to-End Tests**: Critical user flows must have automated E2E coverage

### Test Naming Conventions

#### Unit Tests
- Format: `methodName_condition_expectedResult`
- Examples:
  - `getUserById_validId_returnsUser`
  - `addToCart_invalidProduct_throwsException`
  - `calculateTotal_emptyCart_returnsZero`

#### Integration Tests
- Format: `testEndpointName_scenario_expectedOutcome`
- Examples:
  - `testGetProducts_validRequest_returnsProductList`
  - `testDeleteCategory_existingId_removesCategory`

#### Test Class Naming
- Unit test classes: `{ClassName}Test`
- Integration test classes: `{ClassName}IntegrationTest`
- E2E test classes: `{Feature}E2ETest`

### Required Test Types

#### Controller Layer Tests
- **HTTP Method Validation**: Verify correct HTTP methods (note: system uses GET for delete operations)
- **Request/Response Mapping**: Test parameter binding and response formatting
- **Service Integration**: Mock service dependencies and verify interactions
- **Error Handling**: Test exception scenarios and error responses

#### Service Layer Tests
- **Business Logic**: Test all business rules and calculations
- **Data Validation**: Verify input validation and sanitization
- **Exception Handling**: Test error conditions and recovery
- **Dependency Mocking**: Mock repository and external service calls

#### Entity Tests
- **Serialization**: Verify Serializable implementation works correctly
- **Validation**: Test entity validation rules
- **Relationships**: Test entity associations and cascading operations

### Test Data Management

## Anti-Patterns (Never Do)
### HTTP Method Misuse

```java
// BAD: Using GET for delete operations
@GetMapping("/admin/categorys/delete/{category_id}")
public String deleteCategory(@PathVariable Long category_id) {
    // This violates HTTP semantics
}
```

```java
// GOOD: Use DELETE for delete operations
@DeleteMapping("/admin/categorys/{category_id}")
public ResponseEntity<Void> deleteCategory(@PathVariable Long category_id) {
    categoryService.delete(category_id);
    return ResponseEntity.noContent().build();
}
```

### Entity Design Violations

**❌ NEVER create entities without Serializable**

```java
// BAD: Missing Serializable implementation
@Entity
public class Product {
    // This will cause issues with caching and session storage
}
```

**✅ DO implement Serializable for all entities**

```java
// GOOD: Proper entity with Serializable
@Entity
public class Product implements Serializable {
    private static final long serialVersionUID = 1L;
    // Entity fields...
}
```

### Controller Anti-Patterns

**❌ NEVER create controllers without proper service injection**

```java
// BAD: Missing service dependencies
@Controller
public class ProductController {
    // No service injection - will cause runtime errors
}
```

**❌ NEVER return raw strings for REST APIs**

```java
// BAD: Returning view names for API endpoints
@GetMapping("/api/products")
public String getProducts() {
    return "product-list"; // This should return data, not view names
}
```

### General Development Anti-Patterns

API responses** in the same controller
- **Never use GET requests for state-changing operations** (create, update