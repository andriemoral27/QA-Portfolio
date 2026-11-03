# Act-1 To-Do List API Documentation
 
## Overview
This document contains API requests for a To-Do List application. These requests are designed to manage tasks including creating, retrieving, updating, and deleting tasks. Each example includes the HTTP method, URL, headers, request body, expected HTTP status codes, and notes for testing in Postman.
 
## Table of Contents
1. [Create Task](#1-create-task)
2. [Get All Tasks](#2-get-all-tasks)
3. [Get One Task](#3-get-one-task)
4. [Edit Task](#4-edit-task)
5. [Delete Task](#5-delete-task)
 
---
 
## 1. Create Task
 
**Method:** POST  
**URL:** `{{base_url}}/tasks`
 
**Headers:**
- Content-Type: application/json
 
**Request Body:**
```json
{
  "title": "Buy groceries",
  "completed": false
}
```
 
**Expected Response:**
 
**HTTP Status:** 201 Created  
**Body:**
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
- If title is missing or empty -> 400 Bad Request
- If completed is not provided, it should default to false
 
---
 
## 2. Get All Tasks
 
**Method:** GET  
**URL:** `{{base_url}}/tasks`
 
**Headers:**
- None required
 
**Request Body:** None
 
**Expected Response:**
 
**HTTP Status:** 200 OK  
**Body:**
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
**URL:** `{{base_url}}/tasks/{task_id}`
 
**Headers:**
- None required
 
**Request Body:** None
 
**Expected Response:**
 
**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 29,
  "title": "Buy groceries",
  "completed": false,
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```
 
**Notes:**
- Returns a single task by its ID
- If task ID does not exist -> 404 Not Found
- If task ID is invalid format -> 400 Bad Request
 
---
 
## 4. Edit Task
 
**Method:** PATCH  
**URL:** `{{base_url}}/tasks/{task_id}`
 
**Headers:**
- Content-Type: application/json
 
**Request Body:**
```json
{
  "title": "buy dog toys",
  "description": "IM FULL NOW",
  "completed": true
}
```
 
**Expected Response:**
 
**HTTP Status:** 200 OK  
**Body:**
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
- If task ID does not exist -> 404 Not Found
- If all fields are empty or invalid -> 400 Bad Request
- Should include updatedAt timestamp in response
 
---
 
## 5. Delete Task
 
**Method:** DELETE  
**URL:** `{{base_url}}/tasks/{task_id}`
 
**Headers:**
- None required
 
**Request Body:** None
 
**Expected Response:**
 
**HTTP Status:** 204 No Content  
**Body:** Empty (indicating successful deletion)
 
**Notes:**
- Deleting a non-existent task should return 404 Not Found
- Deleting a task should remove it permanently from the database
- Successful deletion returns 204 with no body
- If task ID is invalid format -> 400 Bad Request
 
---
 
## Common Status Codes
 
| Status Code | Description |
|-------------|-------------|
| 200 OK | Request succeeded (GET, PATCH) |
| 201 Created | Resource successfully created (POST) |
| 204 No Content | Successful deletion (DELETE) |
| 400 Bad Request | Invalid request data or format |
| 404 Not Found | Resource not found |
| 500 Internal Server Error | Server error occurred |
 
## Testing in Postman
 
1. Import the Postman collection: `Act-1 To-Do List.postman_collection.json`
2. Set the `base_url` variable in Postman environment or collection variables
3. Run requests in order: Create → Get All → Get One → Edit → Delete
4. Verify response status codes match expected values
5. Check response body structure and data integrity
