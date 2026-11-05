# 📄 Chatroom API 

## Table of Contents
1. [Create Chatroom](#1-create-chatroom)
2. [Get All Chatrooms](#2-get-all-chatrooms)
3. [Get Chatroom by ID](#3-get-chatroom-by-id)
4. [Send Message](#4-send-message)
5. [Get Messages by Chatroom](#5-get-messages-by-chatroom)
6. [Get Message by ID](#6-get-message-by-id)

## Overview
This document contains API requests for a Chatroom application. These requests are designed to manage chatrooms and messages within them. Each example includes method, URL, headers, request body, expected HTTP status, and response structure.

---

## 1. Create Chatroom

**Method:** POST  
**URL:** https://api.example.com/chatroom

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "name": "General Discussion",
  "description": "A place for general discussions and off-topic conversations"
}
```

**Expected Response:**
**Expected HTTP Status:** 201 Created

```json
{
  "id": 1,
  "name": "General Discussion",
  "description": "A place for general discussions and off-topic conversations",
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Creates a new chatroom
- If name is missing → 400 Bad Request
- Name should be unique → 409 Conflict if already exists

---

## 2. Get All Chatrooms

**Method:** GET  
**URL:** https://api.example.com/chatroom

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
[
  {
    "id": 1,
    "name": "General Discussion",
    "description": "A place for general discussions and off-topic conversations",
    "createdAt": "2024-11-02T12:00:00.000Z"
  },
  {
    "id": 2,
    "name": "Tech Support",
    "description": "Get help with technical issues",
    "createdAt": "2024-11-02T12:05:00.000Z"
  }
]
```

**Notes:**
- Returns all chatrooms
- If no chatrooms exist, returns an empty array []
- Supports pagination with `page` and `limit` parameters

---

## 3. Get Chatroom by ID

**Method:** GET  
**URL:** https://api.example.com/chatroom/1

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 1,
  "name": "General Discussion",
  "description": "A place for general discussions and off-topic conversations",
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Error Response if not found:**
**Expected HTTP Status:** 404 Not Found

```json
{
  "error": "Chatroom not found"
}
```

---

## 4. Send Message

**Method:** POST  
**URL:** https://api.example.com/message

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "chatroomId": 1,
  "content": "Hello everyone! How's it going?",
  "sender": "Alice"
}
```

**Expected Response:**
**Expected HTTP Status:** 201 Created

```json
{
  "id": 1,
  "chatroomId": 1,
  "content": "Hello everyone! How's it going?",
  "sender": "Alice",
  "timestamp": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Creates a new message in a chatroom
- chatroomId must reference an existing chatroom → 400 Bad Request if not found
- If content is missing → 400 Bad Request
- sender should be a valid username or user ID

---

## 5. Get Messages by Chatroom

**Method:** GET  
**URL:** https://api.example.com/message?chatroomId=1

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
[
  {
    "id": 1,
    "chatroomId": 1,
    "content": "Hello everyone! How's it going?",
    "sender": "Alice",
    "timestamp": "2024-11-02T12:00:00.000Z"
  },
  {
    "id": 2,
    "chatroomId": 1,
    "content": "Hey Alice! I'm doing great, thanks for asking.",
    "sender": "Bob",
    "timestamp": "2024-11-02T12:05:00.000Z"
  }
]
```

**Notes:**
- Returns all messages for a specific chatroom
- Requires chatroomId as a query parameter
- If chatroomId is missing → 400 Bad Request
- If chatroom has no messages, returns an empty array []
- Supports pagination with `page` and `limit` parameters

---

## 6. Get Message by ID

**Method:** GET  
**URL:** https://api.example.com/message/1

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 1,
  "chatroomId": 1,
  "content": "Hello everyone! How's it going?",
  "sender": "Alice",
  "timestamp": "2024-11-02T12:00:00.000Z"
}
```

**Error Response if not found:**
**Expected HTTP Status:** 404 Not Found

```json
{
  "error": "Message not found"
}
```

**Notes:**
- Returns a single message by its ID
- If message ID does not exist → 404 Not Found
