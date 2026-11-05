# 📄 Weather API

## Table of Contents
1. [Get Weather by City](#1-get-weather-by-city)

## Overview
This document contains API requests for a Weather application. These requests are designed to retrieve weather data for specific cities. Each example includes method, URL, headers, expected HTTP status, and response structure.

---

## 1. Get Weather by City

**Method:** GET  
**URL:** https://api.example.com/weather?city=London

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "city": "London",
  "temperature": 15,
  "humidity": 72,
  "windSpeed": 12,
  "condition": "Partly Cloudy",
  "updatedAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Accepts a city name as a query parameter
- If city parameter is missing → 400 Bad Request
- If city is not found in the database → 404 Not Found

---

## 2. Get Weather by City (New York)

**Method:** GET  
**URL:** https://api.example.com/weather?city=New%20York

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "city": "New York",
  "temperature": 20,
  "humidity": 65,
  "windSpeed": 18,
  "condition": "Sunny",
  "updatedAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- City names with spaces should be URL-encoded (e.g., New%20York)

---

## 3. Get Weather by City (Tokyo)

**Method:** GET  
**URL:** https://api.example.com/weather?city=Tokyo

**Headers:**
- Accept: application/json

**Expected Response:**
**Expected HTTP Status:** 200 OK

```json
{
  "city": "Tokyo",
  "temperature": 22,
  "humidity": 58,
  "windSpeed": 8,
  "condition": "Clear",
  "updatedAt": "2024-11-02T12:00:00.000Z"
}
```

**Notes:**
- Weather data is typically cached and updated periodically
- updatedAt timestamp indicates when the data was last refreshed
