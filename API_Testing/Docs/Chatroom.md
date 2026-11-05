# Chatroom API Documentation

## Overview
This document contains API requests for a Chatroom application. These requests are designed to manage chatrooms and messages within those chatrooms. Each resource supports full CRUD operations (Create, Read, Update, Delete). Each example includes the HTTP method, URL, headers, request body, expected HTTP status codes, and notes for testing in Postman.

## Table of Contents
1. [Chatrooms](#chatrooms)
   - 1.1 Get All Chatrooms
   - 1.2 Create New Chatroom
   - 1.3 Get Chatroom by ID
   - 1.4 Update Chatroom
   - 1.5 Delete Chatroom
2. [Messages](#messages)
   - 2.1 Get All Messages or Filter by Chatroom
   - 2.2 Create New Message
   - 2.3 Get Message by ID
   - 2.4 Update a Message
   - 2.5 Delete a Message

---

## Chatrooms

### 1.1 Get All Chatrooms

**Method:** GET  
**URL:** `http://localhost:3000/chatroom`

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
    "name": "General",
    "description": "General discussion room",
    "icon": "💬",
    "createdAt": "2024-11-02T12:00:00.000Z"
  },
  {
    "id": 2,
    "name": "Tech Talk",
    "description": "Technology discussions",
    "icon": "💻",
    "createdAt": "2024-11-02T12:05:00.000Z"
  }
]
```

**Notes:**
- Returns an array of all available chatrooms
- If no chatrooms exist, returns an empty array []
- Chatrooms may be sorted by creation date

---

### 1.2 Create New Chatroom

**Method:** POST  
**URL:** `http://localhost:3000/chatroom`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "name": "G-Room",
  "description": "General discussion room",
  "icon": "💬"
}
```

**Expected Response:**

**HTTP Status:** 201 Created  
**Body:**
```json
{
  "id": 6,
  "name": "G-Room",
  "description": "General discussion room",
  "icon": "💬",
  "createdAt": "2024-11-02T12:10:00.000Z"
}
```

**Notes:**
- Creates a new chatroom
- Name is required field
- Description and icon are optional
- If name is missing -> 400 Bad Request
- Name should be unique -> 409 Conflict if name already exists

---

### 1.3 Get Chatroom by ID

**Method:** GET  
**URL:** `{{base_url}}/chatroom/{chatroom_id}`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 6,
  "name": "G-Room",
  "description": "General discussion room",
  "icon": "💬",
  "createdAt": "2024-11-02T12:10:00.000Z"
}
```

**Notes:**
- Returns a single chatroom by its ID
- If chatroom ID does not exist -> 404 Not Found
- If chatroom ID is invalid format -> 400 Bad Request

---

### 1.4 Update Chatroom

**Method:** PATCH  
**URL:** `http://localhost:3000/chatroom/{chatroom_id}`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "name": "Updated Chat",
  "description": "Updated description",
  "icon": "🎉"
}
```

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 7,
  "name": "Updated Chat",
  "description": "Updated description",
  "icon": "🎉",
  "createdAt": "2024-11-02T12:10:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Only updates the fields provided in the request body
- If chatroom ID does not exist -> 404 Not Found
- If name conflicts with existing chatroom -> 409 Conflict
- Unchanged fields retain their original values

---

### 1.5 Delete Chatroom

**Method:** DELETE  
**URL:** `{{base_url}}/chatroom/{chatroom_id}`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 204 No Content  
**Body:** Empty (indicating successful deletion)

**Notes:**
- Deleting a non-existent chatroom should return 404 Not Found
- If chatroom has associated messages, may return 400 Bad Request or cascade delete (implementation dependent)
- Successful deletion returns 204 with no body
- Chatroom is permanently removed from the database

---

## Messages

### 2.1 Get All Messages or Filter by Chatroom

**Method:** GET  
**URL:** `http://localhost:3000/message?chatroomId={chatroom_id}`

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
    "chatroomId": 1,
    "username": "Alice",
    "message": "Hello everyone!",
    "createdAt": "2024-11-02T12:00:00.000Z"
  },
  {
    "id": 2,
    "chatroomId": 1,
    "username": "Bob",
    "message": "Hi Alice!",
    "createdAt": "2024-11-02T12:01:00.000Z"
  }
]
```

**Notes:**
- Returns messages, optionally filtered by chatroom ID
- If chatroomId query parameter is provided, returns only messages from that chatroom
- If no chatroomId is provided, returns all messages across all chatrooms
- If no messages exist, returns an empty array []
- Messages are typically sorted by creation date (oldest first or newest first)

---

### 2.2 Create New Message

**Method:** POST  
**URL:** `http://localhost:3000/message`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "chatroomId": 1,
  "username": "Peter",
  "message": "Arat ML!"
}
```

**Expected Response:**

**HTTP Status:** 201 Created  
**Body:**
```json
{
  "id": 3,
  "chatroomId": 1,
  "username": "Peter",
  "message": "Arat ML!",
  "createdAt": "2024-11-02T12:15:00.000Z"
}
```

**Notes:**
- Creates a new message in a chatroom
- chatroomId, username, and message are required fields
- chatroomId must reference an existing chatroom
- If chatroomId does not exist -> 400 Bad Request
- If required fields are missing -> 400 Bad Request

---

### 2.3 Get Message by ID

**Method:** GET  
**URL:** `{{base_url}}/message/{message_id}`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 4,
  "chatroomId": 1,
  "username": "Peter",
  "message": "Arat ML!",
  "createdAt": "2024-11-02T12:15:00.000Z"
}
```

