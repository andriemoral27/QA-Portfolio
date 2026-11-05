# 📄 Organization and Event API Testing

## Table of Contents
1. [Register User](#1-register-user)
2. [Login User](#2-login-user)
3. [Get All Organizations](#3-get-all-organizations)
4. [Get Pending Organizations](#4-get-pending-organizations)
5. [Create Organization](#5-create-organization)
6. [Approve Organization](#6-approve-organization)
7. [Reject Organization](#7-reject-organization)
8. [Edit Organization](#8-edit-organization)
9. [Delete Organization](#9-delete-organization)
10. [Create Event](#10-create-event)
11. [Get All Events](#11-get-all-events)
12. [Get Event by ID](#12-get-event-by-id)
13. [Edit Event](#13-edit-event)
14. [Delete Event](#14-delete-event)
15. [Join Event](#15-join-event)
16. [Leave Event](#16-leave-event)
17. [Get Event Tickets](#17-get-event-tickets)

## Overview
This document contains API requests for an Organization and Event management application. These requests are designed to manage user accounts, organizations with approval workflows, events, and event participation. The application includes role-based access control with admin-only operations. Each example includes method, URL, headers, request body, expected HTTP status, and response structure.

---

## 1. Register User

**Method:** POST  
**URL:** https://api.example.com/users

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123"
}
```

**Expected Response:**
**Expected HTTP Status:** 201 Created

```json
{
  "id": 1,
  "name": "John Doe",
  "email": "john@example.com",
  "role": "user",
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Creates a new user account
- Email must be unique → 409 Conflict if already exists
- Default role is "user"

---

## 2. Login User

**Method:** POST  
**URL:** https://api.example.com/users/login

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "email": "john@example.com",
  "password": "password123"
}
```

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com",
    "role": "user"
  }
}
```

**Notes:**
- Returns a JWT token for authentication
- Token should be used in Authorization header for protected routes
- If credentials are invalid → 401 Unauthorized

---

## 3. Get All Organizations

**Method:** GET  
**URL:** https://api.example.com/organizations

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
[
  {
    "id": 5,
    "name": "Tech Community",
    "description": "A community for tech enthusiasts",
    "status": "approved",
    "createdAt": "2024-11-02T12:00:00.000Z"
  },
  {
    "id": 6,
    "name": "Sports Club",
    "description": "Local sports activities",
    "status": "approved",
    "createdAt": "2024-11-02T12:05:00.000Z"
  }
]
```

**Notes:**
- Returns only approved organizations
- Pending organizations are not visible to regular users
- Supports pagination with `page` and `limit` parameters

---

## 4. Get Pending Organizations

**Method:** GET  
**URL:** https://api.example.com/organizations/pending

**Headers:**
- Authorization: Bearer <valid_token>
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
[
  {
    "id": 11,
    "name": "Art Society",
    "description": "Art and culture events",
    "status": "pending",
    "createdAt": "2024-11-02T12:00:00.000Z"
  }
]
```

**Notes:**
- Admin-only endpoint → 403 Forbidden for non-admin users
- Returns organizations with "pending" status
- If no pending organizations, returns an empty array []

---

## 5. Create Organization

**Method:** POST  
**URL:** https://api.example.com/organizations

**Headers:**
- Authorization: Bearer <valid_token>
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "name": "Music Club",
  "description": "Music lovers gathering for concerts and jam sessions"
}
```

**Expected Response:**
**Expected HTTP Status:** 201 Created

```json
{
  "id": 11,
  "name": "Music Club",
  "description": "Music lovers gathering for concerts and jam sessions",
  "status": "pending",
  "creatorId": 1,
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Requires valid JWT token
- New organizations start with "pending" status
- Must be approved by admin before becoming visible
- If name is missing → 400 Bad Request

---

## 6. Approve Organization

**Method:** PATCH  
**URL:** https://api.example.com/organizations/11/approve

**Headers:**
- Authorization: Bearer <valid_token>

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "message": "Organization approved successfully",
  "organization": {
    "id": 11,
    "name": "Music Club",
    "description": "Music lovers gathering for concerts and jam sessions",
    "status": "approved",
    "createdAt": "2024-11-02T12:00:00.000Z",
    "updatedAt": "2024-11-02T12:30:00.000Z"
  }
}
```

**Notes:**
- Admin-only endpoint → 403 Forbidden for non-admin users
- Changes organization status from "pending" to "approved"
- If organization ID does not exist → 404 Not Found
- If organization is already approved → 400 Bad Request

---

## 7. Reject Organization

**Method:** PATCH  
**URL:** https://api.example.com/organizations/6/reject

**Headers:**
- Authorization: Bearer <valid_token>

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "message": "Organization rejected successfully",
  "organization": {
    "id": 6,
    "name": "Sports Club",
    "description": "Local sports activities",
    "status": "rejected",
    "createdAt": "2024-11-02T12:05:00.000Z",
    "updatedAt": "2024-11-02T12:30:00.000Z"
  }
}
```

**Notes:**
- Admin-only endpoint → 403 Forbidden for non-admin users
- Changes organization status to "rejected"
- Rejected organizations are not visible to users
- If organization ID does not exist → 404 Not Found

---

## 8. Edit Organization

**Method:** PATCH  
**URL:** https://api.example.com/organizations/5

**Headers:**
- Authorization: Bearer <valid_token>
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "description": "Updated description for Tech Community"
}
```

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 5,
  "name": "Tech Community",
  "description": "Updated description for Tech Community",
  "status": "approved",
  "createdAt": "2024-11-02T12:00:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Only the organization creator or admin can edit → 403 Forbidden otherwise
- Only updates the fields provided in the request body
- If organization ID does not exist → 404 Not Found

---

## 9. Delete Organization

**Method:** DELETE  
**URL:** https://api.example.com/organizations/6

**Headers:**
- Authorization: Bearer <valid_token>

**Expected Response:**
- HTTP Status: 204 No Content
- Body: Empty (indicating successful deletion)

**Notes:**
- Admin-only endpoint → 403 Forbidden for non-admin users
- Cascade delete: associated events should also be deleted
- If organization has active events → 400 Bad Request or 409 Conflict

---

## 10. Create Event

**Method:** POST  
**URL:** https://api.example.com/events

**Headers:**
- Authorization: Bearer <valid_token>
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "title": "Tech Conference 2024",
  "description": "Annual technology conference with workshops and networking",
  "date": "2024-12-15",
  "time": "09:00",
  "location": "Convention Center",
  "maxAttendees": 500,
  "organizationId": 5
}
```

**Expected Response:**
**Expected HTTP Status:** 201 Created

```json
{
  "id": 9,
  "title": "Tech Conference 2024",
  "description": "Annual technology conference with workshops and networking",
  "date": "2024-12-15",
  "time": "09:00",
  "location": "Convention Center",
  "maxAttendees": 500,
  "currentAttendees": 0,
  "organizationId": 5,
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Requires valid JWT token
- organizationId must reference an approved organization → 400 Bad Request if not found
- date should be in ISO 8601 format (YYYY-MM-DD)
- time should be in 24-hour format (HH:MM)
- If title, date, or location is missing → 400 Bad Request

---

## 11. Get All Events

**Method:** GET  
**URL:** https://api.example.com/events

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
[
  {
    "id": 9,
    "title": "Tech Conference 2024",
    "description": "Annual technology conference with workshops and networking",
    "date": "2024-12-15",
    "time": "09:00",
    "location": "Convention Center",
    "maxAttendees": 500,
    "currentAttendees": 25,
    "organizationId": 5,
    "createdAt": "2024-11-02T12:00:00.000Z"
  },
  {
    "id": 7,
    "title": "Workshop: Web Development",
    "description": "Learn modern web development techniques",
    "date": "2024-11-20",
    "time": "14:00",
    "location": "Tech Hub",
    "maxAttendees": 50,
    "currentAttendees": 30,
    "organizationId": 5,
    "createdAt": "2024-11-02T12:05:00.000Z"
  }
]
```

**Notes:**
- Returns all events from approved organizations
- Supports pagination with `page` and `limit` parameters
- Can filter by organizationId with query parameter
- Can filter by date range with query parameters

---

## 12. Get Event by ID

**Method:** GET  
**URL:** https://api.example.com/events/9

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 9,
  "title": "Tech Conference 2024",
  "description": "Annual technology conference with workshops and networking",
  "date": "2024-12-15",
  "time": "09:00",
  "location": "Convention Center",
  "maxAttendees": 500,
  "currentAttendees": 25,
  "organizationId": 5,
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Error Response if not found:**
**Expected HTTP Status:** 404 Not Found

```json
{
  "error": "Event not found"
}
```

---

## 13. Edit Event

**Method:** PATCH  
**URL:** https://api.example.com/events/9

**Headers:**
- Authorization: Bearer <valid_token>
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "maxAttendees": 600,
  "location": "Grand Convention Center"
}
```

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 9,
  "title": "Tech Conference 2024",
  "description": "Annual technology conference with workshops and networking",
  "date": "2024-12-15",
  "time": "09:00",
  "location": "Grand Convention Center",
  "maxAttendees": 600,
  "currentAttendees": 25,
  "organizationId": 5,
  "createdAt": "2024-11-02T12:00:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Only event creator or organization admin can edit → 403 Forbidden otherwise
- Only updates the fields provided in the request body
- If event ID does not exist → 404 Not Found
- Cannot reduce maxAttendees below currentAttendees → 400 Bad Request

---

## 14. Delete Event

**Method:** DELETE  
**URL:** https://api.example.com/events/7

**Headers:**
- Authorization: Bearer <valid_token>

**Expected Response:**
- HTTP Status: 204 No Content
- Body: Empty (indicating successful deletion)

**Notes:**
- Only event creator or organization admin can delete → 403 Forbidden otherwise
- If event has attendees, they should be notified
- If event ID does not exist → 404 Not Found

---

## 15. Join Event

**Method:** POST  
**URL:** https://api.example.com/events/9/join

**Headers:**
- Authorization: Bearer <valid_token>

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "message": "Successfully joined the event",
  "eventId": 9,
  "attendeeCount": 26
}
```

**Notes:**
- Requires valid JWT token
- Users cannot join the same event twice → 400 Bad Request
- If event is full (currentAttendees >= maxAttendees) → 400 Bad Request
- Creates a ticket for the user

---

## 16. Leave Event

**Method:** POST  
**URL:** https://api.example.com/events/9/leave

**Headers:**
- Authorization: Bearer <valid_token>

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "message": "Successfully left the event",
  "eventId": 9,
  "attendeeCount": 25
}
```

**Notes:**
- Requires valid JWT token
- Users can only leave events they have joined → 400 Bad Request
- Decrements currentAttendees count
- Invalidates the user's ticket

---

## 17. Get Event Tickets

**Method:** GET  
**URL:** https://api.example.com/events/tickets/7

**Headers:**
- Authorization: Bearer <valid_token>
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
[
  {
    "id": 1,
    "eventId": 7,
    "userId": 1,
    "userName": "John Doe",
    "status": "active",
    "issuedAt": "2024-11-02T12:00:00.000Z"
  },
  {
    "id": 2,
    "eventId": 7,
    "userId": 2,
    "userName": "Jane Smith",
    "status": "active",
    "issuedAt": "2024-11-02T12:05:00.000Z"
  }
]
```

**Notes:**
- Admin or event creator only → 403 Forbidden otherwise
- Returns all tickets for a specific event
- If event has no attendees, returns an empty array []
- Ticket status can be: active, cancelled, used
