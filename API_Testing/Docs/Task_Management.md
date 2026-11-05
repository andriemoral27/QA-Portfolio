# Task Management API Documentation

## Overview
This document contains API requests for a Task Management application. These requests are designed to manage users, projects, and tasks in a project management system. Each resource supports full CRUD operations (Create, Read, Update, Delete). Each example includes the HTTP method, URL, headers, request body, expected HTTP status codes, and notes for testing in Postman.

## Table of Contents
1. [Users](#users)
   - 1.1 Create User
   - 1.2 Get All Users
   - 1.3 Get One User
   - 1.4 Edit User
   - 1.5 Delete User
2. [Projects](#projects)
   - 2.1 Create Project
   - 2.2 Get All Projects
   - 2.3 Get One Project
   - 2.4 Edit Project
   - 2.5 Delete Project
3. [Tasks](#tasks)
   - 3.1 Create Task
   - 3.2 Get All Tasks of User
   - 3.3 Get All Tasks
   - 3.4 Get All Tasks by Project
   - 3.5 Update Task
   - 3.6 Delete Task

---

## Users

### 1.1 Create User

**Method:** POST  
**URL:** `{{base_url}}/users`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "name": "Doe",
  "email": "doe@example.com"
}
```

**Expected Response:**

**HTTP Status:** 201 Created  
**Body:**
```json
{
  "id": 3,
  "name": "Doe",
  "email": "doe@example.com",
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Creates a new user in the task management system
- Name and email are required fields
- Email must be unique -> 409 Conflict if email already exists
- If required fields are missing -> 400 Bad Request

---

### 1.2 Get All Users

**Method:** GET  
**URL:** `{{base_url}}/users`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
[
  {
    "id": 3,
    "name": "Doe",
    "email": "doe@example.com",
    "createdAt": "2024-11-02T12:00:00.000Z"
  },
  {
    "id": 4,
    "name": "Jane Smith",
    "email": "jane@example.com",
    "createdAt": "2024-11-02T12:05:00.000Z"
  }
]
```

**Notes:**
- Returns an array of all users
- If no users exist, returns an empty array []
- Users may be sorted by creation date

---

### 1.3 Get One User

**Method:** GET  
**URL:** `{{base_url}}/users/{user_id}`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 3,
  "name": "Doe",
  "email": "doe@example.com",
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Returns a single user by their ID
- If user ID does not exist -> 404 Not Found
- If user ID is invalid format -> 400 Bad Request

---

### 1.4 Edit User

**Method:** PATCH  
**URL:** `{{base_url}}/users/{user_id}`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "name": "John DoeDoe"
}
```

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 3,
  "name": "John DoeDoe",
  "email": "doe@example.com",
  "createdAt": "2024-11-02T12:00:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Only updates the fields provided in the request body
- If user ID does not exist -> 404 Not Found
- If email conflicts with existing user -> 409 Conflict
- Email typically cannot be changed (implementation dependent)

---

### 1.5 Delete User

**Method:** DELETE  
**URL:** `{{base_url}}/users/{user_id}`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 204 No Content  
**Body:** Empty (indicating successful deletion)

**Notes:**
- Deleting a non-existent user should return 404 Not Found
- If user has associated tasks or projects, may return 400 Bad Request or cascade delete (implementation dependent)
- Successful deletion returns 204 with no body

---

## Projects

### 2.1 Create Project

**Method:** POST  
**URL:** `{{base_url}}/projects`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "name": "Website Redesign",
  "description": "Redesign the company website"
}
```

**Expected Response:**

**HTTP Status:** 201 Created  
**Body:**
```json
{
  "id": 2,
  "name": "Website Redesign",
  "description": "Redesign the company website",
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Creates a new project
- Name is required field
- Description is optional
- If name is missing -> 400 Bad Request

---

### 2.2 Get All Projects

**Method:** GET  
**URL:** `{{base_url}}/projects`

**Headers:**
- Content-Type: application/json (optional)

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
[
  {
    "id": 2,
    "name": "Website Redesign",
    "description": "Redesign the company website",
    "createdAt": "2024-11-02T12:00:00.000Z"
  },
  {
    "id": 8,
    "name": "Mobile App Development",
    "description": "Build a mobile application",
    "createdAt": "2024-11-02T12:10:00.000Z"
  }
]
```

**Notes:**
- Returns an array of all projects
- If no projects exist, returns an empty array []
- Projects may be sorted by creation date

---

### 2.3 Get One Project

**Method:** GET  
**URL:** `{{base_url}}/projects/{project_id}`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 2,
  "name": "Website Redesign",
  "description": "Redesign the company website",
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Returns a single project by its ID
- If project ID does not exist -> 404 Not Found

---

### 2.4 Edit Project

**Method:** PATCH  
**URL:** `{{base_url}}/projects/{project_id}`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "name": "Updated Project Name"
}
```

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 8,
  "name": "Updated Project Name",
  "description": "Build a mobile application",
  "createdAt": "2024-11-02T12:10:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Only updates the fields provided in the request body
- If project ID does not exist -> 404 Not Found
- Unchanged fields retain their original values

---

### 2.5 Delete Project

**Method:** DELETE  
**URL:** `{{base_url}}/projects/{project_id}`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 204 No Content  
**Body:** Empty (indicating successful deletion)

**Notes:**
- Deleting a non-existent project should return 404 Not Found
- If project has associated tasks, may return 400 Bad Request or cascade delete (implementation dependent)
- Successful deletion returns 204 with no body

---

## Tasks

### 3.1 Create Task

**Method:** POST  
**URL:** `{{base_url}}/tasks`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "title": "Design homepage",
  "description": "Create a modern homepage design",
  "deadline": "2024-12-31",
  "status": "todo",
  "projectId": 7
}
```

**Expected Response:**

**HTTP Status:** 201 Created  
**Body:**
```json
{
  "id": 1,
  "title": "Design homepage",
  "description": "Create a modern homepage design",
  "deadline": "2024-12-31",
  "status": "todo",
  "projectId": 7,
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Creates a new task associated with a project
- Title, status, and projectId are typically required
- projectId must reference an existing project
- If projectId does not exist -> 400 Bad Request
- Status values typically include: todo, in-progress, done

---

### 3.2 Get All Tasks of User

**Method:** GET  
**URL:** `{{base_url}}/users/{user_id}/tasks`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
[
  {
    "id": 1,
    "title": "Design homepage",
    "description": "Create a modern homepage design",
    "deadline": "2024-12-31",
    "status": "todo",
    "projectId": 7,
    "userId": 6,
    "createdAt": "2024-11-02T12:00:00.000Z"
  }
]
```

**Notes:**
- Returns all tasks assigned to a specific user
- If user has no tasks, returns an empty array []
- If user ID does not exist -> 404 Not Found

---

### 3.3 Get All Tasks

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
    "id": 1,
    "title": "Design homepage",
    "description": "Create a modern homepage design",
    "deadline": "2024-12-31",
    "status": "todo",
    "projectId": 7,
    "createdAt": "2024-11-02T12:00:00.000Z"
  },
  {
    "id": 2,
    "title": "Backend API",
    "description": "Develop REST API endpoints",
    "deadline": "2024-12-15",
    "status": "in-progress",
    "projectId": 7,
    "createdAt": "2024-11-02T12:10:00.000Z"
  }
]
```

**Notes:**
- Returns an array of all tasks across all projects
- If no tasks exist, returns an empty array []
- Tasks may be sorted by deadline or creation date

---

### 3.4 Get All Tasks by Project

**Method:** GET  
**URL:** `{{base_url}}/tasks?projectId={project_id}`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
[
  {
    "id": 1,
    "title": "Design homepage",
    "description": "Create a modern homepage design",
    "deadline": "2024-12-31",
    "status": "todo",
    "projectId": 7,
    "createdAt": "2024-11-02T12:00:00.000Z"
  }
]
```

**Notes:**
- Returns all tasks belonging to a specific project
- Uses query parameter `projectId` to filter tasks
- If project has no tasks, returns an empty array []
- If projectId does not exist -> 404 Not Found

---

### 3.5 Update Task

**Method:** PATCH  
**URL:** `{{base_url}}/tasks/{task_id}`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "status": "done"
}
```

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 1,
  "title": "Design homepage",
  "description": "Create a modern homepage design",
  "deadline": "2024-12-31",
  "status": "done",
  "projectId": 7,
  "createdAt": "2024-11-02T12:00:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Only updates the fields provided in the request body
- If task ID does not exist -> 404 Not Found
- Common fields to update: status, title, description, deadline
- Cannot change projectId (implementation dependent)

---

### 3.6 Delete Task

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
- Successful deletion returns 204 with no body
- Task is permanently removed from the database
- Does not affect the associated project or user records

---

## Common Status Codes

| Status Code | Description |
|-------------|-------------|
| 200 OK | Request succeeded (GET, PATCH) |
| 201 Created | Resource successfully created (POST) |
| 204 No Content | Successful deletion (DELETE) |
| 400 Bad Request | Invalid request data or format |
| 404 Not Found | Resource not found |
| 409 Conflict | Resource already exists (duplicate email) |
| 500 Internal Server Error | Server error occurred |

## Request/Response Fields

### User Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | number | Auto | Unique identifier for the user |
| name | string | Yes | User's name |
| email | string | Yes | User's email address (unique) |
| createdAt | string | Auto | ISO 8601 timestamp of creation |
| updatedAt | string | Auto | ISO 8601 timestamp of last update |

### Project Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | number | Auto | Unique identifier for the project |
| name | string | Yes | Project name |
| description | string | No | Project description |
| createdAt | string | Auto | ISO 8601 timestamp of creation |
| updatedAt | string | Auto | ISO 8601 timestamp of last update |

### Task Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | number | Auto | Unique identifier for the task |
| title | string | Yes | Task title |
| description | string | No | Task description |
| deadline | string | No | Task deadline (ISO 8601 date format) |
| status | string | Yes | Task status (todo, in-progress, done) |
| projectId | number | Yes | ID of the associated project |
| userId | number | No | ID of the assigned user |
| createdAt | string | Auto | ISO 8601 timestamp of creation |
| updatedAt | string | Auto | ISO 8601 timestamp of last update |

## Testing in Postman

1. Import the Postman collection: `Task Management.postman_collection.json`
2. Set the `base_url` variable in Postman environment or collection variables
3. Create users first (tasks may require user IDs)
4. Create projects (tasks require project IDs)
5. Test GET endpoints to retrieve user and project data
6. Create tasks using the project IDs
7. Test task retrieval:
   - Get all tasks
   - Get tasks by user
   - Get tasks by project
8. Test PATCH endpoints to update task status
9. Test DELETE endpoints to remove data
10. Verify response status codes match expected values
11. Test error scenarios:
    - Create task with non-existent projectId (should return 400)
    - Get/delete non-existent IDs (should return 404)
    - Create user with duplicate email (should return 409)
12. Test status validation (e.g., invalid status values)
13. Verify relationships between users, projects, and tasks
