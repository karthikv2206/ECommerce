---
applyTo: 'src/api/**'
---
# Provenance / Cognitive



> **Human review required** — this ontology plane requires manual validation and completion.
> This plane is human-owned. Legacy excerpts below are supporting context only.
> **Purpose:** The tacit knowledge that lives in expert heads — expertise, heuristics, and judgment that does not exist in any document.
> **Gap indicators:** Key departures cause disproportionate delivery impact; New engineers take months to become productive; Institutional memory concentrated in individuals

> Organized from existing Repository Intelligence asset (`rules_md`).

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