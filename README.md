# VeloX - Sport Bike Exchange (Backend API)

Welcome to the backend repository for VeloX, a comprehensive C2C marketplace and offline event management platform for sport bicycles. This RESTful API serves as the core engine powering the platform, managing secure transactions, internal wallets, and complex inspection workflows.

Frontend Repository: [Link to Frontend Repo]
Live API Documentation (Swagger): [Link to Swagger/Postman]

## Core Engineering Features

Secure Payment Integration: Seamless integration with VNPay Sandbox for e-wallet top-ups and withdrawals, implementing robust IPN (Instant Payment Notification) webhooks and SHA-512 checksum validation to guarantee financial data integrity.

Complex State-Machine Workflows: Automated state transitions for the entire transaction lifecycle (Deposits -> Inspections -> Payouts -> Refunds/Compensations).

Concurrency & Race Condition Handling: Engineered strict database locks and transaction isolation levels to resolve critical race conditions in payment processing, effectively preventing double-spending and database deadlocks.

Event Sourcing Architecture: Designed a normalized PostgreSQL schema utilizing the Event Sourcing pattern for Wallets, Reservations, and Transactions. This ensures strict ACID compliance, seamless reconciliation, and transparent financial auditing.

Authentication & Authorization: Implemented robust security using Spring Security and JWT (JSON Web Tokens), providing Role-Based Access Control (RBAC) for Admins, Inspectors, Buyers, and Sellers.

Robust File Handling: Integrated with Cloudinary API for secure, optimized storage and retrieval of bicycle and inspection images.

## Tech Stack

Framework: Java 17, Spring Boot 3.x

Database: PostgreSQL, Spring Data JPA / Hibernate

Security: Spring Security, JWT (JSON Web Tokens)

Payment Gateway: VNPay API

Cloud Storage: Cloudinary

Deployment: Render (with custom CORS configurations)

## Architecture Overview

```
src/main/java/com/velox/api/
├── config/         # Security, CORS, VNPay, and Swagger configurations
├── controllers/    # REST API endpoints
├── dtos/           # Data Transfer Objects for request/response payloads
├── entities/       # JPA Entities mapping to PostgreSQL tables
├── enums/          # State machine enums (TransactionState, BikeStatus, Role)
├── exceptions/     # Global exception handler and custom exceptions
├── repositories/   # Spring Data JPA repositories
├── security/       # JWT filters, entry points, and user details service
├── services/       # Core business logic and transaction management
└── utils/          # Helper classes (Hash validation, Date formatters)
```

## Local Setup & Installation

Prerequisites

Java Development Kit (JDK) 17 or higher

PostgreSQL installed and running locally

Maven

Installation Steps

1. Clone the repository:
```
git clone [https://github.com/your-username/velox-backend.git](https://github.com/your-username/velox-backend.git)
cd velox-backend
```

2. Database Setup:

Create a new PostgreSQL database named velox_db.
```
CREATE DATABASE velox_db;
```

3. Configure Environment Variables:
Create an application-dev.yml or update application.properties in the src/main/resources folder:
```
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/velox_db
    username: your_postgres_username
    password: your_postgres_password
  jpa:
    hibernate:
      ddl-auto: update

jwt:
  secret: your_super_secret_jwt_key_here
  expiration: 86400000

vnpay:
  tmnCode: your_vnpay_tmn_code
  hashSecret: your_vnpay_hash_secret
  url: [https://sandbox.vnpayment.vn/paymentv2/vpcpay.html](https://sandbox.vnpayment.vn/paymentv2/vpcpay.html)
  returnUrl: http://localhost:5173/payment/return
```

