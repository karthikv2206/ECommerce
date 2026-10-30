# Constraint / Resilience



> Organized from existing Repository Intelligence asset (`rules_md`).

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

## Async & Concurrency

### Required Async Patterns

- **Service Layer Operations**: All service layer methods that perform database operations or external API calls should be designed to support asynchronous execution
- **Controller Dependencies**: Controllers must properly inject service dependencies to enable async processing patterns
- **Serializable Entities**: All domain entities must implement `Serializable` interface to support async processing and caching mechanisms

### Forbidden Sync Patterns

### Implementation Guidelines

- Use dependency injection for service components to enable proper async execution context
- Ensure all entity classes implement `Serializable` for session storage, data access layers

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