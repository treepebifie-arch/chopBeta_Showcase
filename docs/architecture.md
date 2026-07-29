
# ChopBeta Architecture

ChopBeta is designed using a modular, layered architecture to ensure scalability, maintainability, and separation of concerns.

The system is structured to clearly isolate responsibilities across different layers, making it easier to extend, test, and manage.

---

## Architectural Overview

The application follows a **layered architecture pattern**:

Client → Route → Controller → Service → Model → Database
↓
Response


Each layer has a well-defined responsibility and communicates only with adjacent layers.

---

## Core Layers

### 1. Routing Layer

- Defines all API endpoints
- Maps incoming HTTP requests to the appropriate controller
- Keeps endpoint definitions clean and centralized

---

### 2. Controller Layer

- Handles HTTP requests and responses
- Extracts data from:
  - `req.body`
  - `req.params`
  - `req.query`
- Calls the appropriate service
- Returns structured JSON responses
- Delegates error handling to middleware

> Controllers remain thin and do not contain business logic.

---

### 3. Service Layer

- Contains all core business logic
- Handles:
  - Meal generation logic (budget + time-based)
  - Authentication workflows
  - Session and device management
- Communicates with the data layer (models)
- Ensures reusability and testability

> This layer is framework-agnostic and can function independently of HTTP.

---

### 4.  Data Layer (Models)

- Defines database schemas and structure
- Interacts with the database (MongoDB)
- Handles data persistence and retrieval

---

## Authentication & Authorization Architecture

ChopBeta implements a robust security system:

- **JWT-based Authentication**
  - Tokens issued on login
  - Verified via middleware for protected routes

- **Google OAuth Integration**
  - Supports third-party authentication

- **Role-Based Access Control (RBAC)**
  - Controls access to specific resources

- **Session & Device Management**
  - Tracks active sessions
  - Limits concurrent device logins

---

##  Middleware System

Centralized middleware is used to handle cross-cutting concerns:

-  Authentication Middleware  
  Validates JWT tokens and protects routes  

-  Error Handling Middleware  
  Captures and formats application errors consistently  

-  User Status Middleware  
  Ensures user state is valid before processing requests  

-  Weekly Logic Middleware  
  Handles periodic or time-based system checks  

---

##  Validation Layer

- Implemented using **Zod**
- Ensures all incoming data is properly validated before reaching business logic
- Prevents invalid or malicious input

---

##  Meal Generation System

The core feature of ChopBeta is its intelligent meal recommendation system.

It considers:

- User budget constraints  
- Time of day (breakfast, lunch, dinner, snacks)  
- Nutritional balance  

> The exact algorithm and selection logic are abstracted to protect proprietary implementation details.

---

##  Scalability Considerations

- Modular structure allows easy feature expansion  
- Clear separation of concerns improves maintainability  
- Stateless authentication (JWT) supports horizontal scaling  

---

##  Design Principles

- Separation of concerns  
- Reusability  
- Maintainability  
- Security-first approach  
- Clean and readable code structure  

---

##  Note

This repository showcases the architecture and design of the system.  
Core business logic and implementation details are intentionally abstracted.

---
