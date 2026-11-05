# 🔌 API Testing

## Overview

This folder contains documentation for manual API testing performed on various applications. It includes example requests, testing strategies, and reusable reference notes. All requests and validations are described in Markdown and reflect common practices used in QA portfolios.

---

## 📁 Available Documentation Files

This folder includes categorized `.md` files with manual API requests organized by application. Each file contains examples and notes for:

- Authentication and session validation
- Data creation, retrieval, and deletion
- Header and payload formatting
- Common error scenarios and edge cases

List of available documentation files:

- [To-Do-List-API-Testing.md](./To-Do-List-API-Testing.md) - task management with CRUD operations
- [Notes-API-Testing.md](./Notes-API-Testing.md) - notes application with JWT authentication
- [Bookshelf-API-Testing.md](./Bookshelf-API-Testing.md) - book management (categories, authors, books)
- [Weather-API-Testing.md](./Weather-API-Testing.md) - weather data retrieval by city
- [Blog-Platform-API-Testing.md](./Blog-Platform-API-Testing.md) - blog platform with posts, comments, reactions
- [Movie-Review-API-Testing.md](./Movie-Review-API-Testing.md) - movie and review management system
- [Task-Management-API-Testing.md](./Task-Management-API-Testing.md) - project and task management
- [Chatroom-API-Testing.md](./Chatroom-API-Testing.md) - chatroom and messaging system
- [Mini-Ecommerce-API-Testing.md](./Mini-Ecommerce-API-Testing.md) - e-commerce (products, cart, orders)
- [Organization-and-Event-API-Testing.md](./Organization-and-Event-API-Testing.md) - organization and event management with RBAC

---

## Example Tools Used

- [Postman](https://www.postman.com/)
- [Fiddler Web Debugger](https://www.telerik.com/fiddler)
- [cURL](https://curl.se/)
- [HTTPie](https://httpie.io/)
- [Swagger / OpenAPI](https://swagger.io/)
- [VS Code REST Client plugin](https://marketplace.visualstudio.com/items?itemName=humao.rest-client)
- [Google Chrome built-in DevTools (Network tab)](https://developer.chrome.com/docs/devtools/network/)

---

## Common HTTP Methods Used

These are the most frequently used HTTP methods in REST API testing:

| Method  | Purpose                        | Example Usage                   |
|---------|--------------------------------|----------------------------------|
| **GET**    | Retrieve data                   | Get user list                   |
| **POST**   | Create a new resource           | Create a new review             |
| **PUT**    | Fully replace a resource        | Update all user fields          |
| **PATCH**  | Partially update a resource     | Change only email address       |
| **DELETE** | Remove a resource               | Delete a product                |

Other standard HTTP methods include:

| Method    | Description                                            |
|-----------|--------------------------------------------------------|
| **OPTIONS**  | Returns supported methods for a resource            |
| **HEAD**     | Similar to GET but returns headers only             |
| **TRACE**    | Echoes back the received request (debugging)        |
| **CONNECT**  | Establishes tunnel (used in HTTPS via proxy)        |

---

## Common HTTP Response Codes

Here are the most commonly encountered status codes during testing:

| Code  | Meaning                    | Notes                                |
|-------|----------------------------|--------------------------------------|
| **200**  | OK                         | Successful response                  |
| **201**  | Created                    | Resource successfully created        |
| **204**  | No Content                 | Successful deletion/update, no body |
| **301**  | Moved Permanently          | Resource moved to a new URL         |
| **302**  | Found                      | Temporary redirect                   |
| **304**  | Not Modified               | Client has cached version            |
| **400**  | Bad Request                | Invalid request data                 |
| **401**  | Unauthorized               | Missing or invalid token             |
| **403**  | Forbidden                  | Action not allowed                   |
| **404**  | Not Found                  | Resource does not exist              |
| **409**  | Conflict                   | Data conflict (e.g. product in use)  |
| **429**  | Too Many Requests          | Rate limit exceeded                  |
| **500**  | Internal Server Error      | Server-side issue                    |
| **502**  | Bad Gateway                | Invalid response from upstream server |
| **503**  | Service Unavailable        | Server temporarily overloaded/unavailable |
| **504**  | Gateway Timeout            | Server didn't respond in time        |

---

## Common HTTP Headers

### Request Headers

| Header            | Description                                      | Example                          |
|-------------------|--------------------------------------------------|-----------------------------------|
| Content-Type      | Media type of the request body                   | `application/json`              |
| Accept            | Media types acceptable for the response           | `application/json`              |
| Authorization     | Authentication credentials                        | `Bearer <token>`                 |
| User-Agent        | Client identification                             | `Mozilla/5.0`                     |
| Content-Length    | Size of the request body in bytes                | `1234`                            |

### Response Headers

| Header            | Description                                      | Example                          |
|-------------------|--------------------------------------------------|-----------------------------------|
| Content-Type      | Media type of the response body                  | `application/json`              |
| Content-Length    | Size of the response body in bytes                | `1234`                            |
| Cache-Control     | Caching directives                               | `no-cache`                        |
| ETag              | Identifier for a specific version of a resource    | `"33a64ff55a93c2"`             |
| Location          | URL to redirect to                                | `/new-resource`                   |
| Set-Cookie        | HTTP cookie                                      | `session=abc123`                 |

---

## Testing Best Practices

### 1. Test Happy Path
- Verify successful requests return expected status codes
- Validate response body structure and data
- Ensure all required fields are present

### 2. Test Negative Cases
- Missing required fields → 400 Bad Request
- Invalid data types → 400 Bad Request
- Non-existent resources → 404 Not Found
- Missing authentication → 401 Unauthorized
- Insufficient permissions → 403 Forbidden

### 3. Test Edge Cases
- Empty arrays when no data exists
- Very long strings (buffer overflow testing)
- Special characters in strings
- Boundary values (e.g., max quantity, max attendees)
- Duplicate data (unique constraint violations)

### 4. Test Authentication & Authorization
- Valid token should grant access
- Invalid/expired token should return 401
- Missing token should return 401
- Role-based permissions should be enforced

### 5. Test Data Integrity
- Foreign key relationships should be validated
- Cascade delete behavior should be tested
- Transaction rollback on errors

### 6. Test Pagination
- Verify page and limit parameters work correctly
- Test out-of-bounds page numbers
- Ensure consistent sorting

### 7. Test Rate Limiting
- Send multiple rapid requests
- Verify rate limit headers (if present)
- Test recovery after rate limit window
