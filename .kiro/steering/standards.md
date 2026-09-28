# standards

## Project Overview

This project comprises 2 repositories: pace-lumen-ui, ECommerce. The sections below consolidate repository-specific evidence and shared concepts without dropping any participating repository.

## Repository Architecture

### pace-lumen-ui

### API Design Standards

#### REST API Conventions

##### HTTP Methods

- **GET** - Used for data retrieval operations (AI team agents, GitHub repositories)
- **POST** - Used for creation and action operations (workflow cancellation)
- **PATCH** - Used for partial updates (validation gap updates)
- **DELETE** - Used for resource deletion (configuration removal)

##### Endpoint Patterns

- Root endpoints (`/`) for primary operations
- Named endpoints for specific functionality (`/githubApp`, `/aidlcOrgRepos`)
- RESTful resource naming conventions

#### Service Architecture Standards

##### Service Layer Pattern

Implement layered service architecture:

```
Component → Service → API Service → External API
```

Example: `ChatBotComponent` → `ChatBotService` → `ChatBotApiService`

##### Interface Definitions

- All data structures must be defined as TypeScript interfaces
- Interfaces should be co-located with their primary consumers
- Use descriptive names that indicate purpose (e.g., `ActivityProgressEntry`, `AgentFinding`)

##### Data Formats

- Use JSON for all API responses
- Implement consistent response structure
- Include metadata for complex operations (progress tracking, pagination)

#### Integration Standards

##### External Service Integration

- GitHub integration through dedicated endpoints (`/githubApp`, `/aidlcOrgRepos`)
- Implement proper authentication and authorization
- Handle rate limiting and API quotas

##### Real-time Communication

- Use Server-Sent Events (SSE) for streaming operations
- Implement proper connection management and error recovery
- Support session-based communication for chatbot functionality

### Testing Standards

#### Testing Framework Requirements

**Test Structure Standards**

**Component Testing**

#### Quality Gates

#### Test Data Management

services

teardown methods

#### RESTful Design Principles

- **GET**: Retrieve resources (e.g., `/`, `/githubApp`, `/aidlcOrgRepos`)
- **POST**: Create resources or trigger actions (e.g., cancel workflow runs)
- **PATCH**: Partial updates (e.g., validation gap updates)
- **DELETE**: Remove resources (e.g., configuration deletion)

#### Endpoint Conventions

##### Root Endpoints

The application uses root-level endpoints (`/`) with different HTTP methods for distinct operations:

| Method | Purpose | Handler |
|--------|---------|----------|
| POST | Workflow cancellation | `cancelWorkflowRun` |
| PATCH | Validation updates | `updateValidationGap` |
| DELETE | Error handling | `onStreamError` |

##### Named Endpoints

Specific functionality uses descriptive path names:

- `aidlcOrgRepos` - AI Development Lifecycle organization repositories
- `githubApp` - GitHub application integration endpoints

#### Error Handling Standards

- DELETE operations at root level are handled by `onStreamError` for consistent error processing
- All endpoints implement proper error response patterns

#### API Versioning

- Path-based versioning (e.g., `/v1/`, `/v2/`)
- Header-based versioning for backward compatibility

### Engineering Standards

#### Code Review Standards

##### Component Development

- All components must implement proper lifecycle management through `ngOnInit` and `ngOnDestroy` interfaces
- Components should follow Angular best practices for initialization and cleanup
- Memory leaks must be prevented through proper subscription management in `ngOnDestroy`

##### Classification and Categorization

- Components handling business logic must implement risk and priority classification methods
- Classification methods should provide consistent categorization across the application
- Risk assessment functionality should be implemented where applicable

#### Documentation Standards

##### Code Documentation

- All public methods and properties must include JSDoc comments
- Component lifecycle methods should document their specific responsibilities
- Classification and categorization logic must be clearly documented

##### Component Documentation

- Each component should include usage examples
- Input/output properties must be documented with types and descriptions
- Component responsibilities and dependencies should be clearly stated

#### Development Standards

##### Angular Best Practices

- Follow Angular style guide conventions
- Implement OnInit and OnDestroy interfaces explicitly
- Use TypeScript strict mode for type safety
- Maintain consistent naming conventions across components

##### Quality Assurance

- All components must pass linting checks
- Unit tests required for lifecycle methods and business logic
- Integration tests for component interactions
- Code coverage targets must be maintained

### Code Quality Standards

#### Linting and Formatting

#### Component Quality Requirements

##### Lifecycle Management

- **Requirement**: All Angular components must implement proper lifecycle management
- **Implementation**: Components must implement `ngOnInit` and `ngOnDestroy` interfaces
- **Severity**: Medium
- **Rationale**: Ensures proper resource cleanup and initialization patterns

##### Classification Methods

- **Requirement**: Components handling data categorization must implement risk and priority classification methods
- **Implementation**: Implement standardized classification methods for consistent categorization logic
- **Severity**: Medium
- **Rationale**: Maintains consistent data classification across the application

#### Quality Metrics

##### Code Coverage

- Maintain minimum code coverage thresholds for unit tests
- Focus on critical business logic and component lifecycle methods

##### Static Analysis

- All code must pass static analysis checks
- Address linting warnings before code review
- Follow TypeScript strict mode requirements

##### Performance Standards

- Components must follow Angular performance best practices
- Implement OnPush change detection strategy where applicable
- Avoid memory leaks through proper subscription management

### ECommerce

### API Standards and Conventions

#### REST API Design Standards

##### URL Structure Conventions

