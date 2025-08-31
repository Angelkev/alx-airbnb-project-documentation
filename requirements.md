# Backend Requirement Specifications - Airbnb Clone

This document specifies the technical and functional requirements for key backend features of the Airbnb Clone project.

---

## 1. User Authentication

### Overview
Provides secure registration, login, and session management for users.

### Functional Requirements
- Users must be able to register with email, password, and optional profile details.
- Users must be able to log in with valid credentials.
- Passwords must be hashed before storage.
- Support session management using JWT (JSON Web Tokens).
- Unauthorized users cannot access protected endpoints.

### API Endpoints
- `POST /api/v1/auth/register`
  - **Input:** `{ "email": "string", "password": "string", "name": "string" }`
  - **Output:** `{ "id": "uuid", "email": "string", "token": "jwt" }`
  - **Validation:** Email format, password ≥ 8 chars, unique email.

- `POST /api/v1/auth/login`
  - **Input:** `{ "email": "string", "password": "string" }`
  - **Output:** `{ "token": "jwt", "expiresIn": "3600s" }`
  - **Validation:** Email must exist, password must match hashed record.

### Performance Criteria
- Auth endpoints must respond within **< 500ms** under normal load.
- Tokens must expire within 1 hour.

---

## 2. Property Management

### Overview
Allows hosts to create, update, and manage property listings.

### Functional Requirements
- Hosts can create property listings with details (title, description, location, price, availability).
- Hosts can update or delete their own listings.
- Guests can view property listings and filter/search them.

### API Endpoints
- `POST /api/v1/properties`
  - **Input:** `{ "title": "string", "description": "string", "location": "string", "price": "number", "availability": "boolean" }`
  - **Output:** `{ "id": "uuid", "title": "string", "created_at": "date" }`
  - **Validation:** Required fields, price > 0, valid location string.

- `GET /api/v1/properties`
  - **Input (Query Params):** `?location=string&minPrice=number&maxPrice=number`
  - **Output:** `[ { "id": "uuid", "title": "string", "location": "string", "price": "number" } ]`

- `PUT /api/v1/properties/{id}`
  - **Input:** `{ "title": "string?", "description": "string?", "price": "number?" }`
  - **Output:** `{ "id": "uuid", "updated_at": "date" }`

- `DELETE /api/v1/properties/{id}`
  - **Output:** `{ "message": "Property deleted successfully" }`

### Performance Criteria
- Property search results must load within **< 800ms**.
- API must support pagination for search results.

---

## 3. Booking System

### Overview
Enables guests to book available properties and manage reservations.

### Functional Requirements
- Guests can create booking requests for available properties.
- System must check property availability before confirming booking.
- Hosts and guests can view booking details.
- Guests can cancel bookings before the start date.

### API Endpoints
- `POST /api/v1/bookings`
  - **Input:** `{ "property_id": "uuid", "start_date": "date", "end_date": "date" }`
  - **Output:** `{ "id": "uuid", "status": "confirmed|pending", "total_price": "number" }`
  - **Validation:** Dates must not overlap existing booking, end_date > start_date.

- `GET /api/v1/bookings/{id}`
  - **Output:** `{ "id": "uuid", "property": {...}, "guest": {...}, "status": "string" }`

- `DELETE /api/v1/bookings/{id}`
  - **Output:** `{ "message": "Booking canceled" }`
  - **Validation:** Only the guest who created the booking can cancel.

### Performance Criteria
- Booking confirmation must complete in **< 1s**.
- Prevent double booking through transactional locking.

---

# Notes
- All API responses must use JSON.
- All endpoints should be secured with authentication middleware.
- Database must enforce constraints (unique emails, valid foreign keys).
