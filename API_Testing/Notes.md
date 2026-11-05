# 📄 Notes API

## Table of Contents
1. [Register](#1-register)
2. [Login](#2-login)
3. [Create Note](#3-create-note)
4. [Get Single Note](#4-get-single-note)
5. [Get All Notes](#5-get-all-notes)
6. [Edit Note](#6-edit-note)
7. [Delete Note](#7-delete-note)

## Overview
This document contains API requests for a Notes application with authentication. These requests are designed to manage user accounts and personal notes including registration, login, creating, retrieving, updating, and deleting notes. Each example includes method, URL, headers, request body, expected HTTP status, and response structure.

---

## 1. Register

**Method:** POST  
**URL:** https://api.example.com/auth/register

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "email": "kenny123@gmail.com",
  "password": "password123"
}
```

**Expected Response:**
**Expected HTTP Status:** 201 Created

```json
{
  "message": "User registered successfully",
  "user": {
    "id": 27,
    "email": "kenny123@gmail.com"
  }
}
```

**Notes:**
- Creates a new user account
- Email must be unique → 409 Conflict if email already exists
- Password should meet minimum requirements

---

## 2. Login

**Method:** POST  
**URL:** https://api.example.com/auth/login

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "email": "Grimm21@gmail.com",
  "password": "password123"
}
```

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6IkdyaW1tMjFAZ21haWwuY29tIiwic3ViIjoyNywiaWF0IjoxNzY4NjUzMzIwLCJleHAiOjE3Njg2NTY5MjB9.EyTAdMhFcfl1dRzysbYu2G9-vC1WfBM8BkIaxEB4kiw",
  "user": {
    "id": 27,
    "email": "Grimm21@gmail.com"
  }
}
```

**Notes:**
- Returns a JWT token for authentication
- Token should be used in Authorization header for protected routes
- If credentials are invalid → 401 Unauthorized

---

## 3. Create Note

**Method:** POST  
**URL:** https://api.example.com/notes

**Headers:**
- Authorization: Bearer <valid_token>
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "title": "Meeting Notes",
  "content": "Discussed project timeline and assigned tasks."
}
```

**Expected Response:**
**Expected HTTP Status:** 201 Created

```json
{
  "id": 65,
  "title": "Meeting Notes",
  "content": "Discussed project timeline and assigned tasks.",
  "userId": 27,
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Requires valid JWT token in Authorization header
- If token is missing or invalid → 401 Unauthorized
- Creates a note associated with the authenticated user

---

## 4. Get Single Note

**Method:** GET  
**URL:** https://api.example.com/notes/65

**Headers:**
- Authorization: Bearer <valid_token>
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 65,
  "title": "Meeting Notes",
  "content": "Discussed project timeline and assigned tasks.",
  "userId": 27,
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Error Response if not found:**
**Expected HTTP Status:** 404 Not Found

```json
{
  "error": "Note not found"
}
```

**Notes:**
- Requires valid JWT token in Authorization header
- If note ID does not exist → 404 Not Found
- Users can only access their own notes → 403 Forbidden otherwise

---

## 5. Get All Notes

**Method:** GET  
**URL:** https://api.example.com/notes

**Headers:**
- Authorization: Bearer <valid_token>
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
[
  {
    "id": 65,
    "title": "Meeting Notes",
    "content": "Discussed project timeline and assigned tasks.",
    "userId": 27,
    "createdAt": "2024-11-02T12:00:00.000Z"
  },
  {
    "id": 66,
    "title": "Shopping List",
    "content": "Milk, Bread, Eggs",
    "userId": 27,
    "createdAt": "2024-11-02T12:10:00.000Z"
  }
]
```

**Notes:**
- Requires valid JWT token in Authorization header
- Returns only notes belonging to the authenticated user
- If no notes exist, returns an empty array []

---

## 6. Edit Note

**Method:** PATCH  
**URL:** https://api.example.com/notes/65

**Headers:**
- Authorization: Bearer <valid_token>
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "title": "New Meeting Notes",
  "content": "New Discussed project timeline and assigned tasks."
}
```

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 65,
  "title": "New Meeting Notes",
  "content": "New Discussed project timeline and assigned tasks.",
  "userId": 27,
  "createdAt": "2024-11-02T12:00:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Requires valid JWT token in Authorization header
- Only updates the fields provided in the request body
- If note ID does not exist → 404 Not Found
- Users can only edit their own notes → 403 Forbidden otherwise

---

## 7. Delete Note

**Method:** DELETE  
**URL:** https://api.example.com/notes/80

**Headers:**
- Authorization: Bearer <valid_token>

**Expected Response:**
- HTTP Status: 204 No Content
- Body: Empty (indicating successful deletion)

**Notes:**
- Requires valid JWT token in Authorization header
- If note ID does not exist → 404 Not Found
- Users can only delete their own notes → 403 Forbidden otherwise
