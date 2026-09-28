# rules

## Project Overview

This project comprises 2 repositories: pace-lumen-ui, ECommerce. The sections below consolidate repository-specific evidence and shared concepts without dropping any participating repository.

## Repository Architecture

### pace-lumen-ui

### Naming Conventions

#### Naming Conventions

() => { describe('ngOnInit', () => {...}) })`

() => {...})`

#### Classes

- **Services, Models**: Use PascalCase for class names
  - Examples: `Stories`, `Tasks`
  - Use singular nouns for services

#### Methods and Functions

- **Lifecycle Methods**: Implement standard Angular lifecycle hooks
  - `ngOnInit()` - Component initialization
  - `ngOnDestroy()` - Component cleanup and resource management

- **Classification Methods**: Use descriptive method names for categorization
  - Risk classification methods should clearly indicate their purpose
  - Priority classification methods should follow consistent naming patterns

#### Variables and Properties

#### Files and Modules

`publish.component.ts`

- **Model Files**: Use kebab-case with `.model.ts` or `.interface.ts` suffix

#### General Guidelines

##### Component Layer

- Components must only interact with services through dependency injection
- Direct component-to-component communication is forbidden except through parent-child relationships
- Components should not contain business logic beyond presentation concerns

##### Service Layer

- Services handle all API interactions and business logic
- Services must not directly manipulate DOM elements
- Cross-service dependencies should be minimized and well-documented

##### Data Layer

- All data models must be interface-driven
- Direct API calls from components are forbidden - use services instead
- Data transformation logic belongs in services, not components

##### Allowed Imports

- Angular framework modules (`@angular/*`)
- Application services within the same module
- Shared interfaces and models
- Third-party libraries explicitly approved for the project

##### Forbidden Imports

- Direct HTTP client usage in components
- Cross-module component imports without proper module exports
- Circular dependencies between services
- Direct DOM manipulation libraries in services

##### Component Anti-Patterns

- **Direct API Calls**: Components must not make direct HTTP requests
- **Business Logic**: Complex calculations and data processing belong in services
- **Global State Mutation**: Components should not directly modify global application state

##### Service Anti-Patterns

- **DOM Manipulation**: Services must not interact with DOM elements
- **Component References**: Services should not hold direct references to components
- **Synchronous Operations**: Avoid blocking operations in service methods

#### Lifecycle Management Rules

##### Required Implementations

- All components must implement `ngOnInit` for initialization logic
- Components with subscriptions must implement `ngOnDestroy` for cleanup
- Memory leaks prevention through proper subscription management

##### Classification Requirements

- Components handling risk assessment must implement standardized risk classification methods
- Priority-based components must follow consistent priority classification patterns
- Categorization logic should be centralized in dedicated services

#### Component Security

### Async & Concurrency

#### Required Async Patterns

##### Observable Subscriptions

- **MUST** use `takeUntil()` operator with component destruction signal
- **MUST** implement `ngOnDestroy` to complete subscription cleanup
- **MUST** use `async` pipe in templates when possible to avoid manual subscription management

##### Promise Handling

- **MUST** use `async/await` syntax for Promise-based operations
- **MUST** implement proper error handling with try-catch blocks
- **SHOULD** prefer Observables over Promises for data streams

#### Forbidden Sync Patterns

##### Blocking Operations

- **NEVER** use synchronous HTTP requests
- **NEVER** use `setTimeout()` or `setInterval()` without proper cleanup
- **NEVER** perform heavy computations on the main thread without Web Workers

##### Memory Leaks

- **NEVER** subscribe to Observables without unsubscription strategy
- **NEVER** create infinite loops or recursive calls without termination conditions
- **NEVER** hold references to DOM elements beyond component lifecycle

#### Concurrency Guidelines

##### State Management

- **MUST** use immutable state updates
- **SHOULD** implement proper loading states for async operations
- **MUST** handle race conditions with appropriate operators (`switchMap`, `mergeMap`, `concatMap`)

##### Error Handling

- **MUST** implement global error handling for unhandled Promise rejections
- **MUST** use `catchError` operator for Observable error handling
- **SHOULD** provide user-friendly error messages for failed async operations

### Error Handling

#### Required Patterns

##### Component Lifecycle Error Management

- **MUST** implement proper cleanup in `ngOnDestroy` to prevent memory leaks
- **MUST** handle subscription cleanup to avoid dangling observables
- **MUST** implement error boundaries for component initialization failures

##### Classification Method Error Handling

- **MUST** provide fallback values for risk and priority classification failures
- **MUST** validate input parameters before processing classification logic
- **MUST** log classification errors for debugging purposes

##### Lifecycle Anti-patterns

- **NEVER** ignore `ngOnDestroy` implementation in components with subscriptions
- **NEVER** perform heavy operations in lifecycle hooks without error handling
- **NEVER** leave subscriptions unmanaged in component lifecycle

##### Classification Anti-patterns

