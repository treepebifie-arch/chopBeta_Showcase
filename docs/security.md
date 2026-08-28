# ChopBeta Security

ChopBeta implements multiple layers of security across authentication, authorization, validation, and session management.

## Authentication

* **JWT authentication** for protected API resources.
* **Google OAuth 2.0** as an alternative authentication method.
* **bcrypt** for secure password hashing.

## Authorization

* **Role-Based Access Control (RBAC)** restricts access to protected resources based on user roles.
* Authentication and authorization checks are handled through middleware.

## Input Validation

* **Zod** is used to validate incoming request data before it reaches application logic.
* Invalid input is rejected before being processed by the service layer.

## Session Security

* User sessions and devices are tracked.
* Concurrent-device restrictions help prevent excessive simultaneous logins.

## Error Handling

* Centralized error-handling middleware provides consistent API error responses.
* Internal implementation details are kept separate from client-facing responses.

## Production Code

The production source code is private and is not included in this repository. This showcase intentionally excludes proprietary business logic, credentials, tokens, and other sensitive implementation details.
