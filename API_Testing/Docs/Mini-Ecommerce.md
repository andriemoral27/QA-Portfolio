# Mini-Ecommerce API Documentation

## Overview
This document contains API requests for a Mini-Ecommerce application. These requests are designed to manage products, shopping cart, and orders in an e-commerce system. Each resource supports full CRUD operations (Create, Read, Update, Delete). Each example includes the HTTP method, URL, headers, request body, expected HTTP status codes, and notes for testing in Postman.

## Table of Contents
1. [Products](#products)
   - 1.1 Post Product
   - 1.2 Get All Products
   - 1.3 Get Product by ID
   - 1.4 Patch Product
   - 1.5 Delete Product
2. [Cart](#cart)
   - 2.1 Add Item to Cart
   - 2.2 Get Cart
   - 2.3 Patch Cart
   - 2.4 Delete Cart
3. [Orders](#orders)
   - 3.1 Order Post
   - 3.2 Get All Orders
   - 3.3 Get Order by ID
   - 3.4 Patch Order
   - 3.5 Order Delete

---

## Products

### 1.1 Post Product

**Method:** POST  
**URL:** `{{base_url}}/products`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "name": "Laptop",
  "price": 1500,
  "stock": 10
}
```

**Expected Response:**

**HTTP Status:** 201 Created  
**Body:**
```json
{
  "id": 1,
  "name": "Laptop",
  "price": 1500,
  "stock": 10,
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Creates a new product in the inventory
- Name, price, and stock are required fields
- Price should be a positive number
- Stock should be a non-negative integer
- If required fields are missing -> 400 Bad Request
- If price or stock is invalid -> 400 Bad Request

---

### 1.2 Get All Products

**Method:** GET  
**URL:** `{{base_url}}/products`

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
    "name": "Laptop",
    "price": 1500,
    "stock": 10,
    "createdAt": "2024-11-02T12:00:00.000Z"
  },
  {
    "id": 2,
    "name": "Gameboy ver 2000",
    "price": 1400,
    "stock": 30,
    "createdAt": "2024-11-02T12:05:00.000Z"
  }
]
```

**Notes:**
- Returns an array of all available products
- If no products exist, returns an empty array []
- Products may be sorted by creation date or name

---

### 1.3 Get Product by ID

**Method:** GET  
**URL:** `{{base_url}}/products/{product_id}`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 2,
  "name": "Gameboy ver 2000",
  "price": 1400,
  "stock": 30,
  "createdAt": "2024-11-02T12:05:00.000Z"
}
```

**Notes:**
- Returns a single product by its ID
- If product ID does not exist -> 404 Not Found
- If product ID is invalid format -> 400 Bad Request

---

### 1.4 Patch Product

**Method:** PATCH  
**URL:** `{{base_url}}/products/{product_id}`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "name": "Gameboy ver 2000",
  "price": 1400,
  "stock": 30
}
```

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 2,
  "name": "Gameboy ver 2000",
  "price": 1400,
  "stock": 30,
  "createdAt": "2024-11-02T12:05:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Only updates the fields provided in the request body
- If product ID does not exist -> 404 Not Found
- Price and stock validation applies to updates as well
- Unchanged fields retain their original values

---

### 1.5 Delete Product

**Method:** DELETE  
**URL:** `{{base_url}}/products/{product_id}`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 204 No Content  
**Body:** Empty (indicating successful deletion)

**Notes:**
- Deleting a non-existent product should return 404 Not Found
- If product is in active carts or orders, may return 400 Bad Request or cascade delete (implementation dependent)
- Successful deletion returns 204 with no body
- Product is permanently removed from the database

---

## Cart

### 2.1 Add Item to Cart

**Method:** POST  
**URL:** `http://localhost:3000/cart`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "productId": 2,
  "quantity": 1
}
```

**Expected Response:**

**HTTP Status:** 201 Created  
**Body:**
```json
{
  "id": 1,
  "productId": 2,
  "quantity": 1,
  "userId": 1,
  "createdAt": "2024-11-02T12:15:00.000Z"
}
```

**Notes:**
- Adds a product to the shopping cart
- productId and quantity are required fields
- productId must reference an existing product
- If productId does not exist -> 400 Bad Request
- If quantity exceeds available stock -> 400 Bad Request
- If product already in cart, may update quantity instead (implementation dependent)

---

### 2.2 Get Cart

**Method:** GET  
**URL:** `{{base_url}}/cart`

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
    "productId": 2,
    "quantity": 1,
    "userId": 1,
    "createdAt": "2024-11-02T12:15:00.000Z"
  },
  {
    "id": 2,
    "productId": 4,
    "quantity": 2,
    "userId": 1,
    "createdAt": "2024-11-02T12:20:00.000Z"
  }
]
```

