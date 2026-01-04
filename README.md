# CampusConnect Interest Management System

This repository contains the Interest & RSVP Management System for the CampusConnect platform.
It provides backend APIs and frontend integration for managing event RSVPs, capacity limits, waitlists, and user interest tracking.

The system supports both limited-capacity and unlimited-capacity events, automatically handling waitlisting, cancellations, and RSVP status updates in a transactional and race-condition-safe manner.

---

## Features

- RSVP to events with capacity enforcement
- Support for unlimited-capacity events (interest-only)
- Automatic waitlisting for full events
- Cancel RSVP or interest
- View all available events (including full events for waitlisting)
- “My Events” view showing RSVP status:
  - RSVP’d
  - Interested
  - Waitlisted
- Transaction-safe backend logic to prevent race conditions

---

## Tech Stack

- Frontend: Next.js (App Router, React)
- Backend: Next.js API Routes (Node.js)
- Database: PostgreSQL
- ORM / Queries: SQL + Drizzle (schema definitions)
- Containerization: Docker & Docker Compose

---

## Prerequisites

- Node.js: https://nodejs.org/
- Next.js: https://nextjs.org
- Docker: https://www.docker.com
- golang-migrate: https://github.com/golang-migrate/migrate

---

## Getting Started

```bash
# Install dependencies
npm install

# Configure environment variables
cp .env.example .env

# Start Docker services (PostgreSQL, etc.)
docker compose up -d

# Run database migrations (if applicable)
migrate -path migrations -database "$DATABASE_URL" up

# Start the development server
npm run dev
```

The application will be available at:
http://localhost:3000

---

## API Endpoints

Events:
- GET /api/events/available  
  Returns events that are unlimited capacity, not full, or full but available for waitlisting.

RSVP:
- POST /api/rsvp/:eventId  
  Creates an RSVP and assigns one of: RSVP, INTERESTED, or WAITLISTED.

- GET /api/rsvp/:userId  
  Returns all events the user is associated with, including RSVP status.

- DELETE /api/rsvp/:eventId  
  Cancels RSVP or interest and updates capacity when applicable.

---

## Database Design Notes

- capacity = NULL indicates an unlimited-capacity event
- rsvp_count is only updated for limited-capacity events
- Supported RSVP statuses:
  - RSVP
  - INTERESTED
  - WAITLISTED

All capacity-related updates are handled transactionally to prevent race conditions during concurrent requests.

---

## Notes

- This project was developed as part of CPS714 – CampusConnect.
