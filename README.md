# Auctioneering Platform

A full-stack web application for online auctions similar to eBay, built with React, Spring Boot, and PostgreSQL.

## Features

- **User Authentication**: Registration and login with email/username
- **Role-Based Access**: Auctioneer, Bidder, and Admin roles
- **Auction Management**: Create, list, and manage auctions
- **Bidding System**: Place bids on active auctions with real-time updates
- **Automated Auction Closing**: Automatic closure when auction period ends
- **Notifications**: Bid notifications and auction status updates
- **Dashboards**: Role-specific dashboards for each user type

## Tech Stack

- **Frontend**: React with React Router and Axios
- **Backend**: Spring Boot with Spring Security and Spring Data JPA
- **Database**: PostgreSQL
- **Authentication**: JWT (JSON Web Tokens)
- **Containerization**: Docker & Docker Compose

## Project Structure

```
Auction-Platform/
├── frontend/                 # React application
│   ├── src/
│   │   ├── components/      # React components
│   │   ├── pages/           # Page components
│   │   ├── services/        # API services
│   │   ├── context/         # Context API
│   │   ├── App.js
│   │   └── index.js
│   ├── package.json
│   └── Dockerfile
├── backend/                  # Spring Boot application
│   ├── src/
│   │   ├── main/java/com/auction/
│   │   │   ├── config/      # Configuration classes
│   │   │   ├── controller/  # REST controllers
│   │   │   ├── service/     # Business logic
│   │   │   ├── repository/  # Data access layer
│   │   │   ├── entity/      # JPA entities
│   │   │   ├── dto/         # Data transfer objects
│   │   │   ├── security/    # JWT & Security
│   │   │   ├── exception/   # Custom exceptions
│   │   │   └── Application.java
│   │   └── resources/
│   │       └── application.properties
│   ├── pom.xml
│   └── Dockerfile
├── database/                 # Database schema
│   └── init.sql
├── docker-compose.yml
└── .gitignore
```

## Getting Started

### Prerequisites
- Docker and Docker Compose
- Node.js (for local frontend development)
- Java 17+ (for local backend development)

### Running with Docker Compose

```bash
docker-compose up
```

This will start:
- PostgreSQL database (port 5432)
- Spring Boot backend (port 8080)
- React frontend (port 3000)

### Local Development Setup

#### Backend
```bash
cd backend
mvn clean install
mvn spring-boot:run
```

#### Frontend
```bash
cd frontend
npm install
npm start
```

## API Endpoints

See `BACKEND_API.md` for detailed API documentation.

## Database Schema

See `database/init.sql` for database initialization script.

## User Flow

1. **Registration**: New users create account and choose role (Auctioneer/Bidder)
2. **Login**: Authenticate with email/username and password
3. **Dashboard**: Directed to role-specific dashboard
4. **Auctioneer**: Create auctions, manage listings, view bids
5. **Bidder**: View active auctions, place bids, track bidding history
6. **Admin**: Monitor platform activity and manage users

## License

MIT