**Notes:**
- Returns all items in the user's cart
- If cart is empty, returns an empty array []
- May include product details in the response (implementation dependent)
- Cart is typically associated with a user (via userId or authentication)

---

### 2.3 Patch Cart

**Method:** PATCH  
**URL:** `{{base_url}}/cart/{cart_item_id}`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "productId": 2,
  "quantity": 1
}
```

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 2,
  "productId": 2,
  "quantity": 1,
  "userId": 1,
  "createdAt": "2024-11-02T12:20:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Updates the quantity of a cart item
- If cart item ID does not exist -> 404 Not Found
- If quantity exceeds available stock -> 400 Bad Request
- If quantity is set to 0, may remove the item (implementation dependent)
- Cannot change productId (implementation dependent)

---

### 2.4 Delete Cart

**Method:** DELETE  
**URL:** `http://localhost:3000/cart/{cart_item_id}`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 204 No Content  
**Body:** Empty (indicating successful deletion)

**Notes:**
- Deleting a non-existent cart item should return 404 Not Found
- Successful deletion returns 204 with no body
- Removes the item from the cart
- Does not affect the product record

---

## Orders

### 3.1 Order Post

**Method:** POST  
**URL:** `http://localhost:3000/orders`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "userId": 2,
  "items": [
    {
      "productId": 4,
      "quantity": 5
    }
  ]
}
```

**Expected Response:**

**HTTP Status:** 201 Created  
**Body:**
```json
{
  "id": 1,
  "userId": 2,
  "items": [
    {
      "productId": 4,
      "quantity": 5,
      "price": 1500
    }
  ],
  "total": 7500,
  "status": "pending",
  "createdAt": "2024-11-02T12:25:00.000Z"
}
```

**Notes:**
- Creates a new order from cart items
- userId and items array are required
- Each item must have productId and quantity
- Products must exist and have sufficient stock
- If stock is insufficient -> 400 Bad Request
- Order creation typically reduces product stock
- Total price is calculated automatically

---

### 3.2 Get All Orders

**Method:** GET  
**URL:** `http://localhost:3000/orders`

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
    "userId": 2,
    "items": [
      {
        "productId": 4,
        "quantity": 5,
        "price": 1500
      }
    ],
    "total": 7500,
    "status": "pending",
    "createdAt": "2024-11-02T12:25:00.000Z"
  },
  {
    "id": 2,
    "userId": 3,
    "items": [
      {
        "productId": 2,
        "quantity": 1,
        "price": 1400
      }
    ],
    "total": 1400,
    "status": "completed",
    "createdAt": "2024-11-02T12:30:00.000Z"
  }
]
```

**Notes:**
- Returns an array of all orders
- If no orders exist, returns an empty array []
- Orders may be filtered by user (implementation dependent)
- Orders are typically sorted by creation date

---

### 3.3 Get Order by ID

**Method:** GET  
**URL:** `http://localhost:3000/orders/{order_id}`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 3,
  "userId": 2,
  "items": [
    {
      "productId": 3,
      "quantity": 10,
      "price": 100
    }
  ],
  "total": 1000,
  "status": "pending",
  "createdAt": "2024-11-02T12:35:00.000Z"
}
```

**Notes:**
- Returns a single order by its ID
- If order ID does not exist -> 404 Not Found
- If order ID is invalid format -> 400 Bad Request

---

### 3.4 Patch Order

**Method:** PATCH  
**URL:** `http://localhost:3000/orders/{order_id}`

