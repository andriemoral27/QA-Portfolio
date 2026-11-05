# Blog Platform API Documentation

## Overview
This document contains API requests for a Blog Platform application with authentication. These requests are designed to manage user accounts, blog posts, comments, and reactions (likes/dislikes). Each example includes the HTTP method, URL, headers, request body, expected HTTP status codes, and notes for testing in Postman.

## Table of Contents
1. [Authentication](#authentication)
   - 1.1 Register
   - 1.2 Login
2. [Users](#users)
   - 2.1 Get All Users
   - 2.2 Get Users with Pagination
   - 2.3 Get One User
   - 2.4 Delete User
3. [Posts](#posts)
   - 3.1 Create Post
   - 3.2 Get Posts with Pagination
   - 3.3 Get One Post
   - 3.4 Edit Post
   - 3.5 Delete Post
   - 3.6 Like Post
   - 3.7 Dislike Post
4. [Comments](#comments)
   - 4.1 Create Comment on Post
   - 4.2 Get Comments on Post with Pagination
   - 4.3 Edit Comment
   - 4.4 Delete Comment
   - 4.5 Like Comment
   - 4.6 Dislike Comment
   - 4.7 Get All Reactions

---

## Authentication

### 1.1 Register

**Method:** POST  
**URL:** `{{base_url}}/auth/register`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "email": "john@example.com",
  "name": "John Doe",
  "password": "password123"
}
```

**Expected Response:**

**HTTP Status:** 201 Created  
**Body:**
```json
{
  "message": "User registered successfully",
  "user": {
    "id": 18,
    "email": "john@example.com",
    "name": "John Doe"
  }
}
```

**Notes:**
- Creates a new user account
- Email must be unique -> 409 Conflict if email already exists
- Name and password are required fields
- If any required field is missing -> 400 Bad Request

---

### 1.2 Login

**Method:** POST  
**URL:** `{{base_url}}/auth/login`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "email": "john@example.com",
  "password": "password123"
}
```

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOjE4LCJlbWFpbCI6ImpvaG5AZXhhbXBsZS5jb20iLCJpYXQiOjE3Njg2NTYzMDcsImV4cCI6MTc2OTI2MTEwN30.fdEDykApAreSrshblP55vL2EUbaZgJM7oKil37_w0lY",
  "user": {
    "id": 18,
    "email": "john@example.com",
    "name": "John Doe"
  }
}
```

**Notes:**
- Returns a JWT token for authentication
- Token should be used in Authorization header for protected routes
- If credentials are invalid -> 401 Unauthorized
- Save the token for subsequent authenticated requests

---

## Users

### 2.1 Get All Users

**Method:** GET  
**URL:** `{{base_url}}/users`

**Headers:**
- Authorization: Bearer <valid_token>

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
[
  {
    "id": 18,
    "email": "john@example.com",
    "name": "John Doe",
    "createdAt": "2024-11-02T12:00:00.000Z"
  },
  {
    "id": 19,
    "email": "jane@example.com",
    "name": "Jane Smith",
    "createdAt": "2024-11-02T12:05:00.000Z"
  }
]
```

**Notes:**
- Requires valid JWT token in Authorization header
- If token is missing or invalid -> 401 Unauthorized
- Returns an array of all registered users
- If no users exist, returns an empty array []

---

### 2.2 Get Users with Pagination

**Method:** GET  
**URL:** `{{base_url}}/users?page=1&limit=10`

**Headers:**
- Authorization: Bearer <valid_token>

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
[
  {
    "id": 18,
    "email": "john@example.com",
    "name": "John Doe",
    "createdAt": "2024-11-02T12:00:00.000Z"
  }
]
```

**Notes:**
- Requires valid JWT token in Authorization header
- Supports pagination with `page` and `limit` query parameters
- If page exceeds available data, returns empty array []

---

### 2.3 Get One User

**Method:** GET  
**URL:** `{{base_url}}/users/{user_id}`

**Headers:**
- Authorization: Bearer <valid_token>

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 19,
  "email": "jane@example.com",
  "name": "Jane Smith",
  "createdAt": "2024-11-02T12:05:00.000Z"
}
```

**Notes:**
- Requires valid JWT token in Authorization header
- If user ID does not exist -> 404 Not Found
- Returns user profile information

---

### 2.4 Delete User

**Method:** DELETE  
**URL:** `{{base_url}}/users/{user_id}`

**Headers:**
- Authorization: Bearer <valid_token>

**Request Body:** None

**Expected Response:**

**HTTP Status:** 204 No Content  
**Body:** Empty (indicating successful deletion)

**Notes:**
- Requires valid JWT token in Authorization header
- If user ID does not exist -> 404 Not Found
- Users can typically only delete their own account (403 Forbidden otherwise)
- Successful deletion returns 204 with no body
- May cascade delete associated posts and comments

---

## Posts

### 3.1 Create Post

**Method:** POST  
**URL:** `{{base_url}}/posts`

**Headers:**
- Authorization: Bearer <valid_token>
- Content-Type: application/json

**Request Body:**
```json
{
  "title": "My First Post",
  "content": "This is the content of my first post!"
}
```

**Expected Response:**

**HTTP Status:** 201 Created  
**Body:**
```json
{
  "id": 30,
  "title": "My First Post",
  "content": "This is the content of my first post!",
  "userId": 18,
  "likes": 0,
  "dislikes": 0,
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Requires valid JWT token in Authorization header
- Creates a post associated with the authenticated user
- Title and content are required fields
- If fields are missing -> 400 Bad Request

---

### 3.2 Get Posts with Pagination

**Method:** GET  
**URL:** `{{base_url}}/posts?page=1&limit=10`

**Headers:**
- Authorization: Bearer <valid_token>

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
[
  {
    "id": 30,
    "title": "My First Post",
    "content": "This is the content of my first post!",
    "userId": 18,
    "likes": 5,
    "dislikes": 1,
    "createdAt": "2024-11-02T12:00:00.000Z"
  }
]
```

**Notes:**
- Requires valid JWT token in Authorization header
- Supports pagination with `page` and `limit` query parameters
- Returns an array of posts
- If no posts exist, returns an empty array []

---

### 3.3 Get One Post

**Method:** GET  
**URL:** `{{base_url}}/posts/{post_id}`

**Headers:**
- Authorization: Bearer <valid_token>

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 30,
  "title": "My First Post",
  "content": "This is the content of my first post!",
  "userId": 18,
  "likes": 5,
  "dislikes": 1,
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Requires valid JWT token in Authorization header
- If post ID does not exist -> 404 Not Found
- Returns post details including like/dislike counts

---

### 3.4 Edit Post

**Method:** PATCH  
**URL:** `{{base_url}}/posts/{post_id}`

**Headers:**
- Authorization: Bearer <valid_token>
- Content-Type: application/json

**Request Body:**
```json
{
  "title": "Updated Post Title",
  "content": "Updated content"
}
```

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 30,
  "title": "Updated Post Title",
  "content": "Updated content",
  "userId": 18,
  "likes": 5,
  "dislikes": 1,
  "createdAt": "2024-11-02T12:00:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Requires valid JWT token in Authorization header
- Only updates fields provided in the request body
- If post ID does not exist -> 404 Not Found
- Users can only edit their own posts (403 Forbidden otherwise)

---

### 3.5 Delete Post

**Method:** DELETE  
**URL:** `{{base_url}}/posts/{post_id}`

**Headers:**
- Authorization: Bearer <valid_token>

**Request Body:** None

**Expected Response:**

**HTTP Status:** 204 No Content  
**Body:** Empty (indicating successful deletion)

**Notes:**
- Requires valid JWT token in Authorization header
- If post ID does not exist -> 404 Not Found
- Users can only delete their own posts (403 Forbidden otherwise)
- Successful deletion returns 204 with no body
- May cascade delete associated comments

---

### 3.6 Like Post

**Method:** POST  
**URL:** `{{base_url}}/posts/{post_id}/like`

**Headers:**
- Authorization: Bearer <valid_token>

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "message": "Post liked successfully",
  "likes": 6,
  "dislikes": 1
}
```

**Notes:**
- Requires valid JWT token in Authorization header
- Increments the like count for the post
- If user already liked, may toggle or return error (implementation dependent)
- If post ID does not exist -> 404 Not Found

---

### 3.7 Dislike Post

**Method:** POST  
**URL:** `{{base_url}}/posts/{post_id}/dislike`

**Headers:**
- Authorization: Bearer <valid_token>

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "message": "Post disliked successfully",
  "likes": 5,
  "dislikes": 2
}
```

**Notes:**
- Requires valid JWT token in Authorization header
- Increments the dislike count for the post
- If user already disliked, may toggle or return error (implementation dependent)
- If post ID does not exist -> 404 Not Found

---

## Comments

### 4.1 Create Comment on Post

**Method:** POST  
**URL:** `{{base_url}}/posts/{post_id}/comments`

**Headers:**
- Authorization: Bearer <valid_token>
- Content-Type: application/json

**Request Body:**
```json
{
  "content": "Great post! Thanks for sharing."
}
```

**Expected Response:**

**HTTP Status:** 201 Created  
**Body:**
```json
{
  "id": 22,
  "content": "Great post! Thanks for sharing.",
  "postId": 30,
  "userId": 18,
  "likes": 0,
  "dislikes": 0,
  "createdAt": "2024-11-02T12:15:00.000Z"
}
```

**Notes:**
- Requires valid JWT token in Authorization header
- Creates a comment associated with the post and authenticated user
- Content is required field
- If post ID does not exist -> 404 Not Found

---

### 4.2 Get Comments on Post with Pagination

**Method:** GET  
**URL:** `{{base_url}}/posts/{post_id}/comments?page=1&limit=10`

**Headers:**
- Authorization: Bearer <valid_token>

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
[
  {
    "id": 22,
    "content": "Great post! Thanks for sharing.",
    "postId": 30,
    "userId": 18,
    "likes": 1,
    "dislikes": 0,
    "createdAt": "2024-11-02T12:15:00.000Z"
  }
]
```

**Notes:**
- Requires valid JWT token in Authorization header
- Returns comments for a specific post
- Supports pagination with `page` and `limit` query parameters
- If post has no comments, returns an empty array []

---

### 4.3 Edit Comment

**Method:** PATCH  
**URL:** `{{base_url}}/posts/{post_id}/comments/{comment_id}`

**Headers:**
- Authorization: Bearer <valid_token>
- Content-Type: application/json

**Request Body:**
```json
{
  "content": "Updated comment text"
}
```

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 22,
  "content": "Updated comment text",
  "postId": 30,
  "userId": 18,
  "likes": 1,
  "dislikes": 0,
  "createdAt": "2024-11-02T12:15:00.000Z",
  "updatedAt": "2024-11-02T12:35:00.000Z"
}
```

**Notes:**
- Requires valid JWT token in Authorization header
- Only updates the content field
- If comment ID does not exist -> 404 Not Found
- Users can only edit their own comments (403 Forbidden otherwise)

---

### 4.4 Delete Comment

**Method:** DELETE  
**URL:** `{{base_url}}/posts/{post_id}/comments/{comment_id}`

**Headers:**
- Authorization: Bearer <valid_token>

**Request Body:** None

**Expected Response:**

**HTTP Status:** 204 No Content  
**Body:** Empty (indicating successful deletion)

**Notes:**
- Requires valid JWT token in Authorization header
- If comment ID does not exist -> 404 Not Found
- Users can only delete their own comments (403 Forbidden otherwise)
- Successful deletion returns 204 with no body

---

### 4.5 Like Comment

**Method:** POST  
**URL:** `{{base_url}}/posts/{post_id}/comments/{comment_id}/like`

**Headers:**
- Authorization: Bearer <valid_token>

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "message": "Comment liked successfully",
  "likes": 2,
  "dislikes": 0
}
```

**Notes:**
- Requires valid JWT token in Authorization header
- Increments the like count for the comment
- If comment ID does not exist -> 404 Not Found

---

### 4.6 Dislike Comment

**Method:** POST  
**URL:** `{{base_url}}/posts/{post_id}/comments/{comment_id}/dislike`

**Headers:**
- Authorization: Bearer <valid_token>

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "message": "Comment disliked successfully",
  "likes": 1,
  "dislikes": 1
}
```

