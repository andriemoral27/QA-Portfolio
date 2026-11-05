# Organization and Event API Documentation

## Overview
This document contains API requests for an Organization and Event Management application with role-based authentication. These requests are designed to manage users, organizations, events, and event registrations. The system includes user and admin roles, with admins having special permissions to approve/reject organizations. Each example includes the HTTP method, URL, headers, request body, expected HTTP status codes, and notes for testing in Postman.

## Table of Contents
1. [Authentication](#authentication)
   - 1.1 Register User
   - 1.2 Register Admin
   - 1.3 User Login
   - 1.4 Admin Login
2. [Users](#users)
   - 2.1 Get All Users
3. [Organizations](#organizations)
   - 3.1 Create Organization
   - 3.2 Get All Organizations
   - 3.3 Get Pending Organizations
   - 3.4 Approve Organization (Admin Only)
   - 3.5 Reject Organization (Admin Only)
   - 3.6 Update Organization (Admin Only)
   - 3.7 Delete Organization (Admin Only)
4. [Events](#events)
   - 4.1 Create Event
   - 4.2 Get All Events
   - 4.3 Update Event
   - 4.4 Delete Event
   - 4.5 Join Event
   - 4.6 Leave Event
5. [Tickets](#tickets)
   - 5.1 Get All Tickets from User

---

## Authentication

### 1.1 Register User

**Method:** POST  
**URL:** `{{base3_url}}/users/register`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "email": "grimm@gmail.com",
  "password": "UserPass123!",
  "first_name": "John",
  "last_name": "Doe",
  "gender": "male",
  "dob": "1990-01-15",
  "role": "user"
}
```

**Expected Response:**

**HTTP Status:** 201 Created  
**Body:**
```json
{
  "message": "User registered successfully",
  "user": {
    "id": 5,
    "email": "grimm@gmail.com",
    "first_name": "John",
    "last_name": "Doe",
    "role": "user"
  }
}
```

**Notes:**
- Creates a new user account with "user" role
- Email must be unique -> 409 Conflict if email already exists
- Password should meet minimum requirements
- Role can be "user" or "admin"
- If required fields are missing -> 400 Bad Request

---

### 1.2 Register Admin

**Method:** POST  
**URL:** `{{base3_url}}/users/register`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "email": "chloe@gmail.com",
  "password": "UserPass123!",
  "first_name": "John",
  "last_name": "Doe",
  "gender": "male",
  "dob": "1990-01-15",
  "role": "user"
}
```

**Expected Response:**

**HTTP Status:** 201 Created  
**Body:**
```json
{
  "message": "User registered successfully",
  "user": {
    "id": 4,
    "email": "chloe@gmail.com",
    "first_name": "John",
    "last_name": "Doe",
    "role": "user"
  }
}
```

**Notes:**
- Creates a new user account (role may be set to admin later or via separate endpoint)
- Email must be unique -> 409 Conflict if email already exists
- In some implementations, admin registration may require special privileges

---

### 1.3 User Login

**Method:** POST  
**URL:** `{{base3_url}}/users/login`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "email": "chloe@gmail.com",
  "password": "UserPass123!"
}
```

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6ImNobG9lQGdtYWlsLmNvbSIsInVpZCI6NCwicm9sZSI6InVzZXIiLCJpYXQiOjE3Njg4MTkwNTksImV4cCI6MTc2OTQyMzg1OX0.7D39sDk17fvVIdBM5ExXXZ-ClCUUGtzjatRWjycXKIo",
  "user": {
    "id": 4,
    "email": "chloe@gmail.com",
    "role": "user"
  }
}
```

**Notes:**
- Returns a JWT token for authentication
- Token should be used in Authorization header for protected routes
- If credentials are invalid -> 401 Unauthorized
- Token includes user role for authorization checks

---

### 1.4 Admin Login

**Method:** POST  
**URL:** `{{base3_url}}/users/login`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "email": "adminchloe@gmail.com",
  "password": "AdminPass123!"
}
```

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6ImFkbWluY2hsb2VAZ21haWwuY29tIiwidWlkIjo0LCJyb2xlIjoiYWRtaW4iLCJpYXQiOjE3Njg4MTkwNTksImV4cCI6MTc2OTQyMzg1OX0.7D39sDk17fvVIdBM5ExXXZ-ClCUUGtzjatRWjycXKIo",
  "user": {
    "id": 4,
    "email": "adminchloe@gmail.com",
    "role": "admin"
  }
}
```

**Notes:**
- Returns a JWT token with admin role
- Admin token grants access to admin-only endpoints
- If credentials are invalid -> 401 Unauthorized

---

## Users

### 2.1 Get All Users

**Method:** GET  
**URL:** `{{base3_url}}/users`

**Headers:**
- Authorization: Bearer <valid_token>

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
[
  {
    "id": 5,
    "email": "akira@gmail.com",
    "first_name": "Akira",
    "last_name": "Tanaka",
    "role": "user",
    "createdAt": "2024-11-02T12:00:00.000Z"
  },
  {
    "id": 4,
    "email": "adminchloe@gmail.com",
    "first_name": "Chloe",
    "last_name": "Admin",
    "role": "admin",
    "createdAt": "2024-11-02T11:50:00.000Z"
  }
]
```

**Notes:**
- Requires valid JWT token in Authorization header
- If token is missing or invalid -> 401 Unauthorized
- Returns an array of all registered users
- May require admin role to access all users

---

## Organizations

### 3.1 Create Organization

**Method:** POST  
**URL:** `{{base3_url}}/organizations`

**Headers:**
- Authorization: Bearer <valid_token>
- Content-Type: application/json

**Request Body:**
```json
{
  "name": "Tech Community Manila",
  "description": "A community for tech enthusiasts in Manila",
  "website": "https://techcommunity.com",
  "email": "contact@techcommunity.com",
  "phone": "+63123456789",
  "address": "Manila, Philippines"
}
```

**Expected Response:**

**HTTP Status:** 201 Created  
**Body:**
```json
{
  "id": 11,
  "name": "Tech Community Manila",
  "description": "A community for tech enthusiasts in Manila",
  "website": "https://techcommunity.com",
  "email": "contact@techcommunity.com",
  "phone": "+63123456789",
  "address": "Manila, Philippines",
  "status": "pending",
  "createdAt": "2024-11-02T12:10:00.000Z"
}
```

**Notes:**
- Requires valid JWT token in Authorization header
- Creates organization with "pending" status by default
- Name is required field
- Organization must be approved by admin before it can create events
- If required fields are missing -> 400 Bad Request

---

### 3.2 Get All Organizations

**Method:** GET  
**URL:** `{{base3_url}}/organizations`

**Headers:**
- Authorization: Bearer <valid_token>

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
[
  {
    "id": 9,
    "name": "Tech Community Manila",
    "description": "A community for tech enthusiasts in Manila",
    "website": "https://techcommunity.com",
    "email": "contact@techcommunity.com",
    "phone": "+63123456789",
    "address": "Manila, Philippines",
    "status": "approved",
    "createdAt": "2024-11-02T12:10:00.000Z"
  }
]
```

**Notes:**
- Requires valid JWT token in Authorization header
- Returns all organizations (both approved and pending)
- May return only approved organizations for regular users (implementation dependent)

---

### 3.3 Get Pending Organizations

**Method:** GET  
**URL:** `{{base3_url}}/organizations/pending`

**Headers:**
- Authorization: Bearer <admin_token>

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
[
  {
    "id": 11,
    "name": "Tech Community Manila",
    "description": "A community for tech enthusiasts in Manila",
    "website": "https://techcommunity.com",
    "email": "contact@techcommunity.com",
    "phone": "+63123456789",
    "address": "Manila, Philippines",
    "status": "pending",
    "createdAt": "2024-11-02T12:10:00.000Z"
  }
]
```

**Notes:**
- Requires valid admin JWT token in Authorization header
- If user is not admin -> 403 Forbidden
- Returns only organizations with "pending" status
- If no pending organizations, returns an empty array []

---

### 3.4 Approve Organization (Admin Only)

**Method:** PATCH  
**URL:** `{{base3_url}}/organizations/{organization_id}/approve`

**Headers:**
- Authorization: Bearer <admin_token>

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 11,
  "name": "Tech Community Manila",
  "description": "A community for tech enthusiasts in Manila",
  "website": "https://techcommunity.com",
  "email": "contact@techcommunity.com",
  "phone": "+63123456789",
  "address": "Manila, Philippines",
  "status": "approved",
  "createdAt": "2024-11-02T12:10:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Requires valid admin JWT token in Authorization header
- If user is not admin -> 403 Forbidden
- Changes organization status from "pending" to "approved"
- If organization ID does not exist -> 404 Not Found
- Only pending organizations can be approved

---

### 3.5 Reject Organization (Admin Only)

**Method:** PATCH  
**URL:** `{{base3_url}}/organizations/{organization_id}/reject`

**Headers:**
- Authorization: Bearer <admin_token>

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 6,
  "name": "Rejected Organization",
  "description": "This organization was rejected",
  "website": "https://rejected.com",
  "email": "contact@rejected.com",
  "phone": "+63987654321",
  "address": "Rejected City",
  "status": "rejected",
  "createdAt": "2024-11-02T12:10:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Requires valid admin JWT token in Authorization header
- If user is not admin -> 403 Forbidden
- Changes organization status from "pending" to "rejected"
- If organization ID does not exist -> 404 Not Found
- Only pending organizations can be rejected

---

### 3.6 Update Organization (Admin Only)

**Method:** PATCH  
**URL:** `{{base3_url}}/organizations/{organization_id}`

**Headers:**
- Authorization: Bearer <admin_token>
- Content-Type: application/json

**Request Body:**
```json
{
  "name": "Updated Tech Community Manila",
  "description": "Updated description",
  "website": "https://newtechcommunity.com",
  "email": "new@techcommunity.com"
}
```

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 5,
  "name": "Updated Tech Community Manila",
  "description": "Updated description",
  "website": "https://newtechcommunity.com",
  "email": "new@techcommunity.com",
  "phone": "+63123456789",
  "address": "Manila, Philippines",
  "status": "approved",
  "createdAt": "2024-11-02T12:10:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Requires valid admin JWT token in Authorization header
- If user is not admin -> 403 Forbidden
- Only updates the fields provided in the request body
- If organization ID does not exist -> 404 Not Found

---

### 3.7 Delete Organization (Admin Only)

**Method:** DELETE  
**URL:** `{{base3_url}}/organizations/{organization_id}`

**Headers:**
- Authorization: Bearer <admin_token>

**Request Body:** None

**Expected Response:**

**HTTP Status:** 204 No Content  
**Body:** Empty (indicating successful deletion)

**Notes:**
- Requires valid admin JWT token in Authorization header
- If user is not admin -> 403 Forbidden
- If organization ID does not exist -> 404 Not Found
- If organization has associated events, may return 400 Bad Request or cascade delete (implementation dependent)
- Successful deletion returns 204 with no body

---

## Events

### 4.1 Create Event

**Method:** POST  
**URL:** `{{base3_url}}/events`

**Headers:**
- Authorization: Bearer <valid_token>
- Content-Type: application/json

**Request Body:**
```json
{
  "organizationId": 9,
  "title": "Novel Conference 2026",
  "description": "Annual technology conference featuring the latest innovations",
  "eventDate": "2026-02-15",
  "startTime": "09:00",
  "endTime": "17:00",
  "location": "Manila Convention Center",
  "maxAttendees": 100,
  "registrationDeadline": "2026-02-10"
}
```

**Expected Response:**

**HTTP Status:** 201 Created  
**Body:**
```json
{
  "id": 9,
  "organizationId": 9,
  "title": "Novel Conference 2026",
  "description": "Annual technology conference featuring the latest innovations",
  "eventDate": "2026-02-15",
  "startTime": "09:00",
  "endTime": "17:00",
  "location": "Manila Convention Center",
  "maxAttendees": 100,
  "registrationDeadline": "2026-02-10",
  "currentAttendees": 0,
  "createdAt": "2024-11-02T12:20:00.000Z"
}
```

**Notes:**
- Requires valid JWT token in Authorization header
- organizationId must reference an approved organization
- If organization is not approved -> 403 Forbidden
- If organizationId does not exist -> 400 Bad Request
- Date and time fields should be in valid formats

---

### 4.2 Get All Events

**Method:** GET  
**URL:** `{{base3_url}}/events`

**Headers:**
- Authorization: Bearer <valid_token>

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
[
  {
    "id": 9,
    "organizationId": 9,
    "title": "Novel Conference 2026",
    "description": "Annual technology conference featuring the latest innovations",
    "eventDate": "2026-02-15",
    "startTime": "09:00",
    "endTime": "17:00",
    "location": "Manila Convention Center",
    "maxAttendees": 100,
    "registrationDeadline": "2026-02-10",
    "currentAttendees": 5,
    "createdAt": "2024-11-02T12:20:00.000Z"
  }
]
```

**Notes:**
- Requires valid JWT token in Authorization header
- Returns an array of all events
- If no events exist, returns an empty array []
- Events may be sorted by event date

---

### 4.3 Update Event

**Method:** PATCH  
**URL:** `{{base3_url}}/events/{event_id}`

**Headers:**
- Authorization: Bearer <valid_token>
- Content-Type: application/json

**Request Body:**
```json
{
  "title": "Updated Tech Conference 2026",
  "maxAttendees": 150,
  "description": "Updated conference description"
}
```

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 9,
  "organizationId": 9,
  "title": "Updated Tech Conference 2026",
  "description": "Updated conference description",
  "eventDate": "2026-02-15",
  "startTime": "09:00",
  "endTime": "17:00",
  "location": "Manila Convention Center",
  "maxAttendees": 150,
  "registrationDeadline": "2026-02-10",
  "currentAttendees": 5,
  "createdAt": "2024-11-02T12:20:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Requires valid JWT token in Authorization header
- Only updates the fields provided in the request body
- If event ID does not exist -> 404 Not Found
- User must be from the event's organization (implementation dependent)

---

### 4.4 Delete Event

**Method:** DELETE  
**URL:** `{{base3_url}}/events/{event_id}`

**Headers:**
- Authorization: Bearer <valid_token>

**Request Body:** None

**Expected Response:**

**HTTP Status:** 204 No Content  
**Body:** Empty (indicating successful deletion)

**Notes:**
- Requires valid JWT token in Authorization header
- If event ID does not exist -> 404 Not Found
- User must be from the event's organization or admin (implementation dependent)
- If event has attendees, may return 400 Bad Request or cascade delete (implementation dependent)
- Successful deletion returns 204 with no body

---

### 4.5 Join Event

**Method:** POST  
**URL:** `{{base3_url}}/events/{event_id}/join`

**Headers:**
- Authorization: Bearer <valid_token>
- Content-Type: application/json

**Request Body:**
```json
{
  "userId": 6
}
```

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "message": "Successfully joined event",
  "ticket": {
    "id": 1,
    "eventId": 9,
    "userId": 6,
    "status": "confirmed",
    "createdAt": "2024-11-02T12:25:00.000Z"
  }
}
```

**Notes:**
- Requires valid JWT token in Authorization header
- Creates a ticket/registration for the user
- User must not already be registered for the event
- If event is full (maxAttendees reached) -> 400 Bad Request
- If registration deadline has passed -> 400 Bad Request
- If event ID does not exist -> 404 Not Found

---

### 4.6 Leave Event

**Method:** POST  
**URL:** `{{base3_url}}/events/{event_id}/leave`

**Headers:**
- Authorization: Bearer <valid_token>
- Content-Type: application/json

**Request Body:**
```json
{
  "userId": 5
}
```

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "message": "Successfully left event"
}
```

**Notes:**
- Requires valid JWT token in Authorization header
- Removes the user's ticket/registration for the event
- User must be registered for the event
- If user is not registered -> 400 Bad Request
- If event ID does not exist -> 404 Not Found

---

## Tickets

### 5.1 Get All Tickets from User

**Method:** GET  
**URL:** `{{base3_url}}/events/tickets/{user_id}`

**Headers:**
- Authorization: Bearer <valid_token>

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
[
  {
    "id": 1,
    "eventId": 9,
    "userId": 7,
    "status": "confirmed",
    "event": {
      "id": 9,
      "title": "Novel Conference 2026",
      "eventDate": "2026-02-15",
      "location": "Manila Convention Center"
    },
    "createdAt": "2024-11-02T12:25:00.000Z"
  }
]
```

**Notes:**
- Requires valid JWT token in Authorization header
- Returns all tickets/registrations for a specific user
- If user has no tickets, returns an empty array []
- May include event details in the response
- Users can typically only view their own tickets (403 Forbidden otherwise)

---

## Common Status Codes

| Status Code | Description |
|-------------|-------------|
| 200 OK | Request succeeded (GET, POST for join/leave, PATCH) |
| 201 Created | Resource successfully created (POST) |
| 204 No Content | Successful deletion (DELETE) |
| 400 Bad Request | Invalid request data or format |
| 401 Unauthorized | Missing or invalid authentication token |
| 403 Forbidden | User does not have permission (e.g., non-admin accessing admin endpoints) |
| 404 Not Found | Resource not found |
| 409 Conflict | Resource already exists (e.g., duplicate email) |
| 500 Internal Server Error | Server error occurred |

## Authentication

All protected endpoints require authentication using a JWT token in the Authorization header:

```
Authorization: Bearer <your_jwt_token>
```

The token is obtained from the Login endpoint and includes the user's role for authorization checks.

## Role-Based Access Control

- **User Role**: Can register organizations, create events (if organization is approved), join/leave events, view their own tickets
- **Admin Role**: Can access all user endpoints, approve/reject organizations, update/delete organizations, view all organizations including pending ones

## Testing in Postman

1. Import the Postman collection: `Organization and event.postman_collection.json`
2. Set the `base3_url` variable in Postman environment or collection variables
3. Register a user and an admin using the Register endpoints
4. Login as both user and admin to obtain their respective JWT tokens
5. Copy the tokens and set them in the Authorization header for protected endpoints
6. Test user workflow:
   - User creates an organization (status should be "pending")
   - Try to create an event with pending organization (should fail with 403)
7. Test admin workflow:
   - Admin views pending organizations
   - Admin approves the organization
   - Admin can also reject organizations
8. Test event creation:
   - User (from approved organization) creates an event
   - View all events
9. Test event registration:
   - Different users join the event
   - Users leave the event
   - View user tickets
10. Test authorization:
    - Try to access admin endpoints with user token (should return 403)
    - Try to access other users' tickets (should return 403)
11. Verify response status codes match expected values
12. Test error scenarios:
    - Create event with non-existent organizationId (should return 400)
    - Join event that is full (should return 400)
    - Join event after registration deadline (should return 400)
    - Approve/reject non-existent organization (should return 404)