**Headers:**
- Content-Type: application/json

**Request Body:**
```json
{
  "userId": 2,
  "items": [
    {
      "productId": 3,
      "quantity": 10
    }
  ]
}
```

**Expected Response:**

**HTTP Status:** 200 OK  
**Body:**
```json
{
  "id": 3,
  "userId": 2,
  "items": [
    {
      "productId": 3,
      "quantity": 10,
      "price": 100
    }
  ],
  "total": 1000,
  "status": "pending",
  "createdAt": "2024-11-02T12:35:00.000Z",
  "updatedAt": "2024-11-02T12:40:00.000Z"
}
```

**Notes:**
- Updates order details
- If order ID does not exist -> 404 Not Found
- Cannot change userId (implementation dependent)
- Stock may be adjusted when updating items
- Status updates (e.g., pending -> completed) may be handled separately

---

### 3.5 Order Delete

**Method:** DELETE  
**URL:** `http://localhost:3000/orders/{order_id}`

**Headers:**
- None required

**Request Body:** None

**Expected Response:**

**HTTP Status:** 204 No Content  
**Body:** Empty (indicating successful deletion)

**Notes:**
- Deleting a non-existent order should return 404 Not Found
- Deleting a completed order may be restricted (implementation dependent)
- Successful deletion returns 204 with no body
- May or may not restore product stock (implementation dependent)

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

## Request/Response Fields

### Product Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | number | Auto | Unique identifier for the product |
| name | string | Yes | Product name |
| price | number | Yes | Product price (positive number) |
| stock | number | Yes | Available stock quantity (non-negative) |
| createdAt | string | Auto | ISO 8601 timestamp of creation |
| updatedAt | string | Auto | ISO 8601 timestamp of last update |

### Cart Item Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | number | Auto | Unique identifier for the cart item |
| productId | number | Yes | ID of the product in cart |
| quantity | number | Yes | Quantity of the product |
| userId | number | Auto | ID of the user who owns the cart |
| createdAt | string | Auto | ISO 8601 timestamp of creation |
| updatedAt | string | Auto | ISO 8601 timestamp of last update |

### Order Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | number | Auto | Unique identifier for the order |
| userId | number | Yes | ID of the user who placed the order |
| items | array | Yes | Array of order items (productId, quantity, price) |
| total | number | Auto | Total order price (calculated) |
| status | string | Auto | Order status (pending, completed, cancelled) |
| createdAt | string | Auto | ISO 8601 timestamp of creation |
| updatedAt | string | Auto | ISO 8601 timestamp of last update |

## Testing in Postman

1. Import the Postman collection: `Act-9 Mini-Ecommerce.postman_collection.json`
2. Set the `base_url` variable in Postman environment or collection variables
3. Create products first (cart and orders require product IDs)
4. Test GET endpoints to retrieve product data
5. Add items to cart using product IDs
6. Test cart operations:
   - Get cart items
   - Update cart item quantities
   - Remove items from cart
7. Create orders from cart items
8. Test order operations:
   - Get all orders
   - Get specific order by ID
   - Update order details
   - Delete orders
9. Verify response status codes match expected values
10. Test error scenarios:
    - Add item to cart with non-existent productId (should return 400)
    - Create order with insufficient stock (should return 400)
    - Get/delete non-existent IDs (should return 404)
11. Test stock management:
    - Verify stock decreases when items are added to cart (if implemented)
    - Verify stock decreases when orders are created
    - Verify stock is restored when cart items are removed (if implemented)
12. Test order total calculation
13. Verify that orders correctly reference products and users