**Notes:**
- Requires valid JWT token in Authorization header
- Increments the dislike count for the comment
- If comment ID does not exist -> 404 Not Found

---

### 4.7 Get All Reactions

**Method:** GET  
**URL:** `{{base_url}}/posts/{post_id}/comments/{comment_id}/reaction`

**Headers:**
- Authorization: Bearer <valid_token>

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "commentId": 22,
  "likes": 2,
  "dislikes": 1,
  "reactions": [
    {
      "userId": 18,
      "type": "like",
      "createdAt": "2024-11-02T12:20:00.000Z"
    },
    {
      "userId": 19,
      "type": "dislike",
      "createdAt": "2024-11-02T12:25:00.000Z"
    }
  ]
}
```

**Notes:**
- Requires valid JWT token in Authorization header
- Returns detailed reaction information for a comment
- Shows which users liked/disliked and when
- If comment ID does not exist -> 404 Not Found

---

## Common Status Codes

| Status Code | Description |
|-------------|-------------|
| 200 OK | Request succeeded (GET, POST for reactions) |
| 201 Created | Resource successfully created (POST) |
| 204 No Content | Successful deletion (DELETE) |
| 400 Bad Request | Invalid request data or format |
| 401 Unauthorized | Missing or invalid authentication token |
| 403 Forbidden | User does not have permission to access resource |
| 404 Not Found | Resource not found |
| 409 Conflict | Resource already exists (e.g., duplicate email) |
| 500 Internal Server Error | Server error occurred |

## Authentication

All protected endpoints require authentication using a JWT token in the Authorization header:

```
Authorization: Bearer <your_jwt_token>
```

The token is obtained from the Login endpoint and typically expires after a set time period.

## Testing in Postman

1. Import the Postman collection: `Blog Platform.postman_collection.json`
2. Set the `base_url` variable in Postman environment or collection variables
3. First, register a new user using the Register endpoint
4. Login to obtain the JWT token
5. Copy the token and set it in the Authorization header for protected endpoints
6. Test user endpoints: Get All Users, Get Users with Pagination, Get One User
7. Create posts and test post CRUD operations
8. Add comments to posts and test comment operations
9. Test like/dislike functionality for both posts and comments
10. Verify response status codes match expected values
11. Test authentication by sending requests without the token (should return 401)
12. Test authorization by trying to edit/delete other users' content (should return 403)
