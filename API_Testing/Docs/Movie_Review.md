# 📄 Movie Review API 

## Table of Contents
1. [Create Movie](#1-create-movie)
2. [Get All Movies](#2-get-all-movies)
3. [Get Movie by ID](#3-get-movie-by-id)
4. [Edit Movie](#4-edit-movie)
5. [Delete Movie](#5-delete-movie)
6. [Create Review](#6-create-review)
7. [Get All Reviews](#7-get-all-reviews)
8. [Get Review by ID](#8-get-review-by-id)
9. [Edit Review](#9-edit-review)
10. [Delete Review](#10-delete-review)

## Overview
This document contains API requests for a Movie Review application. These requests are designed to manage movies and user reviews. Each resource supports full CRUD operations. Each example includes method, URL, headers, request body, expected HTTP status, and response structure.

---

## 1. Create Movie

**Method:** POST  
**URL:** https://api.example.com/movies

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "title": "The Matrix",
  "description": "A computer hacker learns about the true nature of reality and his role in the war against its controllers.",
  "releaseDate": "1999-03-31",
  "duration": 136,
  "genre": "Sci-Fi",
  "director": "The Wachowskis"
}
```

**Expected Response:**
**Expected HTTP Status:** 201 Created

```json
{
  "id": 69,
  "title": "The Matrix",
  "description": "A computer hacker learns about the true nature of reality and his role in the war against its controllers.",
  "releaseDate": "1999-03-31",
  "duration": 136,
  "genre": "Sci-Fi",
  "director": "The Wachowskis",
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Creates a new movie record
- releaseDate should be in ISO 8601 format (YYYY-MM-DD)
- duration is in minutes
- If title is missing → 400 Bad Request

---

## 2. Get All Movies

**Method:** GET  
**URL:** https://api.example.com/movies

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
[
  {
    "id": 69,
    "title": "The Matrix",
    "description": "A computer hacker learns about the true nature of reality and his role in the war against its controllers.",
    "releaseDate": "1999-03-31",
    "duration": 136,
    "genre": "Sci-Fi",
    "director": "The Wachowskis",
    "createdAt": "2024-11-02T12:00:00.000Z"
  },
  {
    "id": 70,
    "title": "Inception",
    "description": "A thief who steals corporate secrets through dream-sharing technology is given the inverse task of planting an idea.",
    "releaseDate": "2010-07-16",
    "duration": 148,
    "genre": "Sci-Fi",
    "director": "Christopher Nolan",
    "createdAt": "2024-11-02T12:05:00.000Z"
  }
]
```

**Notes:**
- Returns all movies in the database
- Supports pagination with `page` and `limit` parameters
- Can filter by genre with `genre` query parameter

---

## 3. Get Movie by ID

**Method:** GET  
**URL:** https://api.example.com/movies/69

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 69,
  "title": "The Matrix",
  "description": "A computer hacker learns about the true nature of reality and his role in the war against its controllers.",
  "releaseDate": "1999-03-31",
  "duration": 136,
  "genre": "Sci-Fi",
  "director": "The Wachowskis",
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Error Response if not found:**
**Expected HTTP Status:** 404 Not Found

```json
{
  "error": "Movie not found"
}
```

---

## 4. Edit Movie

**Method:** PATCH  
**URL:** https://api.example.com/movies/69

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "description": "Updated description for The Matrix movie."
}
```

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 69,
  "title": "The Matrix",
  "description": "Updated description for The Matrix movie.",
  "releaseDate": "1999-03-31",
  "duration": 136,
  "genre": "Sci-Fi",
  "director": "The Wachowskis",
  "createdAt": "2024-11-02T12:00:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Only updates the fields provided in the request body
- If movie ID does not exist → 404 Not Found

---

## 5. Delete Movie

**Method:** DELETE  
**URL:** https://api.example.com/movies/69

**Headers:**
- Authorization: Bearer <valid_token>

**Expected Response:**
- HTTP Status: 204 No Content
- Body: Empty (indicating successful deletion)

**Notes:**
- Cascade delete: associated reviews should also be deleted
- If movie has no reviews, still returns 204 No Content

---

## 6. Create Review

**Method:** POST  
**URL:** https://api.example.com/reviews

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "movieId": 69,
  "rating": 5,
  "comment": "Excellent movie! A masterpiece of sci-fi cinema."
}
```

**Expected Response:**
**Expected HTTP Status:** 201 Created

```json
{
  "id": 101,
  "movieId": 69,
  "rating": 5,
  "comment": "Excellent movie! A masterpiece of sci-fi cinema.",
  "createdAt": "2024-11-02T12:15:00.000Z"
}
```

**Notes:**
- Creates a new review for a movie
- movieId must reference an existing movie → 400 Bad Request if not found
- rating should be between 1 and 5 → 400 Bad Request if out of range
- If comment is missing → 400 Bad Request

---

## 7. Get All Reviews

**Method:** GET  
**URL:** https://api.example.com/reviews

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
[
  {
    "id": 101,
    "movieId": 69,
    "rating": 5,
    "comment": "Excellent movie! A masterpiece of sci-fi cinema.",
    "createdAt": "2024-11-02T12:15:00.000Z"
  },
  {
    "id": 102,
    "movieId": 70,
    "rating": 4,
    "comment": "Great concept and execution.",
    "createdAt": "2024-11-02T12:20:00.000Z"
  }
]
```

**Notes:**
- Returns all reviews in the database
- Supports pagination with `page` and `limit` parameters
- Can filter by movie with `movieId` query parameter

---

## 8. Get Review by ID

**Method:** GET  
**URL:** https://api.example.com/reviews/101

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 101,
  "movieId": 69,
  "rating": 5,
  "comment": "Excellent movie! A masterpiece of sci-fi cinema.",
  "createdAt": "2024-11-02T12:15:00.000Z"
}
```

**Error Response if not found:**
**Expected HTTP Status:** 404 Not Found

```json
{
  "error": "Review not found"
}
```

---

## 9. Edit Review

**Method:** PATCH  
**URL:** https://api.example.com/reviews/101

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "rating": 4,
  "comment": "Updated review: Still a great movie but rewatching it, I notice some flaws."
}
```

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 101,
  "movieId": 69,
  "rating": 4,
  "comment": "Updated review: Still a great movie but rewatching it, I notice some flaws.",
  "createdAt": "2024-11-02T12:15:00.000Z",
  "updatedAt": "2024-11-02T12:35:00.000Z"
}
```

**Notes:**
- Only updates the fields provided in the request body
- If review ID does not exist → 404 Not Found
- Updated rating should be between 1 and 5 → 400 Bad Request if out of range

---

## 10. Delete Review

**Method:** DELETE  
**URL:** https://api.example.com/reviews/101

**Headers:**
- Authorization: Bearer <valid_token>

**Expected Response:**
- HTTP Status: 204 No Content
- Body: Empty (indicating successful deletion)

**Notes:**
- Deleting a review does not affect the associated movie
