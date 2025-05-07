# SecureShop API Documentation

## Overview
This document provides the API specifications for the SecureShop mobile application. The API follows REST architectural style with JSON as the data exchange format and JWT for authentication.

## Base Information
- **Base URL**: `https://api.secureshop.com/v1`
- **Data Format**: JSON
- **Authentication**: Bearer token (JWT)
- **Encryption**: HTTPS with TLS 1.3

## Authentication Endpoints

### Register New User
```
POST /auth/register
```

**Headers:**
```
Content-Type: application/json
```

**Request Body:**
```json
{
  "fullName": "string",
  "email": "string",
  "password": "string",
  "phoneNumber": "string"
}
```

**Response (200):**
```json
{
  "status": "success",
  "message": "User registered successfully",
  "data": {
    "userId": "string",
    "email": "string",
    "fullName": "string",
    "createdAt": "timestamp"
  }
}
```

### Login
```
POST /auth/login
```

**Headers:**
```
Content-Type: application/json
```

**Request Body:**
```json
{
  "email": "string",
  "password": "string"
}
```

**Response (200):**
```json
{
  "status": "success",
  "data": {
    "accessToken": "string",
    "refreshToken": "string",
    "expiresIn": 3600,
    "user": {
      "userId": "string",
      "email": "string",
      "fullName": "string",
      "phoneNumber": "string",
      "role": "string"
    }
  }
}
```

### Refresh Token
```
POST /auth/refresh
```

**Headers:**
```
Content-Type: application/json
Authorization: Bearer {refreshToken}
```

**Response (200):**
```json
{
  "status": "success",
  "data": {
    "accessToken": "string",
    "refreshToken": "string",
    "expiresIn": 3600
  }
}
```

### Logout
```
POST /auth/logout
```

**Headers:**
```
Authorization: Bearer {accessToken}
```

**Response (200):**
```json
{
  "status": "success",
  "message": "Logged out successfully"
}
```

### Forgot Password
```
POST /auth/forgot-password
```

**Headers:**
```
Content-Type: application/json
```

**Request Body:**
```json
{
  "email": "string"
}
```

**Response (200):**
```json
{
  "status": "success",
  "message": "Password reset link sent to your email"
}
```

### Reset Password
```
POST /auth/reset-password
```

**Headers:**
```
Content-Type: application/json
```

**Request Body:**
```json
{
  "token": "string",
  "newPassword": "string",
  "confirmPassword": "string"
}
```

**Response (200):**
```json
{
  "status": "success",
  "message": "Password reset successful"
}
```

## User Profile Endpoints

### Get User Profile
```
GET /users/profile
```

**Headers:**
```
Authorization: Bearer {accessToken}
```

**Response (200):**
```json
{
  "status": "success",
  "data": {
    "userId": "string",
    "email": "string",
    "fullName": "string",
    "phoneNumber": "string",
    "addresses": [
      {
        "id": "string",
        "title": "string",
        "name": "string",
        "street": "string",
        "city": "string",
        "state": "string",
        "zipCode": "string",
        "country": "string",
        "isDefault": "boolean"
      }
    ],
    "paymentMethods": [
      {
        "id": "string",
        "type": "string",
        "lastFourDigits": "string",
        "cardHolderName": "string",
        "expiryMonth": "number",
        "expiryYear": "number",
        "isDefault": "boolean"
      }
    ]
  }
}
```

### Update Profile
```
PUT /users/profile
```

**Headers:**
```
Authorization: Bearer {accessToken}
Content-Type: application/json
```

**Request Body:**
```json
{
  "fullName": "string",
  "phoneNumber": "string"
}
```

**Response (200):**
```json
{
  "status": "success",
  "message": "Profile updated successfully",
  "data": {
    "userId": "string",
    "email": "string",
    "fullName": "string",
    "phoneNumber": "string"
  }
}
```

### Add Address
```
POST /users/addresses
```

**Headers:**
```
Authorization: Bearer {accessToken}
Content-Type: application/json
```

**Request Body:**
```json
{
  "title": "string",
  "name": "string",
  "street": "string",
  "city": "string",
  "state": "string",
  "zipCode": "string",
  "country": "string",
  "isDefault": "boolean"
}
```

**Response (201):**
```json
{
  "status": "success",
  "message": "Address added successfully",
  "data": {
    "id": "string",
    "title": "string",
    "name": "string",
    "street": "string",
    "city": "string",
    "state": "string",
    "zipCode": "string",
    "country": "string",
    "isDefault": "boolean"
  }
}
```

### Add Payment Method
```
POST /users/payment-methods
```

**Headers:**
```
Authorization: Bearer {accessToken}
Content-Type: application/json
```

