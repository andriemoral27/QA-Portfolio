# Act-3 Bookshelf API Documentation

## Overview
This document contains API requests for a Bookshelf application. These requests are designed to manage book-related resources including categories, authors, and books. Each resource supports full CRUD operations (Create, Read, Update, Delete). Each example includes the HTTP method, URL, headers, request body, expected HTTP status codes, and notes for testing in Postman.

## Table of Contents
1. [Categories](#categories)
   - 1.1 Create Category
   - 1.2 Get Categories by Start and End
   - 1.3 Get One Category
   - 1.4 Edit Category
   - 1.5 Delete Category
2. [Authors](#authors)
   - 2.1 Create Author
   - 2.2 Get Authors by Start and End
   - 2.3 Get One Author
   - 2.4 Edit Author
   - 2.5 Delete Author
3. [Books](#books)
   - 3.1 Create Book
   - 3.2 Get Books by Start and End
   - 3.3 Get Books by Categories
   - 3.4 Get One Book
   - 3.5 Edit Book
   - 3.6 Delete Book

---

## Categories

### 1.1 Create Category

**Method:** POST  
**URL:** `{{base_url}}/categories`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "name": "Fiction",
  "description": "Fictional books and novels"
}
```

**Expected Response:**

**HTTP Status:** 201 Created  
**Body:**
```json
{
  "id": 13,
  "name": "Fiction",
  "description": "Fictional books and novels",
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Creates a new book category
- Name must be unique -> 409 Conflict if name already exists
- If name is missing -> 400 Bad Request

---

### 1.2 Get Categories by Start and End

**Method:** GET  
**URL:** `{{base_url}}/categories?page=1&limit=10`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
[
  {
    "id": 13,
    "name": "Fiction",
    "description": "Fictional books and novels",
    "createdAt": "2024-11-02T12:00:00.000Z"
  },
  {
    "id": 14,
    "name": "Science",
    "description": "Scientific books and journals",
    "createdAt": "2024-11-02T12:05:00.000Z"
  }
]
```

**Notes:**
- Supports pagination with `page` and `limit` query parameters
- If no parameters provided, returns all categories
- If page exceeds available data, returns empty array []

---

### 1.3 Get One Category

**Method:** GET  
**URL:** `{{base_url}}/categories/{category_id}`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 13,
  "name": "Fiction",
  "description": "Fictional books and novels",
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Returns a single category by its ID
- If category ID does not exist -> 404 Not Found
- If category ID is invalid format -> 400 Bad Request

---

### 1.4 Edit Category

**Method:** PATCH  
**URL:** `{{base_url}}/categories/{category_id}`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "description": "American Comics"
}
```

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 21,
  "name": "Comics",
  "description": "American Comics",
  "createdAt": "2024-11-02T12:00:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Only updates the fields provided in the request body
- If category ID does not exist -> 404 Not Found
- If name conflicts with existing category -> 409 Conflict

---

### 1.5 Delete Category

**Method:** DELETE  
**URL:** `{{base_url}}/categories/{category_id}`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 204 No Content  
**Body:** Empty (indicating successful deletion)

**Notes:**
- Deleting a non-existent category should return 404 Not Found
- If category has associated books, may return 400 Bad Request or cascade delete (implementation dependent)
- Successful deletion returns 204 with no body

---

## Authors

### 2.1 Create Author

**Method:** POST  
**URL:** `{{base_url}}/authors`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "name": "J.K. Rowling",
  "biography": "British author, best known for Harry Potter series",
  "birthDate": "1965-07-31",
  "nationality": "British"
}
```

**Expected Response:**

**HTTP Status:** 201 Created  
**Body:**
```json
{
  "id": 14,
  "name": "J.K. Rowling",
  "biography": "British author, best known for Harry Potter series",
  "birthDate": "1965-07-31",
  "nationality": "British",
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Creates a new author record
- Name is required
- birthDate should be in ISO 8601 format (YYYY-MM-DD)

---

### 2.2 Get Authors by Start and End

**Method:** GET  
**URL:** `{{base_url}}/authors?page=1&limit=10`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
[
  {
    "id": 14,
    "name": "J.K. Rowling",
    "biography": "British author, best known for Harry Potter series",
    "birthDate": "1965-07-31",
    "nationality": "British",
    "createdAt": "2024-11-02T12:00:00.000Z"
  }
]
```

**Notes:**
- Supports pagination with `page` and `limit` query parameters
- Returns an array of authors
- If no authors exist, returns an empty array []

---

### 2.3 Get One Author

**Method:** GET  
**URL:** `{{base_url}}/authors/{author_id}`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 14,
  "name": "J.K. Rowling",
  "biography": "British author, best known for Harry Potter series",
  "birthDate": "1965-07-31",
  "nationality": "British",
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Returns a single author by their ID
- If author ID does not exist -> 404 Not Found

---

### 2.4 Edit Author

**Method:** PATCH  
**URL:** `{{base_url}}/authors/{author_id}`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "biography": "Updated biography with more details"
}
```

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 14,
  "name": "J.K. Rowling",
  "biography": "Updated biography with more details",
  "birthDate": "1965-07-31",
  "nationality": "British",
  "createdAt": "2024-11-02T12:00:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Only updates the fields provided in the request body
- If author ID does not exist -> 404 Not Found

---

### 2.5 Delete Author

**Method:** DELETE  
**URL:** `{{base_url}}/authors/{author_id}`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 204 No Content  
**Body:** Empty (indicating successful deletion)

**Notes:**
- Deleting a non-existent author should return 404 Not Found
- If author has associated books, may return 400 Bad Request or cascade delete (implementation dependent)
- Successful deletion returns 204 with no body

---

## Books

### 3.1 Create Book

**Method:** POST  
**URL:** `{{base_url}}/books`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "title": "Harry Potter and the Philosopher's Stone",
  "description": "The first book in the Harry Potter series",
  "isbn": "978-0-7475-3269-9",
  "publicationYear": 1997,
  "pageCount": 223,
  "price": 29.99,
  "isAvailable": true,
  "authorId": 15,
  "categoryId": 12
}
```

**Expected Response:**

**HTTP Status:** 201 Created  
**Body:**
```json
{
  "id": 25,
  "title": "Harry Potter and the Philosopher's Stone",
  "description": "The first book in the Harry Potter series",
  "isbn": "978-0-7475-3269-9",
  "publicationYear": 1997,
  "pageCount": 223,
  "price": 29.99,
  "isAvailable": true,
  "authorId": 15,
  "categoryId": 12,
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Creates a new book linked to an author and category
- authorId and categoryId must reference existing records
- If authorId or categoryId does not exist -> 400 Bad Request
- ISBN should be unique -> 409 Conflict if ISBN already exists

---

### 3.2 Get Books by Start and End

**Method:** GET  
**URL:** `{{base_url}}/books?page=1&limit=10`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
[
  {
    "id": 25,
    "title": "Harry Potter and the Philosopher's Stone",
    "description": "The first book in the Harry Potter series",
    "isbn": "978-0-7475-3269-9",
    "publicationYear": 1997,
    "pageCount": 223,
    "price": 29.99,
    "isAvailable": true,
    "authorId": 15,
    "categoryId": 12,
    "createdAt": "2024-11-02T12:00:00.000Z"
  }
]
```

**Notes:**
- Supports pagination with `page` and `limit` query parameters
- Returns an array of books
- If no books exist, returns an empty array []

---

### 3.3 Get Books by Categories

**Method:** GET  
**URL:** `{{base_url}}/categories/{category_id}?page=1&limit=10`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
[
  {
    "id": 25,
    "title": "Harry Potter and the Philosopher's Stone",
    "description": "The first book in the Harry Potter series",
    "isbn": "978-0-7475-3269-9",
    "publicationYear": 1997,
    "pageCount": 223,
    "price": 29.99,
    "isAvailable": true,
    "authorId": 15,
    "categoryId": 12,
    "createdAt": "2024-11-02T12:00:00.000Z"
  }
]
```

**Notes:**
- Returns all books belonging to a specific category
- Supports pagination with `page` and `limit` query parameters
- If category has no books, returns an empty array []

---

### 3.4 Get One Book

**Method:** GET  
**URL:** `{{base_url}}/books/{book_id}`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 25,
  "title": "Harry Potter and the Philosopher's Stone",
  "description": "The first book in the Harry Potter series",
  "isbn": "978-0-7475-3269-9",
  "publicationYear": 1997,
  "pageCount": 223,
  "price": 29.99,
  "isAvailable": true,
  "authorId": 15,
  "categoryId": 12,
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Returns a single book by its ID
- If book ID does not exist -> 404 Not Found

---

### 3.5 Edit Book

**Method:** PATCH  
**URL:** `{{base_url}}/books/{book_id}`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "price": 24.99,
  "isAvailable": false
}
```

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 25,
  "title": "Harry Potter and the Philosopher's Stone",
  "description": "The first book in the Harry Potter series",
  "isbn": "978-0-7475-3269-9",
  "publicationYear": 1997,
  "pageCount": 223,
  "price": 24.99,
  "isAvailable": false,
  "authorId": 15,
  "categoryId": 12,
  "createdAt": "2024-11-02T12:00:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Only updates the fields provided in the request body
- If book ID does not exist -> 404 Not Found
- Cannot change authorId or categoryId to non-existent values

---

### 3.6 Delete Book

**Method:** DELETE  
**URL:** `{{base_url}}/books/{book_id}`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 204 No Content  
**Body:** Empty (indicating successful deletion)

**Notes:**
- Deleting a non-existent book should return 404 Not Found
- Successful deletion returns 204 with no body
- Does not affect the associated author or category records

---

