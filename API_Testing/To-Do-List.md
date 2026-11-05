# 📄 To-Do List API 

## Table of Contents
1. [Create Task](#1-create-task)
2. [Get All Tasks](#2-get-all-tasks)
3. [Get One Task](#3-get-one-task)
4. [Edit Task](#4-edit-task)
5. [Delete Task](#5-delete-task)

## Overview
This document contains example API requests for a To-Do List application. These requests are designed to manage tasks including creating, retrieving, updating, and deleting tasks. Each example includes method, URL, headers, expected HTTP status, and response structure.

---

## 1. Create Task

**Method:** POST  
**URL:** https://api.example.com/tasks

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "title": "Buy groceries",
  "completed": false
}
```

**Expected Response:**
**Expected HTTP Status:** 201 Created

```json
{
  "id": 29,
  "title": "Buy groceries",
  "completed": false,
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- The API should return the created task with an auto-generated ID
- If title is missing or empty → 400 Bad Request
- If completed is not provided → defaults to false

---

## 2. Get All Tasks

**Method:** GET  
**URL:** https://api.example.com/tasks

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
[
  {
    "id": 29,
    "title": "Buy groceries",
    "completed": false,
    "createdAt": "2024-11-02T12:00:00.000Z"
  },
  {
    "id": 30,
    "title": "Complete homework",
    "completed": true,
    "createdAt": "2024-11-02T12:05:00.000Z"
  }
]
```

**Notes:**
- Returns an array of all tasks
- If no tasks exist, returns an empty array []
- Should be sorted by creation date (newest first)

---

## 3. Get One Task

**Method:** GET  
**URL:** https://api.example.com/tasks/29

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 29,
  "title": "Buy groceries",
  "completed": false,
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Error Response if not found:**
**Expected HTTP Status:** 404 Not Found

```json
{
  "error": "Task not found"
}
```

**Notes:**
- Returns a single task by its ID
- If task ID does not exist → 404 Not Found

---

## 4. Edit Task

**Method:** PATCH  
**URL:** https://api.example.com/tasks/41

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "title": "buy dog toys",
  "description": "IM FULL NOW",
  "completed": true
}
```

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 41,
  "title": "buy dog toys",
  "description": "IM FULL NOW",
  "completed": true,
  "createdAt": "2024-11-02T12:00:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Only updates the fields provided in the request body
- If task ID does not exist → 404 Not Found
- Should include updatedAt timestamp in response

---

## 5. Delete Task

**Method:** DELETE  
**URL:** https://api.example.com/tasks/44

**Headers:**
- Authorization: Bearer <valid_token>

**Expected Response:**
- HTTP Status: 204 No Content
- Body: Empty (indicating successful deletion)

**Notes:**
- Attempting to delete a non-existent task should return 404 Not Found
- If token is missing or invalid → 401 Unauthorized
