# Constraint / Security



> Organized from existing Repository Intelligence asset (`rules_md`).

## Security Rules

### Authentication & Authorization

- **Controller Access Control**: All controllers must implement proper authentication checks before processing requests
- **Service Layer Security**: Service dependencies (productService, userService, categoryService) must validate user permissions before data operations
- **Session Management**: Implement secure session handling for user authentication state

### Input Validation

`{category_id}`

### HTTP Method Security

### Data Protection

- **Serialization Security**: Entities implementing Serializable interface must not expose sensitive data during serialization
- **Sensitive Data Handling**: Never log or expose user credentials, payment information, or personal data
- **Database Security**: Use parameterized queries to prevent SQL injection

### Secret Management

- **Configuration Security**: Store database credentials