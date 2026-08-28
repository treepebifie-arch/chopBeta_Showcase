# ChopBeta Backend Architecture

## Overview

ChopBeta uses a layered backend architecture built with **Node.js, Express.js, and MongoDB**.

The architecture separates HTTP concerns, business logic, validation, authentication, and database operations into distinct responsibilities. This makes the system easier to maintain, extend, debug, and reason about as functionality grows.

The production implementation is private. This document provides a high-level overview of the architecture without exposing proprietary business logic or source code.

---

## Architecture Overview

```text
                         ┌─────────────────┐
                         │     Client      │
                         └────────┬────────┘
                                  │
                                  ▼
                         ┌─────────────────┐
                         │     Routes      │
                         └────────┬────────┘
                                  │
                                  ▼
                    ┌──────────────────────────┐
                    │       Middleware         │
                    │                          │
                    │ • Authentication         │
                    │ • Request Validation     │
                    │ • User Status Checks     │
                    │ • Other System Checks    │
                    │ • Error Handling         │
                    └────────────┬─────────────┘
                                 │
                                 ▼
                         ┌─────────────────┐
                         │   Controllers   │
                         └────────┬────────┘
                                  │
                                  ▼
                         ┌─────────────────┐
                         │    Services     │
                         └────────┬────────┘
                                  │
                                  ▼
                         ┌─────────────────┐
                         │     Models      │
                         └────────┬────────┘
                                  │
                                  ▼
                         ┌─────────────────┐
                         │    MongoDB      │
                         └─────────────────┘
```

---

## Request Flow

A typical request moves through the backend in the following sequence:

```text
Client Request
      ↓
Route
      ↓
Middleware
      ↓
Controller
      ↓
Service
      ↓
Model / Database
      ↓
Service
      ↓
Controller
      ↓
HTTP Response
```

Each layer has a defined responsibility.

---

## 1. Routes

Routes define the public API interface.

They determine:

* HTTP method
* Endpoint
* Middleware chain
* Controller responsible for handling the request

For example, an authentication request may follow a flow similar to:

```text
POST /api/v1/auth/user/login
        ↓
Authentication Route
        ↓
Validation Middleware
        ↓
Authentication Controller
```

Keeping route definitions separate from business logic makes the API structure easier to understand and maintain.

---

## 2. Middleware

Middleware handles concerns that should occur before or around controller execution.

ChopBeta uses middleware for responsibilities including:

### Authentication

Verifies whether a request contains valid authentication credentials before protected resources are accessed.

### Request Validation

Validates incoming request data before it reaches application logic.

ChopBeta uses **Zod** for request validation.

### User Status Checks

Validates relevant user-state conditions before allowing specific operations to continue.

### System-Level Checks

Additional middleware is used for application-specific checks and rules.

### Error Handling

Centralized error-handling middleware provides a consistent mechanism for handling application errors and generating API responses.

---

## 3. Controllers

Controllers operate at the HTTP layer.

Their responsibilities include:

* Receiving validated requests
* Extracting relevant request data
* Calling the appropriate service
* Returning an HTTP response
* Handling HTTP-specific concerns

Controllers do not serve as the primary location for complex business logic.

Instead, business operations are delegated to the service layer.

---

## 4. Services

The service layer contains the application's core business logic.

This separation allows the backend to keep business rules independent from Express-specific request and response handling.

Examples of responsibilities handled at the service level include:

* User-related operations
* Meal recommendation logic
* Authentication-related operations
* Session/device operations
* Other application-specific business rules

This structure improves maintainability and makes individual pieces of business logic easier to reason about and test.

---

## 5. Models

Models define how application data is represented and persisted.

ChopBeta uses **MongoDB** with **Mongoose** for data management.

The model layer is responsible for:

* Defining data schemas
* Representing application entities
* Interacting with the database
* Supporting database operations required by services

The service layer communicates with the model layer rather than placing database operations directly inside route definitions.

---

## 6. Authentication

ChopBeta supports multiple authentication mechanisms.

### JWT

JSON Web Tokens are used for stateless authentication and access to protected API resources.

A simplified flow is:

