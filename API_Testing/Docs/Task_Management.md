# 📄 Task Management API Testing

## Table of Contents
1. [Create User](#1-create-user)
2. [Get All Users](#2-get-all-users)
3. [Get User by ID](#3-get-user-by-id)
4. [Edit User](#4-edit-user)
5. [Delete User](#5-delete-user)
6. [Create Project](#6-create-project)
7. [Get All Projects](#7-get-all-projects)
8. [Get Project by ID](#8-get-project-by-id)
9. [Edit Project](#9-edit-project)
10. [Delete Project](#10-delete-project)
11. [Create Task](#11-create-task)
12. [Get All Tasks](#12-get-all-tasks)
13. [Get Task by ID](#13-get-task-by-id)
14. [Get Tasks by User](#14-get-tasks-by-user)
15. [Edit Task](#15-edit-task)
16. [Delete Task](#16-delete-task)

## Overview
This document contains API requests for a Task Management application. These requests are designed to manage users, projects, and tasks. The application supports organizing work into projects and assigning tasks to users. Each example includes method, URL, headers, request body, expected HTTP status, and response structure.

---

## 1. Create User

**Method:** POST  
**URL:** https://api.example.com/users

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "name": "Alice Johnson",
  "email": "alice@example.com",
  "role": "developer"
}
```

**Expected Response:**
**Expected HTTP Status:** 201 Created

```json
{
  "id": 1,
  "name": "Alice Johnson",
  "email": "alice@example.com",
  "role": "developer",
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Creates a new user account
- Email must be unique → 409 Conflict if already exists
- Supported roles: developer, manager, admin

---

## 2. Get All Users

**Method:** GET  
**URL:** https://api.example.com/users

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
[
  {
    "id": 1,
    "name": "Alice Johnson",
    "email": "alice@example.com",
    "role": "developer",
    "createdAt": "2024-11-02T12:00:00.000Z"
  },
  {
    "id": 2,
    "name": "Bob Smith",
    "email": "bob@example.com",
    "role": "manager",
    "createdAt": "2024-11-02T12:05:00.000Z"
  }
]
```

**Notes:**
- Returns all users in the system
- Supports pagination with `page` and `limit` parameters
- Can filter by role with `role` query parameter

---

## 3. Get User by ID

**Method:** GET  
**URL:** https://api.example.com/users/1

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 1,
  "name": "Alice Johnson",
  "email": "alice@example.com",
  "role": "developer",
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Error Response if not found:**
**Expected HTTP Status:** 404 Not Found

```json
{
  "error": "User not found"
}
```

---

## 4. Edit User

**Method:** PATCH  
**URL:** https://api.example.com/users/1

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "role": "senior developer"
}
```

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 1,
  "name": "Alice Johnson",
  "email": "alice@example.com",
  "role": "senior developer",
  "createdAt": "2024-11-02T12:00:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Only updates the fields provided in the request body
- If user ID does not exist → 404 Not Found

---

## 5. Delete User

**Method:** DELETE  
**URL:** https://api.example.com/users/1

**Headers:**
- Authorization: Bearer <valid_token>

**Expected Response:**
- HTTP Status: 204 No Content
- Body: Empty (indicating successful deletion)

**Notes:**
- Cascade delete: associated tasks should be reassigned or deleted
- If user is assigned to active tasks → 400 Bad Request or 409 Conflict

---

## 6. Create Project

**Method:** POST  
**URL:** https://api.example.com/projects

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "name": "Website Redesign",
  "description": "Redesign the company website with modern UI/UX",
  "startDate": "2024-11-01",
  "endDate": "2024-12-31"
}
```

**Expected Response:**
**Expected HTTP Status:** 201 Created

```json
{
  "id": 8,
  "name": "Website Redesign",
  "description": "Redesign the company website with modern UI/UX",
  "startDate": "2024-11-01",
  "endDate": "2024-12-31",
  "status": "active",
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Creates a new project
- startDate and endDate should be in ISO 8601 format (YYYY-MM-DD)
- If name is missing → 400 Bad Request

---

## 7. Get All Projects

**Method:** GET  
**URL:** https://api.example.com/projects

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
[
  {
    "id": 8,
    "name": "Website Redesign",
    "description": "Redesign the company website with modern UI/UX",
    "startDate": "2024-11-01",
    "endDate": "2024-12-31",
    "status": "active",
    "createdAt": "2024-11-02T12:00:00.000Z"
  },
  {
    "id": 9,
    "name": "Mobile App Development",
    "description": "Develop a mobile app for iOS and Android",
    "startDate": "2024-12-01",
    "endDate": "2025-03-31",
    "status": "planned",
    "createdAt": "2024-11-02T12:05:00.000Z"
  }
]
```

**Notes:**
- Returns all projects
- Supports pagination with `page` and `limit` parameters
- Can filter by status with `status` query parameter

---

## 8. Get Project by ID

**Method:** GET  
**URL:** https://api.example.com/projects/8

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 8,
  "name": "Website Redesign",
  "description": "Redesign the company website with modern UI/UX",
  "startDate": "2024-11-01",
  "endDate": "2024-12-31",
  "status": "active",
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Error Response if not found:**
**Expected HTTP Status:** 404 Not Found

```json
{
  "error": "Project not found"
}
```

---

## 9. Edit Project

**Method:** PATCH  
**URL:** https://api.example.com/projects/8

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "status": "completed",
  "endDate": "2024-11-15"
}
```

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 8,
  "name": "Website Redesign",
  "description": "Redesign the company website with modern UI/UX",
  "startDate": "2024-11-01",
  "endDate": "2024-11-15",
  "status": "completed",
  "createdAt": "2024-11-02T12:00:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Only updates the fields provided in the request body
- Supported statuses: planned, active, completed, cancelled

---

## 10. Delete Project

**Method:** DELETE  
**URL:** https://api.example.com/projects/8

**Headers:**
- Authorization: Bearer <valid_token>

**Expected Response:**
- HTTP Status: 204 No Content
- Body: Empty (indicating successful deletion)

**Notes:**
- Cascade delete: associated tasks should also be deleted
- If project has active tasks → 400 Bad Request or 409 Conflict

---

## 11. Create Task

**Method:** POST  
**URL:** https://api.example.com/tasks

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "title": "Design homepage mockup",
  "description": "Create wireframes and high-fidelity mockups for the homepage",
  "projectId": 8,
  "assignedTo": 1,
  "priority": "high",
  "dueDate": "2024-11-10"
}
```

**Expected Response:**
**Expected HTTP Status:** 201 Created

```json
{
  "id": 15,
  "title": "Design homepage mockup",
  "description": "Create wireframes and high-fidelity mockups for the homepage",
  "projectId": 8,
  "assignedTo": 1,
  "priority": "high",
  "dueDate": "2024-11-10",
  "status": "pending",
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Creates a new task
- projectId must reference an existing project → 400 Bad Request if not found
- assignedTo must reference an existing user → 400 Bad Request if not found
- dueDate should be in ISO 8601 format (YYYY-MM-DD)
- Supported priorities: low, medium, high, urgent

---

## 12. Get All Tasks

**Method:** GET  
**URL:** https://api.example.com/tasks

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
[
  {
    "id": 15,
    "title": "Design homepage mockup",
    "description": "Create wireframes and high-fidelity mockups for the homepage",
    "projectId": 8,
    "assignedTo": 1,
    "priority": "high",
    "dueDate": "2024-11-10",
    "status": "pending",
    "createdAt": "2024-11-02T12:00:00.000Z"
  },
  {
    "id": 16,
    "title": "Implement navigation",
    "description": "Build the navigation component with responsive design",
    "projectId": 8,
    "assignedTo": 2,
    "priority": "medium",
    "dueDate": "2024-11-15",
    "status": "in_progress",
    "createdAt": "2024-11-02T12:05:00.000Z"
  }
]
```

**Notes:**
- Returns all tasks
- Supports pagination with `page` and `limit` parameters
- Can filter by projectId, assignedTo, status, or priority with query parameters

---

## 13. Get Task by ID

**Method:** GET  
**URL:** https://api.example.com/tasks/15

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 15,
  "title": "Design homepage mockup",
  "description": "Create wireframes and high-fidelity mockups for the homepage",
  "projectId": 8,
  "assignedTo": 1,
  "priority": "high",
  "dueDate": "2024-11-10",
  "status": "pending",
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

---

## 14. Get Tasks by User

**Method:** GET  
**URL:** https://api.example.com/users/1/tasks

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
[
  {
    "id": 15,
    "title": "Design homepage mockup",
    "description": "Create wireframes and high-fidelity mockups for the homepage",
    "projectId": 8,
    "assignedTo": 1,
    "priority": "high",
    "dueDate": "2024-11-10",
    "status": "pending",
    "createdAt": "2024-11-02T12:00:00.000Z"
  }
]
```

**Notes:**
- Returns all tasks assigned to a specific user
- If user has no tasks, returns an empty array []

---

## 15. Edit Task

**Method:** PATCH  
**URL:** https://api.example.com/tasks/15

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "status": "completed",
  "assignedTo": 2
}
```

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 15,
  "title": "Design homepage mockup",
  "description": "Create wireframes and high-fidelity mockups for the homepage",
  "projectId": 8,
  "assignedTo": 2,
  "priority": "high",
  "dueDate": "2024-11-10",
  "status": "completed",
  "createdAt": "2024-11-02T12:00:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Only updates the fields provided in the request body
- Supported statuses: pending, in_progress, completed, cancelled
- Cannot assign to non-existent user → 400 Bad Request

---

## 16. Delete Task

**Method:** DELETE  
**URL:** https://api.example.com/tasks/15

**Headers:**
- Authorization: Bearer <valid_token>

**Expected Response:**
- HTTP Status: 204 No Content
- Body: Empty (indicating successful deletion)

**Notes:**
- Deleting a task does not affect the associated project or user
