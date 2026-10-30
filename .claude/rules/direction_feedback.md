# Direction / Feedback



> Organized from existing Repository Intelligence asset (`context_md`).

## System Overview

### Core Purpose

### Key Functional Areas

## API Inventory

The ECommerce application exposes a comprehensive REST API organized into functional modules. Below is the complete inventory of available endpoints:

### Public Endpoints

| Method | Path | Purpose |
|--------|------|----------|
| GET | `/` | Home page display |
| GET | `/aboutUs` | About us page |
| GET | `/contactUs` | Contact information page |
| GET | `/login` | User login page |
| GET | `/logout` | User logout handler |
| GET | `/Registration` | User registration page |
| GET | `/register` | User registration handler |
| GET | `/Access_Denied` | Access denied error page |

### Product & Catalog Endpoints

| Method | Path | Purpose |
|--------|------|----------|
| GET | `/displayProduct/productsList` | Display all products |
| GET | `/displayProduct/productList/categorywise/{category_id}` | Display products by category |
| GET | `/descriptionPage` | Product description page |
| GET | `/SearchController` | Product search functionality |

### User Account Management

| Method | Path | Purpose |
|--------|------|----------|
| GET | `/account` | User account overview |
| GET | `/user/{username}/account` | User-specific account page |
| GET | `/user/{username}/account/edit-details/{user_id}` | Account edit form |
| POST | `/updatingAccount-{id}` | Update account details |
| POST | `/user/{username}/account/edit-details/updatingAccount/{user_id}` | Update user account details |

### Shopping Cart Operations

| Method | Path | Purpose |
|--------|------|----------|
| GET | `/addCart` | Add item to cart |
| GET | `/{username}/cart` | View user's shopping cart |
| GET | `/{username}/cart/remove-cart/{cart_id}` | Remove item from cart |
| GET | `/customerOrder` | Customer order processing |

### Wishlist Management

| Method | Path | Purpose |
|--------|------|----------|
| GET | `/user/addWishlistItem` | Add item to wishlist |
| GET | `/user/{username}/wishlist` | View user's wishlist |
| GET | `/user/{username}/wishlist/remove-wishlist/{wishlist_id}` | Remove item from wishlist |

### Admin Dashboard

| Method | Path | Purpose |
|--------|------|----------|
| GET | `/admin/dashboard` | Admin dashboard home |
| GET | `/gotoadminsection` | Navigate to admin section |
| GET | `/db` | Database administration |

### Admin - Category Management

| Method | Path | Purpose |
|--------|------|----------|
| GET | `/admin/categorys/category` | Category management page |
| POST | `/admin/categorys/add` | Add new category |
| GET | `/admin/categorys/edit/{category_id}` | Edit category form |
| POST | `/admin/categorys/edit/{category_id}` | Update category |
| GET | `/admin/categorys/delete/{category_id}` | Delete category |

### Admin - Product Management

| Method | Path | Purpose |
|--------|------|----------|
| GET | `/admin/products/product` | Product management page |
| GET | `/admin/products/edit/{product_id}` | Edit product form |
| GET | `/admin/products/delete/{product_id}` | Delete product |

### Admin - Supplier Management

| Method | Path | Purpose |
|--------|------|----------|
| GET | `/admin/suppliers/supplier` | Supplier management page |
| GET | `/admin/suppliers/edit/{supplier_id}` | Edit supplier form |
| GET | `/admin/suppliers/delete/{supplier_id}` | Delete supplier |

### Admin - User Management

| Method | Path | Purpose |
|--------|------|----------|
| GET | `/admin/users/user` | User management page |
| GET | `/admin/users/changeStatus/{id}` | Change user status |
| GET | `/admin/users/delete/{id}` | Delete user account |

### API Architecture Notes

- **Authentication**: Role-based access control with DBA, Admin, and User roles
- **Response Format**: Primarily returns view names for MVC rendering
- **Path Parameters**: Used extensively for entity identification (IDs, usernames)
- **HTTP Methods**: Follows RESTful conventions with GET for retrieval and POST for modifications
- **Security**: Admin endpoints require appropriate role authorization