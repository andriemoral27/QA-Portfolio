# Movie Review API Documentation

## Overview
This document contains API requests for a Movie Review application. These requests are designed to manage movies and their reviews. Each resource supports full CRUD operations (Create, Read, Update, Delete). Each example includes the HTTP method, URL, headers, request body, expected HTTP status codes, and notes for testing in Postman.

## Table of Contents
1. [Movies](#movies)
   - 1.1 Create Movie
   - 1.2 Get All Movies
   - 1.3 Get One Movie
   - 1.4 Edit Movie
   - 1.5 Delete Movie
2. [Reviews](#reviews)
   - 2.1 Create Review
   - 2.2 Get All Reviews
   - 2.3 Get One Review
   - 2.4 Edit Review
   - 2.5 Delete Review

---

## Movies

### 1.1 Create Movie

**Method:** POST  
**URL:** `{{base2_url}}/movies`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "title": "The Matrix",
  "description": "A computer hacker learns about the true nature of reality",
  "genre": "Sci-Fi",
  "imageUrl": "https://example.com/matrix.jpg",
  "releaseYear": 1999
}
```

**Expected Response:**

**HTTP Status:** 201 Created  
**Body:**
```json
{
  "id": 69,
  "title": "The Matrix",
  "description": "A computer hacker learns about the true nature of reality",
  "genre": "Sci-Fi",
  "imageUrl": "https://example.com/matrix.jpg",
  "releaseYear": 1999,
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Creates a new movie record
- Title, description, genre, and releaseYear are typically required
- imageUrl is optional
- If required fields are missing -> 400 Bad Request
- Title should be unique -> 409 Conflict if title already exists

---

### 1.2 Get All Movies

**Method:** GET  
**URL:** `{{base2_url}}/movies`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
[
  {
    "id": 69,
    "title": "The Matrix",
    "description": "A computer hacker learns about the true nature of reality",
    "genre": "Sci-Fi",
    "imageUrl": "https://example.com/matrix.jpg",
    "releaseYear": 1999,
    "createdAt": "2024-11-02T12:00:00.000Z"
  },
  {
    "id": 70,
    "title": "Inception",
    "description": "A thief who steals corporate secrets through dream-sharing",
    "genre": "Sci-Fi",
    "imageUrl": "https://example.com/inception.jpg",
    "releaseYear": 2010,
    "createdAt": "2024-11-02T12:05:00.000Z"
  }
]
```

**Notes:**
- Returns an array of all movies
- If no movies exist, returns an empty array []
- Movies may be sorted by release year or creation date

---

### 1.3 Get One Movie

**Method:** GET  
**URL:** `{{base2_url}}/movies/{movie_id}`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 69,
  "title": "The Matrix",
  "description": "A computer hacker learns about the true nature of reality",
  "genre": "Sci-Fi",
  "imageUrl": "https://example.com/matrix.jpg",
  "releaseYear": 1999,
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Returns a single movie by its ID
- If movie ID does not exist -> 404 Not Found
- If movie ID is invalid format -> 400 Bad Request

---

### 1.4 Edit Movie

**Method:** PATCH  
**URL:** `{{base2_url}}/movies/{movie_id}`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "title": "The Matrix Reloaded",
  "description": "Distopia",
  "releaseYear": 2003
}
```

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 76,
  "title": "The Matrix Reloaded",
  "description": "Distopia",
  "genre": "Sci-Fi",
  "imageUrl": "https://example.com/matrix.jpg",
  "releaseYear": 2003,
  "createdAt": "2024-11-02T12:00:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Only updates the fields provided in the request body
- If movie ID does not exist -> 404 Not Found
- If title conflicts with existing movie -> 409 Conflict
- Unchanged fields retain their original values

---

### 1.5 Delete Movie

**Method:** DELETE  
**URL:** `{{base2_url}}/movies/{movie_id}`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 204 No Content  
**Body:** Empty (indicating successful deletion)

**Notes:**
- Deleting a non-existent movie should return 404 Not Found
- If movie has associated reviews, may return 400 Bad Request or cascade delete (implementation dependent)
- Successful deletion returns 204 with no body
- Movie is permanently removed from the database

---

## Reviews

### 2.1 Create Review

**Method:** POST  
**URL:** `{{base2_url}}/reviews`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "reviewerName": "John Doe",
  "comment": "Great movie! Really enjoyed it.",
  "rating": 5,
  "movieId": 76
}
```

**Expected Response:**

**HTTP Status:** 201 Created  
**Body:**
```json
{
  "id": 135,
  "reviewerName": "John Doe",
  "comment": "Great movie! Really enjoyed it.",
  "rating": 5,
  "movieId": 76,
  "createdAt": "2024-11-02T12:15:00.000Z"
}
```

**Notes:**
- Creates a new review for a movie
- reviewerName, comment, rating, and movieId are required
- movieId must reference an existing movie
- If movieId does not exist -> 400 Bad Request
- Rating should typically be between 1-5 or 1-10 (implementation dependent)

---

### 2.2 Get All Reviews

**Method:** GET  
**URL:** `{{base2_url}}/reviews`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
[
  {
    "id": 135,
    "reviewerName": "John Doe",
    "comment": "Great movie! Really enjoyed it.",
    "rating": 5,
    "movieId": 76,
    "createdAt": "2024-11-02T12:15:00.000Z"
  },
  {
    "id": 136,
    "reviewerName": "Jane Smith",
    "comment": "Average movie, could be better.",
    "rating": 3,
    "movieId": 76,
    "createdAt": "2024-11-02T12:20:00.000Z"
  }
]
```

**Notes:**
- Returns an array of all reviews
- If no reviews exist, returns an empty array []
- May include movie details in the response (implementation dependent)

---

### 2.3 Get One Review

**Method:** GET  
**URL:** `{{base2_url}}/reviews/{review_id}`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 135,
  "reviewerName": "John Doe",
  "comment": "Great movie! Really enjoyed it.",
  "rating": 5,
  "movieId": 76,
  "createdAt": "2024-11-02T12:15:00.000Z"
}
```

**Notes:**
- Returns a single review by its ID
- If review ID does not exist -> 404 Not Found
- If review ID is invalid format -> 400 Bad Request

---

### 2.4 Edit Review

**Method:** PATCH  
**URL:** `{{base2_url}}/reviews/{review_id}`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "comment": "Updated review comment",
  "rating": 4
}
```

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 135,
  "reviewerName": "John Doe",
  "comment": "Updated review comment",
  "rating": 4,
  "movieId": 76,
  "createdAt": "2024-11-02T12:15:00.000Z",
  "updatedAt": "2024-11-02T12:35:00.000Z"
}
```

**Notes:**
- Only updates the fields provided in the request body
- If review ID does not exist -> 404 Not Found
- Cannot change movieId (implementation dependent)
- Reviewer name typically cannot be changed

---

### 2.5 Delete Review

**Method:** DELETE  
**URL:** `{{base2_url}}/reviews/{review_id}`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 204 No Content  
**Body:** Empty (indicating successful deletion)

**Notes:**
- Deleting a non-existent review should return 404 Not Found
- Successful deletion returns 204 with no body
- Review is permanently removed from the database
- Does not affect the associated movie record

---

## Common Status Codes

| Status Code | Description |
|-------------|-------------|
| 200 OK | Request succeeded (GET, PATCH) |
| 201 Created | Resource successfully created (POST) |
| 204 No Content | Successful deletion (DELETE) |
| 400 Bad Request | Invalid request data or format |
| 404 Not Found | Resource not found |
| 409 Conflict | Resource already exists (duplicate title) |
| 500 Internal Server Error | Server error occurred |

## Request/Response Fields

### Movie Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | number | Auto | Unique identifier for the movie |
| title | string | Yes | Title of the movie |
| description | string | Yes | Brief description of the movie |
| genre | string | Yes | Genre of the movie (e.g., Sci-Fi, Drama) |
| imageUrl | string | No | URL to movie poster/image |
| releaseYear | number | Yes | Year the movie was released |
| createdAt | string | Auto | ISO 8601 timestamp of creation |
| updatedAt | string | Auto | ISO 8601 timestamp of last update |

### Review Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | number | Auto | Unique identifier for the review |
| reviewerName | string | Yes | Name of the reviewer |
| comment | string | Yes | Review comment/text |
| rating | number | Yes | Rating score (typically 1-5 or 1-10) |
| movieId | number | Yes | ID of the movie being reviewed |
| createdAt | string | Auto | ISO 8601 timestamp of creation |
| updatedAt | string | Auto | ISO 8601 timestamp of last update |

## Testing in Postman

1. Import the Postman collection: `Movie Review.postman_collection.json`
2. Set the `base2_url` variable in Postman environment or collection variables
3. Create movies first (reviews require movie IDs)
4. Test GET endpoints to retrieve movie data
5. Create reviews using the movie IDs
6. Test review CRUD operations
7. Test PATCH endpoints to update data
8. Test DELETE endpoints to remove data
9. Verify response status codes match expected values
10. Test error scenarios:
    - Create review with non-existent movieId (should return 400)
    - Get/delete non-existent IDs (should return 404)
    - Create movie with duplicate title (should return 409)
11. Test rating validation (e.g., rating outside valid range)
12. Verify that deleting a movie either cascades to reviews or prevents deletion (implementation dependent)
