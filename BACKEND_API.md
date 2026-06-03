# Backend API Documentation

## Base URL
```
http://localhost:8080/api
```

## Authentication
All protected endpoints require a JWT token in the Authorization header:
```
Authorization: Bearer <your_jwt_token>
```

## Endpoints

### Authentication Endpoints

#### Register User
```
POST /auth/register
Content-Type: application/json

{
  "email": "user@example.com",
  "username": "username",
  "password": "password123",
  "role": "BIDDER" // or "AUCTIONEER"
}

Response 201:
{
  "id": 1,
  "email": "user@example.com",
  "username": "username",
  "role": "BIDDER",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

#### Login
```
POST /auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "password123"
}

Response 200:
{
  "id": 1,
  "email": "user@example.com",
  "username": "username",
  "role": "BIDDER",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

### Auction Endpoints

#### Create Auction (Auctioneer only)
```
POST /auctions
Authorization: Bearer <token>
Content-Type: application/json

{
  "title": "Vintage Camera",
  "description": "Used vintage camera in good condition",
  "imageUrl": "https://example.com/image.jpg",
  "startingPrice": 50.00,
  "endDate": "2024-12-31T23:59:59Z"
}

Response 201:
{
  "id": 1,
  "title": "Vintage Camera",
  "description": "Used vintage camera in good condition",
  "imageUrl": "https://example.com/image.jpg",
  "startingPrice": 50.00,
  "currentHighestBid": 50.00,
  "status": "ACTIVE",
  "auctioneer": { "id": 1, "username": "auctioneer1" },
  "createdAt": "2024-12-01T10:00:00Z"
}
```

#### Get All Active Auctions
```
GET /auctions?status=ACTIVE&page=0&size=20
Authorization: Bearer <token>

Response 200:
{
  "content": [
    {
      "id": 1,
      "title": "Vintage Camera",
      "description": "Used vintage camera in good condition",
      "imageUrl": "https://example.com/image.jpg",
      "startingPrice": 50.00,
      "currentHighestBid": 75.00,
      "status": "ACTIVE",
      "auctioneer": { "id": 1, "username": "auctioneer1" },
      "createdAt": "2024-12-01T10:00:00Z"
    }
  ],
  "totalElements": 10,
  "totalPages": 1,
  "currentPage": 0
}
```

#### Get Auction by ID
```
GET /auctions/{auctionId}
Authorization: Bearer <token>

Response 200:
{
  "id": 1,
  "title": "Vintage Camera",
  "description": "Used vintage camera in good condition",
  "imageUrl": "https://example.com/image.jpg",
  "startingPrice": 50.00,
  "currentHighestBid": 75.00,
  "highestBidder": { "id": 2, "username": "bidder1" },
  "status": "ACTIVE",
  "auctioneer": { "id": 1, "username": "auctioneer1" },
  "bids": [
    { "id": 1, "bidder": "bidder1", "amount": 75.00, "time": "2024-12-01T12:00:00Z" },
    { "id": 2, "bidder": "bidder2", "amount": 60.00, "time": "2024-12-01T11:00:00Z" }
  ],
  "createdAt": "2024-12-01T10:00:00Z"
}
```

#### Update Auction (Auctioneer only)
```
PUT /auctions/{auctionId}
Authorization: Bearer <token>
Content-Type: application/json

{
  "title": "Vintage Camera - Updated",
  "description": "Updated description",
  "imageUrl": "https://example.com/new-image.jpg"
}

Response 200: Updated auction object
```

#### Delete Auction (Auctioneer only)
```
DELETE /auctions/{auctionId}
Authorization: Bearer <token>

Response 204: No Content
```

### Bid Endpoints

#### Place Bid
```
POST /bids
Authorization: Bearer <token>
Content-Type: application/json

{
  "auctionId": 1,
  "bidAmount": 80.00
}

Response 201:
{
  "id": 3,
  "auctionId": 1,
  "bidder": { "id": 2, "username": "bidder1" },
  "bidAmount": 80.00,
  "bidTime": "2024-12-01T13:00:00Z"
}
```

#### Get Bid History for Auction
```
GET /auctions/{auctionId}/bids
Authorization: Bearer <token>

Response 200:
[
  { "id": 1, "bidder": "bidder1", "amount": 75.00, "time": "2024-12-01T12:00:00Z" },
  { "id": 2, "bidder": "bidder2", "amount": 60.00, "time": "2024-12-01T11:00:00Z" }
]
```

### Dashboard Endpoints

#### Get Auctioneer Dashboard
```
GET /dashboard/auctioneer
Authorization: Bearer <token>

Response 200:
{
  "myAuctions": [
    {
      "id": 1,
      "title": "Vintage Camera",
      "numberOfBidders": 5,
      "highestBidAmount": 150.00,
      "status": "ACTIVE"
    }
  ],
  "totalAuctions": 10,
  "totalRevenue": 5000.00
}
```

#### Get Bidder Dashboard
```
GET /dashboard/bidder
Authorization: Bearer <token>

Response 200:
{
  "activeAuctions": [
    {
      "id": 1,
      "title": "Vintage Camera",
      "currentBid": 80.00,
      "highestBid": 150.00,
      "status": "ACTIVE",
      "endsAt": "2024-12-31T23:59:59Z"
    }
  ],
  "myBids": [
    {
      "id": 1,
      "auctionTitle": "Vintage Camera",
      "bidAmount": 80.00,
      "isHighestBid": false
    }
  ],
  "wonAuctions": []
}
```

### Notification Endpoints

#### Get Notifications
```
GET /notifications?page=0&size=20
Authorization: Bearer <token>

Response 200:
[
  {
    "id": 1,
    "auctionTitle": "Vintage Camera",
    "type": "BID_PLACED",
    "message": "A new bid has been placed on your auction",
    "isRead": false,
    "createdAt": "2024-12-01T13:00:00Z"
  }
]
```

#### Mark Notification as Read
```
PUT /notifications/{notificationId}/read
Authorization: Bearer <token>

Response 200: Updated notification object
```

## Error Responses

### 400 Bad Request
```json
{
  "error": "Bad Request",
  "message": "Validation failed",
  "details": {
    "email": "Invalid email format"
  }
}
```

### 401 Unauthorized
```json
{
  "error": "Unauthorized",
  "message": "Invalid or missing token"
}
```

### 403 Forbidden
```json
{
  "error": "Forbidden",
  "message": "You do not have permission to perform this action"
}
```

### 404 Not Found
```json
{
  "error": "Not Found",
  "message": "Resource not found"
}
```

### 409 Conflict
```json
{
  "error": "Conflict",
  "message": "Email or username already exists"
}
```

### 500 Internal Server Error
```json
{
  "error": "Internal Server Error",
  "message": "An unexpected error occurred"
}
```
