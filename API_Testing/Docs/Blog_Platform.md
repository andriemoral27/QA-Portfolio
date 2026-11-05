# Blog Platform API Testing

## Overview
This document contains API test cases for a Blog Platform application with JWT authentication. These test cases validate user management, blog posts, comments, and reactions (likes/dislikes). Each test case includes the HTTP method, endpoint, headers, request body, expected HTTP status codes, and response validation criteria.

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

**Test Case:** Create a new user account

**HTTP Method:** POST  
**Endpoint:** `/auth/register`

**Headers:**
```
Content-Type: application/json
```

**Request Body:**
```json
{
  "email": "john@example.com",
  "name": "John Doe",
  "password": "password123"
}
```

**Expected Response:**

**HTTP Status Code:** 201 Created

**Response Body:**
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

**Validation Criteria:**
- Response status code is 201
- Response body contains success message and user object
- Email must be unique → 409 Conflict if duplicate

---

### 1.2 Login

**Test Case:** Authenticate user and receive JWT token

**HTTP Method:** POST  
**Endpoint:** `/auth/login`

**Headers:**
```
Content-Type: application/json
```

**Request Body:**
```json
{
  "email": "john@example.com",
  "password": "password123"
}
```

**Expected Response:**

**HTTP Status Code:** 200 OK

**Response Body:**
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

**Validation Criteria:**
- Response status code is 200
- Response body contains valid JWT token
- Invalid credentials → 401 Unauthorized

---

## Users

### 2.1 Get All Users

**Test Case:** Retrieve all registered users (requires authentication)

**HTTP Method:** GET  
**Endpoint:** `/users`

**Headers:**
```
Authorization: Bearer <valid_token>
Accept: application/json
```

**Request Body:** None

**Expected Response:**

**HTTP Status Code:** 200 OK

**Response Body:**
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

**Validation Criteria:**
- Response status code is 200
- Response body is an array
- Missing/invalid token → 401 Unauthorized

---

### 2.2 Get Users with Pagination

**Test Case:** Retrieve users with pagination

**HTTP Method:** GET  
**Endpoint:** `/users?page=1&limit=10`

**Headers:**
```
Authorization: Bearer <valid_token>
Accept: application/json
```

**Request Body:** None

**Expected Response:**

**HTTP Status Code:** 200 OK

**Response Body:**
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

**Validation Criteria:**
- Response status code is 200
- Pagination parameters are respected

---

### 2.3 Get One User

**Test Case:** Retrieve a specific user by ID

**HTTP Method:** GET  
**Endpoint:** `/users/{user_id}`

**Headers:**
```
Authorization: Bearer <valid_token>
Accept: application/json
```

**Request Body:** None

**Expected Response:**

**HTTP Status Code:** 200 OK

**Response Body:**
```json
{
  "id": 19,
  "email": "jane@example.com",
  "name": "Jane Smith",
  "createdAt": "2024-11-02T12:05:00.000Z"
}
```

**Validation Criteria:**
- Response status code is 200
- `id` matches requested `user_id`
- Non-existent ID → 404 Not Found

---

### 2.4 Delete User

**Test Case:** Delete a user account

**HTTP Method:** DELETE  
**Endpoint:** `/users/{user_id}`

**Headers:**
```
Authorization: Bearer <valid_token>
Accept: application/json
```

**Request Body:** None

**Expected Response:**

**HTTP Status Code:** 204 No Content

**Response Body:** Empty

**Validation Criteria:**
- Response status code is 204
- Response body is empty
- Users can typically only delete their own account → 403 Forbidden otherwise

---

## Posts

### 3.1 Create Post

**Test Case:** Create a new blog post

**HTTP Method:** POST  
**Endpoint:** `/posts`

**Headers:**
```
Authorization: Bearer <valid_token>
Content-Type: application/json
```

**Request Body:**
```json
{
  "title": "My First Post",
  "content": "This is the content of my first post!"
}
```

**Expected Response:**

**HTTP Status Code:** 201 Created

**Response Body:**
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

**Validation Criteria:**
- Response status code is 201
- `userId` matches authenticated user
- Title and content are required

---

### 3.2 Get Posts with Pagination

**Test Case:** Retrieve posts with pagination

**HTTP Method:** GET  
**Endpoint:** `/posts?page=1&limit=10`

**Headers:**
```
Authorization: Bearer <valid_token>
Accept: application/json
```

**Request Body:** None

**Expected Response:**

**HTTP Status Code:** 200 OK

**Response Body:**
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

**Validation Criteria:**
- Response status code is 200
- Response body is an array
- Pagination parameters are respected

