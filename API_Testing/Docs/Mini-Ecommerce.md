# 📄 Mini-Ecommerce API Testing

## Table of Contents
1. [Create Product](#1-create-product)
2. [Get All Products](#2-get-all-products)
3. [Get Product by ID](#3-get-product-by-id)
4. [Edit Product](#4-edit-product)
5. [Delete Product](#5-delete-product)
6. [Add to Cart](#6-add-to-cart)
7. [Get Cart](#7-get-cart)
8. [Update Cart Item](#8-update-cart-item)
9. [Remove from Cart](#9-remove-from-cart)
10. [Create Order](#10-create-order)
11. [Get Order by ID](#11-get-order-by-id)
12. [Get All Orders](#12-get-all-orders)

## Overview
This document contains API requests for a Mini-Ecommerce application. These requests are designed to manage products, shopping cart, and orders. Each example includes method, URL, headers, request body, expected HTTP status, and response structure.

---

## 1. Create Product

**Method:** POST  
**URL:** https://api.example.com/products

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "name": "Wireless Mouse",
  "description": "Ergonomic wireless mouse with customizable DPI",
  "price": 29.99,
  "stock": 100,
  "category": "Electronics"
}
```

**Expected Response:**
**Expected HTTP Status:** 201 Created

```json
{
  "id": 1,
  "name": "Wireless Mouse",
  "description": "Ergonomic wireless mouse with customizable DPI",
  "price": 29.99,
  "stock": 100,
  "category": "Electronics",
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Creates a new product
- If name or price is missing → 400 Bad Request
- Price should be a positive number → 400 Bad Request if invalid
- Stock should be a non-negative integer

---

## 2. Get All Products

**Method:** GET  
**URL:** https://api.example.com/products

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
[
  {
    "id": 1,
    "name": "Wireless Mouse",
    "description": "Ergonomic wireless mouse with customizable DPI",
    "price": 29.99,
    "stock": 100,
    "category": "Electronics",
    "createdAt": "2024-11-02T12:00:00.000Z"
  },
  {
    "id": 2,
    "name": "USB Keyboard",
    "description": "Mechanical keyboard with RGB lighting",
    "price": 79.99,
    "stock": 50,
    "category": "Electronics",
    "createdAt": "2024-11-02T12:05:00.000Z"
  }
]
```

**Notes:**
- Returns all products
- If no products exist, returns an empty array []
- Supports pagination with `page` and `limit` parameters
- Can filter by category with `category` query parameter

---

## 3. Get Product by ID

**Method:** GET  
**URL:** https://api.example.com/products/1

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 1,
  "name": "Wireless Mouse",
  "description": "Ergonomic wireless mouse with customizable DPI",
  "price": 29.99,
  "stock": 100,
  "category": "Electronics",
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Error Response if not found:**
**Expected HTTP Status:** 404 Not Found

```json
{
  "error": "Product not found"
}
```

---

## 4. Edit Product

**Method:** PATCH  
**URL:** https://api.example.com/products/1

**Headers:**
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "price": 24.99,
  "stock": 85
}
```

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 1,
  "name": "Wireless Mouse",
  "description": "Ergonomic wireless mouse with customizable DPI",
  "price": 24.99,
  "stock": 85,
  "category": "Electronics",
  "createdAt": "2024-11-02T12:00:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Only updates the fields provided in the request body
- If product ID does not exist → 404 Not Found
- Price and stock validations apply

---

## 5. Delete Product

**Method:** DELETE  
**URL:** https://api.example.com/products/1

**Headers:**
- Authorization: Bearer <valid_token>

**Expected Response:**
- HTTP Status: 204 No Content
- Body: Empty (indicating successful deletion)

**Notes:**
- Deleting a product may be blocked if it's in active orders → 409 Conflict
- If product has cart items, those items should be invalidated

---

## 6. Add to Cart

**Method:** POST  
**URL:** https://api.example.com/cart

**Headers:**
- Authorization: Bearer <valid_token>
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "productId": 1,
  "quantity": 2
}
```

**Expected Response:**
**Expected HTTP Status:** 201 Created

```json
{
  "id": 1,
  "productId": 1,
  "quantity": 2,
  "userId": 1,
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Adds a product to the user's cart
- Requires valid JWT token
- productId must reference an existing product → 400 Bad Request if not found
- If product is out of stock → 400 Bad Request
- If product already in cart, updates quantity instead

---

## 7. Get Cart

**Method:** GET  
**URL:** https://api.example.com/cart

**Headers:**
- Authorization: Bearer <valid_token>
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
[
  {
    "id": 1,
    "productId": 1,
    "quantity": 2,
    "userId": 1,
    "createdAt": "2024-11-02T12:00:00.000Z",
    "product": {
      "id": 1,
      "name": "Wireless Mouse",
      "price": 29.99
    }
  },
  {
    "id": 2,
    "productId": 2,
    "quantity": 1,
    "userId": 1,
    "createdAt": "2024-11-02T12:05:00.000Z",
    "product": {
      "id": 2,
      "name": "USB Keyboard",
      "price": 79.99
    }
  }
]
```

**Notes:**
- Requires valid JWT token
- Returns all items in the authenticated user's cart
- If cart is empty, returns an empty array []
- Includes product details for each cart item

---

## 8. Update Cart Item

**Method:** PATCH  
**URL:** https://api.example.com/cart/1

**Headers:**
- Authorization: Bearer <valid_token>
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "quantity": 3
}
```

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 1,
  "productId": 1,
  "quantity": 3,
  "userId": 1,
  "createdAt": "2024-11-02T12:00:00.000Z",
  "updatedAt": "2024-11-02T12:30:00.000Z"
}
```

**Notes:**
- Requires valid JWT token
- Users can only update their own cart items → 403 Forbidden otherwise
- If quantity exceeds available stock → 400 Bad Request
- If quantity is 0, item should be removed from cart

---

## 9. Remove from Cart

**Method:** DELETE  
**URL:** https://api.example.com/cart/1

**Headers:**
- Authorization: Bearer <valid_token>

**Expected Response:**
- HTTP Status: 204 No Content
- Body: Empty (indicating successful deletion)

**Notes:**
- Requires valid JWT token
- Users can only remove their own cart items → 403 Forbidden otherwise
- If cart item ID does not exist → 404 Not Found

---

## 10. Create Order

**Method:** POST  
**URL:** https://api.example.com/orders

**Headers:**
- Authorization: Bearer <valid_token>
- Content-Type: application/json
- Accept: application/json

**Body:**
```json
{
  "shippingAddress": {
    "street": "123 Main St",
    "city": "New York",
    "state": "NY",
    "zipCode": "10001",
    "country": "USA"
  },
  "paymentMethod": "credit_card"
}
```

**Expected Response:**
**Expected HTTP Status:** 201 Created

```json
{
  "id": 1,
  "userId": 1,
  "status": "processing",
  "total": 139.97,
  "shippingAddress": {
    "street": "123 Main St",
    "city": "New York",
    "state": "NY",
    "zipCode": "10001",
    "country": "USA"
  },
  "items": [
    {
      "productId": 1,
      "name": "Wireless Mouse",
      "quantity": 2,
      "price": 29.99
    },
    {
      "productId": 2,
      "name": "USB Keyboard",
      "quantity": 1,
      "price": 79.99
    }
  ],
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Requires valid JWT token
- Converts all items in the user's cart to an order
- Clears the cart after order creation
- Validates stock availability before creating order
- If cart is empty → 400 Bad Request
- If insufficient stock → 400 Bad Request

---

## 11. Get Order by ID

**Method:** GET  
**URL:** https://api.example.com/orders/1

**Headers:**
- Authorization: Bearer <valid_token>
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "id": 1,
  "userId": 1,
  "status": "processing",
  "total": 139.97,
  "shippingAddress": {
    "street": "123 Main St",
    "city": "New York",
    "state": "NY",
    "zipCode": "10001",
    "country": "USA"
  },
  "items": [
    {
      "productId": 1,
      "name": "Wireless Mouse",
      "quantity": 2,
      "price": 29.99
    },
    {
      "productId": 2,
      "name": "USB Keyboard",
      "quantity": 1,
      "price": 79.99
    }
  ],
  "createdAt": "2024-11-02T12:00:00.000Z"
}
```

**Error Response if not found:**
**Expected HTTP Status:** 404 Not Found

```json
{
  "error": "Order not found"
}
```

**Notes:**
- Requires valid JWT token
- Users can only view their own orders → 403 Forbidden otherwise
- If order ID does not exist → 404 Not Found

---

## 12. Get All Orders

**Method:** GET  
**URL:** https://api.example.com/orders

**Headers:**
- Authorization: Bearer <valid_token>
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
[
  {
    "id": 1,
    "userId": 1,
    "status": "processing",
    "total": 139.97,
    "createdAt": "2024-11-02T12:00:00.000Z"
  },
  {
    "id": 2,
    "userId": 1,
    "status": "shipped",
    "total": 49.99,
    "createdAt": "2024-11-01T10:00:00.000Z"
  }
]
```

**Notes:**
- Requires valid JWT token
- Returns all orders for the authenticated user
- If user has no orders, returns an empty array []
- Supports pagination with `page` and `limit` parameters
- Can filter by status with `status` query parameter
