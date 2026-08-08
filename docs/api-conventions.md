# API and Server Interface Guide

## Purpose

This document defines conventions for communication between the browser, Next.js application, server-side business logic, database, and external services.

The application should prefer server-side operations for anything involving:

* Authentication
* Authorization
* Product pricing
* Inventory
* Orders
* Payments
* Administrative operations
* Sensitive data

---

# General Rules

1. Treat all browser input as untrusted.
2. Validate all external input on the server.
3. Authenticate users before protected operations.
4. Authorize every protected operation on the server.
5. Retrieve authoritative product and pricing information from the database.
6. Never accept an authoritative order total from the client.
7. Never expose secrets to the browser.
8. Do not expose database errors or stack traces to users.
9. Return predictable errors.
10. Prefer existing Next.js mechanisms before introducing a separate API layer.

---

# Choosing a Server Interface

Use the simplest appropriate Next.js mechanism.

## Server Components

Use Server Components for server-side data fetching needed to render a page when browser-side interactivity is not required.

Example:

```text
Page
  ↓
Server-side data function
  ↓
Prisma
  ↓
PostgreSQL
```

Do not fetch your own application's data through an HTTP API from a Server Component unless there is a specific reason.

---

# Server Actions

Use Server Actions for appropriate mutations originating from application UI.

Good examples:

* Add item to cart
* Remove cart item
* Update cart quantity
* Update user profile
* Update address

Server Actions must still perform:

```text
Authentication
      ↓
Authorization
      ↓
Input validation
      ↓
Business logic
      ↓
Database operation
```

Never assume a Server Action is safe merely because it is not directly visible as a normal API endpoint.

---

# Route Handlers

Use Route Handlers when an HTTP endpoint is actually required.

Good examples:

* Payment webhooks
* External service callbacks
* Public APIs
* Integrations with external applications
* Endpoints consumed by clients outside the Next.js application

Do not create an API endpoint simply to make an internal Server Component call it.

---

# Input Validation

Every external input must be validated at the server boundary.

Validation should consider:

* Required fields
* Data types
* String length
* Numeric ranges
* Allowed enum values
* IDs
* Quantities
* Business rules

For example, a cart request might contain:

```json
{
  "productId": "product-id",
  "quantity": 2
}
```

The server must verify:

```text
productId exists
      ↓
product is active
      ↓
quantity is valid
      ↓
quantity is available
      ↓
operation is authorized
```

---

# Authentication

Authentication determines **who the user is**.

Never trust a browser-provided `userId`.

Bad:

```text
Browser
  ↓
userId
  ↓
Server
```

Preferred:

```text
Browser
  ↓
Authenticated session
  ↓
Server
  ↓
Current authenticated user
```

Protected operations must obtain the current user from the server-side authentication mechanism.

---

# Authorization

Authorization determines **what the authenticated user is allowed to do**.

Authentication alone is not sufficient.

Example:

```text
Is the user authenticated?
        ↓
Is the user authorized?
        ↓
Perform operation
```

Admin functionality must verify the user's administrative role server-side.

Examples:

* Create product
* Edit product
* Delete/archive product
* Change inventory
* View all orders
* Manage users

---

# Product Operations

The browser may send:

```json
{
  "productId": "123",
  "quantity": 2
}
```

The server must retrieve authoritative information from the database.

This includes:

* Product existence
* Product status
* Current price
* Inventory
* Product availability
* Applicable business rules

The client must never determine the authoritative price.

---

# Cart Operations

A typical add-to-cart operation:

```text
User interaction
      ↓
productId + quantity
      ↓
Server
      ↓
Validate input
      ↓
Authenticate user
      ↓
Retrieve product
      ↓
Check product status
      ↓
Check inventory
      ↓
Update cart
      ↓
Return result
```

The server should not trust a cart total supplied by the browser.

---

# Checkout

Checkout is a server-controlled operation.

Expected flow:

```text
Cart
 ↓
Authenticate user
 ↓
Load cart from database
 ↓
Load products from database
 ↓
Validate product availability
 ↓
Validate inventory
 ↓
Calculate authoritative prices
 ↓
Calculate totals
 ↓
Create payment operation
 ↓
Process payment
 ↓
Create/finalize order
 ↓
Update inventory
```

The total displayed by the browser is informational only.

The authoritative total must be calculated on the server.

---

# Payments

Payment processing must be handled through the selected payment provider.

The application must never:

* Store raw card numbers
* Store card security codes
* Trust browser-only payment success
* Mark an order as paid solely because the client says payment succeeded

Payment state should be verified through trusted server-side mechanisms.

---

# Payment Webhooks

Payment webhooks are server-to-server events.

Webhook handling should:

1. Receive the request.
2. Verify the provider signature.
3. Parse the event.
4. Determine whether the event has already been processed.
5. Update the appropriate order/payment state.
6. Return an appropriate HTTP response.

Webhook processing should be idempotent where possible.

Example:

```text
Stripe
  ↓
Webhook
  ↓
Verify signature
  ↓
Check event
  ↓
Check idempotency
  ↓
Update order/payment
```

---

# Error Categories

Use consistent error categories where appropriate:

```text
VALIDATION_ERROR
AUTHENTICATION_REQUIRED
FORBIDDEN
NOT_FOUND
CONFLICT
INSUFFICIENT_INVENTORY
PAYMENT_ERROR
INTERNAL_ERROR
```

Do not expose internal implementation details.

Bad:

```text
PrismaClientKnownRequestError:
Unique constraint failed on...
```

Better:

```text
A product with this identifier already exists.
```

Detailed technical information should remain in server logs.

---

# JSON Response Convention

For JSON endpoints, use predictable structures.

Successful response:

```json
{
  "data": {},
  "error": null
}
```

Error response:

```json
{
  "data": null,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "The request is invalid."
  }
}
```

The exact response structure may evolve as the application grows.

---

# HTTP Status Codes

Where Route Handlers are used, use appropriate HTTP status codes.

Common examples:

```text
200 OK
201 Created
204 No Content
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
409 Conflict
422 Unprocessable Entity
429 Too Many Requests
500 Internal Server Error
```

Do not use `200 OK` for every failure.

---

# Idempotency

Operations that may be retried must avoid creating duplicate side effects.

This is particularly important for:

* Payments
* Payment webhooks
* Order creation
* Inventory updates
* Email sending

Example:

```text
Webhook received
      ↓
Has event already been processed?
      ↓
YES → return safely
      ↓
NO
      ↓
Process event
      ↓
Record processed event
```

---

# Database Access

UI components should not contain complex Prisma queries.

Prefer a clear server-side data-access boundary.

Example conceptual structure:

```text
app/
  products/
    page.tsx

lib/
  products/
    queries.ts
    mutations.ts
```

The exact organization may change as the project grows.

---

# Logging

Logs should contain enough information to diagnose problems without exposing sensitive information.

Never log:

* Passwords
* Authentication secrets
* API keys
* Payment credentials
* Session secrets
* Unnecessary sensitive personal information

When logging identifiers, use the minimum information necessary.

---

# API Security Checklist

Before considering a server operation complete, verify:

* [ ] Input is validated.
* [ ] Authentication is checked when required.
* [ ] Authorization is checked when required.
* [ ] Database values are authoritative.
* [ ] Client-provided prices are ignored.
* [ ] Client-provided totals are ignored.
* [ ] Secrets remain server-side.
* [ ] Errors do not expose internal details.
* [ ] Duplicate execution is handled where necessary.
* [ ] Database operations are transactional when required.

---

# API Documentation Maintenance

When changing a server interface:

1. Update the implementation.
2. Update this document if the contract changes.
3. Update affected consumers.
4. Add or update tests.
5. Consider backward compatibility if external clients exist.

If a change affects architecture, also update:

```text
docs/architecture.md
```

If a change affects the database model, also update:

```text
docs/database.md
```

If a change affects implementation priorities, update:

```text
docs/roadmap.md
```