```text
User Login
    ↓
Credential Verification
    ↓
Token Generation
    ↓
Client Receives Token
    ↓
Protected Request
    ↓
Authentication Middleware
    ↓
Authorized Resource
```

### Google OAuth 2.0

Google OAuth 2.0 provides an alternative authentication flow for users who choose to authenticate through Google.

---

## 7. Authorization

Authentication determines **who the user is**.

Authorization determines **what the authenticated user is allowed to access**.

ChopBeta implements **Role-Based Access Control (RBAC)** to enforce permissions based on user roles.

A simplified flow is:

```text
Request
   ↓
Authentication
   ↓
Identify User
   ↓
Check Role / Permission
   ↓
Allow or Reject Request
```

This prevents authenticated users from automatically gaining access to resources outside their permitted scope.

---

## 8. Session & Device Management

ChopBeta includes session and device tracking functionality.

The system can:

* Track active user sessions
* Associate sessions with devices
* Restrict the number of concurrent logged-in devices
* Support session management for users

This adds an additional layer of account security beyond simply issuing authentication tokens.

---

## 9. Validation Strategy

Incoming data is validated before reaching the relevant business logic.

ChopBeta uses **Zod** to define and enforce request validation rules.

The general flow is:

```text
Incoming Request
       ↓
Validation Middleware
       ↓
Valid?
   ↙       ↘
 No         Yes
 ↓           ↓
Error     Controller
Response      ↓
            Service
```

This prevents invalid input from unnecessarily reaching deeper application layers.

---

## 10. Error Handling

Application errors are handled through centralized middleware rather than requiring every route to implement its own error-response mechanism.

The goal is to provide:

* Consistent error responses
* Cleaner controllers
* Easier debugging
* Better separation of concerns

Centralizing error handling also reduces duplicated error-management logic throughout the application.

---

## 11. Separation of Concerns

A major architectural principle in ChopBeta is **Separation of Concerns (SoC)**.

The main responsibilities are divided as follows:

| Layer       | Primary Responsibility                |
| ----------- | ------------------------------------- |
| Routes      | Define API endpoints and request flow |
| Middleware  | Cross-cutting request checks          |
| Controllers | HTTP request/response handling        |
| Services    | Business logic                        |
| Models      | Data representation and persistence   |
| Database    | Persistent data storage               |

This separation allows changes in one layer to be made with minimal impact on unrelated parts of the application.

---

## 12. Why This Architecture?

The Service-Controller-Route structure was chosen to keep the application modular as the number of features and API endpoints grows.

The main benefits include:

### Maintainability

Business logic is separated from HTTP handling, making individual components easier to modify.

### Scalability

New features can be introduced without placing all application logic inside route handlers or controllers.

### Testability

Business logic contained within services can be reasoned about and tested independently from HTTP-specific concerns.

### Reusability

Services can contain reusable application operations without being tightly coupled to a particular route.

### Debugging

Clear layer boundaries make it easier to identify whether a problem originates in request handling, validation, business logic, or database interaction.

---

## 13. Example Feature Flow

A simplified meal recommendation request can be represented as:

```text
Client
  │
  │ GET /api/v1/meals/generate-meals
  ▼
Route
  │
  ▼
Authentication / Validation Middleware
  │
  ▼
Meal Controller
  │
  ▼
Meal Recommendation Service
  │
  ├── User preferences
  ├── Budget
  ├── Time of day
  └── Meal data
  │
  ▼
MongoDB / Models
  │
  ▼
Service
  │
  ▼
Controller
  │
  ▼
JSON Response
```

The exact recommendation logic remains private as part of the production codebase.

---

## 14. Engineering Principles

The backend architecture is guided by the following principles:

* **Separation of concerns**
* **Modularity**
* **Secure access to protected resources**
* **Centralized validation and error handling**
* **Maintainability**
* **Clear responsibility boundaries**
* **Reusable business logic**

---

## Privacy & Source Code

This repository is a technical showcase.

The production ChopBeta source code is intentionally private because it contains proprietary business logic and implementation details.

The architecture presented here is therefore a **high-level representation** of the production system rather than a complete reproduction of its internal implementation.
