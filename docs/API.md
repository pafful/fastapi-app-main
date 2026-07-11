# API Reference

## Overview
The backend exposes a REST API through FastAPI. The OpenAPI schema is available at `/api/v1/openapi.json` and interactive docs are available at `/api/docs`.

## Base URL
- Local: `http://localhost:8000/api/v1`
- Production-style deployments: `https://api.<your-domain>`

## Authentication
Authentication uses OAuth2 password flow and JWT bearer tokens.

### Login
- `POST /api/v1/login/access-token`
  - Accepts form data with `username` and `password`.
  - Returns a bearer token.

### Test Token
- `POST /api/v1/login/test-token`
  - Requires authentication.
  - Returns the current authenticated user.

## User Management
- `GET /api/v1/users/` — list users (admin only)
- `POST /api/v1/users/` — create a user (admin only)
- `GET /api/v1/users/me` — retrieve the current user
- `PATCH /api/v1/users/me` — update current user profile
- `PATCH /api/v1/users/me/password` — change current password
- `DELETE /api/v1/users/me` — delete current user
- `POST /api/v1/users/signup` — public registration endpoint
- `GET /api/v1/users/{user_id}` — get a specific user
- `PATCH /api/v1/users/{user_id}` — update a specific user
- `DELETE /api/v1/users/{user_id}` — delete a specific user

## Item Management
- `GET /api/v1/items/` — list items for the current user or all items for admins
- `GET /api/v1/items/{id}` — retrieve an item by ID
- `POST /api/v1/items/` — create a new item
- `PUT /api/v1/items/{id}` — update an item
- `DELETE /api/v1/items/{id}` — delete an item

## Utility Endpoints
- `GET /api/v1/utils/health-check/` — simple health check endpoint
- `POST /api/v1/utils/test-email/` — send a test email (admin only)

## Password Recovery
- `POST /api/v1/password-recovery/{email}`
- `POST /api/v1/reset-password/`
- `POST /api/v1/password-recovery-html-content/{email}`

## Example Request
```bash
curl -X POST "http://localhost:8000/api/v1/login/access-token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "username=admin@example.com&password=changeme"
```

## Error Handling
The API returns standard HTTP status codes:
- `200` success
- `201` created
- `400` invalid input
- `401` authentication required
- `403` insufficient permissions
- `404` resource not found
- `409` conflict (for duplicate users)
