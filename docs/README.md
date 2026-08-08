# docs/README.md

# Project Documentation

## Purpose

This directory contains the authoritative documentation for the e-commerce application.

AI agents should use these documents as persistent project context.

## Documentation Index

### AI Guidance

`ai-agent-guide.md`

Defines how AI agents should understand, modify, test, and document the project.

### Project Context

`project-context.md`

Defines the project's purpose, target users, major capabilities, constraints, and current state.

### Architecture

`architecture.md`

Defines application architecture, boundaries, data flow, server/client responsibilities, and project structure.

### Technology Stack

`technology-stack.md`

Defines the selected technologies and the reasons for using them.

### Coding Standards

`coding-standards.md`

Defines naming, TypeScript, React, Next.js, file organization, error handling, and general coding conventions.

### Database Design

`database-design.md`

Defines entities, relationships, constraints, indexes, monetary values, and database rules.

### API Conventions

`api-conventions.md`

Defines Server Actions, Route Handlers, validation, authentication, authorization, errors, and external integrations.

### Security

`security-guidelines.md`

Defines security requirements for authentication, authorization, payments, secrets, input validation, and sensitive data.

### Testing

`testing-strategy.md`

Defines unit, integration, end-to-end, database, and UI testing practices.

### UI/UX

`ui-ux-guidelines.md`

Defines visual, responsive, accessibility, interaction, and component conventions.

### Development Workflow

`development-workflow.md`

Defines how features should be planned, implemented, tested, reviewed, committed, and documented.

### Roadmap

`roadmap.md`

Defines planned features and implementation phases.

### Architectural Decisions

`architectural-decisions.md`

Records important decisions and the reasoning behind them.

### Domain Glossary

`domain-glossary.md`

Defines business terminology so developers and AI agents use consistent language.

### Environment Configuration

`environment-configuration.md`

Documents environment variables, configuration requirements, and local/production differences.

## Documentation Authority

When documentation conflicts with an implementation, the agent should:

1. Determine whether the documentation is outdated.
2. Inspect recent code and Git history when useful.
3. Avoid silently changing architecture.
4. Update the documentation if the implementation represents an intentional decision.

## Documentation Rule

Documentation should describe decisions and conventions that an AI agent would otherwise have to rediscover from the codebase.

Do not document trivial implementation details that can be understood directly from readable code.

---

# docs/ai-agent-guide.md

# AI Agent Development Guide

## Mission

The AI agent's role is to help develop the e-commerce application while preserving:

* Correctness
* Security
* Maintainability
* Consistency
* Performance
* Accessibility
* Simplicity

The agent should behave like a careful senior developer working within an existing codebase.

## Priority Order

When deciding how to implement something, prioritize:

1. Explicit user requirements
2. Security and data integrity
3. Existing project architecture
4. Existing project conventions
5. Documented architectural decisions
6. Existing reusable code
7. Simplicity
8. Performance
9. Generic best practices

Generic advice must not override an explicit project decision without discussion.

## Repository Exploration

Before implementing a non-trivial task, inspect:

```text
Project documentation
        ↓
Relevant routes/pages
        ↓
Relevant components
        ↓
Server logic
        ↓
Database models
        ↓
Tests
```

Search for existing implementations before creating new ones.

For example, before creating a new product card:

```text
Search:
- ProductCard
- product-card
- product components
- existing product pages
```

Reuse an existing component if appropriate.

## Minimal Change Principle

Do not turn:

```text
"Add a button"
```

into:

```text
"Refactor the entire UI system"
```

unless the existing system prevents the requested feature.

Small, focused changes are easier to review, test, revert, and understand.

## Avoid Premature Abstraction

Do not create a generic abstraction simply because two pieces of code look similar.

Abstract when:

* The behavior is genuinely shared.
* The abstraction has a clear responsibility.
* It reduces meaningful duplication.
* It does not make the code harder to understand.

## Client/Server Boundary

Default to server-side execution.

Use client-side code only when required for:

* Browser APIs
* Local interactive state
* Event handlers
* Client-only libraries
* Real-time interaction

Avoid adding `"use client"` to large component trees unnecessarily.

## Business Logic

Business logic must not depend on presentation.

Bad:

```text
React component
    ↓
calculates order total
    ↓
creates order
```

Preferred:

```text
React UI
    ↓
server operation
    ↓
business logic
    ↓
database
```

## Financial Logic

Never trust client-side:

* Price
* Discount
* Tax
* Shipping cost
* Total

The server must calculate authoritative financial values.

## Inventory

Inventory must be checked server-side.

Consider concurrency when multiple customers can purchase the same product.

Do not assume:

