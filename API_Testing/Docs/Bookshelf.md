
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
 
## Common Status Codes
 
| Status Code | Description |
|-------------|-------------|
| 200 OK | Request succeeded (GET, PATCH) |
| 201 Created | Resource successfully created (POST) |
| 204 No Content | Successful deletion (DELETE) |
| 400 Bad Request | Invalid request data or format |
| 404 Not Found | Resource not found |
| 409 Conflict | Resource already exists (duplicate name/ISBN) |
| 500 Internal Server Error | Server error occurred |
 
## Testing in Postman
 
1. Import the Postman collection: `Act-3 Bookshelf.postman_collection.json`
2. Set the `base_url` variable in Postman environment or collection variables
3. Create categories first (books require category IDs)
4. Create authors (books require author IDs)
5. Create books using the category and author IDs
6. Test GET endpoints to retrieve data
7. Test PATCH endpoints to update data
8. Test DELETE endpoints to remove data
9. Verify response status codes match expected values
10. Test error scenarios (invalid IDs, missing fields) to ensure proper error handling
