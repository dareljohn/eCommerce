# docs/coding-standards.md

# Coding Standards

## General

Code should prioritize clarity over cleverness.

Prefer code that another developer can understand quickly.

## TypeScript

Use TypeScript throughout the application.

Avoid `any`.

Prefer explicit domain types.

Use:

```text
unknown
```

when data is genuinely unknown and narrow it before use.

## Naming

Use:

```text
PascalCase
```

for:

* React components
* Classes
* Types
* Interfaces

Use:

```text
camelCase
```

for:

* Variables
* Functions
* Parameters

Use:

```text
kebab-case
```

for:

* URL slugs
* Documentation filenames
* CSS-oriented naming where appropriate

Use descriptive names.

Avoid:

```text
data
thing
temp
foo
bar
x
```

unless the scope genuinely makes the meaning obvious.

## React

Prefer functional components.

Keep components focused.

Avoid very large components.

Extract components when:

* A section has a distinct responsibility.
* It is reused.
* It has meaningful internal complexity.
* Extraction improves readability.

## Props

Avoid passing excessive unrelated props.

Prefer clear domain-specific props.

## State

Use the smallest appropriate state scope.

Prefer:

```text
local state
```

when state is only needed by one component.

Use shared/global state only when there is a real cross-component requirement.

## Server State

Prefer server-side data fetching and framework-supported mechanisms where appropriate rather than duplicating server state into client state.

## Error Handling

Errors should be meaningful and actionable.

Do not silently swallow exceptions.

## Comments

Write comments explaining:

* Why something is necessary.
* Non-obvious business rules.
* Security requirements.
* Workarounds.
* External constraints.

Do not comment obvious code.

Bad:

```text
// Increment counter
counter++;
```

Good:

```text
// Inventory is decremented only after payment confirmation.
```

## Functions

Functions should have one clear responsibility.

Avoid functions that simultaneously:

* Fetch data
* Validate input
* Render UI
* Send email
* Update multiple unrelated domains

## Database

Database queries belong in server-side code.

Do not query PostgreSQL directly from client components.

## Imports

Prefer project-consistent import aliases.

Avoid unnecessarily deep relative imports.

## Formatting

Use the repository's configured formatter and lint rules.

Do not manually introduce a conflicting style.

---

# docs/environment-configuration.md

# Environment Configuration

## Purpose

This document describes application configuration that differs between environments.

## Environment Files

Expected local development files may include:

```text
.env
.env.local
.env.example
```

The exact Next.js environment-file strategy should follow the framework's conventions.

## Secrets

Real secrets must never be committed.

Examples:

```text
DATABASE_URL
AUTH_SECRET
STRIPE_SECRET_KEY
STRIPE_WEBHOOK_SECRET
EMAIL_API_KEY
```

## Example File

`.env.example` should contain variable names without real credentials.

Example:

```text
DATABASE_URL=

AUTH_SECRET=

STRIPE_SECRET_KEY=
STRIPE_WEBHOOK_SECRET=

EMAIL_API_KEY=
```

## Client Exposure

Only variables intentionally designed for browser exposure should use the appropriate public environment-variable convention.

Never expose:

* Database credentials
* Private API keys
* Authentication secrets
* Stripe secret keys
* Webhook secrets

## Local Development

Local development should use isolated development resources.

Do not point local development at production databases unless explicitly required and safely controlled.

## Database

The application should eventually use:

```text
DATABASE_URL
```

for Prisma/PostgreSQL connection configuration.

## Third-Party Services

External integrations should be configured through environment variables.

Do not hard-code credentials.

## Configuration Validation

As the application grows, required environment variables should be validated at application startup or integration boundaries.

Missing required configuration should produce a clear developer-facing error.

## Production

Production configuration must be managed by the deployment platform's secure environment-variable system.

Do not commit production credentials to Git.
