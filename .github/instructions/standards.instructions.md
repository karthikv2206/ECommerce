---
applyTo: '**'
---
# standards

## API Standards
### REST API Design Standards

#### URL Structure
- Use lowercase paths with hyphens for word separation
- Follow RESTful resource naming conventions
- Admin endpoints prefixed with `/admin/`
- User-specific endpoints include `{username}` parameter

#### HTTP Methods
- `GET` for data retrieval operations
- `POST` for data creation and updates
- Follow HTTP status code conventions

#### Path Parameters
- Use descriptive parameter names: `{category_id}`, `{product_id}`, `{user_id}`
- Maintain consistency across similar endpoints
- Include username in user-specific operations: `/{username}/cart`

### Controller Standards

#### Naming Conventions
- Controllers suffixed with "Controller": `CartController`, `ProductController`
- Admin controllers prefixed with "Admin": `AdminCategoryController`, `AdminProductController`
- Use descriptive class names reflecting functionality

#### Package Organization

```
com.example.controller/
├── HelloWorldController
├── CartController
├── ProductController
├── UserAccountController
├── WishlistController
└── admin/
    ├── AdminController
    ├── AdminCategoryController
    ├── AdminProductController
    ├── AdminSupplierController
    └── AdminUserController
```

### Service Layer Standards

#### Interface Design
- Define service interfaces for all business logic
- Implementation classes suffixed with "Impl": `CategoryServiceImpl`
- Separate interfaces from implementations

#### Package Structure

```
com.example.service/
├── CategoryService
├── ProductService
├── UserService
├── SupplierService
├── MailService
└── impl/
    ├── CategoryServiceImpl
    ├── ProductServiceImpl
    ├── UserServiceImpl
    ├── SupplierServiceImpl
    └── MailServiceImpl
```

### Data Access Standards

#### DAO Pattern
- Define DAO interfaces for all data access operations
- Implementation classes suffixed with "DaoImpl": `CategoryDaoImpl`
- Consistent CRUD operation naming

#### Package Organization

```
com.example.dao/
├── CategoryDao
├── ProductDao
├── UserDao
├── SupplierDao
├── CartDao
└── impl/
    ├── CategoryDaoImpl
    ├── ProductDaoImpl
    ├── UserDaoImpl
    ├── SupplierDaoImpl
    └── CartDaoImpl
```

### Security Standards

#### Authentication
- Use Spring Security for authentication and authorization
- Implement custom success handlers for role-based redirects
- Protect admin endpoints with appropriate security constraints

#### Role-Based Access
- Support multiple user roles: DBA, Admin, User
- Implement proper authorization checks
- Secure sensitive operations and data access

### Configuration Standards

#### Spring Configuration
- Separate configuration classes by concern:
  - `SecurityConfiguration` for security settings
  - `HibernateConfiguration` for ORM setup
  - `HelloWorldConfiguration` for MVC configuration
  - `WebFlowConfig` for web flow setup

#### Initialization
- Use proper Spring initializers:
  - `SpringMvcInitializer` for MVC setup
  - `SecurityWebApplicationInitializer` for security

### Error Handling Standards

#### Access Control
- Provide dedicated access denied page: `/Access_Denied`
- Implement proper error responses for unauthorized access
- Use consistent error messaging across the application

## Testing Standards
### Testing Framework Requirements

#### Primary Testing Stack
- **JUnit 5**: Primary testing framework for unit and integration tests
- **Mockito**: Mocking framework for service dependencies
- **Spring Boot Test**: Integration testing with Spring context
- **TestContainers**: Database integration testing (recommended)

### Code Coverage Standards

| Layer | Minimum Coverage | Target Coverage |
|-------|-----------------|----------------|
| Service Layer | 80% | 90% |
| Controller Layer | 70% | 85% |
| Repository Layer | 60% | 80% |
| Entity Layer | 50% | 70% |

### Test Organization

#### Directory Structure

```
src/test/java/
├── unit/
│   ├── service/
│   ├── controller/
│   └── entity/
├── integration/
│   ├── api/
│   └── repository/
└── e2e/
    └── scenarios/
```

#### Test Categories
- **@Tag("unit")**: Fast, isolated unit tests
- **@Tag("integration")**: Tests with Spring context
- **@Tag("e2e")**: Full application tests
- **@Tag("slow")**: Tests that require external resources

### Quality Gates

#### Pre-commit Requirements
- All unit tests must pass
- Code coverage must not decrease
- No test compilation errors

#### CI/CD Pipeline Requirements
- Unit tests run on every commit
- Integration tests run on pull requests
- E2E tests run on main branch merges
- Performance tests run nightly

### Test Data Patterns

#### Builder Pattern for Test Objects

#### Test Profiles
- `application-test.properties`: Test-specific configuration
- In-memory database for unit tests
- TestContainers for integration tests

### Assertion Standards

## API Standards
### HTTP Method Conventions