- **NEVER** throw unhandled exceptions in classification methods
- **NEVER** return `null` or `undefined` from classification functions
- **NEVER** perform classification without input validation

#### Error Recovery Strategies

##### Graceful Degradation

- Provide default values when classification fails
- Display user-friendly error messages instead of technical details
- Maintain application functionality even when non-critical features fail

##### Logging Requirements

- Log all classification errors with context information
- Include component lifecycle errors in application logs
- Maintain error tracking for debugging and monitoring purposes

#### ORM Usage

#### API Service Patterns

Since this is a frontend application, follow these service interaction rules:

- **Service Layer**: All data operations must go through dedicated service classes
- **Type Safety**: Use TypeScript interfaces for all API request/response models
- **Error Handling**: Implement proper error handling for all HTTP operations

#### Raw SQL Rules

#### Migration Conventions

#### Data Flow Rules

- All data operations must use the established service layer patterns
- Components should not directly handle HTTP requests
- Implement proper loading states and error handling for all data operations

### Testing Standards

#### Test Types

**Unit Tests**
- Required for all components, services, and utilities
- Must test component lifecycle methods (ngOnInit, ngOnDestroy)
- Must verify risk and priority classification logic

**End-to-End Tests**
- Required for critical user workflows
- Must cover main application features

#### Coverage Requirements

**Critical Components**
- Components with lifecycle management: 90% coverage
- Risk/priority classification methods: 95% coverage
- Core services: 85% coverage

### Anti-Patterns (Never Do)

#### Component Lifecycle Violations

**✅ Always implement proper lifecycle**

```typescript
// GOOD: Proper lifecycle management
export class GoodComponent implements OnInit, OnDestroy {
  ngOnInit(): void {
    // Initialization logic here
  }
  
  ngOnDestroy(): void {
    // Cleanup logic here
  }
}
```

#### Classification Method Violations

**✅ Always implement dynamic classification**

#### Memory Leak Anti-Patterns

### ECommerce

#### DAO Pattern Implementation

- **Use DAO interfaces**: Define data access operations through interfaces to maintain loose coupling
- **Implement concrete DAOs**: Create specific implementations for each entity type
- **Follow naming convention**: DAO interfaces should end with `DAO` (e.g., `UserDAO`, `ProductDAO`)
- **Single responsibility**: Each DAO should handle operations for one entity type only

#### Service Layer Integration

- **No direct database access in controllers**: Controllers must delegate all data operations to the service layer
- **Services coordinate DAOs**: Business logic in services should orchestrate multiple DAO operations when needed
- **Transaction boundaries**: Define transaction scope at the service layer, not in DAOs

#### Data Access Guidelines

DAO levels

#### Dependency Injection

- **Inject DAO dependencies**: Use dependency injection to wire DAOs into service classes
- **Interface-based injection**: Inject DAO interfaces, not concrete implementations
- **Configuration-driven**: Use Spring configuration for DAO bean definitions

## Architecture Rules

## Security Rules

## Database Access

### Authentication & Authorization

#### pace-lumen-ui

- All components must implement proper lifecycle management through `ngOnInit`, unauthorized access to destroyed components
- Components handling sensitive data must implement proper cleanup in `ngOnDestroy` to clear any cached authentication tokens or user data

#### ECommerce

- Implement proper access control mechanisms for all endpoints
- Provide dedicated access denied pages for unauthorized access attempts
- Validate user permissions before granting access to protected resources

### Forbidden Patterns

### Import Rules

#### pace-lumen-ui

#### ECommerce

```
❌ DAO → Service (DAOs cannot import Services)
❌ DAO → Controller (DAOs cannot import Controllers)
❌ Model → Service (Models cannot import Services)
❌ Model → DAO (Models cannot import DAOs)
❌ Model → Controller (Models cannot import Controllers)
```

```
✅ Controller → Service
✅ Controller → Model
✅ Service → DAO
✅ Service → Model
✅ DAO → Model
```

### Input Validation

#### pace-lumen-ui

priority classification methods must validate input parameters before categorization

#### ECommerce

and content

### Layer Boundaries

#### pace-lumen-ui

#### ECommerce

**Controller Layer**
- Controllers must only handle HTTP request/response concerns
- No direct database access - must delegate to Service Layer
- Should not contain business logic beyond input validation and response formatting

**Service Layer**
- Contains all business logic and transaction management
- Must not directly handle HTTP concerns (request/response objects)
- Should coordinate between multiple DAOs when needed
- Acts as the transaction boundary

**Data Access Layer (DAO)**
- Responsible only for data persistence operations
- Must not contain business logic
- Should provide clean abstraction over data storage
- One DAO per entity or aggregate root

**Model Layer**
- Entity classes should be plain data objects
- No business logic in model classes
- Should represent the domain model clearly

### OWASP Compliance

#### pace-lumen-ui

timeout handling

#### ECommerce

### Secret Management

#### pace-lumen-ui

#### ECommerce

implement proper key lifecycle management