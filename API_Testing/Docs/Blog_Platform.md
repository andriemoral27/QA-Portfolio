# 📄 Blog Platform API 

## Table of Contents
1. [Register](#1-register)
2. [Login](#2-login)
3. [Create User](#3-create-user)
4. [Get All Users](#4-get-all-users)
5. [Get User by ID](#5-get-user-by-id)
6. [Edit User](#6-edit-user)
7. [Delete User](#7-delete-user)
8. [Create Post](#8-create-post)
9. [Get All Posts](#9-get-all-posts)
10. [Get Post by ID](#10-get-post-by-id)
11. [Edit Post](#11-edit-post)
12. [Delete Post](#12-delete-post)
13. [Like Post](#13-like-post)
14. [Dislike Post](#14-dislike-post)
15. [Create Comment](#15-create-comment)
16. [Get Post Comments](#16-get-post-comments)
17. [Get Comment by ID](#17-get-comment-by-id)
18. [Edit Comment](#18-edit-comment)
19. [Delete Comment](#19-delete-comment)
20. [React to Comment](#20-react-to-comment)

## Overview
This document contains API requests for a Blog Platform application. These requests are designed to manage users, posts, comments, and reactions. The platform includes authentication, CRUD operations for all resources, and social features like likes and comments. Each example includes method, URL, headers, request body, expected HTTP status, and response structure.

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
  "username": "johndoe",
  "email": "john@example.com",
  "password": "password123"
}
```

**Expected Response:**
**Expected HTTP Status:** 201 Created

```json
{
  "message": "User registered successfully",
  "user": {
    "id": 1,
    "username": "johndoe",
    "email": "john@example.com"
  }
}
```

**Notes:**
- Creates a new user account
- Email and username must be unique → 409 Conflict if already exists
- Password should meet minimum security requirements

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
    "username": "johndoe",
    "email": "john@example.com"
  }
}
```

**Notes:**
- Returns a JWT token for authentication
- Token should be used in Authorization header for protected routes
- If credentials are invalid → 401 Unauthorized

---

## 3. Create User

**Method:** POST  
**URL:** https://api.example.com/users

**Headers:**
- Authorization: Bearer <valid_token>
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "username": "janedoe",
  "email": "jane@example.com",
  "password": "password123",
  "bio": "Software developer and tech enthusiast"
}
```

**Expected Response:**
**Expected HTTP Status:** 201 Created

```json
{
  "id": 2,
  "username": "janedoe",
  "email": "jane@example.com",
  "bio": "Software developer and tech enthusiast",
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Requires valid JWT token (admin only)
- Non-admin users → 403 Forbidden

---

## 4. Get All Users

**Method:** GET  
**URL:** https://api.example.com/users

**Headers:**
- Authorization: Bearer <valid_token>
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
[
  {
    "id": 1,
    "username": "johndoe",
    "email": "john@example.com",
    "bio": null,
    "createdAt": "2024-11-02T12:00:00.000Z"
  },
  {
    "id": 2,
    "username": "janedoe",
    "email": "jane@example.com",
    "bio": "Software developer and tech enthusiast",
    "createdAt": "2024-11-02T12:05:00.000Z"
  }
]
```

**Notes:**
- Requires valid JWT token
- Returns list of all users
- Pagination may be supported with `page` and `limit` parameters

---

## 5. Get User by ID

**Method:** GET  
**URL:** https://api.example.com/users/1

**Headers:**
- Authorization: Bearer <valid_token>
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 1,
  "username": "johndoe",
  "email": "john@example.com",
  "bio": null,
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Error Response if not found:**
**Expected HTTP Status:** 404 Not Found

```json
{
  "error": "User not found"
}
```

---

## 6. Edit User

**Method:** PATCH  
**URL:** https://api.example.com/users/1

**Headers:**
- Authorization: Bearer <valid_token>
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "bio": "Passionate blogger and developer"
}
```

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 1,
  "username": "johndoe",
  "email": "john@example.com",
  "bio": "Passionate blogger and developer",
  "createdAt": "2024-11-02T12:00:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Users can only edit their own profile → 403 Forbidden otherwise
- Only updates the fields provided in the request body

---

## 7. Delete User

**Method:** DELETE  
**URL:** https://api.example.com/users/1

**Headers:**
- Authorization: Bearer <valid_token>

**Expected Response:**
- HTTP Status: 204 No Content
- Body: Empty (indicating successful deletion)

**Notes:**
- Users can only delete their own account → 403 Forbidden otherwise
- Cascade delete: associated posts and comments should also be deleted

---

## 8. Create Post

**Method:** POST  
**URL:** https://api.example.com/posts

**Headers:**
- Authorization: Bearer <valid_token>
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "title": "Getting Started with API Testing",
  "content": "API testing is crucial for ensuring your backend services work correctly..."
}
```

**Expected Response:**
**Expected HTTP Status:** 201 Created

```json
{
  "id": 1,
  "title": "Getting Started with API Testing",
  "content": "API testing is crucial for ensuring your backend services work correctly...",
  "authorId": 1,
  "likes": 0,
  "dislikes": 0,
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Requires valid JWT token
- authorId is automatically set to the authenticated user's ID
- If title or content is missing → 400 Bad Request

---

## 9. Get All Posts

**Method:** GET  
**URL:** https://api.example.com/posts

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
[
  {
    "id": 1,
    "title": "Getting Started with API Testing",
    "content": "API testing is crucial for ensuring your backend services work correctly...",
    "authorId": 1,
    "likes": 5,
    "dislikes": 1,
    "createdAt": "2024-11-02T12:00:00.000Z"
  },
  {
    "id": 2,
    "title": "Understanding REST APIs",
    "content": "REST is an architectural style for designing networked applications...",
    "authorId": 2,
    "likes": 3,
    "dislikes": 0,
    "createdAt": "2024-11-02T12:10:00.000Z"
  }
]
```

**Notes:**
- No authentication required for viewing posts
- Supports pagination with `page` and `limit` parameters
- Can filter by author with `authorId` query parameter

---

## 10. Get Post by ID

**Method:** GET  
**URL:** https://api.example.com/posts/1

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 1,
  "title": "Getting Started with API Testing",
  "content": "API testing is crucial for ensuring your backend services work correctly...",
  "authorId": 1,
  "likes": 5,
  "dislikes": 1,
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Error Response if not found:**
**Expected HTTP Status:** 404 Not Found

```json
{
  "error": "Post not found"
}
```

---

## 11. Edit Post

**Method:** PATCH  
**URL:** https://api.example.com/posts/1

**Headers:**
- Authorization: Bearer <valid_token>
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "title": "Getting Started with API Testing - Updated Guide",
  "content": "Updated content with more details..."
}
```

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 1,
  "title": "Getting Started with API Testing - Updated Guide",
  "content": "Updated content with more details...",
  "authorId": 1,
  "likes": 5,
  "dislikes": 1,
  "createdAt": "2024-11-02T12:00:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Users can only edit their own posts → 403 Forbidden otherwise
- Only updates the fields provided in the request body

---

## 12. Delete Post

**Method:** DELETE  
**URL:** https://api.example.com/posts/1

**Headers:**
- Authorization: Bearer <valid_token>

**Expected Response:**
- HTTP Status: 204 No Content
- Body: Empty (indicating successful deletion)

**Notes:**
- Users can only delete their own posts → 403 Forbidden otherwise
- Cascade delete: associated comments should also be deleted

---

## 13. Like Post

**Method:** POST  
**URL:** https://api.example.com/posts/1/like

**Headers:**
- Authorization: Bearer <valid_token>

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "message": "Post liked successfully",
  "likes": 6,
  "dislikes": 1
}
```

**Notes:**
- Requires valid JWT token
- Users cannot like their own posts → 400 Bad Request
- If already liked → 400 Bad Request or removes the like

---

## 14. Dislike Post

**Method:** POST  
**URL:** https://api.example.com/posts/1/dislike

**Headers:**
- Authorization: Bearer <valid_token>

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "message": "Post disliked successfully",
  "likes": 5,
  "dislikes": 2
}
```

**Notes:**
- Requires valid JWT token
- Users cannot dislike their own posts → 400 Bad Request
- If already disliked → 400 Bad Request or removes the dislike

---

## 15. Create Comment

**Method:** POST  
**URL:** https://api.example.com/posts/1/comments

**Headers:**
- Authorization: Bearer <valid_token>
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "content": "Great article! Very helpful."
}
```

**Expected Response:**
**Expected HTTP Status:** 201 Created

```json
{
  "id": 1,
  "content": "Great article! Very helpful.",
  "postId": 1,
  "authorId": 2,
  "likes": 0,
  "createdAt": "2024-11-02T12:15:00.000Z"
}
```

**Notes:**
- Requires valid JWT token
- postId is derived from the URL
- authorId is automatically set to the authenticated user's ID
- If content is missing → 400 Bad Request

---

## 16. Get Post Comments

**Method:** GET  
**URL:** https://api.example.com/posts/1/comments

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
[
  {
    "id": 1,
    "content": "Great article! Very helpful.",
    "postId": 1,
    "authorId": 2,
    "likes": 0,
    "createdAt": "2024-11-02T12:15:00.000Z"
  },
  {
    "id": 2,
    "content": "Thanks for sharing!",
    "postId": 1,
    "authorId": 3,
    "likes": 1,
    "createdAt": "2024-11-02T12:20:00.000Z"
  }
]
```

**Notes:**
- No authentication required for viewing comments
- Returns all comments for a specific post
- If post has no comments, returns an empty array []

---

## 17. Get Comment by ID

**Method:** GET  
**URL:** https://api.example.com/comments/1

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 1,
  "content": "Great article! Very helpful.",
  "postId": 1,
  "authorId": 2,
  "likes": 0,
  "createdAt": "2024-11-02T12:15:00.000Z"
}
```

**Error Response if not found:**
**Expected HTTP Status:** 404 Not Found

```json
{
  "error": "Comment not found"
}
```

---

## 18. Edit Comment

**Method:** PATCH  
**URL:** https://api.example.com/comments/1

**Headers:**
- Authorization: Bearer <valid_token>
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "content": "Great article! Very helpful. Thanks for writing it."
}
```

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 1,
  "content": "Great article! Very helpful. Thanks for writing it.",
  "postId": 1,
  "authorId": 2,
  "likes": 0,
  "createdAt": "2024-11-02T12:15:00.000Z",
  "updatedAt": "2024-11-02T12:35:00.000Z"
}
```

**Notes:**
- Users can only edit their own comments → 403 Forbidden otherwise
- Only updates the fields provided in the request body

---

## 19. Delete Comment

**Method:** DELETE  
**URL:** https://api.example.com/comments/1

**Headers:**
- Authorization: Bearer <valid_token>

**Expected Response:**
- HTTP Status: 204 No Content
- Body: Empty (indicating successful deletion)

**Notes:**
- Users can only delete their own comments → 403 Forbidden otherwise

---

## 20. React to Comment

**Method:** POST  
**URL:** https://api.example.com/comments/1/react

**Headers:**
- Authorization: Bearer <valid_token>
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "reaction": "like"
}
```

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "message": "Reaction added successfully",
  "likes": 1
}
```

**Notes:**
- Requires valid JWT token
- Supported reactions: like, dislike, love, laugh
- Users cannot react to their own comments → 400 Bad Request
