# 📄 Bookshelf API 

## Table of Contents
1. [Create Category](#1-create-category)
2. [Get Categories with Pagination](#2-get-categories-with-pagination)
3. [Get One Category](#3-get-one-category)
4. [Edit Category](#4-edit-category)
5. [Delete Category](#5-delete-category)
6. [Create Author](#6-create-author)
7. [Get Authors with Pagination](#7-get-authors-with-pagination)
8. [Get One Author](#8-get-one-author)
9. [Edit Author](#9-edit-author)
10. [Delete Author](#10-delete-author)
11. [Create Book](#11-create-book)
12. [Get Books with Pagination](#12-get-books-with-pagination)
13. [Get Books by Category](#13-get-books-by-category)
14. [Get One Book](#14-get-one-book)
15. [Edit Book](#15-edit-book)
16. [Delete Book](#16-delete-book)

## Overview
This document contains API requests for a Bookshelf application. These requests are designed to manage book-related resources including categories, authors, and books. Each resource supports full CRUD operations. Each example includes method, URL, headers, request body, expected HTTP status, and response structure.

---

## 1. Create Category

**Method:** POST  
**URL:** https://api.example.com/categories

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "name": "Fiction",
  "description": "Fictional books and novels"
}
```

**Expected Response:**
**Expected HTTP Status:** 201 Created

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
- Name must be unique → 409 Conflict if name already exists
- If name is missing → 400 Bad Request

---

## 2. Get Categories with Pagination

**Method:** GET  
**URL:** https://api.example.com/categories?page=1&limit=10

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

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

---

## 3. Get One Category

**Method:** GET  
**URL:** https://api.example.com/categories/13

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 13,
  "name": "Fiction",
  "description": "Fictional books and novels",
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Error Response if not found:**
**Expected HTTP Status:** 404 Not Found

```json
{
  "error": "Category not found"
}
```

---

## 4. Edit Category

**Method:** PATCH  
**URL:** https://api.example.com/categories/21

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "description": "American Comics"
}
```

**Expected Response:**
**Expected HTTP Status:** 200 OK

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
- If category ID does not exist → 404 Not Found

---

## 5. Delete Category

**Method:** DELETE  
**URL:** https://api.example.com/categories/24

**Headers:**
- Authorization: Bearer <valid_token>

**Expected Response:**
- HTTP Status: 204 No Content
- Body: Empty (indicating successful deletion)

**Notes:**
- If category has associated books, may return 400 Bad Request or cascade delete

---

## 6. Create Author

**Method:** POST  
**URL:** https://api.example.com/authors

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "name": "J.K. Rowling",
  "biography": "British author, best known for Harry Potter series",
  "birthDate": "1965-07-31",
  "nationality": "British"
}
```

**Expected Response:**
**Expected HTTP Status:** 201 Created

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
- birthDate should be in ISO 8601 format (YYYY-MM-DD)

---

## 7. Get Authors with Pagination

**Method:** GET  
**URL:** https://api.example.com/authors?page=1&limit=10

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

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

---

## 8. Get One Author

**Method:** GET  
**URL:** https://api.example.com/authors/14

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

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

**Error Response if not found:**
**Expected HTTP Status:** 404 Not Found

```json
{
  "error": "Author not found"
}
```

---

## 9. Edit Author

**Method:** PATCH  
**URL:** https://api.example.com/authors/14

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "biography": "Updated biography with more details"
}
```

**Expected Response:**
**Expected HTTP Status:** 200 OK

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

---

## 10. Delete Author

**Method:** DELETE  
**URL:** https://api.example.com/authors/14

**Headers:**
- Authorization: Bearer <valid_token>

**Expected Response:**
- HTTP Status: 204 No Content
- Body: Empty (indicating successful deletion)

**Notes:**
- If author has associated books, may return 400 Bad Request or cascade delete

---

## 11. Create Book

**Method:** POST  
**URL:** https://api.example.com/books

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
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
**Expected HTTP Status:** 201 Created

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
- authorId and categoryId must reference existing records
- If authorId or categoryId does not exist → 400 Bad Request
- ISBN should be unique → 409 Conflict if ISBN already exists

---

## 12. Get Books with Pagination

**Method:** GET  
**URL:** https://api.example.com/books?page=1&limit=10

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

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

---

## 13. Get Books by Category

**Method:** GET  
**URL:** https://api.example.com/categories/12?page=1&limit=10

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

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
- If category has no books, returns an empty array []

---

## 14. Get One Book

**Method:** GET  
**URL:** https://api.example.com/books/25

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

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

**Error Response if not found:**
**Expected HTTP Status:** 404 Not Found

```json
{
  "error": "Book not found"
}
```

---

## 15. Edit Book

**Method:** PATCH  
**URL:** https://api.example.com/books/25

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "price": 24.99,
  "isAvailable": false
}
```

**Expected Response:**
**Expected HTTP Status:** 200 OK

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
- Cannot change authorId or categoryId to non-existent values

---

## 16. Delete Book

**Method:** DELETE  
**URL:** https://api.example.com/books/25

**Headers:**
- Authorization: Bearer <valid_token>

**Expected Response:**
- HTTP Status: 204 No Content
- Body: Empty (indicating successful deletion)

**Notes:**
- Does not affect the associated author or category records