```text
check inventory
↓
later update inventory
```

is automatically safe.

Use appropriate database transaction/concurrency mechanisms.

## Authentication

Never identify a user based solely on browser-provided identifiers.

Use the authenticated server-side session.

## Authorization

Every protected mutation must verify permissions.

Examples:

```text
Admin creates product
Admin edits product
Admin changes inventory
Customer views private order
Customer edits own account
```

Authorization must occur on the server.

## Error Handling

Expected failures should be handled deliberately.

Examples:

* Product not found
* Product unavailable
* Insufficient inventory
* Invalid form
* Unauthorized access
* Payment failure
* Duplicate operation

Do not use broad silent catches.

Bad:

```text
try {
  ...
} catch {
}
```

## Loading and Empty States

Every asynchronous user-facing feature should consider:

```text
Loading
Success
Empty
Error
```

## Accessibility

UI changes should consider:

* Semantic HTML
* Keyboard navigation
* Focus states
* Labels
* Form errors
* Screen-reader context
* Sufficient contrast
* Touch target size

## Performance

Do not optimize prematurely.

Prefer:

* Server rendering where appropriate
* Efficient database queries
* Pagination for large datasets
* Proper image optimization
* Avoiding unnecessary client JavaScript
* Avoiding unnecessary network requests

Measure before introducing complex optimization.

## Dependency Policy

Before installing a package:

1. Determine whether the functionality already exists.
2. Check whether the platform/framework provides it.
3. Consider implementation complexity.
4. Consider package maintenance.
5. Consider bundle size and security.
6. Add the dependency only if justified.

## AI Hallucination Prevention

The agent must not assume:

* A package is installed.
* A function exists.
* A database field exists.
* An API endpoint exists.
* An environment variable exists.
* A service is configured.
* A test passed.
* A migration was executed.
* A deployment succeeded.

Inspect the repository or verify the command before making such claims.

## When Requirements Are Ambiguous

If ambiguity materially affects architecture, data integrity, security, or user-visible behavior, ask for clarification.

If the ambiguity is minor, choose the simplest reasonable implementation and document the assumption.

## Completion Checklist

Before declaring a task complete:

```text
[ ] Requirement implemented
[ ] Existing behavior preserved
[ ] TypeScript passes
[ ] Lint passes
[ ] Tests pass where applicable
[ ] Security reviewed
[ ] Authorization reviewed
[ ] Loading state considered
[ ] Error state considered
[ ] Empty state considered
[ ] Mobile behavior considered
[ ] Accessibility considered
[ ] Documentation updated if necessary
[ ] Git diff reviewed
```

---

# docs/project-context.md

# Project Context

## Project Name

E-commerce Store

## Project Type

Full-stack e-commerce web application.

## Primary Goal

Build a reliable online store where customers can:

* Browse products
* Search products
* Filter products
* View product details
* Manage a shopping cart
* Create an account
* Manage addresses
* Checkout
* Pay securely
* View order history
* Track order status

Administrators should be able to:

* Manage products
* Manage categories
* Manage inventory
* Manage orders
* Manage users
* Monitor store activity

## Target Users

### Customers

Customers browse products and purchase items.

### Administrators

Administrators manage the store and its operational data.

## Core Business Domains

The application contains these primary domains:

```text
Catalog
Products
Categories
Inventory
Customers
Authentication
Cart
Checkout
Payments
Orders
Administration
```

## Primary User Journey

```text
Visit store
    ↓
Browse products
    ↓
View product
    ↓
Add to cart
    ↓
Review cart
    ↓
Sign in / provide customer information
    ↓
Checkout
    ↓
Payment
    ↓
Order confirmation
    ↓
Order history
```

## Administrative Journey

```text
Admin login
    ↓
Dashboard
    ↓
Products / Categories / Inventory / Orders / Users
    ↓
Perform authorized operation
    ↓
Database update
    ↓
Audit/operational result
```

## Project Priorities

Priority order:

1. Correctness
2. Security
3. Data integrity
4. User experience
5. Maintainability
6. Performance
7. Scalability

Do not sacrifice correctness or security for premature optimization.

## Current Development Stage

The project is currently in the foundation/setup stage.

Completed:

* Git repository
* GitHub repository
* Node.js LTS
* Next.js
* React
* TypeScript
* Tailwind CSS
* App Router
* Initial AI documentation

Next major milestone:

```text
PostgreSQL
    ↓
Prisma
    ↓
Initial database schema
    ↓
Database migrations
    ↓
Seed data
```

## Initial Product Scope

The first version should focus on:

* Product catalog
* Product details
* Categories
* Shopping cart
* Authentication
* Checkout
* Payments
* Orders
* Basic administration