---

### 3.3 Get One Post

**Test Case:** Retrieve a specific post by ID

**HTTP Method:** GET  
**Endpoint:** `/posts/{post_id}`

**Headers:**
```
Authorization: Bearer <valid_token>
Accept: application/json
```

**Request Body:** None

**Expected Response:**

**HTTP Status Code:** 200 OK

**Response Body:**
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

**Validation Criteria:**
- Response status code is 200
- `id` matches requested `post_id`
- Non-existent ID → 404 Not Found

---

### 3.4 Edit Post

**Test Case:** Update an existing post

**HTTP Method:** PATCH  
**Endpoint:** `/posts/{post_id}`

**Headers:**
```
Authorization: Bearer <valid_token>
Content-Type: application/json
```

**Request Body:**
```json
{
  "title": "Updated Post Title",
  "content": "Updated content"
}
```

**Expected Response:**

**HTTP Status Code:** 200 OK

**Response Body:**
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

**Validation Criteria:**
- Response status code is 200
- Only provided fields are updated
- Users can only edit their own posts → 403 Forbidden otherwise

---

### 3.5 Delete Post

**Test Case:** Delete a post

**HTTP Method:** DELETE  
**Endpoint:** `/posts/{post_id}`

**Headers:**
```
Authorization: Bearer <valid_token>
Accept: application/json
```

**Request Body:** None

**Expected Response:**

**HTTP Status Code:** 204 No Content

**Response Body:** Empty

**Validation Criteria:**
- Response status code is 204
- Users can only delete their own posts → 403 Forbidden otherwise

---

### 3.6 Like Post

**Test Case:** Like a post

**HTTP Method:** POST  
**Endpoint:** `/posts/{post_id}/like`

**Headers:**
```
Authorization: Bearer <valid_token>
```

**Request Body:** None

**Expected Response:**

**HTTP Status Code:** 200 OK

**Response Body:**
```json
{
  "message": "Post liked successfully",
  "likes": 6,
  "dislikes": 1
}
```

**Validation Criteria:**
- Response status code is 200
- Like count increments by 1
- Non-existent post → 404 Not Found

---

### 3.7 Dislike Post

**Test Case:** Dislike a post

**HTTP Method:** POST  
**Endpoint:** `/posts/{post_id}/dislike`

**Headers:**
```
Authorization: Bearer <valid_token>
```

**Request Body:** None

**Expected Response:**

**HTTP Status Code:** 200 OK

**Response Body:**
```json
{
  "message": "Post disliked successfully",
  "likes": 5,
  "dislikes": 2
}
```

**Validation Criteria:**
- Response status code is 200
- Dislike count increments by 1

---

## Comments

### 4.1 Create Comment on Post

**Test Case:** Add a comment to a post

**HTTP Method:** POST  
**Endpoint:** `/posts/{post_id}/comments`

**Headers:**
```
Authorization: Bearer <valid_token>
Content-Type: application/json
```

**Request Body:**
```json
{
  "content": "Great post! Thanks for sharing."
}
```

**Expected Response:**

**HTTP Status Code:** 201 Created

**Response Body:**
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

**Validation Criteria:**
- Response status code is 201
- `postId` matches the post
- `userId` matches authenticated user

---

### 4.2 Get Comments on Post with Pagination

**Test Case:** Retrieve comments for a post

**HTTP Method:** GET  
**Endpoint:** `/posts/{post_id}/comments?page=1&limit=10`

**Headers:**
```
Authorization: Bearer <valid_token>
Accept: application/json
```

**Request Body:** None

**Expected Response:**

**HTTP Status Code:** 200 OK

**Response Body:**
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

**Validation Criteria:**
- Response status code is 200
- All comments belong to the specified post

---

### 4.3 Edit Comment

**Test Case:** Update a comment

**HTTP Method:** PATCH  
**Endpoint:** `/posts/{post_id}/comments/{comment_id}`

**Headers:**
```
Authorization: Bearer <valid_token>
Content-Type: application/json
```

**Request Body:**
```json
{
  "content": "Updated comment text"
}
```

**Expected Response:**

**HTTP Status Code:** 200 OK

**Response Body:**
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

**Validation Criteria:**
- Response status code is 200
- Users can only edit their own comments → 403 Forbidden otherwise

---

### 4.4 Delete Comment

**Test Case:** Delete a comment

**HTTP Method:** DELETE  
**Endpoint:** `/posts/{post_id}/comments/{comment_id}`

**Headers:**
```
Authorization: Bearer <valid_token>
Accept: application/json
```

**Request Body:** None

**Expected Response:**

