# Authentication Flow

1. User logs in (email/password or Google OAuth)
2. Server validates credentials
3. JWT access and refresh tokens are generated
4. Access token expires in 15 mins
5. Access is automatically generated with a new request
6. Token is stored client-side
7. Protected routes require token
8. Middleware verifies token