**Notes:**
- Returns a single message by its ID
- If message ID does not exist -> 404 Not Found
- If message ID is invalid format -> 400 Bad Request

---

### 2.4 Update a Message

**Method:** PATCH  
**URL:** `http://localhost:3000/message/{message_id}`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "message": "hello"
}
```

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 1,
  "chatroomId": 1,
  "username": "Alice",
  "message": "hello",
  "createdAt": "2024-11-02T12:00:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Only updates the message field
- If message ID does not exist -> 404 Not Found
- Cannot change chatroomId or username (implementation dependent)
- Unchanged fields retain their original values

---

### 2.5 Delete a Message

**Method:** DELETE  
**URL:** `http://localhost:3000/message/{message_id}`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 204 No Content  
**Body:** Empty (indicating successful deletion)

**Notes:**
- Deleting a non-existent message should return 404 Not Found
- Successful deletion returns 204 with no body
- Message is permanently removed from the database
- Does not affect the associated chatroom record

---

## Common Status Codes

| Status Code | Description |
|-------------|-------------|
| 200 OK | Request succeeded (GET, PATCH) |
| 201 Created | Resource successfully created (POST) |
| 204 No Content | Successful deletion (DELETE) |
| 400 Bad Request | Invalid request data or format |
| 404 Not Found | Resource not found |
| 409 Conflict | Resource already exists (duplicate chatroom name) |
| 500 Internal Server Error | Server error occurred |

## Request/Response Fields

### Chatroom Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | number | Auto | Unique identifier for the chatroom |
| name | string | Yes | Chatroom name (unique) |
| description | string | No | Chatroom description |
| icon | string | No | Emoji or icon for the chatroom |
| createdAt | string | Auto | ISO 8601 timestamp of creation |
| updatedAt | string | Auto | ISO 8601 timestamp of last update |

### Message Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | number | Auto | Unique identifier for the message |
| chatroomId | number | Yes | ID of the chatroom the message belongs to |
| username | string | Yes | Username of the message sender |
| message | string | Yes | Message content/text |
| createdAt | string | Auto | ISO 8601 timestamp of creation |
| updatedAt | string | Auto | ISO 8601 timestamp of last update |

## Query Parameters

### Messages Endpoint

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| chatroomId | number | No | Filter messages by chatroom ID |

## Testing in Postman

1. Import the Postman collection: `Chatroom.postman_collection.json`
2. Set the `base_url` variable in Postman environment or collection variables
3. Create chatrooms first (messages require chatroom IDs)
4. Test GET endpoints to retrieve chatroom data
5. Create messages using the chatroom IDs
6. Test message retrieval:
   - Get all messages (without filter)
   - Get messages filtered by chatroom ID
   - Get a specific message by ID
7. Test PATCH endpoints to update chatroom and message data
8. Test DELETE endpoints to remove data
9. Verify response status codes match expected values
10. Test error scenarios:
    - Create message with non-existent chatroomId (should return 400)
    - Get/delete non-existent IDs (should return 404)
    - Create chatroom with duplicate name (should return 409)
11. Test message filtering with different chatroom IDs
12. Verify that messages are correctly associated with their chatrooms
13. Test that deleting a chatroom either cascades to messages or prevents deletion (implementation dependent)
