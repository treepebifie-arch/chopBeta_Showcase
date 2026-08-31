# ChopBeta — Backend Engineering Showcase

ChopBeta is a food-tech backend system that generates personalized meal recommendations based on a user's budget and time of day.

This repository is a **public technical showcase** of the ChopBeta backend. It documents the system's architecture, API design, security mechanisms, and selected API examples. The production source code remains private to protect proprietary business logic.

---

## Overview

ChopBeta is designed to help users discover suitable meals based on factors such as:

* Available budget
* Time of day
* Meal category
* Nutritional considerations

The backend provides RESTful APIs that support authentication, authorization, meal recommendations, meal exploration, and session management.

---

## Key Features

### Meal & Recommendation System

* Budget-based meal recommendations
* Time-of-day meal suggestions
* Categorized meal recommendations
* Meal exploration
* Personalized meal options

### Authentication & Security

* JWT-based authentication
* Google OAuth 2.0 authentication
* Role-Based Access Control (RBAC)
* Password hashing with bcrypt
* Session and device tracking
* Concurrent-device login limitations

### Backend Engineering

* RESTful API architecture
* Service-Controller-Route separation
* Centralized middleware
* Request validation with Zod
* Centralized error handling
* User-status checks
* Modular backend structure

---

## Architecture

ChopBeta follows a layered backend architecture designed to separate HTTP handling, business logic, and data access.

```text
Client
   │
   ▼
Routes
   │
   ▼
Middleware
   │
   ├── Authentication
   ├── Request Validation
   ├── User Status Checks
   ├── Other Business Rules
   └── Error Handling
   │
   ▼
Controllers
   │
   ▼
Services
   │
   ▼
Models
   │
   ▼
MongoDB
```

### Architecture Responsibilities

**Routes**

Define API endpoints and connect incoming requests to the appropriate middleware and controllers.

**Middleware**

Handles cross-cutting concerns such as authentication, validation, user-status checks, and error handling.

**Controllers**

Handle HTTP requests and responses while coordinating operations with the service layer.

**Services**

Contain application-specific business logic and keep it separate from HTTP-related concerns.

**Models**

Define database structures and provide the interface for interacting with persistent data.

---

## Authentication & Authorization

ChopBeta implements multiple layers of authentication and authorization.

### JWT Authentication

JWT is used for stateless authentication and protected API access.

### Google OAuth 2.0

Users can authenticate through Google OAuth 2.0.

### Role-Based Access Control

RBAC is used to control access to protected resources according to user roles and permissions.

### Session & Device Management

The system tracks user sessions and devices and supports restrictions on the number of devices that can be logged in simultaneously.

---

## API Preview

The backend exposes RESTful endpoints for authentication, meal recommendations, and session management.

### Authentication

| Method | Endpoint                   | Description         |
| ------ | -------------------------- | ------------------- |
| `POST` | `/api/v1/auth/user/signup` | Register a new user |
| `POST` | `/api/v1/auth/user/login`  | Authenticate a user |

### Meal Recommendations

| Method | Endpoint                       | Description                         |
| ------ | ------------------------------ | ----------------------------------- |
| `GET`  | `/api/v1/meals/generate-meals` | Generate meal recommendations       |
| `GET`  | `/api/v1/meals/quick-meals`    | Retrieve quick meal recommendations |

### Sessions

| Method | Endpoint                     | Description                   |
| ------ | ---------------------------- | ----------------------------- |
| `GET`  | `/api/v1/auth/user/sessions` | Retrieve active user sessions |

> The repository contains a selected API overview rather than the complete production API specification.

For more detailed API examples, see the [`docs`](./docs) directories.

---

## API Documentation

A selected overview of the ChopBeta API is available in docs/API.md. The complete development Postman collection remains private for team development.


##  Technology Stack

| Category          | Technologies   |
| ----------------- | -------------- |
| Language          | JavaScript     |
| Runtime           | Node.js        |
| Framework         | Express.js     |
| Database          | MongoDB        |
| ODM               | Mongoose       |
| Authentication    | JWT, OAuth 2.0 |
| Authorization     | RBAC           |
| Validation        | Zod            |
| Password Security | bcrypt         |
| API Testing       | Postman        |

---

## Showcase Structure

```text
chopBeta_Showcase/
│
├── docs/
│   ├── ARCHITECTURE.md
│   ├── API.md
│   └── SECURITY.md
│
├── images/
│   ├── api-preview.png
│   ├── api-preview-2.png
│   ├── chopbeta_entry_file.png
│   ├── generate-meals.png
│   ├── login-success.png
│   ├── mongoDB_Collection_view.png
│   └── session-preview.png
│
└── README.md
```

The showcase intentionally excludes the production source code and proprietary business logic.

---

## My Contribution

**Treasure Epebifie — Backend Developer, ASAP Team**

My backend contributions include:

* Developing RESTful backend APIs using Node.js and Express.js
* Implementing authentication and authorization using JWT and Google OAuth 2.0
* Implementing Role-Based Access Control (RBAC)
* Developing session and device tracking functionality
* Implementing request validation with Zod
* Developing centralized middleware for authentication, validation, user-status checks, and error handling
* Building backend functionality for meal recommendations and exploration
* Structuring backend logic using separation of concerns

---

## Live API

**ChopBeta API:**
https://api-chopbeta.onrender.com/

---

##  About This Repository

This repository is intentionally designed as a **technical showcase**.

The complete ChopBeta production codebase is private because it contains proprietary implementation details and business logic.

Instead of exposing the source code, this repository provides an overview of:

* System architecture
* Backend design
* API structure
* Authentication and authorization
* Security mechanisms
* API examples
* Engineering decisions

This allows the project to be evaluated from an engineering perspective while protecting the underlying production codebase.

---

## Team

ChopBeta is a product of the **ASAP Team**.

**Backend Developer:** Treasure Epebifie

---

##  Documentation

Additional technical documentation can be found in:

* [`Architecture`](./docs/architecture.md)
* [`API Documentation`](./docs/API.md)
* [`Security`](./docs/security.md)

---

## Contact

**Treasure Epebifie**

* GitHub: https://github.com/treepebifie-arch
* LinkedIn: https://www.linkedin.com/in/tamarauseri-treasure-epebifie-854532350
* Email: [treepebifie@gmail.com](mailto:treepebifie@gmail.com)
