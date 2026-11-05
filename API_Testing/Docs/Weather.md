# Weather API Documentation
 
## Overview
This document contains API requests for a Weather application. These requests are designed to retrieve weather information for different cities using query parameters. Each example includes the HTTP method, URL, headers, expected HTTP status codes, and notes for testing in Postman.
 
## Table of Contents
1. [Get Weather by City](#1-get-weather-by-city)
 
---
 
## 1. Get Weather by City
 
**Method:** GET  
**URL:** `{{base_url}}/weather?city={city_name}`
 
**Headers:**
- None required
 
**Request Body:** None
 
**Expected Response:**
 
**HTTP Status:** 200 OK  
**Body:**
```json
{
  "city": "London",
  "temperature": 15,
  "condition": "Cloudy",
  "humidity": 75,
  "windSpeed": 12,
  "timestamp": "2024-11-02T12:00:00.000Z"
}
```
 
### Example Requests
 
#### Example 1: London Weather
**URL:** `{{base_url}}/weather?city=London`
 
**Expected Response:**
```json
{
  "city": "London",
  "temperature": 15,
  "condition": "Cloudy",
  "humidity": 75,
  "windSpeed": 12,
  "timestamp": "2024-11-02T12:00:00.000Z"
}
```
 
#### Example 2: New York Weather
**URL:** `{{base_url}}/weather?city=New York`
 
**Expected Response:**
```json
{
  "city": "New York",
  "temperature": 18,
  "condition": "Sunny",
  "humidity": 60,
  "windSpeed": 8,
  "timestamp": "2024-11-02T12:00:00.000Z"
}
```
 
#### Example 3: Tokyo Weather
**URL:** `{{base_url}}/weather?city=Tokyo`
 
**Expected Response:**
```json
{
  "city": "Tokyo",
  "temperature": 22,
  "condition": "Rainy",
  "humidity": 85,
  "windSpeed": 15,
  "timestamp": "2024-11-02T12:00:00.000Z"
}
```
 
**Notes:**
- City name is passed as a query parameter
- City names are case-sensitive (may vary by implementation)
- If city parameter is missing -> 400 Bad Request
- If city is not found in the database -> 404 Not Found
- Weather data may be real-time or cached depending on implementation
- Temperature is typically in Celsius (may vary by implementation)
- The response includes current weather conditions and additional metrics
 
---
 
## Common Status Codes
 
| Status Code | Description |
|-------------|-------------|
| 200 OK | Request succeeded, weather data returned |
| 400 Bad Request | Missing or invalid city parameter |
| 404 Not Found | City not found in database |
| 500 Internal Server Error | Server error occurred or weather service unavailable |
 
## Query Parameters
 
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| city | string | Yes | Name of the city to get weather for |
 
## Response Fields
 
| Field | Type | Description |
|-------|------|-------------|
| city | string | Name of the city |
| temperature | number | Current temperature (in Celsius) |
| condition | string | Weather condition (e.g., Sunny, Cloudy, Rainy) |
| humidity | number | Humidity percentage (0-100) |
| windSpeed | number | Wind speed (in km/h) |
| timestamp | string | ISO 8601 timestamp of the weather data |
 
## Testing in Postman
 
1. Import the Postman collection: `Weather.postman_collection.json`
2. Set the `base_url` variable in Postman environment or collection variables
3. Test with different city names: London, New York, Tokyo
4. Verify response status codes match expected values
5. Check that response contains all expected fields
6. Test error scenarios:
   - Request without city parameter (should return 400)
   - Request with non-existent city (should return 404)
7. Test with city names containing spaces (e.g., "New York", "Los Angeles")
8. Verify that temperature, humidity, and windSpeed are numeric values
9. Check that timestamp is in valid ISO 8601 format
 
## Additional Testing Scenarios
 
- Test with different case variations of city names (london, LONDON, London)
- Test with special characters in city names (if supported)
- Test with very long city names
- Test with numeric-only "city names" (should fail gracefully)
- Test with empty string for city parameter (should return 400)
