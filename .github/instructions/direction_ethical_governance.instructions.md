---
applyTo: 'src/api/**'
---
# Direction / Ethical_Governance



> Organized from existing Repository Intelligence asset (`context_md`).

## Domain Model

The ECommerce application is built around several core domain entities that represent the business concepts and their relationships:

### Core Entities

#### User Management
- **User**: Central entity representing system users with email, mobile, role, and active status properties. Users can have different roles (DBA, Admin, User) and maintain relationships with cart and wishlist entities
- **Roles**: Defines user permission levels within the system
- **ShippingDetails**: Manages delivery address information for orders

#### Product Catalog
- **Product**: Core business entity containing price, image, description, discount information, and relationships to categories and suppliers
- **Category**: Organizes products into logical groupings with name and description properties
- **Supplier**: Represents product suppliers and maintains supply chain relationships

#### Shopping Experience
- **Cart**: Shopping cart entity with quantity, total, user association, and status tracking
- **Wishlist**: User's saved items for future consideration
- **CustomerOrder**: Order entity managing payment mode, grand total, status, and address information

### Entity Relationships

The domain model follows these key relationships:

- **Product-Category**: Products belong to categories (many-to-one)
- **Product-Supplier**: Products are supplied by suppliers (many-to-one)
- **User-Cart**: Users own shopping carts (one-to-many)
- **User-Wishlist**: Users maintain wishlists (one-to-many)
- **User-Order**: Users place orders (one-to-many)

### Business Rules

- Users must be authenticated to access cart and wishlist functionality
- Products must be associated with both a category and supplier
- Cart items maintain quantity and total calculations
- Orders track payment methods and shipping details
- Role-based access controls determine user permissions (Admin, DBA, User)

### Domain Services

The application implements service interfaces for each major domain area:
- **UserService**: User account management and authentication
- **ProductService**: Product catalog operations
- **CategoryService**: Category management
- **SupplierService**: Supplier relationship management
- **MailService**: Communication and notification services

These services encapsulate business logic and coordinate between the web layer and data access layer, ensuring proper separation of concerns and maintainable domain operations.

## System Overview

### Core Purpose

### Key Functional Areas