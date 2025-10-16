---
name: API Development Guide
description: REST API scaffolding with standard patterns
when_to_use: when building new API endpoints or structuring API projects
version: 1.0.0
---

# API Development Guide

## Overview

Standard patterns for building REST APIs with consistent structure, error handling, and validation.

**Core principle:** Convention over configuration with clear, predictable API design.

## REST Endpoint Structure

### Standard HTTP Methods

- **GET** - Retrieve resources (read-only, idempotent)
- **POST** - Create new resources
- **PUT** - Replace entire resource
- **PATCH** - Update partial resource
- **DELETE** - Remove resource

### URL Patterns

```
GET    /api/resources          # List all
GET    /api/resources/:id      # Get one
POST   /api/resources          # Create
PUT    /api/resources/:id      # Replace
PATCH  /api/resources/:id      # Update
DELETE /api/resources/:id      # Delete
```

## Response Format

### Success Response (200, 201)

```json
{
  "data": {
    "id": "123",
    "name": "Example",
    "created_at": "2025-10-15T12:00:00Z"
  }
}
```

### Error Response (400, 404, 500)

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input data",
    "details": [
      {
        "field": "email",
        "issue": "Invalid email format"
      }
    ]
  }
}
```

### Status Codes

- **200 OK** - Successful GET, PUT, PATCH, DELETE
- **201 Created** - Successful POST
- **400 Bad Request** - Invalid input
- **401 Unauthorized** - Missing/invalid authentication
- **403 Forbidden** - Insufficient permissions
- **404 Not Found** - Resource doesn't exist
- **500 Internal Server Error** - Server error

## Request Validation

### Validate Early

```javascript
// Example validation middleware
function validateRequest(schema) {
  return (req, res, next) => {
    const { error } = schema.validate(req.body);
    if (error) {
      return res.status(400).json({
        error: {
          code: 'VALIDATION_ERROR',
          message: error.message,
          details: error.details
        }
      });
    }
    next();
  };
}
```

### Required Fields

Always validate:
- Required fields exist
- Data types are correct
- Values are within acceptable ranges
- Format matches expected pattern (email, URL, etc.)

## Basic Project Structure

```
api/
├── routes/
│   ├── index.js           # Route registration
│   └── resources.js       # Resource endpoints
├── controllers/
│   └── resourceController.js  # Business logic
├── models/
│   └── resource.js        # Data models
├── middleware/
│   ├── auth.js            # Authentication
│   ├── validate.js        # Validation
│   └── errorHandler.js    # Error handling
└── tests/
    └── resources.test.js  # API tests
```

## Error Handling Pattern

```javascript
// Centralized error handler
app.use((err, req, res, next) => {
  console.error(err.stack);

  const status = err.status || 500;
  const message = err.message || 'Internal server error';

  res.status(status).json({
    error: {
      code: err.code || 'INTERNAL_ERROR',
      message: message
    }
  });
});
```

## Quick Start Checklist

- [ ] Define resource models
- [ ] Set up routes with proper HTTP methods
- [ ] Add request validation
- [ ] Implement error handling
- [ ] Write API tests
- [ ] Document endpoints (OpenAPI/Swagger)

## Testing APIs

```bash
# Test with curl
curl -X GET http://localhost:3000/api/resources
curl -X POST http://localhost:3000/api/resources \
  -H "Content-Type: application/json" \
  -d '{"name":"test"}'
```

## Remember

- This is a basic template, not production-ready
- Add authentication/authorization as needed
- Use validation libraries (Joi, Zod, etc.)
- Log all errors with context
- Version your API (/api/v1/resources)
- Document with OpenAPI/Swagger