**HTTP Status Code:** 204 No Content

**Response Body:** Empty

**Validation Criteria:**
- Response status code is 204
- Users can only delete their own comments → 403 Forbidden otherwise

---

### 4.5 Like Comment

**Test Case:** Like a comment

**HTTP Method:** POST  
**Endpoint:** `/posts/{post_id}/comments/{comment_id}/like`

**Headers:**
```
Authorization: Bearer <valid_token>
```

**Request Body:** None

**Expected Response:**

**HTTP Status Code:** 200 OK

**Response Body:**
```json
{
  "message": "Comment liked successfully",
  "likes": 2,
  "dislikes": 0
}
```

**Validation Criteria:**
- Response status code is 200
- Like count increments by 1

---

### 4.6 Dislike Comment

**Test Case:** Dislike a comment

**HTTP Method:** POST  
**Endpoint:** `/posts/{post_id}/comments/{comment_id}/dislike`

**Headers:**
```
Authorization: Bearer <valid_token>
```

**Request Body:** None

**Expected Response:**

**HTTP Status Code:** 200 OK

**Response Body:**
```json
{
  "message": "Comment disliked successfully",
  "likes": 1,
  "dislikes": 1
}
```

**Validation Criteria:**
- Response status code is 200
- Dislike count increments by 1

---

### 4.7 Get All Reactions

**Test Case:** Retrieve all reactions for a comment

**HTTP Method:** GET  
**Endpoint:** `/posts/{post_id}/comments/{comment_id}/reaction`

**Headers:**
```
Authorization: Bearer <valid_token>
Accept: application/json
```

**Request Body:** None

**Expected Response:**

**HTTP Status Code:** 200 OK

**Response Body:**
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

**Validation Criteria:**
- Response status code is 200
- Returns detailed reaction information
- Shows which users liked/disliked

---

## Test Execution

### Using cURL
```bash
# Register
curl -X POST http://api.example.com/auth/register \
  -H "Content-Type: application/json" \
  -d '{"email":"john@example.com","name":"John Doe","password":"password123"}'

# Login
curl -X POST http://api.example.com/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"john@example.com","password":"password123"}'

# Create Post
curl -X POST http://api.example.com/posts \
  -H "Authorization: Bearer TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"title":"My First Post","content":"This is the content of my first post!"}'

# Like Post
curl -X POST http://api.example.com/posts/30/like \
  -H "Authorization: Bearer TOKEN"

# Create Comment
curl -X POST http://api.example.com/posts/30/comments \
  -H "Authorization: Bearer TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"content":"Great post! Thanks for sharing."}'
```

### Using HTTPie
```bash
# Register
http POST http://api.example.com/auth/register \
  email="john@example.com" name="John Doe" password="password123"

# Login
http POST http://api.example.com/auth/login \
  email="john@example.com" password="password123"

# Create Post
http POST http://api.example.com/posts \
  Authorization:"Bearer TOKEN" \
  title="My First Post" content="This is the content of my first post!"

# Like Post
http POST http://api.example.com/posts/30/like \
  Authorization:"Bearer TOKEN"

# Create Comment
http POST http://api.example.com/posts/30/comments \
  Authorization:"Bearer TOKEN" \
  content="Great post! Thanks for sharing."
```

---

## Data Schema

### User Object
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | integer | Auto-generated | Unique identifier |
| email | string | Yes | User email (unique) |
| name | string | Yes | User name |
| createdAt | string (ISO 8601) | Auto-generated | Creation timestamp |

### Post Object
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | integer | Auto-generated | Unique identifier |
| title | string | Yes | Post title |
| content | string | Yes | Post content |
| userId | integer | Auto-generated | Author ID |
| likes | integer | Auto | Like count |
| dislikes | integer | Auto | Dislike count |
| createdAt | string (ISO 8601) | Auto-generated | Creation timestamp |
| updatedAt | string (ISO 8601) | Auto-generated | Update timestamp |

### Comment Object
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | integer | Auto-generated | Unique identifier |
| content | string | Yes | Comment text |
| postId | integer | Auto-generated | Post ID |
| userId | integer | Auto-generated | Commenter ID |
| likes | integer | Auto | Like count |
| dislikes | integer | Auto | Dislike count |
| createdAt | string (ISO 8601) | Auto-generated | Creation timestamp |
| updatedAt | string (ISO 8601) | Auto-generated | Update timestamp |

---

## Authentication

All protected endpoints require authentication using a JWT token in the Authorization header:

```
Authorization: Bearer <your_jwt_token>
```

The token is obtained from the Login endpoint and typically expires after a set time period.