- **Resource-based URLs**: Use nouns to represent resources (`/products`, `/categories`, `/users`)
- **Hierarchical structure**: Nested resources follow parent-child relationships (`/user/{username}/cart`)
- **Admin namespace**: Administrative endpoints prefixed with `/admin/` for clear separation
- **Parameterized routes**: Use path parameters for resource identification (`{username}`, `{id}`)

##### HTTP Method Usage

- **GET**: Retrieve resources and render pages
- **POST**: Create new resources and update operations
- **RESTful operations**: Follow standard HTTP semantics for CRUD operations

#### Controller Architecture Standards

##### Package Organization

```
com.example.controller/
├── admin/                    # Administrative controllers
│   ├── AdminCategoryController
│   ├── AdminProductController
│   ├── AdminSupplierController
│   └── AdminUserController
├── CartController           # Shopping cart operations
├── ProductController        # Product display and search
├── UserAccountController    # User account management
└── HelloWorldController     # Main application controller
```

##### Controller Naming Conventions

- **Descriptive names**: Controllers named after primary resource or function
- **Admin prefix**: Administrative controllers prefixed with "Admin"
- **Consistent suffixes**: All controllers end with "Controller"

#### Data Access Standards

##### DAO Pattern Implementation

- **Interface-based design**: All data access through DAO interfaces
- **Implementation separation**: Concrete implementations in `impl` package
- **Consistent naming**: DAO interfaces follow `{Entity}Dao` pattern
- **Service layer**: Business logic separated into service interfaces and implementations

##### Service Layer Standards

```
com.example.service/
├── CategoryService          # Category business logic interface
├── ProductService           # Product business logic interface
├── UserService             # User business logic interface
├── MailService             # Email service interface
└── impl/                   # Service implementations
    ├── CategoryServiceImpl
    ├── ProductServiceImpl
    └── UserServiceImpl
```

#### Security Standards

##### Authentication Flow

- **Role-based access**: Different access levels (admin, DBA, user)
- **Custom success handling**: Role-specific redirection after authentication
- **Protected endpoints**: Administrative functions require appropriate roles

##### Authentication Flow

```
GET /login          # Login form
POST /authenticate  # Login processing
GET /logout         # Logout handling
GET /Access_Denied  # Unauthorized access
```

##### Configuration Standards

- **Separation of concerns**: Security, web, and database configurations in separate classes
- **Spring Security integration**: Standardized security configuration patterns
- **Initialization classes**: Proper web application initialization setup

##### View Resolution

- **Consistent view naming**: Views follow resource-based naming conventions
- **Template organization**: Logical grouping of view templates
- **Error handling**: Standardized error pages and access denied responses

##### Model Binding

- **Entity-based models**: Controllers work with domain entities
- **Consistent model attributes**: Standardized attribute naming in views
- **Form handling**: Consistent form submission and validation patterns

#### URL Structure and Naming Conventions

##### Path Naming

- Use lowercase paths with hyphens for multi-word resources
- Follow RESTful resource naming patterns
- Admin routes are prefixed with `/admin/`
- User-specific routes include username in path: `/user/{username}/`

##### Resource Hierarchy

```
/                           # Home page
/admin/                     # Admin section
  ├── /dashboard           # Admin dashboard
  ├── /categorys/          # Category management
  ├── /products/           # Product management
  ├── /suppliers/          # Supplier management
  └── /users/              # User management
/user/{username}/           # User-specific resources
  ├── /account             # User account
  ├── /cart                # Shopping cart
  └── /wishlist            # User wishlist
```

#### HTTP Methods and Operations

##### Standard CRUD Operations

- **GET**: Retrieve resources and render pages
- **POST**: Create new resources and update existing ones
- **DELETE**: Remove resources (implemented via GET for web compatibility)

##### Method Usage Patterns

| Operation | Method | Path Pattern | Example |
|-----------|--------|--------------|----------|
| List | GET | `/admin/{resource}/{resource}` | `/admin/products/product` |
| Create | POST | `/admin/{resource}/add` | `/admin/products/add` |
| Edit Form | GET | `/admin/{resource}/edit/{id}` | `/admin/products/edit/{product_id}` |
| Update | POST | `/admin/{resource}/edit/{id}` | `/admin/products/edit/{product_id}` |
| Delete | GET | `/admin/{resource}/delete/{id}` | `/admin/products/delete/{product_id}` |

#### Parameter Conventions

##### Path Parameters

- Use descriptive parameter names: `{product_id}`, `{category_id}`, `{supplier_id}`
- User identification: `{username}`, `{user_id}`, `{id}`
- Resource-specific identifiers: `{cart_id}`, `{wishlist_id}`

##### Query Parameters

- Search functionality handled via `/SearchController` endpoint
- Cart operations use query parameters for item addition

##### Return Types

- **String**: Used for page redirects and status messages
- **ModelAndView**: Standard for page rendering with data
- **Redirect**: For post-operation navigation

##### Status Handling

- Access control enforced via `/Access_Denied` endpoint
- Authentication handled through `/login` and `/logout` endpoints
- Registration process via `/Registration` and `/register` endpoints

#### Security Conventions

##### Access Control

- Admin operations require appropriate authorization
- User-specific resources validate ownership via username parameter
- Status changes restricted to authorized users

#### Versioning Strategy

##### Current Implementation

- No explicit versioning in current API structure
- Backward compatibility maintained through consistent path patterns
- Future versioning should follow `/api/v{version}/` pattern

##### Standard Error Pages

- `/Access_Denied`: Authorization failures
- Validation errors handled at controller level
- Redirect patterns for operation feedback

## API Standards

### API Response Standards

#### Error Handling

##### pace-lumen-ui

- Implement consistent error handling across all endpoints
- Use appropriate HTTP status codes
- Provide meaningful error messages

#### ECommerce

### Response Standards