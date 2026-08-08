# AI Development Guide

## Purpose

This document provides context and development rules for AI assistants working on this e-commerce application.

The AI should read this document before making changes to the codebase.

## Project Goal

Build a production-oriented e-commerce web application that is:

* Maintainable
* Secure
* Responsive
* Accessible
* Performant
* Easy to extend
* Suitable for AI-assisted development

## Technology Stack

### Core

* Next.js
* React
* TypeScript
* Tailwind CSS
* PostgreSQL
* Prisma
* Node.js LTS
* Git
* GitHub

### Planned Integrations

* Authentication
* Stripe for payments
* Email provider
* Product image/object storage
* Production hosting

Do not introduce additional technologies without a clear reason.

## General Development Principles

1. Prefer simple solutions over unnecessary abstraction.
2. Follow existing project patterns before introducing new patterns.
3. Use TypeScript strictly.
4. Avoid `any` unless there is a documented reason.
5. Keep business logic separate from UI presentation.
6. Keep server-only logic on the server.
7. Never expose secrets or private credentials to the client.
8. Validate external input at application boundaries.
9. Do not trust values received from the browser.
10. Make the smallest reasonable change required by the task.
11. Avoid adding dependencies unless they provide meaningful value.
12. Do not rewrite unrelated code.

## Before Changing Code

The AI should:

1. Inspect the relevant files.
2. Understand the existing architecture.
3. Search for existing implementations of similar functionality.
4. Identify affected database models and server operations.
5. Check existing naming and coding conventions.
6. Determine whether the requested behavior already exists.
7. Plan the smallest coherent implementation.

## After Changing Code

The AI should:

1. Run the relevant checks.
2. Check TypeScript errors.
3. Check lint errors.
4. Run relevant tests when available.
5. Review the resulting diff.
6. Check for accidental unrelated changes.
7. Check loading, error, and empty states.
8. Check authorization requirements.
9. Check mobile responsiveness for UI changes.
10. Summarize the changes and remaining concerns.

## E-commerce Security Rules

Never trust client-provided:

* Product prices
* Discounts
* Inventory quantities
* Order totals
* User roles
* Payment status
* Product ownership
* Administrative permissions

All sensitive values must be determined or verified server-side.

### Prices

The browser may send a product ID and quantity, but the server must retrieve the current product price.

Never calculate the authoritative order total using a price supplied by the client.

### Inventory

Inventory must be checked and modified server-side.

Inventory-sensitive operations should use appropriate database transactions or concurrency controls.

### Payments

Payment state must not be considered successful simply because the browser reports success.

The server must verify payment status using the payment provider's trusted server-side mechanism.

### Orders

Orders must preserve the information necessary to understand what the customer purchased at the time of purchase.

Historical orders should not depend on mutable product data.

## Authentication and Authorization

Authentication determines who the user is.

Authorization determines what the user is allowed to do.

Never rely solely on hiding UI elements to protect functionality.

Sensitive operations must perform authorization checks on the server.

Examples:

* Admin product management
* Order management
* User management
* Refund operations
* Inventory changes

## UI Requirements

User-facing features should consider:

* Loading state
* Error state
* Empty state
* Mobile layout
* Keyboard accessibility
* Focus states
* Semantic HTML
* Form validation
* Useful error messages

## Code Quality

Prefer:

* Small components
* Clear names
* Explicit data flow
* Reusable utilities when genuinely useful
* Server-side validation
* Strong TypeScript types
* Predictable error handling

Avoid:

* Huge components
* Deeply nested conditional logic
* Duplicate business rules
* Global state when local/server state is sufficient
* Premature abstractions
* Unnecessary dependencies

## Git Conventions

Use focused commits.

Examples:

```text
feat: add product listing
feat: add shopping cart
fix: prevent negative cart quantity
fix: validate product inventory
refactor: extract product card
docs: update database documentation
```

Do not commit:

* `.env` files containing secrets
* API keys
* Passwords
* Private credentials
* Build output
* Local database files

## AI Behavior

When a task is ambiguous, inspect the repository and existing conventions before inventing a new approach.

When multiple solutions are possible, prefer the solution that:

1. Fits the existing architecture.
2. Has fewer moving parts.
3. Is easy to maintain.
4. Is secure.
5. Is easy for another developer to understand.

Do not make broad architectural changes for a small feature.

If a requested change conflicts with an existing architectural rule, explain the conflict before making a major change.
