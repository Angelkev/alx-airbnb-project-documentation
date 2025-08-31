# Features & Functionalities — Airbnb Clone (Backend)

**Objective:** document the key backend features and functionalities required by the Airbnb Clone project.

![Features Diagram](features-diagram.png)

## Overview
This document lists the backend features the project must support, organized by domain. Use it as a blueprint for API design, data modeling, and tests.

---

## Core Features

### 1. Authentication & Authorization
- User signup (email/password) and login.
- Password reset & email verification.
- Social login / OAuth (optional).
- JWT or session tokens.
- Role management: `guest`, `host`, `admin`.
- Rate limiting and brute force protection on auth endpoints.

### 2. User Management
- User profile CRUD (name, contact, photo, verified status).
- Host onboarding flow (ID verification, payout details).
- Account settings, language/currency preferences.

### 3. Property / Listing Management
- Create / read / update / delete listings.
- Listing metadata: title, description, images, location (lat/long), address, amenities, house rules, cancellation policy.
- Pricing: base price, cleaning fee, discounts, seasonal pricing.
- Upload and manage images (with validation & CDN integration).

### 4. Search & Filters
- Search by location (geospatial), date range, number of guests.
- Filters: price range, property type, amenities, instant book, host rating.
- Pagination and sorting (relevance, price low→high, rating).

### 5. Availability & Calendar
- Per-listing availability calendar.
- Block/unblock dates, minimum/maximum stay.
- Prevent double-booking with atomic availability checks.
- Calendar sync (iCal) — optional.

### 6. Booking System (Reservation)
- Booking workflow: initiate, hold / tentative, confirm after payment.
- Availability checks & conflict resolution.
- Cancellation policies and automated refunds.
- Guest/Host booking statuses.
- Booking history, invoices.

### 7. Payments & Payouts
- Integration with payment gateway (e.g., Stripe).
- Payment intents, capture, refunds, disputes.
- Host payouts (stripe connect or equivalent).
- Handling fees, taxes, and currency conversion basics.
- Secure storage of minimal payment metadata; never store card numbers.

### 8. Reviews & Ratings
- Post-stay reviews and ratings (guest reviews host and host reviews guest).
- Moderation / report inappropriate content.

### 9. Messaging & Notifications
- In-app messaging between guest & host.
- Email notifications (booking confirmed, reminders, cancellations).
- Push notifications or webhooks for critical events.

### 10. Admin & Moderation
- Dashboard: users, listings, bookings, revenue stats.
- Tools to remove listings, suspend users, handle disputes.

### 11. Integrations & Extras
- Email provider (SendGrid/Mailgun), storage (S3), payment gateway, analytics.
- Webhooks for payment events and external sync.

---

## Data model (high-level entities)
- **User** (id, name, email, role, verification_status)
- **Property/Listing** (id, host_id, title, description, location, price, rules)
- **Availability** (listing_id, date, status)
- **Booking/Reservation** (id, listing_id, guest_id, start_date, end_date, status, total_amount)
- **Payment** (id, booking_id, provider_id, status, amount)
- **Review** (id, booking_id, author_id, rating, comment)
- **Message** (id, from_id, to_id, booking_id, body, timestamp)

---

## Important API endpoints (examples)
- `POST /api/auth/signup`
- `POST /api/auth/login`
- `POST /api/auth/password-reset`
- `GET  /api/users/:id`
- `POST /api/listings` (host)
- `GET  /api/listings?location=&start=&end=&guests=`
- `GET  /api/listings/:id`
- `POST /api/listings/:id/book`
- `POST /api/payments/intent`
- `POST /api/webhooks/payments`

---

## Non-functional requirements & security
- Input validation and strong authentication mechanisms.
- Use HTTPS, secure cookies, CSP, and other hardening headers.
- Logging, monitoring, and alerting for production.
- Unit and integration tests for booking/payment flows.
- Data backups and disaster recovery plan.

---

## Testing & CI/CD
- Unit tests for business logic.
- Integration tests for booking/payment flows (use test/sandbox of payment provider).
- Continuous Integration (GitHub Actions) running tests and linters.
- Deploy to staging then production (Automated deployments).

---

## Notes
- Where payment data is involved, follow PCI compliance best practices: never store raw card details.
- Design the booking flow carefully to avoid race conditions (use DB transactions or optimistic locking).
