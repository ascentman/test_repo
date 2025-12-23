## Table of Contents

1. [Sign In / Login](#1-sign-in--login)
2. [Token Refresh](#2-token-refresh)
3. [Logout](#3-logout)
4. [Reset Password](#4-reset-password)

---

## 1. Sign In / Login

Authenticate a user with email and password using Auth0.

### Endpoint

```
POST /auth/login
```

### Headers

```
Content-Type: application/json
```

### Request Body

```json
{
  "email": "user@example.com",
  "password": "SecurePassword123!"
}
```

#### Request Schema

| Field      | Type   | Required | Description                                    |
|------------|--------|----------|------------------------------------------------|
| `email`    | string | Yes      | User's email address (must be valid format)    |
| `password` | string | Yes      | User's password (minimum 8 characters)         |

### Success Response

**Code**: `200 OK`

```json
{
  "token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6IjEyMzQ1Njc4OTAifQ.eyJpc3MiOiJodHRwczovL2lkNG1lLmF1dGgwLmNvbS8iLCJzdWIiOiJhdXRoMHw2MTdhOGI3ZjQxYzE4ZjAwNjkwYzQ5ZWEiLCJhdWQiOlsiaHR0cHM6Ly9hcGkuaWQ0bWUuY29tIiwiaHR0cHM6Ly9pZDRtZS5hdXRoMC5jb20vdXNlcmluZm8iXSwiZXhwIjoxNzM1MDE1MjAwLCJpYXQiOjE3MzUwMTE2MDAsImF6cCI6ImFiY2RlZmdoaWprbG1ub3BxcnN0dXZ3eHl6MTIzNDUiLCJzY29wZSI6Im9wZW5pZCBwcm9maWxlIGVtYWlsIG9mZmxpbmVfYWNjZXNzIn0.signature",
  "refresh_token": "v1.MRqPZT_ijF8aaJNPqGxYVh1234567890abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ",
  "expires_in": 3600,
  "user": {
    "id": "auth0|617a8b7f41c18f00690c49ea",
    "email": "user@example.com",
    "has_active_subscription": true,
    "subscription_expiry_date": "2026-12-31T23:59:59Z"
  }
}
```

#### Response Schema

| Field              | Type    | Description                                          |
|--------------------|---------|------------------------------------------------------|
| `token`            | string  | JWT access token (Bearer token)                      |
| `refresh_token`    | string  | Refresh token for obtaining new access tokens        |
| `expires_in`       | integer | Access token expiration time in seconds (e.g., 3600) |
| `user`             | object  | User information object                              |
| `user.id`          | string  | Unique user identifier from Auth0 (format: `auth0\|{id}`) |
| `user.email`       | string  | User's email address                                 |
| `user.has_active_subscription` | boolean | Whether user has an active iD4me subscription |
| `user.subscription_expiry_date` | string | ISO 8601 date of subscription expiry (nullable) |

### Error Responses

#### 401 Unauthorized - Invalid Credentials

```json
{
  "error": "unauthorized",
  "error_code": "invalid_credentials",
  "message": "Invalid email or password.",
  "status_code": 401
}
```

#### 403 Forbidden - No Active Account

```json
{
  "error": "forbidden",
  "error_code": "inactive_account",
  "message": "This isn't an active iD4me account",
  "status_code": 403
}
```

#### 403 Forbidden - No Active Subscription

```json
{
  "error": "forbidden",
  "error_code": "no_subscription",
  "message": "No active iD4me subscription found.",
  "status_code": 403
}
```

#### 403 Forbidden - Expired Subscription

```json
{
  "error": "forbidden",
  "error_code": "expired_subscription",
  "message": "Your iD4me subscription has expired.",
  "status_code": 403
}
```

#### 500 Internal Server Error

```json
{
  "error": "internal_server_error",
  "error_code": "server_error",
  "message": "Server error. Please try again later.",
  "status_code": 500
}
```

---

## 2. Token Refresh

Obtain a new access token using a valid refresh token.

### Endpoint

```
POST /auth/refresh
```

### Headers

```
Content-Type: application/json
```

### Request Body

```json
{
  "refresh_token": "v1.MRqPZT_ijF8aaJNPqGxYVh1234567890abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ"
}
```

#### Request Schema

| Field           | Type   | Required | Description                              |
|-----------------|--------|----------|------------------------------------------|
| `refresh_token` | string | Yes      | Valid refresh token from login response  |

### Success Response

**Code**: `200 OK`

```json
{
  "token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6IjEyMzQ1Njc4OTAifQ.eyJpc3MiOiJodHRwczovL2lkNG1lLmF1dGgwLmNvbS8iLCJzdWIiOiJhdXRoMHw2MTdhOGI3ZjQxYzE4ZjAwNjkwYzQ5ZWEiLCJhdWQiOlsiaHR0cHM6Ly9hcGkuaWQ0bWUuY29tIiwiaHR0cHM6Ly9pZDRtZS5hdXRoMC5jb20vdXNlcmluZm8iXSwiZXhwIjoxNzM1MDE5MjAwLCJpYXQiOjE3MzUwMTU2MDAsImF6cCI6ImFiY2RlZmdoaWprbG1ub3BxcnN0dXZ3eHl6MTIzNDUiLCJzY29wZSI6Im9wZW5pZCBwcm9maWxlIGVtYWlsIn0.new_signature",
  "refresh_token": "v1.NewRefreshToken1234567890abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ",
  "expires_in": 3600,
  "user": {
    "id": "auth0|617a8b7f41c18f00690c49ea",
    "email": "user@example.com",
    "has_active_subscription": true,
    "subscription_expiry_date": "2026-12-31T23:59:59Z"
  }
}
```

#### Response Schema

Same as login response.

| Field              | Type    | Description                                          |
|--------------------|---------|------------------------------------------------------|
| `token`            | string  | New JWT access token                                 |
| `refresh_token`    | string  | New refresh token (rotation enabled)                 |
| `expires_in`       | integer | Access token expiration time in seconds              |
| `user`             | object  | Updated user information                             |

### Error Responses

#### 401 Unauthorized - Invalid Refresh Token

```json
{
  "error": "unauthorized",
  "error_code": "invalid_refresh_token",
  "message": "Invalid or expired refresh token.",
  "status_code": 401
}
```

#### 403 Forbidden - Revoked Token

```json
{
  "error": "forbidden",
  "error_code": "token_revoked",
  "message": "This refresh token has been revoked. Please login again.",
  "status_code": 403
}
```

---

## 3. Logout

Logout a user by revoking their refresh token on the server.

### Endpoint

```
POST /auth/logout
```

### Headers

```
Content-Type: application/json
Authorization: Bearer {access_token}
```

### Request Body

```json
{
  "refresh_token": "v1.MRqPZT_ijF8aaJNPqGxYVh1234567890abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ"
}
```

#### Request Schema

| Field           | Type   | Required | Description                                    |
|-----------------|--------|----------|------------------------------------------------|
| `refresh_token` | string | Yes      | Refresh token to be revoked                    |

### Success Response

**Code**: `200 OK` or `204 No Content`

```json
{
  "success": true,
  "message": "Successfully logged out."
}
```

#### Response Schema

| Field     | Type    | Description                          |
|-----------|---------|--------------------------------------|
| `success` | boolean | Indicates successful logout          |
| `message` | string  | Human-readable success message       |

### Error Responses

#### 401 Unauthorized - Missing or Invalid Token

```json
{
  "error": "unauthorized",
  "error_code": "invalid_token",
  "message": "Invalid or missing authentication token.",
  "status_code": 401
}
```

#### 400 Bad Request - Missing Refresh Token

```json
{
  "error": "bad_request",
  "error_code": "missing_refresh_token",
  "message": "Refresh token is required.",
  "status_code": 400
}
```

## 4. Reset Password

Initiate password reset flow by sending a password reset email.

### Endpoint

```
POST /auth/password-reset
```

### Headers

```
Content-Type: application/json
```

### Request Body

```json
{
  "email": "user@example.com"
}
```

#### Request Schema

| Field   | Type   | Required | Description                         |
|---------|--------|----------|-------------------------------------|
| `email` | string | Yes      | Email address for password reset    |

### Success Response

**Code**: `200 OK`

```json
{
  "success": true,
  "message": "Password reset email sent successfully. Please check your inbox.",
  "email": "user@example.com"
}
```

#### Response Schema

| Field     | Type    | Description                                  |
|-----------|---------|----------------------------------------------|
| `success` | boolean | Indicates successful email send              |
| `message` | string  | Human-readable success message               |
| `email`   | string  | Email address where reset link was sent      |

### Error Responses

#### 404 Not Found - Email Not Found

```json
{
  "error": "not_found",
  "error_code": "email_not_found",
  "message": "No account found with this email address.",
  "status_code": 404
}
```
