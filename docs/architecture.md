# Application Architecture

## Overview

This application is a full-stack e-commerce application built with Next.js.

The initial architecture intentionally avoids a separate Express or Node.js API server.

Next.js provides:

* React-based UI
* Routing
* Server-side rendering
* Server Components
* Server Actions where appropriate
* Route Handlers where appropriate
* Server-side business logic

## High-Level Architecture

```text
                    Browser
                       |
                       v
                  Next.js App
                       |
          +------------+------------+
          |                         |
          v                         v
      UI Layer                Server Layer
      React                   Business Logic
      Components              Validation
      Pages                   Authorization
                              |
                              v
                         Prisma ORM
                              |
                              v
                         PostgreSQL
```

External services will be connected from the server side:

```text
                       Next.js
                          |
          +---------------+---------------+
          |               |               |
          v               v               v
      PostgreSQL        Stripe          Email
       + Prisma                       Provider
```

## Application Layers

### 1. Presentation Layer

Responsible for:

* Pages
* Layouts
* Components
* Forms
* Loading states
* Error states
* User interaction

Presentation code should not contain complex business rules.

### 2. Application Layer

Responsible for:

* Business operations
* Validation
* Authorization
* Cart operations
* Order creation
* Inventory operations
* Payment orchestration

### 3. Data Layer

Responsible for:

* Database access
* Prisma queries
* Transactions
* Persistence

Database-specific logic should not be scattered throughout UI components.

### 4. External Services

External integrations include:

* Payment provider
* Authentication
* Email
* Image storage

Secrets and credentials must remain server-side.

## Suggested Project Structure

```text
ecommerce-store/
│
├── app/
│   ├── page.tsx
│   │
│   ├── products/
│   │   ├── page.tsx
│   │   └── [slug]/
│   │       └── page.tsx
│   │
│   ├── cart/
│   │   └── page.tsx
│   │
│   ├── checkout/
│   │   └── page.tsx
│   │
│   ├── account/
│   │   └── ...
│   │
│   ├── admin/
│   │   └── ...
│   │
│   └── api/
│       └── ...
│
├── components/
│   ├── ui/
│   ├── layout/
│   ├── products/
│   ├── cart/
│   └── checkout/
│
├── lib/
│   ├── db/
│   ├── auth/
│   ├── payments/
│   ├── email/
│   └── validation/
│
├── prisma/
│   ├── schema.prisma
│   └── migrations/
│
├── public/
│
├── docs/
│
├── .env
├── .env.example
├── package.json
└── tsconfig.json
```

The structure may evolve as the application grows.

Do not create directories merely because they appear in this document. Create them when the corresponding functionality is implemented.

## Server vs Client Components

Prefer Server Components by default.

Use Client Components when the UI requires browser-side interactivity or client-only APIs.

Examples that may require Client Components:

* Interactive cart controls
* Product image galleries
* Dropdown menus requiring client state
* Modal dialogs
* Browser APIs
* Client-side form interactions

Do not add `"use client"` unnecessarily.

## Data Flow

A typical product page should follow:

```text
Browser
   |
   v
Next.js Page
   |
   v
Server-side data access
   |
   v
Prisma
   |
   v
PostgreSQL
```

A typical cart operation:

```text
User interaction
       |
       v
Server-side operation
       |
       v
Validate user
       |
       v
Validate product
       |
       v
Validate quantity
       |
       v
Check inventory
       |
       v
Update cart
       |
       v
Return result
```

## Order Creation

Order creation should follow a server-controlled process:

```text
Cart
 |
 v
Validate user
 |
 v
Retrieve products from database
 |
 v
Validate quantities
 |
 v
Check inventory
 |
 v
Calculate authoritative totals
 |
 v
Create payment session/payment intent
 |
 v
Confirm payment through trusted mechanism
 |
 v
Create/finalize order
 |
 v
Update inventory
 |
 v
Send confirmation
```

Exact implementation may evolve with the payment architecture.

## Environment Variables

Secrets must be stored in environment variables.

Example:

```text
DATABASE_URL=
AUTH_SECRET=
STRIPE_SECRET_KEY=
STRIPE_WEBHOOK_SECRET=
EMAIL_API_KEY=
```

`.env` must never be committed when it contains real secrets.

Maintain `.env.example` with variable names but no secrets.

## Architectural Rule

Before introducing a new library, service, architectural layer, or state-management system, determine whether the existing Next.js architecture already solves the problem.

The goal is a simple system that can grow without unnecessary complexity.
