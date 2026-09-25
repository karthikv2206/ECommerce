---
applyTo: '**'
---
# README

## Repository Overview
The ECommerce application is a Spring MVC-based e-commerce platform that provides comprehensive online shopping functionality with administrative management capabilities.

### What This Application Provides

- **Customer Portal**: Complete shopping experience with product browsing, cart management, wishlist, and user accounts
- **Admin Dashboard**: Full administrative control over products, categories, suppliers, and user management
- **Security Integration**: Role-based access control separating customer and administrative functions

### Technology Foundation

- Spring MVC for web application structure
- Spring Security for authentication and authorization
- Hibernate ORM for data persistence
- Spring Web Flow for complex user workflows

### Key Features

- User registration and authentication system
- Product catalog with category organization
- Shopping cart and wishlist functionality
- Order processing capabilities
- Administrative management interfaces
- Supplier and inventory management

The application follows enterprise Java development patterns with a layered architecture ensuring maintainability and scalability for e-commerce operations.

## Deployment Architecture
### Environment Setup

### Deployment Requirements

**Framework Dependencies:**
- Spring Framework (MVC, Security, Web Flow)
- Hibernate ORM
- Java Servlet API

### Architecture Overview

The application follows a layered deployment model:

1. **Web Layer**: Controllers handle HTTP requests
2. **Service Layer**: Business logic processing
3. **Data Access Layer**: Database operations via DAO pattern
4. **Model Layer**: Domain entities and data structures

### Deployment Considerations

- **Stateless Design**: Web controllers are stateless for horizontal scaling
- **Database Connectivity**: Requires proper JDBC configuration
- **Security Configuration**: Spring Security integration for authentication/authorization
- **Session Management**: Web Flow manages complex user interactions

## Prerequisites
Before setting up the ECommerce application, dependencies installed:

### Required Frameworks and Libraries

- **Spring Framework** - Core framework for dependency injection and application context
- **Hibernate ORM** - Object-relational mapping for database operations
- **Spring Security** - Authentication and authorization framework
- **Spring Web Flow** - Web application flow management

### Development Environment

### Additional Requirements

## Installation
### Prerequisites

Before installing the ECommerce application

application context
- **Hibernate ORM** - Object-relational mapping for database operations
- **Spring Security** - Authentication, authorization framework
- **Spring Web Flow** - Web application flow management
- **Java Development Kit (JDK)** - Version 8 or higher
- **Maven** - For dependency management

### Installation Steps

1. **Clone the Repository**

   ```bash
   git clone <repository-url>
   cd ECommerce
   ```

2. **Install Dependencies**

3. **Database Setup**
   - Create a new database for the application
   - Update database connection properties in `application.properties`
   - Run database migrations if available

4. **Build the Application**

   ```bash
   mvn clean package
   ```

5. **Run the Application**

   ```bash
   mvn spring-boot:run
   ```

Or run the JAR file:

   ```bash
   java -jar target/ecommerce-*.jar
   ```

### Troubleshooting

resolve conflicts

## Configuration
### Environment Variables

| Variable | Description | Required | Default |
|----------|-------------|----------|----------|
| `SPRING_PROFILES_ACTIVE` | Active Spring profile (dev, test, prod) | Yes | dev |
| `DATABASE_USERNAME` | Database username | Yes | - |

### Configuration Files

The application uses Spring Boot's configuration system with the following files:

- `application.yml` - Main configuration file
- `application-dev.yml` - Development environment settings
- `application-test.yml` - Test environment settings
- `application-prod.yml` - Production environment settings

### Framework Configuration

- **Spring Framework**: Core dependency injection and application context
- **Hibernate ORM**: Database entity mapping and persistence
- **Spring Security**: Authentication and authorization configuration
- **Spring Web Flow**: Web application flow management

### Sample Configuration

Create a `.env` file in the project root:

## Development Environment
### Prerequisites

- **Spring Framework** - Core application framework
- **Hibernate ORM** - Object-relational mapping for database operations
- **Spring Security** - Authentication and authorization framework
- **Spring Web Flow** - Web application flow management

### Local Setup

1. **Clone the Repository**

   ```bash
   git clone <repository-url>
   cd ECommerce
   ```

2. **Environment Configuration**
   - Configure your IDE to support Spring Framework development
   - Set up database connections for Hibernate ORM
   - Configure Spring Security settings for local development

3. **Development Workflow**
   - Use Spring Web Flow for managing application navigation flows
   - Leverage Spring Security for testing authentication scenarios
   - Utilize Hibernate ORM for database schema management and data operations

### IDE Recommendations

> **Note**: Detailed configuration files

## Troubleshooting
### Common Issues and Solutions

#### Spring Framework Issues

#### Hibernate ORM Issues

#### Spring Security Issues

**Problem**: Authentication or authorization failures
- **Solution**: Review Spring Security configuration
- **Check**: Verify user roles

#### Spring Web Flow Issues

#### General Troubleshooting Steps

#### Getting Help
