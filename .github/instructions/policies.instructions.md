---
applyTo: '**'
---
# policies

## Security Architecture
### Authentication and Authorization

**Controller-Level Security**
- All controllers must implement proper service layer dependencies for user authentication
- User-facing and admin-facing controllers are separated to enforce role-based access control
- Controllers delegate authentication logic to the service layer rather than handling it directly

**Data Access Security**
- All domain entities (User, Product, Cart) implement Serializable interface to ensure secure data transmission
- DAO layer provides controlled data access patterns, preventing direct database manipulation
- Service layer acts as security boundary between controllers and data access

**HTTP Method Security**
- **SECURITY CONCERN**: Current implementation uses GET methods for delete operations, which violates REST security principles
- DELETE operations should use proper HTTP DELETE methods to prevent accidental data loss via URL access
- GET requests are cached

### Security Controls

**Layered Security Architecture**
1. **Presentation Layer**: Controllers validate user input and enforce view-level security
2. **Business Logic Layer**: Services implement business rules and authorization logic
3. **Data Access Layer**: DAOs provide controlled database access with proper query validation

**Required Security Implementations**
- Service layer must validate user permissions before executing business operations
- All user inputs must be validated at controller level before passing to services
- Session management and user authentication must be handled through dedicated service components

## Security Policies
### Authentication Policy

**Requirement**: All user-facing endpoints must implement authentication

**Implementation**:
- User authentication required for cart operations
- Admin authentication required for category and product management
- Session timeout after 30 minutes of inactivity

### Authorization Policy

**Requirement**: Role-based access control for administrative functions

**Implementation**:
- Regular users: Access to product browsing and cart operations
- Admin users: Access to category and product management endpoints
- Service layer must validate user roles before data operations

### Data Validation Policy

**Standards**:
- Input sanitization for all user-provided data
- Parameter validation in controller methods
- Business rule validation in service layer

### Secure Communication Policy

### Incident Response Policy

## Business Rules
### Access Control Rules

#### Administrative Access
- **Rule**: All administrative functions must be segregated under the `/admin` path prefix
- **Severity**: High
- **Rationale**: Ensures clear separation between administrative and user-facing functionality
- **Implementation**: Administrative endpoints like `/admin/dashboard` are protected and isolated

#### Role-Based Access Control (RBAC)
- **Rule**: System must enforce role-based access with three distinct privilege levels
- **Severity**: High
- **Roles Supported**:
  - **DBA**: Database administration privileges
  - **Admin**: Administrative system access
  - **User**: Standard user operations
- **Implementation**: Custom authentication handler manages role-based routing and permissions

#### User Resource Isolation
- **Rule**: User-specific resources must be accessed via username-parameterized paths
- **Severity**: Medium
- **Scope**: Applies to personal resources including:
  - User accounts: `/user/{username}/account`
  - Shopping carts: `/{username}/cart`
  - Personal data and preferences
- **Purpose**: Ensures users can only access their own resources

### E-Commerce Business Rules

#### Shopping Cart Management
- **Rule**: Shopping cart operations must maintain data integrity for quantity and totals
- **Severity**: Medium
- **Requirements**:
  - Users can add items to cart with specified quantities
  - Users can remove items from cart
  - System must track and calculate cart totals accurately
  - Cart state must persist across user sessions

#### Product Organization
- **Rule**: All products must be properly categorized with supplier relationships
- **Severity**: Medium
- **Requirements**:
  - Every product must belong to at least one category
  - Products must have associated supplier information
  - Category hierarchy must be maintained for navigation
  - Supplier relationships must be tracked for inventory management

### Validation Rules

#### Data Integrity
- All user inputs must be validated before processing
- Monetary calculations must maintain precision for financial accuracy
- User authentication must be verified for all protected operations

#### Business Logic Constraints
- Cart operations require valid user authentication
- Administrative functions require appropriate role permissions
- Product modifications require supplier validation

## Policies & Governance
### Access Control Policies

#### Administrative Access
- **Policy**: All administrative functions must be segregated under the `/admin` path prefix
- **Enforcement**: Administrative dashboard and management functions are restricted to authorized personnel only
- **Compliance Level**: High Priority

#### Role-Based Access Control (RBAC)
- **Supported Roles**:
  - **DBA**: Database administration privileges
  - **Admin**: System administration and user management
  - **User**: Standard customer access to shopping features
- **Policy Enforcement**: Custom success handler manages role-based redirections and access controls
- **Compliance Level**: High Priority

#### User Resource Access
- **Policy**: User-specific resources (cart, wishlist, account) must be accessed via username-parameterized paths
- **Implementation**: Resources follow the pattern `/user/{username}/resource` or `/{username}/resource`
- **Privacy Control**: Ensures users can only access their own data
- **Compliance Level**: Medium Priority

### Data Governance

#### Product Data Management
- **Policy**: All products must be properly categorized and linked to verified suppliers
- **Requirements**:
  - Products must belong to defined categories
  - Supplier relationships must be established and maintained
  - Product data integrity must be preserved
- **Compliance Level**: Medium Priority

#### Shopping Cart Governance
- **Policy**: Cart operations must maintain data consistency and user ownership
- **Controls**:
  - Quantity tracking and validation
  - Total calculation accuracy
  - User-specific cart isolation
- **Compliance Level**: Medium Priority

### Compliance Framework

#### Security Controls
1. **Authentication**: Required for all user-specific operations
2. **Authorization**: Role-based access enforcement at application level
3. **Data Segregation**: Administrative and user functions are clearly separated
4. **Audit Trail**: Access patterns tracked through role-based success handling

#### Operational Controls
1. **Resource Isolation**: User data accessed only through authenticated, parameterized endpoints
2. **Data Integrity**: Product categorization and supplier relationship validation
3. **Business Logic**: Cart management follows established business rules for quantity and pricing