**Request Body:**
```json
{
  "cardNumber": "string",
  "cardHolderName": "string",
  "expiryMonth": "number",
  "expiryYear": "number",
  "cvv": "string",
  "isDefault": "boolean"
}
```

**Response (201):**
```json
{
  "status": "success",
  "message": "Payment method added successfully",
  "data": {
    "id": "string",
    "type": "string",
    "lastFourDigits": "string",
    "cardHolderName": "string",
    "expiryMonth": "number",
    "expiryYear": "number",
    "isDefault": "boolean"
  }
}
```

## Product Endpoints

### Get Products List
```
GET /products
```

**Optional Query Parameters:**
- `page`: number (current page)
- `limit`: number (items per page)
- `category`: string (filter by category)
- `minPrice`: number (minimum price)
- `maxPrice`: number (maximum price)
- `sort`: string (e.g., 'price_asc', 'price_desc', 'newest')
- `search`: string (search for products)

**Response (200):**
```json
{
  "status": "success",
  "data": {
    "products": [
      {
        "id": "string",
        "name": "string",
        "description": "string",
        "price": "number",
        "discountedPrice": "number",
        "images": ["string"],
        "category": "string",
        "rating": "number",
        "reviewCount": "number",
        "inStock": "boolean",
        "createdAt": "timestamp"
      }
    ],
    "pagination": {
      "currentPage": "number",
      "totalPages": "number",
      "totalItems": "number",
      "limit": "number"
    }
  }
}
```

### Get Product Details
```
GET /products/{productId}
```

**Response (200):**
```json
{
  "status": "success",
  "data": {
    "id": "string",
    "name": "string",
    "description": "string",
    "price": "number",
    "discountedPrice": "number",
    "images": ["string"],
    "category": "string",
    "specifications": {
      "key1": "value1",
      "key2": "value2"
    },
    "rating": "number",
    "reviewCount": "number",
    "inStock": "boolean",
    "stockCount": "number",
    "createdAt": "timestamp"
  }
}
```

### Get Product Reviews
```
GET /products/{productId}/reviews
```

**Optional Query Parameters:**
- `page`: number
- `limit`: number
- `sort`: string ('newest', 'highest_rating', 'lowest_rating')

**Response (200):**
```json
{
  "status": "success",
  "data": {
    "reviews": [
      {
        "id": "string",
        "userId": "string",
        "userName": "string",
        "rating": "number",
        "comment": "string",
        "createdAt": "timestamp"
      }
    ],
    "pagination": {
      "currentPage": "number",
      "totalPages": "number",
      "totalItems": "number",
      "limit": "number"
    }
  }
}
```

### Add Product Review
```
POST /products/{productId}/reviews
```

**Headers:**
```
Authorization: Bearer {accessToken}
Content-Type: application/json
```

**Request Body:**
```json
{
  "rating": "number",
  "comment": "string"
}
```

**Response (201):**
```json
{
  "status": "success",
  "message": "Review added successfully",
  "data": {
    "id": "string",
    "userId": "string",
    "userName": "string",
    "rating": "number",
    "comment": "string",
    "createdAt": "timestamp"
  }
}
```

## Cart Endpoints

### Get Cart
```
GET /cart
```

**Headers:**
```
Authorization: Bearer {accessToken}
```

**Response (200):**
```json
{
  "status": "success",
  "data": {
    "items": [
      {
        "id": "string",
        "productId": "string",
        "name": "string",
        "image": "string",
        "price": "number",
        "discountedPrice": "number",
        "quantity": "number",
        "subtotal": "number"
      }
    ],
    "summary": {
      "subtotal": "number",
      "shipping": "number",
      "tax": "number",
      "discount": "number",
      "total": "number"
    }
  }
}
```

### Add Item to Cart
```
POST /cart/items
```

**Headers:**
```
Authorization: Bearer {accessToken}
Content-Type: application/json
```

**Request Body:**
```json
{
  "productId": "string",
  "quantity": "number"
}
```

**Response (201):**
```json
{
  "status": "success",
  "message": "Item added to cart",
  "data": {
    "id": "string",
    "productId": "string",
    "name": "string",
    "image": "string",
    "price": "number",
    "discountedPrice": "number",
    "quantity": "number",
    "subtotal": "number"
  }
}
```

### Update Cart Item Quantity
```
PUT /cart/items/{itemId}
```

**Headers:**
```
Authorization: Bearer {accessToken}
Content-Type: application/json
```

**Request Body:**
```json
{
  "quantity": "number"
}
```

**Response (200):**
```json
{
  "status": "success",
  "message": "Cart item updated",
  "data": {
    "id": "string",
    "quantity": "number",
    "subtotal": "number"
  }
}
```

