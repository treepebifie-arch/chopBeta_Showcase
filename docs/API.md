# ChopBeta API

ChopBeta provides RESTful APIs for authentication, meal discovery, personalized recommendations, and meal tracking.

> This is a selected API overview. The production source code and proprietary business logic remain private.

## Base URL

```text
https://api-chopbeta.onrender.com
```

---

##  Authentication

| Method   | Endpoint                               | Description              |
| -------- | -------------------------------------- | ------------------------ |
| `POST`   | `/api/v1/auth/user/signup`             | Register a user          |
| `POST`   | `/api/v1/auth/user/login`              | Authenticate a user      |
| `POST`   | `/api/v1/auth/user/logout`             | End the current session  |
| `GET`    | `/api/v1/auth/user/sessions`           | Retrieve active sessions |
| `DELETE` | `/api/v1/auth/user/deactivate-account` | Deactivate account       |

Protected endpoints use Bearer authentication:

```http
Authorization: Bearer <JWT>
```

---

## Meal APIs

| Method | Endpoint                          | Description                    |
| ------ | --------------------------------- | ------------------------------ |
| `GET`  | `/api/v1/meals/generate-meals`    | Generate meals based on budget |
| `GET`  | `/api/v1/meals/search-meals`      | Search or filter meals         |
| `GET`  | `/api/v1/meals/quick-meals`       | Retrieve quick meal options    |
| `POST` | `/api/v1/meals/add`               | Add a meal                     |
| `POST` | `/api/v1/meals/add-image/:mealId` | Add a meal image               |

The meal generation endpoint accepts a price parameter, for example:

```http
GET /api/v1/meals/generate-meals?price=1000
```

## The search endpoint supports pagination and search/filter parameters such as meal title and meal type.

## Meal Tracking

ChopBeta also provides APIs for tracking meal activity.

| Method | Endpoint                              | Description                    |
| ------ | ------------------------------------- | ------------------------------ |
| `GET`  | `/api/v1/track/generated-today`       | View meals generated today     |
| `GET`  | `/api/v1/track/all-generated-meals`   | View generated meal history    |
| `GET`  | `/api/v1/track/daily-partial-meals`   | View partially completed meals |
| `GET`  | `/api/v1/track/daily-completed-meals` | View completed meals           |
| `GET`  | `/api/v1/track/all-completed-meals`   | View completed meal history    |

Several tracking endpoints support pagination and optional date filtering.

## Response Format

API responses generally follow a structured format:

```json
{
  "success": true,
  "statusCode": 200,
  "message": "Request successful",
  "data": {}
}
```

---

## Security

The API implements:

* JWT authentication
* Google OAuth 2.0
* Role-Based Access Control
* Password hashing
* Request validation
* Session/device tracking
* Centralized error handling

For the architectural overview, see [`ARCHITECTURE.md`](./ARCHITECTURE.md).
