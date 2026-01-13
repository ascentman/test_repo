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

## 5. Contacts API Specification

This document specifies the API contract that the backend must implement to provide contact data for the iD4me Caller ID system. The PIR service will consume this API and transform the data into a privacy-preserving PIR database.

---

## Data Flow

```
┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│  Backend API    │  ──────▶│  PIR Service    │  ──────▶│  iOS App        │
│  (Your system)  │  JSON   │  (Transformer)  │   PIR   │  (Caller ID)    │
└─────────────────┘         └─────────────────┘         └─────────────────┘
```

---

## API Endpoint Requirements

### GET /contacts

Returns all contacts for the Caller ID database.

#### Request

```http
GET /api/v1/contacts
Authorization: Bearer <api_key>
Accept: application/json
```

#### Response

```json
{
  "contacts": [
    {
      "phone_number": "+61415790018",
      "name": "John Smith",
      "category": "person",
      "block": false,
      "cache_expiry_minutes": 1440,
      "icon_url": "https://example.com/icons/contact-123.png"
    }
  ],
  "total_count": 1,
  "generated_at": "2026-01-13T10:30:00Z"
}
```

---

## Data Model

### Contact Object

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `phone_number` | string | **Yes** | E.164 format with `+` prefix (e.g., `+61415790018`) |
| `name` | string | **Yes** | Display name (max 100 chars) |
| `category` | string | **Yes** | One of: `person`, `business`, `unspecified` |
| `block` | boolean | No | If `true`, marks as spam/blocked (default: `false`) |
| `cache_expiry_minutes` | integer | No | How long iOS caches result (default: `1440` = 24 hours) |
| `icon_url` | string | No | URL to contact icon/avatar (PNG, max 256x256) |

### Category Mapping

| API Value | PIR Database Value | Description |
|-----------|-------------------|-------------|
| `person` | `IDENTITY_CATEGORY_PERSON` | Individual contact |
| `business` | `IDENTITY_CATEGORY_BUSINESS` | Business/organization |
| `unspecified` | `IDENTITY_CATEGORY_UNSPECIFIED` | Unknown category |

---

## Phone Number Format

### Requirements

- **Must** be in E.164 international format
- **Must** start with `+` followed by country code
- **Must not** contain spaces, dashes, or parentheses
- **Must** be unique (no duplicates)

### Examples

| Valid | Invalid |
|-------|---------|
| `+61415790018` | `0415790018` (missing country code) |
| `+14155551234` | `+61 415 790 018` (contains spaces) |
| `+380956414068` | `(415) 555-1234` (wrong format) |

### Normalization

The backend should normalize all phone numbers before returning. Example logic:

```python
import phonenumbers

def normalize_phone(phone: str, default_region: str = "AU") -> str:
    parsed = phonenumbers.parse(phone, default_region)
    return phonenumbers.format_number(parsed, phonenumbers.PhoneNumberFormat.E164)
```

---

## Pagination (For Large Datasets)

If returning more than 10,000 contacts, implement cursor-based pagination:

### Request

```http
GET /api/v1/contacts?limit=10000&cursor=<cursor_token>
```

### Response

```json
{
  "contacts": [...],
  "total_count": 150000,
  "next_cursor": "eyJvZmZzZXQiOjEwMDAwfQ==",
  "has_more": true
}
```

---

## Incremental Updates (Optional)

For efficient updates, implement a delta endpoint:

### GET /contacts/delta

```http
GET /api/v1/contacts/delta?since=2026-01-12T00:00:00Z
```

### Response

```json
{
  "added": [
    { "phone_number": "+61400111222", "name": "New Contact", ... }
  ],
  "updated": [
    { "phone_number": "+61415790018", "name": "Updated Name", ... }
  ],
  "deleted": [
    "+61400333444"
  ],
  "generated_at": "2026-01-13T10:30:00Z"
}
```

---

## Icon/Avatar Requirements

If providing contact icons:

| Property | Requirement |
|----------|-------------|
| Format | PNG (preferred) or JPEG |
| Size | 256x256 pixels (will be scaled down) |
| File size | Max 50 KB |
| Background | Transparent (PNG) or solid color |
| Accessibility | Must be publicly accessible URL or provide auth |

---

## Error Responses

### Standard Error Format

```json
{
  "error": {
    "code": "INVALID_REQUEST",
    "message": "Invalid phone number format",
    "details": {
      "phone_number": "+61 invalid",
      "reason": "Contains spaces"
    }
  }
}
```

### Error Codes

| HTTP Status | Code | Description |
|-------------|------|-------------|
| 400 | `INVALID_REQUEST` | Malformed request |
| 401 | `UNAUTHORIZED` | Invalid or missing API key |
| 429 | `RATE_LIMITED` | Too many requests |
| 500 | `INTERNAL_ERROR` | Server error |

---

## Performance Requirements

| Metric | Requirement |
|--------|-------------|
| Response time | < 30 seconds for full dataset |
| Availability | 99.9% uptime |
| Rate limit | At least 10 requests/minute |
| Timeout | Support requests up to 60 seconds |

---

## Security Requirements

| Requirement | Description |
|-------------|-------------|
| Transport | HTTPS only (TLS 1.2+) |
| Authentication | API key in `Authorization` header |
| IP Whitelist | Optional - whitelist PIR server IPs |
| Data encryption | Encrypt PII at rest |

---

## Sample Test Data

```json
{
  "contacts": [
    {
      "phone_number": "+61415790018",
      "name": "John Smith",
      "category": "person",
      "block": false,
      "cache_expiry_minutes": 1440
    },
    {
      "phone_number": "+61280001234",
      "name": "Acme Corporation",
      "category": "business",
      "block": false,
      "cache_expiry_minutes": 10080
    },
    {
      "phone_number": "+61400999888",
      "name": "Known Scammer",
      "category": "unspecified",
      "block": true,
      "cache_expiry_minutes": 43200
    }
  ]
}
```