### Remove Item from Cart
```
DELETE /cart/items/{itemId}
```

**Headers:**
```
Authorization: Bearer {accessToken}
```

**Response (200):**
```json
{
  "status": "success",
  "message": "Item removed from cart"
}
```

### Apply Coupon
```
POST /cart/apply-coupon
```

**Headers:**
```
Authorization: Bearer {accessToken}
Content-Type: application/json
```

**Request Body:**
```json
{
  "couponCode": "string"
}
```

**Response (200):**
```json
{
  "status": "success",
  "message": "Coupon applied successfully",
  "data": {
    "couponCode": "string",
    "discountAmount": "number",
    "summary": {
      "subtotal": "number",
      "shipping": "number",
      "tax": "number",
      "discount": "number",
      "total": "number"
    }
  }
}
```

## Order Endpoints

### Create Order
```
POST /orders
```

**Headers:**
```
Authorization: Bearer {accessToken}
Content-Type: application/json
```

**Request Body:**
```json
{
  "addressId": "string",
  "paymentMethodId": "string",
  "couponCode": "string (optional)"
}
```

**Response (201):**
```json
{
  "status": "success",
  "message": "Order created successfully",
  "data": {
    "orderId": "string",
    "orderNumber": "string",
    "status": "string",
    "createdAt": "timestamp",
    "paymentStatus": "string",
    "summary": {
      "subtotal": "number",
      "shipping": "number",
      "tax": "number",
      "discount": "number",
      "total": "number"
    }
  }
}
```

### Get Orders List
```
GET /orders
```

**Headers:**
```
Authorization: Bearer {accessToken}
```

**Optional Query Parameters:**
- `page`: number
- `limit`: number
- `status`: string (filter by status)

**Response (200):**
```json
{
  "status": "success",
  "data": {
    "orders": [
      {
        "id": "string",
        "orderNumber": "string",
        "status": "string",
        "total": "number",
        "itemCount": "number",
        "createdAt": "timestamp"
      }
    ],
    "pagination": {
      "currentPage": "number",
      "totalPages": "number",
      "totalItems": "number",
      "limit": "number"
    }
  }
}
```

### Get Order Details
```
GET /orders/{orderId}
```

**Headers:**
```
Authorization: Bearer {accessToken}
```

**Response (200):**
```json
{
  "status": "success",
  "data": {
    "id": "string",
    "orderNumber": "string",
    "status": "string",
    "paymentStatus": "string",
    "createdAt": "timestamp",
    "items": [
      {
        "id": "string",
        "productId": "string",
        "name": "string",
        "image": "string",
        "price": "number",
        "quantity": "number",
        "subtotal": "number"
      }
    ],
    "shippingAddress": {
      "name": "string",
      "street": "string",
      "city": "string",
      "state": "string",
      "zipCode": "string",
      "country": "string"
    },
    "paymentMethod": {
      "type": "string",
      "lastFourDigits": "string"
    },
    "summary": {
      "subtotal": "number",
      "shipping": "number",
      "tax": "number",
      "discount": "number",
      "total": "number"
    },
    "trackingInfo": {
      "provider": "string",
      "trackingNumber": "string",
      "estimatedDelivery": "timestamp"
    }
  }
}
```

## Category Endpoints

### Get Categories
```
GET /categories
```

**Response (200):**
```json
{
  "status": "success",
  "data": {
    "categories": [
      {
        "id": "string",
        "name": "string",
        "image": "string",
        "productCount": "number"
      }
    ]
  }
}
```

## Error Handling

### Standard Error Format
```json
{
  "status": "error",
  "code": "string",
  "message": "string",
  "details": "object (optional)"
}
```

### Common Status Codes
- `400`: Bad Request
- `401`: Unauthorized
- `403`: Forbidden
- `404`: Not Found
- `422`: Unprocessable Entity
- `429`: Too Many Requests
- `500`: Internal Server Error

## Security Considerations

1. **HTTPS Only**: All API communication must use HTTPS
2. **Token Expiry**: Access tokens expire in 1 hour, refresh tokens in 30 days
3. **Brute Force Protection**: Account lockout after 5 failed login attempts
4. **Rate Limiting**: 60 requests/minute per user
5. **PCI DSS Compliance**: Credit card data storage follows PCI DSS standards
6. **Trusted Devices**: IP addresses and device fingerprinting for session validation

## Additional Notes

- CORS requests properly implemented
- All text values provided according to ISO standards
- Dates in ISO 8601 format (YYYY-MM-DDTHH:MM:SSZ)
- Currency in USD by default
