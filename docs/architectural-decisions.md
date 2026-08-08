docs/architectural-decisions.md
Architectural Decisions

This document records significant technical decisions.

ADR-001 — Use Next.js
Status

Accepted

Decision

Use Next.js as the primary application framework.

Reason

Next.js provides:

React
Routing
Server Components
Server-side rendering
Server Actions
Route Handlers
Production optimization

It allows the initial application to remain a modular monolith without requiring a separate frontend and backend deployment.

ADR-002 — Use TypeScript
Status

Accepted

Decision

Use TypeScript throughout the application.

Reason

Type safety is valuable for:

Product models
Orders
Cart state
API contracts
Database interactions
Component props
ADR-003 — Use PostgreSQL
Status

Accepted

Decision

Use PostgreSQL as the primary database.

Reason

E-commerce data has strong relationships between:

Users
Products
Categories
Carts
Orders
Inventory
Addresses

A relational database is a strong fit.

ADR-004 — Use Prisma
Status

Accepted

Decision

Use Prisma for database access.

Reason

Prisma provides:

Type-safe database access
Schema management
Migrations
Strong TypeScript integration
Good developer experience
ADR-005 — Modular Monolith
Status

Accepted

Decision

Start as a modular monolith.

Reason

The initial application does not justify the operational complexity of microservices.

Modules should have clear responsibilities so the architecture can evolve later if required.

ADR-006 — Server Is Authoritative
Status

Accepted

Decision

Server-side business logic is authoritative for financial and security-sensitive information.

Applies To
Prices
Discounts
Inventory
Order totals
Payment state
Permissions
Reason

The browser is an untrusted environment.