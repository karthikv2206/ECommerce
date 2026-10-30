---
applyTo: 'src/api/**'
---
# Provenance / Stakeholder



> **Human review required** — this ontology plane requires manual validation and completion.
> This plane is human-owned. Legacy excerpts below are supporting context only.
> **Purpose:** The intent behind requirements — what stakeholders mean vs. what they said, and the political and strategic dimensions that shape delivery priorities.
> **Gap indicators:** Delivery frequently pivots without explanation; Requirements taken literally causing delivered-but-rejected work; Stakeholder intent absent from backlog items

> Organized from existing Repository Intelligence asset (`context_md`).

## System Overview

The ECommerce repository is a comprehensive Java-based e-commerce web application built using the Spring MVC framework. This system provides a complete online shopping platform with distinct interfaces for customers and administrators.

### Core Purpose

The application serves as a full-featured e-commerce solution that enables:
- **Customer Experience**: Product browsing, shopping cart management, wishlist functionality, and user account management
- **Administrative Control**: Complete backend management of products, categories, suppliers, and users
- **Secure Operations**: Role-based authentication and authorization with support for multiple user types (DBA, Admin, User)

### Key Functional Areas

**Customer-Facing Features:**
- Product catalog with category-based browsing and search capabilities
- Shopping cart system for item management and checkout processes
- Personal wishlist functionality for saving desired products
- User account management including registration, login, and profile editing
- Order processing and tracking system

**Administrative Features:**
- Comprehensive admin dashboard for system oversight
- Category management with full CRUD operations
- Product management including inventory and supplier relationships
- User management with status control and account administration
- Supplier management for vendor relationships

**Technical Foundation:**
- Spring MVC architecture with layered design (Controller-Service-DAO-Model)
- Hibernate ORM for database operations and entity management
- Spring Security for authentication and role-based access control
- RESTful API endpoints for both user and administrative operations

The system follows enterprise-grade architectural patterns including the MVC pattern, DAO pattern, and Service Layer pattern, ensuring maintainable and scalable code organization. The application supports multiple user roles with customized authentication flows and provides a robust foundation for e-commerce operations.

## User Journeys

### Customer Journey

#### Product Discovery & Shopping
1. **Browse Products**: Customers visit the home page (`/`) and can view all products (`/displayProduct/productsList`) or filter by category (`/displayProduct/productList/categorywise/{category_id}`)
2. **Product Details**: View detailed product information on the description page (`/descriptionPage`)
3. **Add to Cart**: Add desired items to shopping cart (`/addCart`) and manage cart contents (`/{username}/cart`)
4. **Wishlist Management**: Save items for later (`/user/addWishlistItem`) and manage wishlist (`/user/{username}/wishlist`)
5. **Account Management**: Register (`/register`), login (`/login`), and manage account details (`/user/{username}/account`)
6. **Order Placement**: Complete purchases through the customer order flow (`/customerOrder`)

#### Authentication Flow
- **Registration**: New users register via `/Registration` endpoint
- **Login**: Users authenticate through `/login` with role-based redirection handled by `CustomSuccessHandler`
- **Role-based Access**: System redirects users to appropriate sections (DBA, Admin, User) based on their role

### Administrator Journey

#### Admin Dashboard Access
1. **Login**: Administrators authenticate and are redirected to admin dashboard (`/admin/dashboard`)
2. **Navigation**: Access admin section via `/gotoadminsection`

#### Content Management
1. **Category Management**:
   - View categories (`/admin/categorys/category`)
   - Add new categories (`POST /admin/categorys/add`)
   - Edit existing categories (`/admin/categorys/edit/{category_id}`)
   - Delete categories (`/admin/categorys/delete/{category_id}`)

2. **Product Management**:
   - View products (`/admin/products/product`)
   - Edit product details (`/admin/products/edit/{product_id}`)
   - Delete products (`/admin/products/delete/{product_id}`)

3. **Supplier Management**:
   - View suppliers (`/admin/suppliers/supplier`)
   - Edit supplier information (`/admin/suppliers/edit/{supplier_id}`)
   - Delete suppliers (`/admin/suppliers/delete/{supplier_id}`)

4. **User Management**:
   - View all users (`/admin/users/user`)
   - Change user status (`/admin/users/changeStatus/{id}`)
   - Delete users (`/admin/users/delete/{id}`)

### Developer Journey

#### Application Setup
1. **Configuration**: Set up Spring MVC configuration via `HelloWorldConfiguration`
2. **Security Setup**: Configure authentication and authorization through `SecurityConfiguration`
3. **Database Setup**: Configure Hibernate ORM and data source via `HibernateConfiguration`
4. **Web Flow**: Set up application flow using `WebFlowConfig`

#### Development Workflow
1. **Controller Development**: Implement MVC controllers for different functional areas:
   - `CartController` for shopping cart operations
   - `ProductController` for product display and search
   - `UserAccountController` for account management
   - `WishlistController` for wishlist functionality
   - Admin controllers for backend management

2. **Service Layer**: Implement business logic through service interfaces and implementations:
   - `CategoryService`/`CategoryServiceImpl`
   - `ProductService`/`ProductServiceImpl`
   - `UserService`/`UserServiceImpl`
   - `SupplierService`/`SupplierServiceImpl`

3. **Data Access**: Implement data persistence through DAO pattern:
   - `CategoryDao`/`CategoryDaoImpl`
   - `ProductDao`/`ProductDaoImpl`
   - `UserDao`/`UserDaoImpl`
   - `CartDao`/`CartDaoImpl`

4. **Model Development**: Define domain entities:
   - `User` with email, mobile, role, and relationships
   - `Product` with price, description, category, and supplier relationships
   - `Cart` with quantity, total, and user associations
   - `Category` with name, description, and product relationships
   - `CustomerOrder` with payment and shipping details

### Common User Flows

#### Shopping Cart Flow
1. Browse products → Add to cart (`/addCart`) → View cart (`/{username}/cart`) → Remove items if needed (`/{username}/cart/remove-cart/{cart_id}`) → Proceed to checkout

#### Wishlist Flow
1. Browse products → Add to wishlist (`/user/addWishlistItem`) → View wishlist (`/user/{username}/wishlist`) → Remove items (`/user/{username}/wishlist/remove-wishlist/{wishlist_id}`) → Move to cart

#### Account Management Flow
1. Register (`/register`) → Login (`/login`) → Access account (`/user/{username}/account`) → Edit details → Update account (`POST /user/{username}/account/edit-details/updatingAccount/{user_id}`)