**Non-Standard DELETE Operations**
- The application uses GET requests for delete operations instead of the standard DELETE method
- Examples:
  - `GET /admin/categorys/delete/{category_id}` - Deletes a category
  - `GET /admin/products/delete/{product_id}` - Deletes a product
  - `GET /admin/users/delete/{id}` - Deletes a user

**Standard Method Usage**
- GET: Used for retrieving data and navigation (home page, dashboards, forms)
- POST: Used for data creation and updates (adding categories, products, updating accounts)

### URL Path Conventions

**Admin Routes**
- Admin functionality follows `/admin/{resource}/{action}` pattern
- Examples: `/admin/categorys/category`, `/admin/products/product`, `/admin/users/user`

**User-Specific Routes**
- User-specific functionality uses `/{username}` or `/user/{username}` patterns
- Examples: `/{username}/cart`, `/user/{username}/wishlist`, `/user/{username}/account`

**Resource Management**
- Edit operations: `/{resource}/edit/{id}`
- Delete operations: `/{resource}/delete/{id}` (using GET method)
- Add operations: `/{resource}/add` (using POST method)

### Response Standards

**Return Types**
- Most endpoints return String type, typically representing view names or redirect paths
- This suggests a server-side rendered application using template engines

### API Versioning

follow direct path routing

### Controller Dependencies

**Service Injection Pattern**
- Controllers must inject required service dependencies:
  - `productService` for product operations
  - `userService` for user management
  - `categoryService` for category management

### Data Serialization

**Entity Requirements**
- All domain entities must implement the `Serializable` interface
- Applies to core entities: Cart, Product, User
- Ensures proper data transfer and session management

## Engineering Standards
### Code Review Standards

#### API Design Requirements
- **HTTP Method Consistency**: Review endpoints for proper HTTP method usage. Currently, some delete operations use GET method instead of DELETE, which should be addressed for RESTful compliance
- **Return Type Validation**: Ensure endpoint return types are appropriate - most controllers return String types for view names or redirect paths

#### Code Quality Standards

##### Entity Design
- **Serialization Compliance**: All domain entities (Cart, Product, User) must implement the Serializable interface
- **Data Integrity**: Validate that entity relationships and constraints are properly defined

##### Controller Standards
- **Dependency Injection**: Controllers must properly inject required service dependencies:
  - ProductController requires productService
  - CartController requires userService and categoryService
  - Validate all service dependencies are correctly wired

##### Language Standards
- **Primary Language**: Java is the established primary programming language for this repository
- **Code Consistency**: Maintain consistent Java coding patterns across all modules

### Documentation Standards

#### API Documentation
- Document all endpoint behaviors, especially non-standard implementations
- Include parameter validation rules and response formats
- Note any deviations from REST conventions with justification

#### Code Documentation
- Service layer methods must include JavaDoc comments
- Entity relationships should be clearly documented
- Controller endpoint purposes and expected behaviors must be documented

### Development Standards

#### Review Checklist
- [ ] Service dependencies properly injected in controllers
- [ ] Entities implement required interfaces (Serializable)
- [ ] HTTP methods align with operation semantics
- [ ] Return types match endpoint purposes
- [ ] Code follows established Java conventions

#### Quality Gates
- All domain entities must pass serialization validation
- Controller-service wiring must be verified during code review
- API endpoint compliance with documented standards

## Code Quality Standards
### Linting and Formatting

#### Java Code Standards
- **Primary Language**: Java is the primary programming language for this repository
- **Code Formatting**: Follow standard Java formatting conventions
- **Import Organization**: Organize imports according to Java best practices

### Quality Metrics and Rules

#### High Priority Standards
- **Serializable Entities**: All domain entities must implement the `Serializable` interface
  - Applies to: `Cart`, `Product`, `User` classes
  - Ensures proper serialization support for session management and caching

- **Service Dependencies**: Controllers must properly inject required service dependencies
  - Required services: `productService`, `userService`, `categoryService`
  - Applies to: `CartController`, `ProductController`
  - Use proper dependency injection patterns

#### Medium Priority Standards
- **HTTP Method Consistency**: Review and standardize HTTP method usage
  - Current issue: DELETE operations using GET method (e.g., `/admin/categorys/delete/{category_id}`)
  - Recommendation: Use appropriate HTTP methods (DELETE for delete operations)

#### Low Priority Standards
- **Return Type Consistency**: Standardize controller return types
  - Current pattern: Most endpoints return `String` type for view names or redirect paths
  - Ensure consistent return type patterns across controllers

### Quality Checks

#### Pre-commit Validation
- Verify all entities implement required interfaces
- Check service dependency injection in controllers
- Validate HTTP method usage patterns
- Ensure proper return type consistency

#### Code Review Guidelines
- Review serialization implementation in domain entities
- Validate controller-service dependency patterns
- Check for proper HTTP method usage
- Verify consistent return type patterns