Advanced functionality should be added only after the core purchasing flow works reliably.

## Out of Scope Initially

Unless explicitly requested, do not prematurely build:

* Complex recommendation engines
* Microservices
* Event-driven architecture
* Advanced analytics
* Multi-region infrastructure
* Multiple databases
* Custom payment processing
* Custom authentication infrastructure

Start with a modular monolith.

## Architecture Philosophy

The application should begin as a modular monolith.

Conceptually:

```text
One application
    |
    +-- Catalog
    +-- Customers
    +-- Cart
    +-- Checkout
    +-- Orders
    +-- Admin
    |
    +-- PostgreSQL
```

The architecture can evolve if scale or requirements justify it.

## Non-Functional Requirements

The application should aim for:

* Secure authentication
* Server-side authorization
* Reliable financial calculations
* Consistent inventory
* Responsive UI
* Accessibility
* SEO-friendly product pages
* Good performance
* Maintainable code
* Clear error handling
* Testable business logic

## Important Principle

The database and server-side business logic are authoritative.

The browser is an untrusted client.

---

# docs/architecture.md

# Application Architecture

## Architectural Style

The application uses a modular monolith architecture.

The initial application should remain a single deployable application unless there is a demonstrated reason to split services.

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
     Presentation              Server Logic
        React                 Business Rules
     Components                Validation
        Pages                  Authorization
          |                         |
          +------------+------------+
                       |
                       v
                    Prisma
                       |
                       v
                  PostgreSQL
```

External services:

```text
                       Next.js
                          |
             +------------+------------+
             |            |            |
             v            v            v
          Database      Stripe       Email
          PostgreSQL                Provider
```

## Application Layers

### Presentation

Responsible for:

* Pages
* Layouts
* Components
* Forms
* Visual states
* User interactions

Should not contain authoritative business logic.

### Application

Responsible for:

* Business operations
* Validation
* Authorization
* Cart operations
* Checkout
* Order creation
* Inventory management

### Data

Responsible for:

* Prisma
* Database queries
* Transactions
* Persistence

### Integrations

Responsible for:

* Payments
* Authentication provider
* Email
* Image storage
* Other external services

## Suggested Project Structure

```text
app/
├── page.tsx
├── products/
├── cart/
├── checkout/
├── account/
├── admin/
└── api/

components/
├── ui/
├── layout/
├── products/
├── cart/
├── checkout/
├── account/
└── admin/

lib/
├── auth/
├── db/
├── products/
├── cart/
├── orders/
├── payments/
├── email/
├── validation/
└── utils/

prisma/
├── schema.prisma
├── migrations/
└── seed.ts

docs/
```

This is a guideline, not a requirement to create every directory immediately.

## Server Components

Server Components should be the default.

Use them for:

* Product pages
* Product listings
* Server-side data fetching
* SEO-relevant content
* Database-backed rendering

## Client Components

Use Client Components only where necessary.

Examples:

* Interactive cart controls
* Image galleries
* Dialogs
* Client-side UI state
* Browser APIs
* Interactive forms

Do not use Client Components simply because they are familiar.

## Data Flow

### Product Page

```text
Request
 ↓
Next.js route
 ↓
Server Component
 ↓
Product data access
 ↓
Prisma
 ↓
PostgreSQL
 ↓
Render
```

### Cart Mutation

```text
Browser
 ↓
Server Action
 ↓
Authentication
 ↓
Validation
 ↓
Business logic
 ↓
Prisma
 ↓
PostgreSQL
 ↓
Updated state
```

### Payment

```text
Customer
 ↓
Checkout
 ↓
Server validation
 ↓
Server calculates total
 ↓
Payment provider
 ↓
Webhook
 ↓
Server verifies webhook
 ↓
Order state update
```

## Module Boundaries

Business domains should remain conceptually separate.

Example:

```text
lib/products/
lib/cart/
lib/orders/
lib/payments/
lib/auth/
```

A cart module should not directly implement payment-provider behavior.

An order module should not contain UI rendering.

## Dependency Direction

Prefer:

```text
UI
 ↓
Application/business logic
 ↓
Data/integration layer
 ↓
External systems
```

Avoid:

```text
Database
 ↓
UI
```

or allowing low-level infrastructure to depend on presentation code.

## Transactions

Use database transactions when multiple changes must remain consistent.

Examples:

* Creating an order and order items
* Updating inventory and recording a purchase
* Creating related records atomically

## Scalability

Do not introduce microservices initially.

A modular monolith provides:

* Lower operational complexity
* Easier local development
* Easier debugging
* Easier deployment
* Strong module boundaries

Split services only when actual requirements justify it.
